# Workflows
Workflows are another powerful feature of the PrivCloud platform that allow you to create logic and actions for
your specific use, industry, and environment. You are able to define the events (API actions) that trigger the 
workflow along with the exact steps you would like the workflow to take, inclusive of conditionals and 
input/output analysis. Here are some great use cases for Workflows:

  
| Use Case      | Example |
| ----------- | ----------- |
| Governance Alerting      | Be notified via text or email when a record is updated.       |
| Custom Governance   | Drop any new records that contain social         |
| Data Lake Ingestion   | Automate data copy events to your data lake.        |
| Custom User Management   | Disallow creation of any users who are not created with a valid company email address.        |
| Third Party Integration   | Integration with a third-party provider quickly and easily.        |

---

## Examples
Our [repository](https://github.com/privcloud-com/privcloud-documentation/tree/main/examples/workflows) has some good examples
that can be used to get started with Workflows.

---

## Hard Limits for Workflows
Workflows have a few hard limits that you need to be aware of. 

 * **Supported Actions** : Workflows can be run just prior to any create (POST), update (PUT), and delete (DELETE) action within the system. Workflows 
cannot be run when reading (GET) an object or entity.

 * **Workflow Timeout** : Workflows can run for a total of 20 seconds before the entire workflow will timeout. If a workflow times out, the system will
cancel the currently running step and all subsequent steps.

 * **Step Timeout** : Workflows steps can run for a maximum of 10 seconds before the step will timeout. If a workflow step times out, the system will
cancel the currently running step and all subsequent steps.

---

## JSON Structure
Workflows are defined in json format and are structured as follows:

  * version (required) : This is the version of the JSON definition structure, currently only version 1 is supported.
  * comment (optional) : An optional comment describing the workflow.
  * scope (required) : A dictionary that defines the action/event that triggers the workflow and specific.  
  * start_at (required) : The name of the step to start at.
  * steps (required) : A dictionary that defines that steps within the workflow.

#### Example Workflow JSON Definition
```json
{
    "version": 1,
    "comment": "This is an example workflow that shows how to call a lambda function and, based on the response, send an email or end the workflow",
    "scope": {
        "on_action": "record:create",
        "filter": {
            "field": "{{record.container_guid}}",
            "operator": "in",
            "value": [
                "5d0a5876dc8d4963a9b84655e06943f8",
                "1ef08b54fae94f49a725f06f8002b81d"
            ]
        }
    },
    "start_at": "call_lambda",
    "steps": {
        "call_lambda": {
            "comment": "This step calls a lambda function in AWS, passing the decrypted record provided to the record:create action to the function",
            "type": "lambda",
            "parameters": {
                "arn": "arn:aws:lambda:us-east-2:123456789012:function:my-function:1",
                "role": "arn:aws:iam::123456789012:role/lambda-role"
            },
            "next": "lambda_response_choice"
        },
        "lambda_response_choice": {
            "comment": "This step recieves the response from the lambda function and, based on the 'success' and 'count' response attributes, send an email or stops the workflow",
            "type": "if",
            "parameters": {
                "conditions": [
                    {
                        "comment": "This condition checks the response of the lambda function for 'success' being true and 'count' being less than 10",
                        "type": "condition",
                        "condition": {
                            "and": {
                                "condition1": {
                                    "field": "{{call_lambda.result}}",
                                    "operator": "=",
                                    "value": "success"
                                },
                                "condition2": {
                                    "field": "{{call_lambda.count}}",
                                    "operator": "<",
                                    "value": 10
                                }
                            }
                        },
                        "next": "send_email"
                    },
                    {
                        "comment": "This condition checks the response of the lambda function for 'success' being true and 'count' being between 11 and 20",
                        "type": "condition",
                        "condition": {
                            "and": {
                                "condition1": {
                                    "field": "{{call_lambda.result}}",
                                    "operator": "=",
                                    "value": "success"
                                },
                                "and": {
                                    "condition2": {
                                        "field": "{{call_lambda.count}}",
                                        "operator": ">",
                                        "value": 11
                                    },
                                    "condition3": {
                                        "field": "{{call_lambda.count}}",
                                        "operator": "<",
                                        "value": 20
                                    }
                                }
                            }
                        },
                        "next": "send_email"
                    },
                    {
                        "comment": "This is the default condition if no other conditions are met",
                        "type": "stop"
                    }
                ]
            }
        },
        "send_email": {
            "comment": "This step sends an email to the security team, notifying them of the record creation",
            "type": "email",
            "next": "none",
            "parameters": {
                "subject": "Record {{record.guid}} Created After Lambda Count {{lambda_result.count}}",
                "body": "This is an example body with interpolated variables like record.guid={{record.guid}} and lambda_result.result={{lambda_result}} and lambda_result.count={{lambda_result.count}}. Variables passed as output from previous functions are always available for interpolation.\n\nFrom PrivCloud",
                "recipients": [
                    "danny@privcloud.com",
                    "{{call_lambda.admin_email}}"
                ]
            }
        }
    }
}
```

---

### Scope Structure
Workflows start with a scope. A scope defined the PrivCloud action that the workflow will hook into and that will trigger 
the workflow. Optionally, filters can be provided to further limit the types of calls to the API that would trigger the
workflow.

  * on_action (required) : The action that will trigger the workflow. 
  * filter (optional) : A filter can be provided to analyze the associate object or entity and limit the actions that would trigger the workflow.

#### Example Scope Definition
```json
    "scope": {
        "on_action": "{{record:create}}",
        "filter": {
            "field": "record.container_guid",
            "operator": "in",
            "value": [
                "5d0a5876dc8d4963a9b84655e06943f8",
                "1ef08b54fae94f49a725f06f8002b81d"
            ]
        }
    }
```

---

### Step Structure
Steps are where the logic of the workflow is defined. Each step has a name, a type, and depending on the type, required parameters.
Steps can refer to previously executed step output for analysis or string interpolation. 

  * name (required): Each step must have a name. This name is what is used to refer to its output in subsequent steps.
  * comment (optional) : An optional comment describing the workflow.
  * type (required) : Each step requires a type. There are a limited number of step [types](#supported-step-types) types in PrivCloud.
  * parameters (required) : Some step [types](#supported-step-types) require name parameters in order to properly function.
  * next (optional) : The next step to execute or "none" to finish the workflow. If not included, none is implied.

#### Example Step Definition
```json
    "call_lambda": {
        "comment": "This step calls a lambda function in AWS, passing the decrypted record provided to the record:create action to the function",
        "type": "lambda",
        "parameters": {
            "function": "arn:aws:lambda:us-east-2:123456789012:function:my-function:1",
            "role": "arn:aws:iam::123456789012:role/lambda-role"
        },
        "next": "lambda_response_choice"
    }
```

---

### Step Data Dictionary
Each step within a workflow is passed a data dictionary as input. The data can be used within the step definition (json) 
itself for analysis and interpolation. The data is also passed as input to lambda and webhook step types for custom, run-time
analysis. This can be done by using curly bracket syntax. For example, "{{record.guid}}" will index into the "guid" field of the
"record" object in the data dictionary. Another example is, "{{record.record['first_name']}}" will index into the record content
of the record and evaluate the "first_name" field. The data dictionary always containers the request context and the referenced object 
or entity from the API call. As the workflow steps are executed, the output of each is also added to the data dictionary 
and can be referenced in subsequent steps.


#### Example Step Data Dictionary
```json
{
    "request_context": {
        "entity_pcrn": "pcrn:12345678:entity/user:danny@privcloud.com",
        "client_ip": "1.2.3.4",
        "client_country": "US",
        "action": "record:create",
        "request_is_signed": true,
        "http_verb": "POST",
        "mfa_present": false
    },
    "record": {
      "guid": "5d0a5876dc8d4963a9b84655e06943f8",
      "record_type_id": 1,
      "otherfield": "..."
    },
    "call_lambda": {
        "some_lambda_result_var": 1,
        "other_lambda_output": "this can be used in other steps"
    },
    "lambda_response_choice": {
        "condition_path_selected": "condition1"
    }
}
```

When evaluating an entry in the data dictionary as part of a condition or filter, you can use the following structure to perform the
analysis. 

 * field (string) : The field or value used to form the left part of the comparison. 
 * operator (string) : The operator to use for the comparison. Supported operators are: "=" (equals), "!=" (not equal), "~" (python regular expression), ">" (greater than), ">=" (greater than or equal to), "<" (less than), "<=" (less than or equal to), "empty" (is empty or not set), "not_empty" (is not empty or is set)
 * value (list or string or int) : Either a list of possible right-hand values or a single right-hand value to compare against.

**Example**
```json
{
    "field": "{{user.email}}",
    "operator": "~",
    "value": "danny@(privcloud|test)\.com"
}
```

---

### Supported Step Types
PrivCloud Workflow support a number of step types that are designed for maximum flexibility and orchestration capability.
The following step types are supported.

### lambda
The [lambda](https://aws.amazon.com/lambda/) type is used to call Amazon Web Services Lambda functions from within a PrivCloud workflow.

**Example Use Case** 
Automatically ingest all new records into your data warehouse.

**Parameters**
 * arn (string) : The [ARN](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html) of the lambda function       
 * role (string) : The [cross-account role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html) for PrivCloud to assume when calling your function

**Example**
```json
"call_lambda": {
    "comment": "This step calls a lambda function in AWS, passing the decrypted record provided to the record:create action to the function",
    "type": "lambda",
    "parameters": {
        "arn": "arn:aws:lambda:us-east-2:123456789012:function:my-function:1",
        "role": "arn:aws:iam::123456789012:role/lambda-role"
    },
    "next": "lambda_response_choice"
}
```

**Output**
The raw json output returned from the lambda is added to the data dictionary
```json
{
    "some_lambda_result_var": 1,
    "other_lambda_output": "this can be used in other steps"
}
```

--- 

### email
The email type is used to send an email from the workflow.

**Example Use Case** 
Send an email to the compliance team when customer records are deleted.

**Parameters**
 * subject (string) : The subject of the email when sent       
 * body (string) : The body of the email when sent
 * recipients (list of string) : The recipients of the email         

**Example**
```json
"send_email": {
    "comment": "This step sends an email to the privacy team, notifying them of the record deletion",
    "type": "email",
    "next": "none",
    "parameters": {
        "subject": "Record {{record.guid}} Deleted After Lambda Count {{lambda_result.count}}",
        "body": "This is an example email body with interpolated variables like record.guid={{record.guid}} and lambda_result.result={{lambda_result}} and lambda_result.count={{lambda_result.count}}. Variables passed as output from previous functions are always available for interpolation.\\n\\nFrom PrivCloud",
        "recipients": ["danny@privcloud.com"]
    }
}
```

**Output**  
Success or failure along with an error message in the case of a failure.
```json
{
    "success": true,
    "error_message": ""
}
```

--- 

### sms
The sms type is used to send a text message from the workflow.

**Example Use Case** 
Notify the security team if new records are modified in a critical container.

**Parameters**
 * body (string) : The body of the sms when sent         
 * recipients (list of string) : The recipients of the sms         

**Example**
```json
"send_sms": {
    "comment": "This step sends an sms to the security team, notifying them of the record creation",
    "type": "sms",
    "next": "none",
    "parameters": {
        "body": "This is an example sms body with interpolated variables like record.guid={{record.guid}} and lambda_result.result={{lambda_result}} and lambda_result.count={{lambda_result.count}}. Variables passed as output from previous functions are always available for interpolation.\\n\\nFrom PrivCloud",
        "recipients": ["15127399099"]
    }
}
```

**Output**
Success or failure along with an error message in the case of a failure.
```json
{
    "success": true,
    "error_message": ""
}
```

--- 

### webhook
The webhook type is used to call a a user-defined webhook.

**Example Use Case** 
Automatically ingest all new records into your data warehouse.

**Parameters**
 * url (string) : The url for the webhook to be called

**Example**
```json
"call_webhook": {
    "comment": "This calls a user-defined webhook to evaluate something prior to inserting a new record",
    "type": "webhook",
    "next": "none",
    "parameters": {
        "url": "https://mywebapi.com/webhook/privacy"
    }
}
```

**Output**
The HTTP response code along with the body of the response. 
```json
{
    "response_code": 200,
    "body": {
        "somevar": "this was returned by the webhook as json",
        "othervar": "so was this"
    }
}
```

--- 

### drop
The drop type is a special type of step. When called, the drop type will stop the workflow completely and the API call
to PrivCloud will return an error. This type is useful

**Example Use Case**  
Drop a change to a record if certain conditions are not met.

**Parameters**  
None

**Output**  
None

**Example**
```json
"drop_event": {
    "comment": "Drop the API call if this record creation isn't allow",
    "type": "drop",
    "next": "none"
}
```
--- 

### stop
The stop type is a special type of step. When called, the stop type will stop the workflow completely but the API call within
PrivCloud will continue to execute. 

**Example Use Case** 
Drop a change to a record if certain conditions are not met.

**Parameters**  
None

**Output**  
None

**Example**
```json
"stop_event": {
    "comment": "Stop the work after necessary steps have been complete",
    "type": "stop",
    "next": "none"
}
```

---

### if
The if type is the step to use when building a decision tree based on contents of the [data dictionary](#step-data-dictionary). 
If types use require a list of conditions that are evaluated in sequence. If a condition is met, the "next" key can be used
to select the next step in the workflow. Conditions can be nested using "and" and "or" boolean logic to test objects found in
[data dictionary](#step-data-dictionary). 

**Example Use Case** 
If a lambda function response returns success, then send an email and stop, otherwise call a webhook.

**Parameters** 
* conditions (list of objects) : A list of conditions to evaluate in sequence. If a final "else" condition is not provided, the workflow will stop.


**Example**
```json
"lambda_response_choice": {
    "comment": "This step recieves the response from the lambda function and, based on the 'success' and 'count' response attributes, send an email or stops the workflow",
    "type": "if",
    "parameters": {
        "conditions": [
            {
                "comment": "This condition checks the response of the lambda function for 'success' being true and 'count' being less than 10",
                "type": "condition",
                "condition": {
                    "and": {
                        "condition1": {
                            "field": "{{call_lambda.result}}",
                            "operator": "=",
                            "value": "success"
                        },
                        "condition2": {
                            "field": "{{call_lambda.count}}",
                            "operator": "<",
                            "value": 10
                        }
                    }
                },
                "next": "send_email"
            },
            {
                "comment": "This condition checks the response of the lambda function for 'success' being true and 'count' being between 11 and 20",
                "type": "condition",
                "condition": {
                    "and": {
                        "condition1": {
                            "field": "{{call_lambda.result}}",
                            "operator": "=",
                            "value": "success"
                        },
                        "and": {
                            "condition2": {
                                "field": "{{call_lambda.count}}",
                                "operator": ">",
                                "value": 11
                            },
                            "condition3": {
                                "field": "{{call_lambda.count}}",
                                "operator": "<",
                                "value": 20
                            }
                        }
                    }
                },
                "next": "send_email"
            },
            {
                "comment": "This is the default condition if no other conditions are met",
                "type": "stop"
            }
        ]
    }
}
```

**Output**  
The name of the condition selected
```json
{
    "condition_selected": "condition1"
}
```


---
