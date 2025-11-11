# Auth Service API

Authentication and authorization service for TukShopp platform.

## Overview

The Auth Service handles all authentication and authorization operations including user registration, login, token management, and password recovery.

**Base URL:** `https://api.tukshopp.ng`

**Swagger Documentation:** [https://api.tukshopp.ng/docs/auth-service](https://api.tukshopp.ng/docs/auth-service)

---

## Authentication Flow

### Standard Login Flow

```
User enters credentials
    ↓
POST /auth/login
    ↓
Validate credentials
    ↓
Generate access & refresh tokens
    ↓
Return tokens to client
    ↓
Client stores tokens
    ↓
Use access token for API requests
```

### Token Refresh Flow

```
Access token expires
    ↓
POST /auth/refresh
    ↓
Validate refresh token
    ↓
Generate new access token
    ↓
Return new token
```

---

## Endpoints

### Register User

Create a new user account.

**Endpoint:** `POST /auth/register`

**Request Body:**
```json
{
  "email": "user@example.com",
  "phone": "+1234567890",
  "password": "SecureP@ssw0rd",
  "first_name": "John",
  "last_name": "Doe",
  "user_type": "customer",
  "accept_terms": true
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user_id": "usr_1234567890",
    "email": "user@example.com",
    "user_type": "customer",
    "verification_required": true
  },
  "message": "Registration successful. Please verify your email."
}
```

---

### Login

Authenticate user and receive access tokens.

**Endpoint:** `POST /auth/login`

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecureP@ssw0rd"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600,
    "user": {
      "id": "usr_1234567890",
      "email": "user@example.com",
      "user_type": "customer",
      "verified": true
    }
  }
}
```

---

### Logout

Invalidate user session and tokens.

**Endpoint:** `POST /auth/logout`

**Headers:**
```http
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

### Refresh Token

Get a new access token using refresh token.

**Endpoint:** `POST /auth/refresh`

**Request Body:**
```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600
  }
}
```

---

### Request Password Reset

Initiate password reset process.

**Endpoint:** `POST /auth/password/reset-request`

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Password reset instructions sent to your email"
}
```

---

### Reset Password

Complete password reset with token.

**Endpoint:** `POST /auth/password/reset`

**Request Body:**
```json
{
  "reset_token": "reset_token_here",
  "new_password": "NewSecureP@ssw0rd",
  "confirm_password": "NewSecureP@ssw0rd"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

---

### Verify Email

Verify user email address.

**Endpoint:** `POST /auth/verify-email`

**Request Body:**
```json
{
  "verification_token": "verification_token_here"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Email verified successfully"
}
```

---

### Resend Verification

Resend email verification.

**Endpoint:** `POST /auth/verify-email/resend`

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Verification email sent"
}
```

---

### Verify Phone (OTP)

Send OTP for phone verification.

**Endpoint:** `POST /auth/verify-phone/send`

**Request Body:**
```json
{
  "phone": "+1234567890"
}
```

**Response:**
```json
{
  "success": true,
  "message": "OTP sent to your phone"
}
```

---

### Confirm Phone OTP

Verify phone with OTP.

**Endpoint:** `POST /auth/verify-phone/confirm`

**Request Body:**
```json
{
  "phone": "+1234567890",
  "otp": "123456"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Phone verified successfully"
}
```

---

## OAuth 2.0

### OAuth Flow

TukShopp supports OAuth 2.0 for third-party integrations.

### Supported Grant Types

- Authorization Code
- Refresh Token
- Client Credentials (for server-to-server)

### OAuth Endpoints

**Authorization:** `GET /auth/oauth/authorize`

**Token:** `POST /auth/oauth/token`

**Revoke:** `POST /auth/oauth/revoke`

---

## Token Management

### Access Tokens

- **Type:** JWT (JSON Web Token)
- **Expiry:** 1 hour
- **Format:** Bearer token
- **Header:** `Authorization: Bearer {token}`

### Refresh Tokens

- **Expiry:** 30 days
- **Usage:** Get new access token
- **Security:** Rotate on use

### Token Payload

```json
{
  "sub": "usr_1234567890",
  "email": "user@example.com",
  "user_type": "customer",
  "iat": 1699747200,
  "exp": 1699750800
}
```

---

## User Types

- `customer` - End user/customer
- `vendor` - Business owner/vendor
- `rider` - Delivery personnel
- `admin` - Platform administrator

---

## Security

### Password Requirements

- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character

### Rate Limiting

- Login attempts: 5 per 15 minutes
- Password reset: 3 per hour
- Registration: 3 per hour per IP

### Best Practices

✅ Store tokens securely  
✅ Use HTTPS only  
✅ Implement token refresh  
✅ Handle token expiry  
✅ Never expose tokens in URLs  

---

## Error Codes

| Code | Description |
|------|-------------|
| `AUTH_001` | Invalid credentials |
| `AUTH_002` | Account not verified |
| `AUTH_003` | Token expired |
| `AUTH_004` | Invalid token |
| `AUTH_005` | Account suspended |
| `AUTH_006` | Email already exists |
| `AUTH_007` | Weak password |
| `AUTH_008` | Rate limit exceeded |

---

## Examples

### Complete Authentication Flow

```javascript
// 1. Register
const registerResponse = await fetch('https://api.tukshopp.ng/auth/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'john@example.com',
    password: 'SecureP@ss123',
    first_name: 'John',
    last_name: 'Doe',
    user_type: 'customer',
    accept_terms: true
  })
});

// 2. Verify email (user clicks link in email)

// 3. Login
const loginResponse = await fetch('https://api.tukshopp.ng/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'john@example.com',
    password: 'SecureP@ss123'
  })
});
const { data: { access_token, refresh_token } } = await loginResponse.json();

// 4. Use access token for API calls
const profileResponse = await fetch('https://api.tukshopp.ng/account/profile', {
  headers: { 'Authorization': `Bearer ${access_token}` }
});

// 5. Refresh when token expires
const refreshResponse = await fetch('https://api.tukshopp.ng/auth/refresh', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ refresh_token })
});
```

---

## Related Documentation

- [Account Service](./account-service.md) - Manage user profiles
- [API Overview](./README.md) - Getting started with TukShopp API

---

**Official Swagger Documentation:** [https://api.tukshopp.ng/docs/auth-service](https://api.tukshopp.ng/docs/auth-service)

Last Updated: November 11, 2025

