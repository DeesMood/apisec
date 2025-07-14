![](attachments/Pasted%20image%2020250714074208.png)

![](attachments/Pasted%20image%2020250714074218.png)

Basic authentication relies on the HTTP authorization header. and the -basic scheme of that header. It's essentially sending the username password in plain text basics that were encoded in the authorization header.

![](attachments/Pasted%20image%2020250714074224.png)

Very often this is to be able to throttle the number of requests the, application can make to the API or it's to monetize how per request that type of thing.

![](attachments/Pasted%20image%2020250714074231.png)

With TLS authentication, as we're meeting here, we're talking about mutual TLS. So both the client, the caller and the server. have their own certificates and keys and they present themselves and authenticate themselves to each other. So both the application and the API would present proof of who they are and then set up a secure encrypted tunnel that they can communicate over.

![](attachments/Pasted%20image%2020250714074236.png)

Token based authentication is where a trusted third party issues tokens to the application, or the caller, as we say it here. These tokens are, beneficial because they can expire. They can have audiences, meaning they can only be sent to certain parties. The API then trusts the issuer of these tokens.

![](attachments/Pasted%20image%2020250714074241.png)

you can combine this and provide a true authorization system built upon the token based architecture because the token can convey so much more than just a caller ID. It can contain a lot of details that can be useful for authorization. If I need to authorize a request and I know Not only who you are, but where you are, or how old you are, or which shoe size you have, or which account you belong to.

![](attachments/Pasted%20image%2020250714074250.png)

![](attachments/Pasted%20image%2020250714074319.png)

Authentication is the process of figuring out who you are, who is the user that made this call. It can also be who is the machine that made this call. And authorization is the process of figuring out what are you allowed to do.

