## states

- account does not exist
- account exists (unverified)
- account has user logged out
- account has user logged in
- account is deleted

## events

- sign up
- verify account
- log in
- log out
- delete account
- re-sign up

## state transition table

| current State | event | next state | expected outcome |
| --------------- | --------------- | --------------- | --------------- |
| account does not exist | sign up | account exists | account pending verification |
| account exists (unverified) | verify account | account is verified | account verified |
| account has user logged out | log in | account is logged in | user session created |
| account has user logged in | log out | account is logged out | user logged out |
| account has user logged in | delete account | account is deleted | account deleted by user |
| account is deleted | delete account | account is deleted | account deleted by user |

