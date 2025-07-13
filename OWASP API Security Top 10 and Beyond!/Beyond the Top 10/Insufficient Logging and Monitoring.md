Logging and monitoring are necessary and important layer of API security. In order to detect an attack against an API an organization must have monitoring in place. Without sufficient logging and monitoring an API provider is operating in the dark and API attacks are guaranteed to go unnoticed until it is far too late.

## [OWASP 2019 Attack Vector Description](https://owasp.org/API-Security/editions/2019/en/0xaa-insufficient-logging-monitoring/)

_Attackers take advantage of lack of logging and monitoring to abuse systems without being noticed._

## [OWASP 2019 Security Weakness Description](https://owasp.org/API-Security/editions/2019/en/0xaa-insufficient-logging-monitoring/)

_Without logging and monitoring, or with insufficient logging and monitoring, it is almost impossible to track suspicious activities and respond to them in a timely fashion._

## [OWASP 2019 Impacts Description](https://owasp.org/API-Security/editions/2019/en/0xaa-insufficient-logging-monitoring/)

_Without visibility over ongoing malicious activities, attackers have plenty of time to fully compromise systems._

## [OWASP 2019 Preventative Measures](https://owasp.org/API-Security/editions/2019/en/0xaa-insufficient-logging-monitoring/)

- _Log all failed authentication attempts, denied access, and input validation errors._
- _Logs should be written using a format suited to be consumed by a log management solution and should include enough detail to identify the malicious actor._
- _Logs should be handled as sensitive data, and their integrity should be guaranteed at rest and transit._
- _Configure a monitoring system to continuously monitor the infrastructure, network, and API functioning._
- _Use a Security Information and Event Management (SIEM) system to aggregate and manage logs from all components of the API stack and hosts._
- _Configure custom dashboards and alerts, enabling suspicious activities to be detected and responded to earlier._
