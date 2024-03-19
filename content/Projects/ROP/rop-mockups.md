# ROP mockups
[link](./rop.pdf)

[figma](https://www.figma.com/file/CcOcWyLO4Gm1XnUQTa2i2c/RLS-ADMIN?type=design)

[description](https://obs.nodeart.app/Notes/RLS-operator-platform%20(ROP))


### Roles
| Role Name      | What                                                                                  |
| -------------- | ------------------------------------------------------------------------------------- |
| KYC            | Add new users, Change user status (new, approved, blocked), upload document screen    |
| Cashier        | See user balance, add new transactions (deposit, withdrawal)                          |
| ResetPasswords | Option to reset password for existing users                                           |
| Audit          | Access to reports (all transactions, user profile updates, all cashiers transactions) |
| admin          | Assign/detach roles to users                                                          |
### Feature Flags
| Flag              | Feature                                        |
| ----------------- | ---------------------------------------------- |
| allowRegistration | allow user self registration with 'New' status |
| uploadDocuments   | allow documents upload                         |


## RLS API
[link](https://realliveslots.com/docs/api)