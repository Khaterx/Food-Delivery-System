# 📄 Use Case: **User Registration & Login**

## 1. Overview

Enables new users to create accounts and returning users to securely authenticate.  
Supports role-based access, session management, password security, and account lifecycle controls.

---
## 2. Actors

| Actor      | Responsibilities                                    |
| ---------- | --------------------------------------------------- |
| Guest      | Registers for a new account                         |
| Registered | Logs in to access personalized and secured features |
| Admin      | Manages users/accounts (enable, disable, audit)     |

---
## 3. Functional Requirements

- Register new users with unique email and/or username.
- Validate input fields: email, username, password.
- Authenticate users based on credentials.
- Manage user roles (admin, manager, user).
- Provide forgot password and password reset options.
- Profile management: allow view/edit profile.
- Enable/disable user accounts.
- Support social login (Google, Facebook, etc.).
- Store and hash passwords securely.
- Enforce session management (login/logout).

---
## 4. Database Tables (Key Entities)

- **user:** id, username, email, password_hash, status, created_at, updated_at
- **role:** id, name, description
- **user_role:** user_id, role_id
- **user_group, group_role, user_group:** (for advanced permissions if needed)
- **cart:** Each user has a cart created at registration or on first item add

---
## 5. API Endpoints

| Endpoint                    | Method  | Description                |
| --------------------------- | ------- | -------------------------- |
| `/api/auth/signup`          | POST    | Register a new user        |
| `/api/auth/login`           | POST    | Authenticate and login     |
| `/api/auth/forgot-password` | POST    | Begin password reset flow  |
| `/api/auth/reset-password`  | POST    | Complete password reset    |
| `/api/auth/logout`          | POST    | End user session           |
| `/api/user/profile`         | GET/PUT | View & update user profile |
| `/api/auth/verify`          | POST    | Email or OTP verification  |
| `/api/auth/social`          | POST    | Login via social accounts  |
| `/api/user/account/status`  | PUT     | Enable/disable accounts    |

---
## 6. Main Flow

## Registration

1. User submits signup form: username, email, password.
2. System validates fields (format, duplicates, strength).
3. Password is hashed.
4. User is saved to database; default role assigned.
5. (Optional) Verification email/OTP sent.
6. User receives registration confirmation.

## Login

1. User submits credentials (email/username, password).
2. System validates credentials.
3. On success, a session/JWT is created.
4. User is redirected to dashboard.
5. On failure, error message is shown.

## Forgot/Reset Password

1. User submits “Forgot password”.
2. System sends reset link or OTP.
3. User sets a new password using the token/OTP.
4. System updates the user’s password.
   
---

## 7. Validation and Security

- **Password**: Min length, complexity checks, hashed (e.g., bcrypt).
- **Email/Username**: Uniqueness and format checked.
- **API Security**: Rate limiting, account lockout on brute-force.
- **Sessions**: Tokens/cookies must be secure, may have timeouts/invalidation.

---
## 8. Error Handling

| Scenario                 | Error Message/Response          |
| ------------------------ | ------------------------------- |
| Invalid input            | 400 Bad Request (details)       |
| Duplicate email/username | 409 Conflict ("Already exists") |
| Incorrect password       | 401 Unauthorized                |
| Account locked/disabled  | 403 Forbidden                   |
| Verification required    | 401 Unauthorized ("Verify...")  |
| Server/internal error    | 500 Internal Server Error       |

---
## 9. Flow Chart Diagram

![[user-registration-login-flowchart.png | User Registration & Login Flow Chart]]

---
## 10. Sequence Diagram

![[user-registration-login-sequence.png]]

---
## 11. Pseudocode
Refer to the [user-registration-login-pseudocode.md](user-registration-login-pseudocode.md) for a detailed implementation guide.

---
## Notes
- Every user gets a cart (created at registration or first item add).    
- Redis memory DB may cache user/carts for high performance.
- All user actions (login, logout, register, reset, etc.) should be audited.