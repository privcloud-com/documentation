# Permissions
Permissions are used to define how users and applications can interact with the records, containers, workspaces, and other
configuration stored in PrivCloud. Permissions are defined as JSON documents and can be applied to either a role 
(for users and applications) or an object (for containers and records). Examples of permissions can be found in the 
permissions  [examples](examples/permissions) directory.  A full description of how role-based and object-based permissions work can be 
found below.

## Structure
Permissions provide a great way for you to define at a granular level who is allowed to access what and under what conditions in PrivCloud.
They are evaluated and enforced for each API call made to the PrivCloud API. There are two kinds of permissions: role-level and object-level. 
Role-level permissions are attached to roles in the system and define what a user or application account can do. Object-level 
permissions are defined on the container or record itself and can be used to define "hard and fast" rules about how the 
objects are accessed. For example, if data in a container or an individual record must not be accessed from outside of Canada, you
can define a deny will on the container or object to disallow it if the "not_from_countries" condition is not met. 
Permissions are defined as JSON  objects that have two top-level required properties and an optional comment 
property. These property definitions are below.

  * version (required) : the is the version of the JSON definition structure, currently only version 1 is supported
  * rules (required) : a list of rules 
  * comment (optional) : an optional comment for the rule

#### Example rule
```json
{
  "version": 1,
  "rules": [
    {
      "comment": "This example shows how to create a permission that can be used to grant super-administrative privileges over the entire instance. In this case, this permission is allowing the user dtijerina-superadministrator@privcloud.com or the application app-integration-superadministrator these privileges. Once this permissions is created, it needs to be assigned to a role and then the role granted to dtijerina-superadministrator@privcloud.com or the application app-integration-superadministrator.",
      "requestors": [
        "pcrn:12345678:entity/user:dtijerina-superadministrator@privcloud.com",
        "pcrn:12345678:entity/application:app-integration-superadministrator"
      ],
      "actions": [
        "pcrn:12345678:action/*:*"
      ],
      "on_objects": [
        "pcrn:12345678:entity/*:*",
        "pcrn:12345678:object/*:*"

      ],
      "decision": "allow",
      "conditions": {
        "record_type" : ["PII"],
        "not_from_countries" : ["IQ", "IR"]
      }
    }
  ],
  "comment" : "This is a comment"
}
 
```

#### Important Notes
* If there is a conflict between an allow rule and a deny rule within role or object permissions, the request will always be denied.
* Deny is always the default action, so if you do not explicitly allow access to something, it will be denied by default.

#### Use Of Wildcards
Wildcards can be used across the permissions system to specify "any" object of the object_type, but cannot be used to specify arbtirary
text before, between, or after an object name.

Good use of wildcards
```
pcrn:12345678:object/workspace:SaaS-Applications:container:Product1:record:*
```

Bad used of wildcards
```
pcrn:12345678:object/workspace:SaaS-Applications:container:Product1:record:82bf08a66f3*1e04f47c4a5d35d

pcrn:12345678:object/workspace:SaaS-Applications:container:*SaaSProduct:record:82bf08a66f344741b1e04f47c4a5d35d

pcrn:12345678:object/workspace:SaaS*App:container:Product1:record:82bf08a66f344741b1e04f47c4a5d35d
```

#### Rules
Rules are a list of rules that define what is allowed or denied. They consist of four top-level required properties, an optional 
conditions property, and an optional comment property.

  * requestors (required) : a list of [users](../README.md#users) or [applications](../README.md#applications) in [PCRN format](#pcrn-format) that the rule applies to
  * actions (required) : a list of [actions](actions.md) in scope of the rule
  * on_objects (required) : a list of [objects](../README.md#object) in [PCRN format](#pcrn-format) scope for the rule
  * decision (required) : either "allow" or "deny"
  * conditions (optional) : [conditions](conditions.md) are a powerful tool that allow administrators to define the conditions that must
  be met in order for the rule to be enforced
  * comment (optional) : an optional comment
  
---
  
#### PCRN Format
PCRN stands for PrivCloud Resource Name and it is the format used to define individual resources across PrivCloud. The format
for PCRN is as follows:

#### Format & Example
```
prcn:<privcloud_account_id>:<namespace>/<object_type>:<object>
```

```
pcrn:12345678:entity/user:dtijerina-superadministrator@privcloud.com
```

 * privcloud_account_id - your PrivCloud account ID
 * namespace - the namespace of the object with the options defined below
   * entity - a user or application
   * action - an API action
   * object - data, metadata, or configuration in PrivCloud
 * object_type - the object type for the object with the options defined below
   * user - used to specify a user 
   * application - used to specify an application
   * workspace - used to specify a workspace
   * container - used to specify a container
   * record - used to specify a record
   * role - used to specify a role
   * container_permission - used to specify a container_permission
   * role_permission - used to specify a role_permission
   * record_permission - used to specify a record_permission
   * record_type - used to specify a record_type
 * object - the unique identifier for the object as defined below
   * user - the id or email address of a user 
   * application - the id or name of the application
   * workspace - the id or name of the workspace 
   * container - the GUID or name of the container 
   * record - the GUID of the record
   * role - the id or name of the role
   * container_permission - the id or name of the container permission
   * record_permission - the id or name of the role permission
   * role_permission - the id or name of the role permission
   * record_type - the id or name of the record type
   
   
---
