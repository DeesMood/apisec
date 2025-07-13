API4:2023 Unrestricted Resource Consumption is an API issue where the provider of the API does not have safeguards in place to prevent excessive use of their API.

When there are no restrictions for resource consumption the API provider could become a victim of Denial of Service (DoS) attacks or experience unnecessary financial costs.

Unrestricted Resource Consumption is an updated version of API4:2019 Lack of Resources and Rate Limiting.

## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/)

_Exploitation requires simple API requests. Multiple concurrent requests can be performed from a single local computer or by using cloud computing resources. Most of the automated tools available are designed to cause DoS via high loads of traffic, impacting APIs’ service rate._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/)

_It's common to find APIs that do not limit client interactions or resource consumption. Crafted API requests, such as those including parameters that control the number of resources to be returned and performing response status/time/length analysis should allow identification of the issue. The same is valid for batched operations. Although threat agents don't have visibility over costs impact, this can be inferred based on service providers’ (e.g. cloud provider) business/pricing model._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/)

_Exploitation can lead to DoS due to resource starvation, but it can also lead to operational costs increase such as those related to the infrastructure due to higher CPU demand, increasing cloud storage needs, etc._

**The OWASP API Security Project states:**

An API is vulnerable if at least one of the following limits is missing or set inappropriately (e.g. too low/high):

- Execution timeouts
- Maximum allocable memory
- Maximum number of file descriptors
- Maximum number of processes
- Maximum upload file size
- Number of operations to perform in a single API client request (e.g. GraphQL batching)
- Number of records per page to return in a single request-response
- Third-party service providers' spending limit

## [OWASP Preventative Measures](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/)

- _Docker makes it easy to limit [memory](https://docs.docker.com/config/containers/resource_constraints/#memory), [CPU](https://docs.docker.com/config/containers/resource_constraints/#cpu), [number of restarts](https://docs.docker.com/engine/reference/commandline/run/#restart-policies---restart), [file descriptors, and processes](https://docs.docker.com/engine/reference/commandline/run/#set-ulimits-in-container---ulimit)._
- _Implement a limit on how often a client can call the API within a defined timeframe._
- _Notify the client when the limit is exceeded by providing the limit number and the time at which the limit will be reset._
- _Add proper server-side validation for query string and request body parameters, specifically the one that controls the number of records to be returned in the response._
- _Define and enforce maximum size of data on all incoming parameters and payloads such as maximum length for strings and maximum number of elements in arrays._

