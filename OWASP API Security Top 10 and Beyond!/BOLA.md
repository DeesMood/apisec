## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)

_Attackers can exploit API endpoints that are vulnerable to broken object-level authorization by manipulating the ID of an object that is sent within the request. Object IDs can be anything from sequential integers, UUIDs, or generic strings. Regardless of the data type, they are easy to identify in the request target (path or query string parameters), request headers, or even as part of the request payload._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)

_This has been the most common and impactful attack on APIs. Authorization and access control mechanisms in modern applications are complex and widespread. Even if the application implements a proper infrastructure for authorization checks, developers might forget to use these checks before accessing a sensitive object. Access control detection is not typically amenable to automated static or dynamic testing._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)

_Unauthorized access can result in data disclosure to unauthorized parties, data loss, or data manipulation. Unauthorized access to objects can also lead to full account takeover._

For instance, imagine that an authenticated user, Bruce, sends a GET request to [https://herohospital.com/api/v3/users?id=2727](https://herohospital.com/api/v3/users?id=2727) and receives the following response:

`{`

`"id": "2727",`

`"fname": "Bruce",`

`"lname": "Wayne",`

 `"dob": "1975-02-19",`

`"username": "bman",`

`"diagnosis": "Depression",`

`}`

This poses no problem; Bruce should be able to access Bruce's own information. However, if Bruce is able to access another user’s information then a BOLA vulnerability would be present. A weakness can be tested by using other resource IDs in place of 2727. Say Bruce is able to obtain information about another user by sending a request for [https://herohospital.com/api/v3/users?id=2728](https://herohospital.com/api/v3/users?id=2728) and receives the following response:

`{`

`"id": "2728",`

`"fname": "Harvey",`

`"lname": "Dent",`

 `"dob": "1979-03-30",`

`"username": "twoface",`

`"diagnosis": "Dissociative Identity Disorder",`

`}`

Below are a few examples of how different resources may be accessed via API requests. The bold resource IDs in the following API requests should catch your attention:

- GET /api/user/**1**
- GET /user/account/find?**user_id=aE1230000token**
- POST /company/account/**Apple**/balance
- GET /admin/settings/account/**bman**

In these instances, you can probably guess other potential resources, like the following, by altering the bold values:

- GET /api/resource/**3**
- GET /user/account/find?user_id=**23**
- POST /company/account/**Google**/balance
- POST /admin/settings/account/**hdent**

![](attachments/Pasted%20image%2020250713161626.png)
## [OWASP Preventative Measures](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)

Developers should perform tests that specifically test authorization controls. 

- _Implement a proper authorization mechanism that relies on the user policies and hierarchy._
- _Use the authorization mechanism to check if the logged-in user has access to perform the requested action on the record in every function that uses an input from the client to access a record in the database._
- _Prefer the use of random and unpredictable values as GUIDs for records' IDs._
- _Write tests to evaluate the vulnerability of the authorization mechanism. Do not deploy changes that make the tests fail._

