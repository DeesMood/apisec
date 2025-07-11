Which of the following would make for a good vAPI request to test for SSRF?
**/vapi/serversurfer**

What HTTP status code does vAPI's serversurfer respond with for a successful SSRF attack?
**200**

What happens to data retrieved by vAPI with a successful in-band SSRF attack against the serversurfer?
**The resources retrieved are sent back base64 encoded**