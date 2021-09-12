# Concepts

## PrivCloud Concepts
Below you can find concept definitions for some common structures used in PrivCloud.

#### Workspace
The top level structure within PrivCloud. You can use workspaces to isolate data at a very high level. For example, 
if multiple business units are going to be using PrivCloud at your company, you may consider creating a workspace for each 
business unit to ensure data is always isolated and defining permissions is simple.

#### Workspace Tag
Workspace tags can be used to classify and organize your workspaces in a way that makes sense. Tags can be used both for searching
as well as for applying permissions for your users and applications. Note: workspace tags are *never* encrypted.

#### Container
Containers are where the magic happens. Containers hold encrypted records and each container has a separate [encryption wrapping 
key](#encryption-wrapping-key). They are designed to allow you to define your encryption rotation policy and encryption keys. 
The key can be managed by PrivCloud or you can optionally bring your own key to use for encryption. Containers must be initialized
after they are created and prior to data being loaded into them. By default, container 
keys are rotated every month, but you can configure your policy for rotation. The container  is configured to hold one or 
more record types and the structure of each record created in the container must match the configured record types. 
Containers are great for isolating data within a workspace. For example, if you have multiple SaaS applications using 
PrivCloud for storage, you should create a separate container for each. In another example, if you want to provide BYOK 
capabilities to your customers for your SaaS application, you should create separate containers for each customer and 
have them upload their own encryption key to be used for encrypting data in their container. Once containers are initialized, data can
be loaded using [import](actions.md) functionality. Asynchronous imports make it possible to bulk load large amounts of data into PrivCloud
very quickly and easily. [Scripts](https://github.com/privcloud-com/privcloud-scripts) are available to assist with this process.
Once data is loaded, containers can be "frozen". A frozen container means that the data held within the container can only be read but not
modified or deleted. If a container is frozen it also cannot have additional data loaded into it. Freezing a container is a great option
if you would like to share critical data but do not want it modified for any reason by anyone that you are sharing it with.

#### Encryption Wrapping Key
PrivCloud uses the wrapping key best practice methodology for encryption. Wrapping keys are used to generate data keys, 
which are the actual keys we use to encrypt your records. This creates additional isolation benefits and ensures that your 
data is secure while being stored in PrivCloud. More information on the general process can be found [here](https://cloud.google.com/kms/docs/key-wrapping)

#### Container Tag
Container tags can be used to classify and organize your containers in a way that makes sense. Tags can be used both for searching
as well as for applying permissions for your users and applications. Note: container tags are *never* encrypted.

#### Record
Records are the actual data that you care about. They are semi-structured (JSON) based on their record types. Records can 
be as large or as small as you want. Records can be [searched](actions.md), but only unencrypted fields defined in the record type, 
tags, and metadata are searched. Once created, records can be frozen. If a record is frozen, it can be read but not 
modified or deleted. Records can be transformed when they are retrieved. By default, records are encrypted, but they can also
be retrieved in decrypted, anonymized, or redacted form. Only fields with encryption applied have transformations applied
when they are retrieved. Transformations are useful for sharing data in a secure way (EX: redacted) or for testing applications in
a secure way (EX: anonymizing data). Definitions of each transformation is below.

 * **encrypt** : fields are returned encrypted (EX: "John" returned as "ftYcQTahxTvtrA==")
 * **decrypt** : fields are returned decrypted (EX: "John" returned as "John")
 * **anonymize** : fields are anonymized prior to being returned (EX: "John" may become "Joe") 
 * **redact** : fields are fully redacted prior to being returned (EX: "John" returned as "****")

Example Record Of Record Type [Credit Card Account](examples/credit_cards_account_record.json)

```
{
	"accountNumber": 12345678,
	"firstName": "Danny",
	"lastName": "Tijerina",
	"billingAddress": {
		"street": "1234 street",
		"state": "TX",
		"country": "United States",
		"zip": "67743"
	},
	"creditCards": [{
		"number": "1234-5678-9101-1213",
		"expireMonth": 11,
		"expireDay": 20,
		"isDefault": true
	}, {
		"number": "4321-5678-9101-9786",
		"expireMonth": 1,
		"expireDay": 12,
		"isDefault": false
	}]
}
```

#### Record Metadata
Record metadata are key-value pairs that you can use to add context to your data or add additional information that aren't a part 
of your record type structure. These key-value pairs are great for searching against your record set to extract the data
you care about. Note: Record metadata is *never* encrypted.

#### Record Tag
Record tags can be used to classify and organize your records in a way that makes sense. Tags can be used both for searching
as well as for applying permissions for your users and applications. Note: record tags are *never* encrypted.

#### Record Types
Record types are used to define the structure and encryption policy of the records that are stored in PrivCloud. They are 
defined as JSON documents and can applied to containers to configure the types of records that can be stored in that 
container.  The format used to define record types is [JSON Schema](https://json-schema.org/). Examples of record types 
can be found in the [record types](examples/record_types) directory. For more details on record types, go to the 
[record types](documentation/record-types.md) documentation. Record types cannot be modified once they are created.

#### Object
Data or organization structure in PrivCloud. This can include workspaces, containers, and records. 

#### Entity
A user or application.

#### Users
Users are entities in the system that can access PrivCloud both programmatically with [access tokens](#access-tokens) as well as through 
the UI portal with an email/password combination. Users can also upload public keys for request signing and can be forced 
to authenticate with TOTP multifactor authentication. By default, new users in the system do not have any rights. They must 
be assigned one or more [roles](#roles) to have any access.

#### Applications
Applications are entities in the system that can access PrivCloud only programmatically with API keys. They are akin to 
"service accounts" and should be used to grant code or systems access to PrivCloud. Applications can also upload public keys 
for request signing and can be forced to authenticate with TOTP multifactor authentication. By default, new applications
 in the system do not have any rights. They must be assigned one or more [roles](#roles) to have any access.

#### Access Tokens
Access tokens are JWT tokens generated for [users](#users) and [applications](#applications) and used to authenticate to PrivCloud.
Access tokens can be configured to have [conditions](documentation/conditions.md) that ensure entities can only access the
system under certain constraints. Access tokens are passed to PrivCloud as part of the [Authorization HTTP](https://swagger.io/docs/specification/authentication/bearer-authentication/)
header. Once a user is authenticated, their access token gets converted to an authorization token, which can be used for the 
remainder of their session. All authorization tokens retain their conditions.

##### Example Access Token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXJzaW9uIjoxLCJ0eXBlIjoiYXBwbGljYXRpb25fYWNjZXNzIiwic3VidHlwZSI6ImFwaSIsIm5hbWUiOiJmb28iLCJpZCI6MSwidXVpZCI6IjEyZWJiNWMwMTE3NzRiNzc5NGI3NGZhYWVhYTdkNmYxIiwiZm9yX3NpdGUiOiJodHRwczovL3RoaXNvbmUucHJpdmNsb3VkLmNvbSIsImlzc3VlZF90aW1lc3RhbXAiOiIyMDIxLTAxLTMwVDEyOjM1OjMxLjU4NDA2NiIsInNpZ25hdHVyZV9wdWJsaWNfa2V5IjoibGFramFsc2tkZmthc2pkZmtzamRmIiwidGFncyI6WyJtZmFfcHJlc2VudCJdLCJqdGlfYXV0aF90b2tlbiI6ImVhMTY4ZWU5MmIzOTQwNDY5OGU0YThjNmNhYmY5YmNhIiwiY29uZGl0aW9ucyI6e319.xl0eEqeOKXetWtVIymz8RQsep0TyAWmq8tRc2jJCCAE
```

#### Roles
Roles are used to design sets of role-permissions that can be assigned to entities. Roles can one or more
[permissions](permissions.md) assigned to them. When a user or application is assigned a role they automatically
inherit the role-permissions assigned to the role. 

#### Permissions
Permissions are used to define how entities can interact with the records, containers, workspaces, and other
configuration stored in PrivCloud. Permissions are defined as JSON documents and can be applied to either a role 
 or an object (for containers and records). Examples of permissions can be found in the permissions [examples](examples/permissions) 
 directory.  Full details of how role-based and object-based permissions can be used and defined can be found in the 
 [permissions documentation](permissions.md).
