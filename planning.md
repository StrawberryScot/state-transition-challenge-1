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
| account exists (unverified) | log in | account exists (unverified) | no changes |
| account exists (unverified) | sign up | account exists (unverified) | no changes |
| account has user logged out | log in | account is logged in | user session created |
| account has user logged out | sign up | account has user logged out | no changes |
| account has user logged in | log out | account is logged out | user logged out |
| account has user logged in | delete account | account is deleted | account deleted by user |
| account is deleted | delete account | account is deleted | account deleted by user |

## state transition diagram

```mermaid
stateDiagram-v2
  account-does-not-exist ->> account-exists-but-unverified : sign up
  account-exists-but-unverified ->> account-has-user-logged-out : verify account
  account-has-user-logged-out ->> account-has-user-logged-in : log in
  account-has-user-logged-in ->> account-has-user-logged-out : log out
  account-has-user-logged-in ->> account-is-deleted : delete account
  account-is-deleted ->> account-does-not-exist : 30 day period
```

## test case

### starting from 'account-does-not-exist'

1. **sign up** --> moves to account-exists-but-unverified
  - check the account appears in the database (as unverified)
2. **verify account** --> moves to account-has-user-logged-out
  - check the account in the database is now marked as verified
3. **log in** --> moves to account-has-user-logged-in
  - check there is a new session active
4. **log out** --> moves to account-has-user-logged-out
  - check the session has been cleared
5. **log in** --> moves to account-has-user-logged-in
  - check there is a new session active
6. **delete account** --> moves to account-is-deleted
  - check account has been moved to deleted accounts in the database
7. **30 days pass (mock)** --> moves to account-does-not-exist
  - check account is no longer in the database
8. **sign up** --> moves to account-exists-but-unverified
  - check the account appears in the database (as unverified)
