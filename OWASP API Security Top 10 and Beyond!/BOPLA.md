API3:2023 Broken Object Property Level Authorization (BOPLA) is the combination of two items from the 2019 OWASP API Security Top Ten, excessive data exposure and mass assignment.

Excessive Data Exposure takes place when an API provider responds to a request with an entire data object. When the data object is shared without being filtered there is an increased risk of exposing sensitive information.

Mass Assignment is a weakness that allows for user input to alter sensitive object properties. If, for example, an API uses a special property to create admin accounts only authorized users should be able to make requests that successfully alter those administrative properties. If there are no restrictions in place then an attacker would be able to elevate their privileges and perform administrative actions.

## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)

_APIs tend to expose endpoints that return all object’s properties. This is particularly valid for REST APIs. For other protocols such as GraphQL, it may require crafted requests to specify which properties should be returned. Identifying these additional properties that can be manipulated requires more effort, but there are a few automated tools available to assist in this task._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)

_Inspecting API responses is enough to identify sensitive information in returned objects’ representations. Fuzzing is usually used to identify additional (hidden) properties. Whether they can be changed is a matter of crafting an API request and analyzing the response. Side-effect analysis may be required if the target property is not returned in the API response._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)

_Unauthorized access to private/sensitive object properties may result in data disclosure, data loss, or data corruption. Under certain circumstances, unauthorized access to object properties can lead to privilege escalation or partial/full account takeover._

The OWASP API Security Project states that an API endpoint is vulnerable if:
- The API endpoint exposes properties of an object that are considered sensitive and should not be read by the user. (previously named: "[Excessive Data Exposure](https://github.com/OWASP/API-Security/blob/master/2019/en/src/0xa3-excessive-data-exposure.md)")

![](attachments/Pasted%20image%2020250713172245.png)
![](attachments/Pasted%20image%2020250713172254.png)

- The API endpoint allows a user to change, add/or delete the value of a sensitive object's property which the user should not be able to access (previously named: "[Mass Assignment](https://github.com/OWASP/API-Security/blob/master/2019/en/src/0xa6-mass-assignment.md)")

Imagine an API is called to create an account with parameters for “User” and “Password”:

{

“User”: “hapi_hacker”,

“Password”: “GreatPassword123”

}

While reading the API documentation regarding the account creation process, say an attacker discovers that there is an additional properties, “isAdmin” that the API provider uses to create administrative accounts. An attacker could add this to a request and set the value to true:

{

“User”: “hapi_hacker”,

“Password”: “GreatPassword123”,

“isAdmin”: true

}

## [OWASP Preventative Measures](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)

- _When exposing an object using an API endpoint, always make sure that the user should have access to the object's properties you expose._
- _Avoid using generic methods such as to_json() and to_string(). Instead, cherry-pick specific object properties you specifically want to return._
- _If possible, avoid using functions that automatically bind a client's input into code variables, internal objects, or object properties ("Mass Assignment")._
- _Allow changes only to the object's properties that should be updated by the client._
- _Implement a schema-based response validation mechanism as an extra layer of security. As part of this mechanism, define and enforce data returned by all API methods._
- _Keep returned data structures to the bare minimum, according to the business/functional requirements for the endpoint._