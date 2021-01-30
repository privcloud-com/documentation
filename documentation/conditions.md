# Conditions
Conditions are a powerful mechanism built into the authentication and authorization permissions system that empowers administrators
to define the constraints under which a user can access PrivCloud and any underlying data. Conditions are created when a permission
or access token are created and can be set to constraint how the role permission, object permission, or access token are
used or accessed. Examples of how conditions can be used within object and role permissions can be 
found in the [permissions examples](../examples/permissions). Conditions can be combined to form very powerful constraints on
how PrivCloud is accessed. If there are multiple conditions defined within an access token or permission, a logical "AND" is
used for evaluation. In other words, all conditions must be met for the access to be granted. 

Below are the types of conditions available when creating your permission or access token.

* between_times (object) : used to define the start and end time of the day when PrivCloud can be accessed in 24-hour "HH:MM:SS" 
format. Typically used in combination with the "days_of_the_week" condition to define the windows of time during the week
that PrivCloud can be accessed. Invalid entries will be ignored but both start and end times *must* be defined. All times
are UTC so you must convert your timezone first.

Example: Only allow access to PrivCloud between 10:00:00 AM UTC and 5:00:00 PM UTC.
```json
{
	"between_times": {
		"start_time": "10:00:00",
		"end_time": "17:00:00"
	}
}

```
* days_of_the_week (list of string): used to define the days of the week that PrivCloud can be accessed. Typically used
in combination with the "between_times" condition to define the windows of time during the week that PrivCloud can be accessed.
Invalid entries will be ignored.

Example: Only allow access to PrivCloud on monday, wednesday, and friday.
```json
{
  "days_of_the_week": ["monday","wednesday","friday"]
}

```

* from_IP_cidrs (list of string) : used to define the source IP addresses or CIDRs that can be used to access PrivCloud.
This condition is great for ensuring that requests come from a certain set of machines, offices, or environments through the
use of an allow-list. Invalid entries will be ignored.

Example: Only allow access to PrivCloud when the requests originate from 213.23.43.45 or an IP within the CIDRs 1.1.1.0/24 and 245.43.13.0/28.  
```json
{
  "from_IP_cidrs": ["1.1.1.0/24", "213.23.43.45", "245.43.13.0/28"]
}

```

* not_from_IP_cidrs (list of string) : used to define the source IP addresses or CIDRs that can not be used to access PrivCloud.
This condition is great for ensuring that requests do not come from a certain set of machines, offices, or environments through the
use of a block-list. Invalid entries will be ignored.

Example: Do not allow access to PrivCloud when the requests originate from 8.8.8.8 or an IP within the CIDR 9.9.9.0/24.
```json
{
  "not_from_IP_cidrs": ["8.8.8.8", "9.9.9.0/24"]
}

```

* from_countries (list of string) : used to define what countries can access PrivCloud based on the geolocation of their source 
IP address. The [ISO Alpha-2](https://www.nationsonline.org/oneworld/country_code_list.htm) country code is used to populate
this list. This is great to use when you want to allow access to data or all of PrivCloud from specific countries through an
allow list. For example, you may have regulatory restrictions or company policies on where data can be accessed from. This condition will
ensure that policy is followed.

Example: Only allow access to PrivCloud when the request originates from the United Kingdom, Ireland, or Germany. 
```json
{
  "from_countries": ["UK", "IE", "DE"]
}

```

* not_from_countries (list of string) : used to define what countries cannot access PrivCloud based on the geolocation of their source 
IP address. The [ISO Alpha-2](https://www.nationsonline.org/oneworld/country_code_list.htm) country code is used to populate
this list. This is great to use when you want to deny access to data or all of PrivCloud from specific countries through a 
 block-list. For example, you may have regulatory restrictions or company policies on where data cannot be accessed from. This condition will
ensure that policy is followed.

Example: Do not allow access to PrivCloud when the request originates from Iraq, Iran, Russia, or China. 
```json
{
  "not_from_countries": ["IQ", "IR", "RU", "CN"]
}

```

* multifactor_authentication_present (boolean) : used to define whether the entity must authenticate with MFA in order to
access PrivCloud. This can be used as an extra layer of security to ensure that extra critical data requires MFA to be performed
prior to access.

Example: Enforce multifactor authentication to happen before allowing access to PrivCloud.
```
{
  "multifactor_authentication_present": true
}
```

* request_is_signed (boolean) : used to define whether the entity must sign the request with their private key in order to
access PrivCloud. This can be used as an extra layer of security to ensure that extra critical data requires message signing
prior to being considered valid. 

Example: Force the entity to sign the request with their private key in order for the request to be processed by PrivCloud
```
{
  "request_is_signed": true
}
```


Below are additional conditions you can use when creating object or role level permissions.

* record_type (list of string) : this can be used to grant or deny access to particular record types to entities within
role or object permissions. For example, if your COO needs to see both employee records as well as customer leads, you can 
grant access to these record types through a role permission across all workspaces and containers in PrivCloud.

Example: Allow or deny access to records of the type "employee records" and "customer leads"
```json
{
  "record_type": ["employee records","customer leads"]
}
```

* record_tag (list of string) : this can be used to grant or deny access to records tagged a certain way to entities within
role or object permissions. For example, if your HR administrator needs access to all records tagged "employee info", you can 
grant access to these records through a role permission across all workspaces and containers in PrivCloud.

Example: Allow or deny access to records tagged "employee info"
```json
{
  "record_tag": ["employee info"]
}
```

* container_tag (list of string) : this can be used to grant or deny access to containers tagged a certain way to entities within
role or object permissions. For example, if your infrastructure team needs access to all containers tagged "SaaS", you can 
grant access to these containers through a role permission across all workspaces in PrivCloud.

Example: Allow or deny access to containers tagged "SaaS"
```json
{
  "container_tag": ["SaaS"]
}
```

* workspace_tag : this can be used to grant or deny access to workspaces tagged a certain way to entities within
role or object permissions. For example, if your COO needs access to all workspaces tagged "Operations", you can 
grant access to these all of these workspaces through a role permission.

Example: Allow or deny access to workspaces tagged "Operations"
```
{
  "workspace_tag": ["Operations"]
}
```
