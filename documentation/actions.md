## Actions
Actions correspond to API calls in PrivCloud. The mapping of actions to HTTP Method and URI is defined below. This is a high
level list of endpoints available in PrivCloud. For a full and always up-to-date list of endpoints, visit our swagger UI
 by pointing your browser at the /api url in your instance of PrivCloud. You can also find the swagger [here](https://app.swaggerhub.com/apis/VantagePoint/PrivCloud-API/0.0.9).

| URI | HTTP Method  | Action | Description | 
|:-------------|:-------------| :-------------| :-------------| 
| /application | GET | application:list | Get a list of PrivCloud applications |
| /application | POST | application:create | Create a new PrivCloud application |
| /application/{application_id}/role | GET | application:list_role | Get a list of the PrivCloud roles associated with the application |
| /application/{application_id}/role | POST | application:attach_role | Attach a role to a application within PrivCloud |
| /application/{application_id}/role/{role_id} | DELETE | application:delete_role | Delete the role attachment from the PrivCloud application |
| /application/{application_id}/role/{role_id} | GET | application:get_role | Get the role attached to the PrivCloud application |
| /application/{id} | DELETE | application:delete | Delete a application by ID |
| /application/{id} | GET | application:get | Retrieve a application by ID |
| /application/{id} | PATCH | application:update | Update a application by ID |
| /application/{id}/token | GET | application:list_token | Get a list of application auth tokens for the user with id |
| /application/{id}/token | POST | application:create_token | Generate a new access token for the user with id |
| /application/{id}/token/{jti} | DELETE | application:delete_token | Delete a application access token based on its jti |
| /audit_log | GET | audit_log:list | Get a list of PrivCloud audit logs |
| /audit_log/{id} | GET | audit_log:get | Retrieve a audit log by ID |
| /container | GET | container:list | Get a list of PrivCloud containers |
| /container | POST | container:create | Create a new PrivCloud container |
| /container/permission | GET | container:list_all_permission | Get a list of all PrivCloud container permissions |
| /container/{guid} | DELETE | container:delete | Delete a container by GUID |
| /container/{guid} | GET | container:get | Retrieve a container by GUID |
| /container/{guid} | PATCH | container:update | Update a container by ID |
| /container/{guid}/import_records/{record_type_id} | POST | container:import_records | Import records of record_type_id record type into the container identified by the guid. |
| /container/{guid}/initialize | POST | container:initialize | Initialize a BYOK container. |
| /container/{guid}/key_rotation_history | GET | container:get_key_rotation_history | Retrieve the rotation history for the container |
| /container/{guid}/permission | GET | container:list_permission | Get a list of PrivCloud container permissions for the container identified by guid |
| /container/{guid}/permission | POST | container:create_container_permission | Create a new PrivCloud container permission associated with the container identified by guid |
| /container/{guid}/permission/{id} | DELETE | container:delete_permission | Delete a container permission by ID |
| /container/{guid}/permission/{id} | GET | container:get_permission | Retrieve a container permission by ID |
| /container/{guid}/permission/{id} | PATCH | container:update_container_permission | Update a container permission by ID |
| /container/{guid}/record_type | GET | container:list_record_type | Get a list of PrivCloud record types that are attached to the container |
| /container/{guid}/record_type | POST | container:create_record_type | Create a new PrivCloud attachment between a Container and a record type |
| /container/{guid}/record_type/{record_type_id} | DELETE | container:delete_record_type | Delete a record type attachment from a container. Only valid if the container does not contain any records of the record type id. |
| /container/{guid}/record_type/{record_type_id} | GET | container:get_record_type | Retrieve a record type that is associated with a container |
| /container/{guid}/records | GET | container:list_records | Get a list of record GUIDs and record type ids in the container identified by the guid. |
| /container/{guid}/rotate_key | POST | container:rotate_key | Rotate a container key. |
| /container/{guid}/search/{record_type_id}/{search} | GET | container:search | Search the container records of record type ID for the search text. The following areas will be searched - record metadata | record tags | unencrypted record fields. Encrypted fields will not be searched. Encrypted records will be returned. |
| /container/{guid}/search/{record_type_id}/{search}/anonymize | GET | container:search_with_anonymize | Search the container records of record type ID for the search text. The following areas will be searched - record metadata | record tags | unencrypted record fields. Encrypted fields will not be searched. Anonymized records will be returned. |
| /container/{guid}/search/{record_type_id}/{search}/decrypt | GET | container:search_with_decrypt | Search the container records of record type ID for the search text. The following areas will be searched - record metadata | record tags | unencrypted record fields. Encrypted fields will not be searched. Decrypted records will be returned. |
| /container/{guid}/search/{record_type_id}/{search}/redact | GET | container:search_with_redact | Search the container records of record type ID for the search text. The following areas will be searched - record metadata | record tags | unencrypted record fields. Encrypted fields will not be searched. Redacted records will be returned. |
| /container/{guid}/tag | POST | container:create_tag | Create a new PrivCloud container tag on the container identified by guid |
| /container/{guid}/tag/{tag} | DELETE | container:delete_tag | Delete a container tag by tag text |
| /login | POST | auth:login | Login to PrivCloud |
| /login/saml | GET | saml_auth:login | SAML Login of PrivCloud |
| /logout | GET | auth:logout | Logout of PrivCloud |
| /logout/saml | GET | saml_auth:logout | SAML Logout of PrivCloud |
| /record | POST | record:create | Create a new PrivCloud record |
| /record/bulk | POST | record:get_with_bulk | Retrieve a set of encrypted records by GUIDs |
| /record/bulk/anonymize | POST | record:get_with_anonymize_bulk | Retrieve a set of anonymized records by GUIDs |
| /record/bulk/decrypt | POST | record:get_with_decrypt_bulk | Retrieve a set of decrypted records by GUIDs |
| /record/bulk/redact | POST | record:get_with_redact_bulk | Retrieve a set of redacted records by GUIDs |
| /record/import/check/{task_id} | GET | record:get_status | Get the status of async task for bulk import records |
| /record/permission | GET | record:list_all_permission | Get a list of all PrivCloud record permissions |
| /record/{guid} | DELETE | record:delete | Delete a record by GUID |
| /record/{guid} | GET | record:get | Retrieve an encrypted record by GUID |
| /record/{guid} | PATCH | record:update | Update a record by GUID |
| /record/{guid}/anonymize | GET | record:get_with_anonymize | Retrieve an anonymized record by GUID |
| /record/{guid}/decrypt | GET | record:get_with_decrypt | Retrieve a decrypted record by GUID |
| /record/{guid}/metadata | POST | record:create_metadata | Create a new PrivCloud record metadata |
| /record/{guid}/metadata | PUT | record:update_metadata | Create or replace  PrivCloud record metadata |
| /record/{guid}/metadata/{key} | DELETE | record:delete_metadata | Delete a record metadata by key |
| /record/{guid}/permission | GET | record:list_permission | Get a list of PrivCloud permissions associated with the record identified by guid |
| /record/{guid}/permission | POST | record:create_permission | Create a new PrivCloud record permission on the record identified by guid |
| /record/{guid}/permission/{id} | DELETE | record:delete_permission | Delete a record permission by ID |
| /record/{guid}/permission/{id} | GET | record:get_permission | Retrieve a record permission by ID from the record identified by guid |
| /record/{guid}/permission/{id} | PATCH | record:update_permission | Update a record permission by ID |
| /record/{guid}/redact | GET | record:get_with_redact | Retrieve a redacted record by GUID |
| /record/{guid}/tag | POST | record:create_tag | Create a new PrivCloud record tag on the record identified by guid |
| /record/{guid}/tag/{tag} | DELETE | record:delete_tag | Delete a record tag by the tag |
| /record_type | GET | record_type:list | Get a list of PrivCloud record types |
| /record_type | POST | record_type:create | Create a new PrivCloud record type |
| /record_type/{id} | DELETE | record_type:delete | Delete a record type by ID |
| /record_type/{id} | GET | record_type:get | Retrieve a record type by ID |
| /record_type/{id} | PATCH | record_type:update | Update a record type by ID |
| /refresh_token | POST | auth:refresh_token | Refresh access token by providing auth token |
| /role | GET | role:list | Get a list of PrivCloud roles |
| /role | POST | role:create | Create a new PrivCloud role |
| /role/permission | GET | role:list_permission | Get a list of PrivCloud role permissions |
| /role/permission | POST | role:create_permission | Create a new PrivCloud role permission |
| /role/permission/{id} | DELETE | role:delete_permission | Delete a role permission by ID |
| /role/permission/{id} | GET | role:get_permission | Retrieve a role permission by ID |
| /role/permission/{id} | PATCH | role:update_permission | Update a role permission by ID |
| /role/{id} | DELETE | role:delete | Delete a role by ID |
| /role/{id} | GET | role:get | Retrieve a role by ID |
| /role/{id} | PATCH | role:update | Update a role by ID |
| /setting/auth_mode | PATCH | auth_mode_setting:update | Update an api auth mode setting by key |
| /setting/auth_mode | POST | auth_mode_setting:create | Create a new api auth mode setting |
| /setting/saml_idp_settings | DELETE | saml_idp_setting:delete | Delete an api saml idp settings by key |
| /setting/saml_idp_settings | PATCH | saml_idp_setting:update | Update an api saml idp settings by key |
| /setting/saml_idp_settings | POST | saml_idp_setting:create | Create a new api saml idp settings |
| /user | GET | user:list | Get a list of PrivCloud users |
| /user | POST | user:create | Create a new PrivCloud user |
| /user/{id} | DELETE | user:delete | Delete a user by ID |
| /user/{id} | GET | user:get | Retrieve a user by ID |
| /user/{id} | PATCH | user:update | Update a user by ID |
| /user/{id}/token | GET | user:list_token | Get a list of user auth tokens for the user with id |
| /user/{id}/token | POST | user:create_token | Generate a new access token for the user with id |
| /user/{id}/token/{jti} | DELETE | user:delete_token | Delete a user access token based on its jti |
| /user/{user_id}/role | GET | user:list_role | Get a list of the PrivCloud roles associated with the user |
| /user/{user_id}/role | POST | user:attach_role | Attach a role to a user within PrivCloud |
| /user/{user_id}/role/{role_id} | DELETE | user:delete_role | Delete the role attachment from the PrivCloud user |
| /user/{user_id}/role/{role_id} | GET | user:get_role | Get the role attached to the PrivCloud user |
| /version | GET | version | Get the version of the PrivCloud API that is deployed |
| /workspace | GET | workspace:list | Get a list of PrivCloud workspaces |
| /workspace | POST | workspace:create | Create a new PrivCloud workspace |
| /workspace/{id} | DELETE | workspace:delete | Delete a workspace by ID |
| /workspace/{id} | GET | workspace:get | Retrieve a workspace by ID |
| /workspace/{id} | PATCH | workspace:update | Update a workspace by ID |
| /workspace/{id}/container | GET | workspace:list_container | Retrieve a list of containers associated with the workspace by ID |
| /workspace/{id}/tag | POST | workspace:create_tag | Create a new PrivCloud workspace tag on the workspace identified by id |
| /workspace/{id}/tag/{tag} | DELETE | workspace:delete_tag | Delete a workspace tag by tag text |
