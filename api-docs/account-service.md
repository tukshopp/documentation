# Account Service API

User account management service for TukShopp platform.

## Overview

The Account Service manages user profiles, addresses, payment methods, preferences, and account settings for customers, vendors, and riders.

**Base URL:** `https://api.tukshopp.ng`

**Swagger Documentation:** [https://api.tukshopp.ng/docs/account-service](https://api.tukshopp.ng/docs/account-service)

---

## Authentication

All endpoints require authentication via Bearer token:

```http
Authorization: Bearer {access_token}
```

---

## Endpoints

### Profile Management

#### Get Profile

Retrieve user profile information.

**Endpoint:** `GET /account/profile`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "usr_1234567890",
    "email": "john@example.com",
    "phone": "+1234567890",
    "first_name": "John",
    "last_name": "Doe",
    "user_type": "customer",
    "avatar_url": "https://cdn.tukshopp.ng/avatars/...",
    "email_verified": true,
    "phone_verified": true,
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-11-11T08:20:00Z"
  }
}
```

---

#### Update Profile

Update user profile information.

**Endpoint:** `PUT /account/profile`

**Request Body:**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "phone": "+1234567890",
  "avatar_url": "https://cdn.tukshopp.ng/avatars/new_avatar.jpg"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "usr_1234567890",
    "first_name": "John",
    "last_name": "Doe",
    "updated_at": "2025-11-11T09:15:00Z"
  },
  "message": "Profile updated successfully"
}
```

---

#### Upload Avatar

Upload profile picture.

**Endpoint:** `POST /account/profile/avatar`

**Request:** Multipart form data with image file

**Response:**
```json
{
  "success": true,
  "data": {
    "avatar_url": "https://cdn.tukshopp.ng/avatars/usr_1234567890.jpg"
  },
  "message": "Avatar uploaded successfully"
}
```

---

### Address Management

#### List Addresses

Get all saved addresses.

**Endpoint:** `GET /account/addresses`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "addr_1234",
      "label": "Home",
      "street_address": "123 Main St",
      "apartment": "Apt 4B",
      "city": "Lagos",
      "state": "Lagos State",
      "postal_code": "100001",
      "country": "Nigeria",
      "landmark": "Near City Mall",
      "delivery_instructions": "Ring doorbell twice",
      "is_default": true,
      "latitude": 6.5244,
      "longitude": 3.3792
    }
  ]
}
```

---

#### Add Address

Create a new address.

**Endpoint:** `POST /account/addresses`

**Request Body:**
```json
{
  "label": "Work",
  "street_address": "456 Office Plaza",
  "apartment": "Suite 300",
  "city": "Lagos",
  "state": "Lagos State",
  "postal_code": "100001",
  "country": "Nigeria",
  "landmark": "Next to Central Bank",
  "delivery_instructions": "Call on arrival",
  "latitude": 6.5244,
  "longitude": 3.3792
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "addr_5678",
    "label": "Work",
    "street_address": "456 Office Plaza",
    "is_default": false,
    "created_at": "2025-11-11T10:00:00Z"
  },
  "message": "Address added successfully"
}
```

---

#### Update Address

Update an existing address.

**Endpoint:** `PUT /account/addresses/{address_id}`

**Request Body:**
```json
{
  "delivery_instructions": "Security code is 1234",
  "is_default": true
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "addr_5678",
    "updated_at": "2025-11-11T10:15:00Z"
  },
  "message": "Address updated successfully"
}
```

---

#### Delete Address

Remove an address.

**Endpoint:** `DELETE /account/addresses/{address_id}`

**Response:**
```json
{
  "success": true,
  "message": "Address deleted successfully"
}
```

---

### Payment Methods

#### List Payment Methods

Get all saved payment methods.

**Endpoint:** `GET /account/payment-methods`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "pm_1234",
      "type": "card",
      "card_brand": "visa",
      "last4": "4242",
      "expiry_month": 12,
      "expiry_year": 2026,
      "is_default": true,
      "created_at": "2025-01-15T10:30:00Z"
    }
  ]
}
```

---

#### Add Payment Method

Add a new payment method.

**Endpoint:** `POST /account/payment-methods`

**Request Body:**
```json
{
  "type": "card",
  "card_number": "4242424242424242",
  "expiry_month": 12,
  "expiry_year": 2026,
  "cvv": "123",
  "cardholder_name": "John Doe"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "pm_5678",
    "type": "card",
    "card_brand": "visa",
    "last4": "4242",
    "is_default": false
  },
  "message": "Payment method added successfully"
}
```

---

#### Set Default Payment Method

Set a payment method as default.

**Endpoint:** `PUT /account/payment-methods/{method_id}/default`

**Response:**
```json
{
  "success": true,
  "message": "Default payment method updated"
}
```

---

#### Delete Payment Method

Remove a payment method.

**Endpoint:** `DELETE /account/payment-methods/{method_id}`

**Response:**
```json
{
  "success": true,
  "message": "Payment method deleted successfully"
}
```

---

### Preferences & Settings

#### Get Preferences

Retrieve user preferences.

**Endpoint:** `GET /account/preferences`

**Response:**
```json
{
  "success": true,
  "data": {
    "notifications": {
      "order_updates": true,
      "promotions": true,
      "new_vendors": false,
      "email_notifications": true,
      "sms_notifications": false,
      "push_notifications": true
    },
    "language": "en",
    "currency": "NGN",
    "dietary_preferences": ["vegetarian"],
    "favorite_cuisines": ["italian", "chinese"]
  }
}
```

---

#### Update Preferences

Update user preferences.

**Endpoint:** `PUT /account/preferences`

**Request Body:**
```json
{
  "notifications": {
    "promotions": false,
    "push_notifications": true
  },
  "language": "en"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Preferences updated successfully"
}
```

---

### Favorites

#### Get Favorite Vendors

List favorite vendors.

**Endpoint:** `GET /account/favorites/vendors`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "vendor_id": "vnd_1234",
      "name": "Tasty Bites",
      "category": "Restaurant",
      "rating": 4.8,
      "added_at": "2025-10-01T12:00:00Z"
    }
  ]
}
```

---

#### Add to Favorites

Add a vendor to favorites.

**Endpoint:** `POST /account/favorites/vendors/{vendor_id}`

**Response:**
```json
{
  "success": true,
  "message": "Added to favorites"
}
```

---

#### Remove from Favorites

Remove a vendor from favorites.

**Endpoint:** `DELETE /account/favorites/vendors/{vendor_id}`

**Response:**
```json
{
  "success": true,
  "message": "Removed from favorites"
}
```

---

### Account Security

#### Change Password

Change account password.

**Endpoint:** `PUT /account/security/password`

**Request Body:**
```json
{
  "current_password": "OldP@ssw0rd",
  "new_password": "NewSecureP@ss123",
  "confirm_password": "NewSecureP@ss123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Password changed successfully"
}
```

---

#### Two-Factor Authentication

Enable 2FA.

**Endpoint:** `POST /account/security/2fa/enable`

**Response:**
```json
{
  "success": true,
  "data": {
    "qr_code": "data:image/png;base64,...",
    "secret": "JBSWY3DPEHPK3PXP"
  },
  "message": "Scan QR code with authenticator app"
}
```

---

#### Verify 2FA

Verify 2FA setup.

**Endpoint:** `POST /account/security/2fa/verify`

**Request Body:**
```json
{
  "code": "123456"
}
```

**Response:**
```json
{
  "success": true,
  "message": "2FA enabled successfully"
}
```

---

### Account Activity

#### Get Login History

View recent login activity.

**Endpoint:** `GET /account/activity/logins`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "log_1234",
      "ip_address": "192.168.1.1",
      "device": "iPhone 13",
      "browser": "Safari",
      "location": "Lagos, Nigeria",
      "timestamp": "2025-11-11T08:00:00Z",
      "success": true
    }
  ]
}
```

---

#### Get Activity Log

View account activity.

**Endpoint:** `GET /account/activity/log`

**Query Parameters:**
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 20)
- `type` - Activity type filter

**Response:**
```json
{
  "success": true,
  "data": {
    "activities": [
      {
        "id": "act_1234",
        "type": "profile_updated",
        "description": "Profile information updated",
        "timestamp": "2025-11-11T09:15:00Z"
      },
      {
        "id": "act_1235",
        "type": "address_added",
        "description": "New address added",
        "timestamp": "2025-11-11T10:00:00Z"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 95,
      "items_per_page": 20
    }
  }
}
```

---

### Account Deletion

#### Request Account Deletion

Initiate account deletion process.

**Endpoint:** `POST /account/delete-request`

**Request Body:**
```json
{
  "reason": "No longer need the service",
  "password": "userPassword123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Account deletion request submitted. You have 30 days to cancel."
}
```

---

#### Cancel Deletion Request

Cancel pending account deletion.

**Endpoint:** `POST /account/delete-request/cancel`

**Response:**
```json
{
  "success": true,
  "message": "Account deletion cancelled"
}
```

---

## User-Type Specific Endpoints

### Customer-Specific

#### Get Order History

**Endpoint:** `GET /account/customer/order-history`

See [Marketplace Service](./marketplace-service.md) for details.

---

### Vendor-Specific

#### Get Business Profile

**Endpoint:** `GET /account/vendor/business-profile`

See [Vendor Service](./vendor-service.md) for details.

---

### Rider-Specific

#### Get Rider Profile

**Endpoint:** `GET /account/rider/profile`

**Response:**
```json
{
  "success": true,
  "data": {
    "rider_id": "rdr_1234",
    "vehicle_type": "motorcycle",
    "vehicle_number": "ABC123",
    "license_number": "DL123456",
    "status": "active",
    "zones": ["zone_a", "zone_b"],
    "rating": 4.9,
    "total_deliveries": 1250
  }
}
```

---

## Error Codes

| Code | Description |
|------|-------------|
| `ACC_001` | Profile not found |
| `ACC_002` | Invalid address format |
| `ACC_003` | Payment method validation failed |
| `ACC_004` | Cannot delete default address |
| `ACC_005` | Cannot delete default payment method |
| `ACC_006` | Weak password |
| `ACC_007` | Incorrect current password |
| `ACC_008` | Avatar file too large |

---

## Examples

### Complete Profile Setup

```javascript
const token = 'your_access_token';

// 1. Update profile
await fetch('https://api.tukshopp.ng/account/profile', {
  method: 'PUT',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    first_name: 'John',
    last_name: 'Doe',
    phone: '+1234567890'
  })
});

// 2. Add address
await fetch('https://api.tukshopp.ng/account/addresses', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    label: 'Home',
    street_address: '123 Main St',
    city: 'Lagos',
    state: 'Lagos State'
  })
});

// 3. Add payment method
await fetch('https://api.tukshopp.ng/account/payment-methods', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    type: 'card',
    card_number: '4242424242424242',
    expiry_month: 12,
    expiry_year: 2026,
    cvv: '123'
  })
});
```

---

## Related Documentation

- [Auth Service](./auth-service.md) - Authentication
- [Marketplace Service](./marketplace-service.md) - Orders and deliveries
- [Vendor Service](./vendor-service.md) - Vendor operations

---

**Official Swagger Documentation:** [https://api.tukshopp.ng/docs/account-service](https://api.tukshopp.ng/docs/account-service)

Last Updated: November 11, 2025

