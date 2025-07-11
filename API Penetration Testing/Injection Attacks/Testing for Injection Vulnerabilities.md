Fuzzing APIs is the process of sending various types of input to an endpoint to provoke an unintended response.

If an endpoint does not sanitize or validate user input then the right payload could cause a verbose response, a delay in processing time, an internal server error, or an error with the database.

You should attempt fuzzing against all potential inputs and especially within the following:
- Headers
- Query string parameters
- Parameters in POST/PUT requests

In particular, look for any indication that your payload bypassed security controls and was interpreted as a command, either at the operating system, programming, or database level. 

This response could be as obvious as a message such as “SQL Syntax Error” or something more subtle like taking a little more time to process a request.

You could even receive an entire verbose error dump that can provide you with plenty of details about the host.

## Discovering Injection Vulnerabilities

if the API is expecting a certain type of input (number, string, boolean value) send:
- A very large number
- A very large string
- A negative number
- A string (instead of a number or boolean value)
- Random characters
- Boolean values
- Meta characters

## SQL Injection Metacharacters

SQL Metacharacters are characters that SQL treats as functions rather than data. For example, -- is a metacharacter that tells the SQL interpreter to ignore the following input because it is a comment.

SQL injection, allows a remote attacker to interact with the application’s backend SQL database. With this access, an attacker could obtain or delete sensitive data such as credit card numbers, usernames, passwords, and other gems. In addition, an attacker could leverage SQL database functionality to bypass authentication, exfiltrate private data, and gain system access.

By requesting the unexpected, you could to discover a situation the developers didn’t predict, and the database might return an error in the response. These errors are often verbose, revealing sensitive information about the database.

When looking for requests to target for database injections, seek out those that allow client input and can be expected to interact with a database.

Here are some SQL metacharacters that can cause some issues:
`'`

`''`

`;%00`

`--`

`-- -`

`""`

`;`

`' OR '1`

`' OR 1 -- -`

`" OR "" = "`

`" OR 1 = 1 -- -`

`' OR '' = '`

`OR 1=1`

Imagine that the backend is programmed to handle the API authentication process with a SQL query like the following, which is a SQL authentication query that checks for username and password:

`SELECT * FROM userdb WHERE username = 'hAPI_hacker' AND password = 'Password1!'`

The query retrieves the values hAPI_hacker and Password1! from the user input. If, instead of a password, we supplied the API with the value ' OR 1=1-- -, the SQL query might instead look like this:

`SELECT * FROM userdb WHERE username = 'hAPI_hacker' OR 1=1-- -`

## NoSQL Injection

Practically speaking, you’ll conduct many similar attacks and target similar requests to SQL Injection, but your actual payloads will vary.

The following are common NoSQL metacharacters you could send in an API request to manipulate the database:

`$gt` 

`{"$gt":""}`

`{"$gt":-1}`

`$ne`

`{"$ne":""}`

`{"$ne":-1}`

 `$nin`

`{"$nin":1}`

`{"$nin":[1]}`

`{"$where":  "sleep(1000)"}`

$gt is a MongoDB NoSQL query operator that selects documents that are greater than the provided value. The $ne query operator selects documents where the value is not equal to the provided value. The $nin operator is the “not in” operator, used to select documents where the field value is not within the specified array. Many of the others in the list contain symbols that are meant to cause verbose errors or other interesting behavior, such as bypassing authentication or waiting 10 seconds.

## OS Injection

Operating system command injection typically requires being able to leverage system commands that the application has access to or escaping the application altogether. Some key places to target include URL query strings, request parameters, and headers, as well as any request that has thrown unique or verbose errors (especially those containing any operating system information) during fuzzing attempts.

If a web application is vulnerable, it would allow an attacker to add command separators to existing command and then follow it with additional operating system commands:

`|`

`||`

`&`

`&&`

`'`

`"`

`;`

`'"`

**Common Operating System Commands to Use in Injection Attacks**

| Operating system      | Command                                                                                                                                                                                             |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Windows               | **ipconfig** shows the network configuration.<br><br>**dir** prints the contents of a directory.<br><br>**ver** prints the operating system and version.<br><br>**whoami** prints the current user. |
| *nix (Linux and Unix) | **ifconfig** shows the network configuration.<br><br>**ls** prints the contents of a directory.<br><br>**pwd** prints the current working directory.<br><br>**whoami** prints the current user.     |

## Fuzzing Deep with WFuzz

You can start building out the WFuzz attack. For additional information about WFuzz, use:  
$wfuzz --help

Start by specifying the payload that you will use with **-z**. Use **-H** to add necessary headers like "Content-Type:application/json" and the authorization token. Then use **-d** to specify the post body. When you are using quotes in a post body, you will need to use backslashes (\) for those to show up in the request. Finally, add the URL that you are targeting.

`$ wfuzz -z file,usr/share/wordlists/nosqli  -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{\"coupon_code\":FUZZ} http://crapi.apisec.ai/community/api/v2/coupon/validate-coupon`

Use the show code option **--sc 200** to filter out the results.

