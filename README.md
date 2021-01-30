#PrivCloud 
PrivCloud is a secure, high-performing, cloud data-layer for storing sensitive, private information and records. Our platform is designed 
for you to securely store, encrypt, access, process, and grant access to semi-structured sensitive data. We do this by tokenizing your 
data and making it available via our easy-to-use API, very similar to how Stripe manages credit card processing. Once in 
PrivCloud, you decide whether you want your data encrypted with keys that we manage or you can provide your own. We automatically
 rotate keys and follow industry-recognized best practices for both encrypting data and managing keys. We also handle all 
 aspects of security, compliance, and infrastructure management so you can focus on whats important.
 
---
 
## Use Cases
PrivCloud is designed to be flexible an meet the needs of several use cases. Whether you are a CISO trying to retain control 
over sensitive records or a SaaS provider looking to avoid managing PII, credit cards, or healthcare data, PrivCloud can 
help you achieve your goals.

### CISO - Maintaining Control Of Data
As a CISO, your primary objective is to reduce risk and protect critical data. In the age of cloud, businesses rely on 
many third party as-a-service providers and the only way to leverage those providers is to hand them your critical data 
and trust they can properly secure it. PrivCloud provides a centralized, secure, managed data layer where you can grant 
access to exact amount of access you want to third parties while maintaining control over it. All activity in PrivCloud is
 logged so you can see what is happening to your data, when it happened, and who did it. If your third party providers are 
 breached, you can validate that your data was not access rather than taking their word for it. PrivCloud allows you to 
 always maintain control of critical data while still leveraging the massive benefits of cloud.
 
### CISO - Securely Share Redacted Data
Sharing data with third-parties who need access to it for various purposes is a complex task. This is typically an all-or-nothing
scenario and, if "all" isn't acceptable, it is typically a process to extract, transform, redact, package, deliver, and host
redacted data. With PrivCloud, this becomes a simple task that can be done securely, with real-time data, and all access is 
logged. This shortcut will ensure that business continues to operate smoothly, quickly, AND securely. 

### SaaS Provider - Transfer Risk
No matter your size or industry, as a SaaS provider it is a daunting task to secure your customers data. There are so many requirements
 that have to be considered: NIST, ISO27001, PCI, customer contractual requirements, HIPAA, and the list goes on. It requires
 specialized expertise, resources, and investment to ensure data is secure. With PrivCloud, this daunting task becomes
 much easier. The data that you choose can be stored in our secure data layer so that you can focus on your core competency,
 making customers happy, and building your software business. 

### SaaS Provider - Easily Implement BYOK For Your Customers
As a SaaS provider, you are constantly inundated with requests from customers for features and improvements. In the age
 of privacy, the inability to "see" customers data is becoming increasingly important from a liability and customer requirements
 perspective. This can be accomplished by providing your customers the ability to bring their own encryption key to your 
 software. This sounds easy, but if it hasn't been carefully architected into your application it can be a very resource-intensive
 process. PrivCloud's data layer offers a turn-key way to implement BYOK for your customers  to ensure that you can meet
 their security requirements, maintain their trust in you, and continually make them happy customers.
 
 ### SaaS Provider - The Test Data Challenge
 If you are a SaaS provider, you know that testing your software with data that is a close to customer data as possible
 is critical to providing a stable, performant experience for customers. Generating this type of data to either replicate
 customer issues or run test suites with "real-world" data is a very, very complex task involving resources and time. PrivCloud
 makes this process dead simple by giving you the ability to generate data records on the fly using your customer data as a basis.
 This process ensures that the test data is the same size, structure, and complexity of the data you are seeing in production, 
 giving you the confidence that your test suites and bug fixes are great representations of what the software will run like
 in production. 
 

---

## Overview
PrivCloud is architected to be flexible and meet your needs of securing critical data. At the highest level, data can be 
 isolated by workspaces. Within workspaces, you are able to create one or many containers. [Containers](documentation/containers.md) 
 are highly configurable and able to store semi-structured individual records. The structure of these records defined by a 
 [record type](documentation/record-types.md). Containers can only store records of specific record types that you configure. 
 A visual representation of this architecture is below. Granting access to workspaces, containers, and records is all configurable
 through our [permissions system](documentation/permissions.md). You can grant individual [users](#users) or [applications](#applications) 
 through this system.

![PrivCloud Overview](images/privcloud_overview.png)


## PrivCloud Vocabulary
Below you can find definitions for some common vocabulary used by PrivCloud

#### Workspace
The top level structure within PrivCloud. You can use workspaces to isolate data at a very high level. For example, 
if multiple business units are going to be using PrivCloud at your company, you may consider creating a workspace for each 
business unit to ensure data is always isolated and defining permissions is simple.

#### Workspace Tag
Workspace tags can be used to classify and organize your workspaces in a way that makes sense. Tags can be used both for searching
as well as for applying permissions for your users and applications. Note: workspace tags are *never* encrypted.

#### Container
Containers are where the magic happens. Containers hold encrypted records and each container has a separate [encryption wrapping 
key](#encryption-wrapping-key). The key can be managed by PrivCloud or you can optionally bring your own key to use for 
encryption. By default, container keys are rotated every month, but you can configure your policy for rotation. The container 
is configured to hold one or more record types and the structure of each record created in the container must match the 
configured record types. Containers are great for isolating data within a workspace. For example, if you have multiple SaaS 
application using PrivCloud for storage, you should create a separate container for each. In another example, if you want 
to provide BYOK capabilities to your customers for your SaaS application, you should create separate containers for each 
customer and have them upload their own encryption key to be used for encrypting data in their container.

#### Encryption Wrapping Key
PrivCloud uses the wrapping key best practice methodology for encryption. Wrapping keys are used to generate data keys, 
which are the actual keys we use to encrypt your records. This creates additional isolation benefits and ensures that your 
data is secure while being stored in PrivCloud. More information on the general process can be found [here](https://cloud.google.com/kms/docs/key-wrapping)

#### Container Tag
Container tags can be used to classify and organize your containers in a way that makes sense. Tags can be used both for searching
as well as for applying permissions for your users and applications. Note: container tags are *never* encrypted.

#### Record
Records are the actual data that you care about. They are semi-structured (JSON) based on their record types. Records can 
be as large or as small as you want. 


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
		"zip": 67743
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
[record types](documentation/record-types.md) documentation.

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

Example Access Token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXJzaW9uIjoxLCJ0eXBlIjoiYXBwbGljYXRpb25fYWNjZXNzIiwic3VidHlwZSI6ImFwaSIsIm5hbWUiOiJmb28iLCJpZCI6MSwidXVpZCI6IjEyZWJiNWMwMTE3NzRiNzc5NGI3NGZhYWVhYTdkNmYxIiwiZm9yX3NpdGUiOiJodHRwczovL3RoaXNvbmUucHJpdmNsb3VkLmNvbSIsImlzc3VlZF90aW1lc3RhbXAiOiIyMDIxLTAxLTMwVDEyOjM1OjMxLjU4NDA2NiIsInNpZ25hdHVyZV9wdWJsaWNfa2V5IjoibGFramFsc2tkZmthc2pkZmtzamRmIiwidGFncyI6WyJtZmFfcHJlc2VudCJdLCJqdGlfYXV0aF90b2tlbiI6ImVhMTY4ZWU5MmIzOTQwNDY5OGU0YThjNmNhYmY5YmNhIiwiY29uZGl0aW9ucyI6e319.xl0eEqeOKXetWtVIymz8RQsep0TyAWmq8tRc2jJCCAE
```

#### Roles
Roles are used to design sets of role-permissions that can be assigned to entities. Roles can one or more
[permissions](documentation/permissions.md) assigned to them. When a user or application is assigned a role they automatically
inherit the role-permissions assigned to the role. 

#### Permissions
Permissions are used to define how entities can interact with the records, containers, workspaces, and other
configuration stored in PrivCloud. Permissions are defined as JSON documents and can be applied to either a role 
 or an object (for containers and records). Examples of permissions can be found in the permissions [examples](examples/permissions) 
 directory.  Full details of how role-based and object-based permissions can be used and defined can be found in the 
 [permissions documentation](documentation/permissions.md).
