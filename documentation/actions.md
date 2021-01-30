## Actions
Actions correspond to API calls in PrivCloud. The mapping of actions to HTTP Method and URI is defined below. 

| Action | HTTP Method + URI | Description | 
|:-------------:|:-------------:| :-------------| 
| audit_log:list | GET /api/audit_log | Get a list of audit logs |
| audit_log:get | GET /api/audit_log/{id} | Get an audit log based on the audit log's ID |
| user:list | GET /api/user | Get a list of users |
| user:get | GET /api/user/{id} | Get a user based on user's ID |
| user:delete | DELETE /api/user/{id} | Delete a user based on user's ID |
| user:update | PATCH /api/user/{id} | Update a user based on the user's ID |
| user:create | POST /api/user | Create a user |
| role:list | GET /api/role | Get a list of roles |
| role:get | GET /api/role/{id} | Get a role based on roles' ID |
| role:delete | DELETE /api/role/{id} | Delete a role based on roles' ID |
| role:update | PATCH /api/role/{id} | Update a role based on the roles' ID |
| role:create | POST /api/role | Create a role |
| container_permission:list | GET /api/container_permission | Get a list of container permissions |
| container_permission:get | GET /api/container_permission/{id} | Get a container_permission based on container permissions' ID |
| container_permission:delete | DELETE /api/container_permission/{id} | Delete a container_permission based on container permissions' ID |
| container_permission:update | PATCH /api/container_permission/{id} | Update a container_permission based on the container permissions' ID |
| container_permission:create | POST /api/container_permission | Create a container permission |
| record_permission:list | GET /api/record_permission | Get a list of record permissions |
| record_permission:get | GET /api/record_permission/{id} | Get a record_permission based on record permissions' ID |
| record_permission:delete | DELETE /api/record_permission/{id} | Delete a record_permission based on record permissions' ID |
| record_permission:update | PATCH /api/record_permission/{id} | Update a record_permission based on the record permissions' ID |
| record_permission:create | POST /api/record_permission | Create a record permission |
| role_permission:list | GET /api/role_permission | Get a list of role permissions |
| role_permission:get | GET /api/role_permission/{id} | Get a role_permission based on role permissions' ID |
| role_permission:delete | DELETE /api/role_permission/{id} | Delete a role_permission based on role permissions' ID |
| role_permission:update | PATCH /api/role_permission/{id} | Update a role_permission based on the role permissions' ID |
| role_permission:create | POST /api/role_permission | Create a role permission |
| application:list | GET /api/application | Get a list of applications |
| application:get | GET /api/application/{id} | Get a application based on applications' ID |
| application:delete | DELETE /api/application/{id} | Delete a application based on applications' ID |
| application:update | PATCH /api/application/{id} | Update a application based on the applications' ID |
| application:create | POST /api/application | Create a application |
| workspace:list | GET /api/workspace | Get a list of workspaces |
| workspace:get | GET /api/workspace/{id} | Get a workspace based on workspaces' ID |
| workspace:delete | DELETE /api/workspace/{id} | Delete a workspace based on workspaces' ID |
| workspace:update | PATCH /api/workspace/{id} | Update a workspace based on the workspaces' ID |
| workspace:create | POST /api/workspace | Create a workspace |
| workspace_tag:create | POST /api/workspace_tag | Create a workspace tag |
| workspace_tag:delete | DELETE /api/workspace_tag/{workspace_id}/{tag} | Delete a workspace tag |
| container:list | GET /api/workspace/{id}/containers | Get a list of containers in workspace ID |
| container:get | GET /api/container/{guid} | Get a container based on containers' GUID |
| container:delete | DELETE /api/container/{guid} | Delete a container based on containers' GUID |
| container:update | PATCH /api/container/{guid} | Update a container based on the containers' GUID |
| container:create | POST /api/container | Create a container |
| container_tag:create | POST /api/container_tag | Create a container tag |
| container_tag:delete | DELETE /api/container_tag/{container_guid}/{tag} | Delete a container tag |
| record_type:list | GET /api/record_type | Get a list of record types |
| record_type:get | GET /api/record_type/{id} | Get a record_type based on record types' ID |
| record_type:delete | DELETE /api/record_type/{id} | Delete a record_type based on record types' ID |
| record_type:update | PATCH /api/record_type/{id} | Update a record_type based on the record types' ID |
| record_type:create | POST /api/record_type | Create a record type |
| record:list | GET /api/container/{guid}/records | List records in a container | 
| record:get | GET /api/record/{guid} | Get an encrypted record based on its GUID | 
| record:get_with_redact | GET /api/record/{guid}?option=redact | Get an redacted record based on its GUID | 
| record:get_with_anonymize | GET /api/record/{guid}?option=anonymize | Get an anonymized record based on its GUID | 
| record:get_with_decrypt | GET /api/record/{guid}?option=decrypt | Get an decrypted record based on its GUID | 
| record:get_bulk | POST /api/record | Get a bulk list of encrypted records | 
| record:get_with_redact_bulk | POST /api/record?option=redact | Get a bulk list of redacted records | 
| record:get_with_anonymize_bulk | POST /api/record?option=anonymize | Get a bulk list of redacted records | 
| record:get_with_decrypt_bulk | POST /api/record?option=decrypt | Get a bulk list of redacted records | 
| record:delete | DELETE /api/record/{guid} | Delete a record based on its GUID |
| record:create | POST /api/record | Create a record |
| record:update | PATCH /api/record/{guid} | Update a record |
| record_metadata:create | POST /api/record/{guid}/metadata | Create a metadata key-value pair on a record |
| record_metadata:delete | DELETE /api/record/{guid}/metadata/{key} | Delete a metadata key-value pair on a record based on the key |
