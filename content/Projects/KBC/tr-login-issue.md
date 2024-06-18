When I try to register on a landing domain, like https://kbc-tr-landings-stage.pages.dev/example

I got a link with system domain inside:

https://kunitokotachi.org/action?data=M923RnqDGFx8f%2FCLIexuh5UrJCO9KkXM3V5XUl8xiypAYuJ3r0Q%2BJvvNrpyrZ%2F3XWgnfWbD%2B3OwehqP%2F2lW7eX2%2F7NqBheI%2BMUY1DvO3KFtmG7XOSOynMNy4kXK%2BG1XwQOJbxkN%2BwAP1C8zxp85gzuHuF4YB2g6gpw%3D%3D

Where https://kunitokotachi.org is a system API domain, that should not be exposed to clients. Somehow it just sent to user's email. And actually link doesn't work. It shows error, preventing user from email verification or login, it does not even lead to site.
![[tr-error-confirmation-mail.png]]
From api responce I think that there should be domain https://yam.ninlil.world/ instead, like:

https://yam.ninlil.world/action?data=M923RnqDGFx8f%2FCLIexuh5UrJCO9KkXM3V5XUl8xiypAYuJ3r0Q%2BJvvNrpyrZ%2F3XWgnfWbD%2B3OwehqP%2F2lW7eX2%2F7NqBheI%2BMUY1DvO3KFtmG7XOSOynMNy4kXK%2BG1XwQOJbxkN%2BwAP1C8zxp85gzuHuF4YB2g6gpw%3D%3D

### Authorization
Actually I expect that after visiting link with token at first, user will be authorized on a product.
Currently, if I fix link from email end point it on a working mirror, I will see responce like that:
https://yam.ninlil.world/api/profile/send-action?language=en&data=dFsQGhneIYiT%2FhvyyqCYjMu22qkaa8xvkYOIgYwqmzS91W2%2B9Zykg1MnX7LWCxpF%2Bv8n3CNj1AMv8pHNko2fziZiYJ0kgtOPMuJkJa7lIn%2FYUVMpCEJ7Oe5opbMU3SBNY14Aoev8m%2FpVgBtZOfgYJMv%2BCQVgj0xU

that will lead to error:
```json
{
  "message": "Your session has expired.",
  "code": 27,
  "error": "error_code_27"
}
```

It means, that now there is now way to login into product using confirmation link.