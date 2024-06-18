As a user I register on a domain kingbillywin10.com, this is a current AU mirror.
After registration, I got the link, like that:
https://www.kbredir.com/users/confirmation?confirmation_token=XSAJWhLRPJUnjxYZSix2&locale=en-CA

You may mention that it points to our redirector domain.

In our settings inside redirector for AU region, there are currently 3 mirrors:
- kingbillywin9.com
- kingbillywin10.com
- kingbillywin11.com

It means, that depending on internet connection, when user will click the link in a mail, he could be redirected to any of that domains.

For example: https://www.kingbillywin11.com/users/confirmation?confirmation_token=XSAJWhLRPJUnjxYZSix2&locale=en-CA

Which is actually another domain, on which user is not authorized.

We expect that user will be authorized based on confirmation token. It means, that if user opens link within correct `confirmation_token`, he will be authorized on any domain, whenever he gets. It means that it works as autologin.


