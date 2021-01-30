# Record Types
Record types are used to define the structure and encryption policy of the records that are stored in PrivCloud. They are 
defined as JSON documents and can applied to containers to configure the types of records that can be stored in that 
container. If you attempt to create a record or modify an existing record with a structure that isn't configured in your container
the system will deny it. The format used to define record types is [JSON Schema](https://json-schema.org/). Examples of record types 
can be found in the [record types](examples/record_types) directory.

An important consideration is which fields in your record types is which fields are encrypted. You *cannot* search against 
record type values that are encrypted. You *can* search against record type values that are not encrypted.

Below are a few scenarios describing when value encryption is enforced and when it isn't. In this example, you will not be
able to search against the "firstName" field but you will be able to search against the "jobRole" field. 

Note: record keys are *never* encrypted in PrivCloud, do not store sensitive data in the keys of your record types!

```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Person",
  "description": "Example person",
  "type": "object",
  "required": ["firstName", "lastName", "jobRole", "hasJob"],
  "properties": {
    "firstName": {
      "type": "string",
      "encrypt": true,
      "description": "The person's first name.",
      "comment": "Record values *will* be encrypted in PrivCloud because of the explicit encrypt=true"
    },
    "lastName": {
      "type": "string",
      "description": "The person's last name.",
      "comment": "Record values *will* be encrypted in PrivCloud because we encrypt by default if no setting is provided"
    },
    "hasJob": {
      "type": "boolean",
      "description": "Whether the person has a job or not.",
      "comment": "Record values *will not* be encrypted in PrivCloud because booleans are not encrypted"
    },
    "jobRole": {
      "description": "The person's job description",
      "type": "string",
      "encrypt": false,
      "comment": "By default, all records in PrivCloud are encrypted. Use 'encrypt': false to store the data in PrivCloud unencrypted. This is useful for searching and reporting."
    }
  }
}
```