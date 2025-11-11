# TukShopp API Documentation

Welcome to the TukShopp API documentation. This guide provides comprehensive information about TukShopp's microservices architecture and API endpoints.

## 🏗️ Architecture Overview

TukShopp uses a microservices architecture with the following core services:

- **[Auth Service](./auth-service.md)** - Authentication and authorization
- **[Account Service](./account-service.md)** - User account management
- **[Vendor Service](./vendor-service.md)** - Vendor operations and management
- **[Marketplace Service](./marketplace-service.md)** - Order and delivery management

## 📚 Service Documentation

### Authentication Service
Handles user authentication, token management, and authorization.

**Base URL:** `https://api.tukshopp.ng`

**Documentation:** [Auth Service Swagger Docs](https://api.tukshopp.ng/docs/auth-service)

[View detailed documentation →](./auth-service.md)

---

### Account Service
Manages user accounts, profiles, and account-related operations for customers, vendors, and riders.

**Base URL:** `https://api.tukshopp.ng`

**Documentation:** [Account Service Swagger Docs](https://api.tukshopp.ng/docs/account-service)

[View detailed documentation →](./account-service.md)

---

### Vendor Service
Handles vendor-specific operations including outlet management, menu/catalog, orders, and vendor analytics.

**Base URL:** `https://api.tukshopp.ng`

**Documentation:** [Vendor Service Swagger Docs](https://api.tukshopp.ng/docs/vendor-service)

[View detailed documentation →](./vendor-service.md)

---

### Marketplace Service
Manages the marketplace operations including order placement, delivery management, tracking, and rider operations.

**Base URL:** `https://api.tukshopp.ng`

**Documentation:** [Marketplace Service Swagger Docs](https://api.tukshopp.ng/docs/marketplace-service)

[View detailed documentation →](./marketplace-service.md)

---

## 🚀 Getting Started

### Prerequisites

- API access credentials (API key or OAuth tokens)
- Understanding of RESTful APIs
- HTTPS support
- JSON data format knowledge

### Authentication

All API requests require authentication. TukShopp uses token-based authentication:

```http
Authorization: Bearer {your_access_token}
```

See [Auth Service](./auth-service.md) for authentication workflows.

### Base URL

All API requests should be made to:

```
https://api.tukshopp.ng
```

### Response Format

All responses are returned in JSON format:

```json
{
  "success": true,
  "data": {},
  "message": "Success",
  "timestamp": "2025-11-11T00:00:00Z"
}
```

### Error Handling

Error responses follow this structure:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": {}
  },
  "timestamp": "2025-11-11T00:00:00Z"
}
```

### HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Validation Error |
| 429 | Rate Limit Exceeded |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

---

## 🔐 Authentication & Authorization

### OAuth 2.0 Flow

TukShopp implements OAuth 2.0 for secure authentication:

1. **Authorization Request** - User authorizes application
2. **Token Exchange** - Exchange authorization code for access token
3. **API Requests** - Use access token for API calls
4. **Token Refresh** - Refresh expired tokens

### User Roles

- **Customer** - End users ordering items
- **Vendor** - Business owners managing outlets
- **Rider** - Delivery personnel
- **Admin** - Platform administrators

Each role has specific permissions and accessible endpoints.

---

## 📊 Rate Limiting

API requests are rate-limited to ensure fair usage:

- **Standard tier**: 1000 requests per hour
- **Premium tier**: 5000 requests per hour
- **Enterprise tier**: Custom limits

Rate limit headers:
```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1699747200
```

---

## 🔔 Webhooks

Subscribe to real-time events via webhooks:

### Available Events

- `order.created` - New order placed
- `order.accepted` - Vendor accepts order
- `order.ready` - Order ready for pickup
- `order.picked_up` - Rider picks up order
- `order.delivered` - Order delivered
- `order.cancelled` - Order cancelled
- `payment.completed` - Payment processed
- `payout.processed` - Vendor/rider payout completed

### Webhook Configuration

```json
{
  "url": "https://your-domain.com/webhooks",
  "events": ["order.created", "order.delivered"],
  "secret": "your_webhook_secret"
}
```

---

## 📱 SDK & Libraries

### Official SDKs

- **JavaScript/TypeScript** - Coming soon
- **Python** - Coming soon
- **PHP** - Coming soon
- **Java** - Coming soon

### Community Libraries

Check our [GitHub organization](https://github.com/tukshopp) for community-contributed SDKs.

---

## 🧪 Testing

### Sandbox Environment

Test your integration in the sandbox environment:

```
https://api.sandbox.tukshopp.ng
```

### Test Credentials

Contact developer support for sandbox credentials.

### Postman Collection

Download our Postman collection for easy API testing:
[TukShopp API Postman Collection](#)

---

## 📖 API Reference by Service

### Auth Service

- User registration
- Login/Logout
- Token management
- Password reset
- OAuth flows

[Full Auth Service Documentation →](./auth-service.md)

### Account Service

- Profile management
- Address management
- Payment methods
- Preferences
- Account settings

[Full Account Service Documentation →](./account-service.md)

### Vendor Service

- Outlet management
- Menu/Catalog CRUD
- Order management
- Inventory tracking
- Analytics & reports

[Full Vendor Service Documentation →](./vendor-service.md)

### Marketplace Service

- Order placement
- Order tracking
- Delivery management
- Rider operations
- Search & discovery
- Ratings & reviews

[Full Marketplace Service Documentation →](./marketplace-service.md)

---

## 🔧 Integration Examples

### Quick Start: Place an Order

```javascript
// 1. Authenticate
const authResponse = await fetch('https://api.tukshopp.ng/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'user@example.com',
    password: 'password123'
  })
});
const { token } = await authResponse.json();

// 2. Browse vendors
const vendorsResponse = await fetch('https://api.tukshopp.ng/marketplace/vendors', {
  headers: { 'Authorization': `Bearer ${token}` }
});
const vendors = await vendorsResponse.json();

// 3. Place order
const orderResponse = await fetch('https://api.tukshopp.ng/marketplace/orders', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    items: [
      { vendor_id: 'vendor123', product_id: 'prod456', quantity: 2 }
    ],
    delivery_address_id: 'addr789',
    payment_method: 'card'
  })
});
const order = await orderResponse.json();
```

---

## 🛠️ Developer Tools

### API Explorer

Interactive API explorer available at:
[https://api.tukshopp.ng/explorer](https://api.tukshopp.ng/explorer)

### Swagger/OpenAPI

Access complete OpenAPI specifications:
- [Auth Service Swagger](https://api.tukshopp.ng/docs/auth-service)
- [Account Service Swagger](https://api.tukshopp.ng/docs/account-service)
- [Vendor Service Swagger](https://api.tukshopp.ng/docs/vendor-service)
- [Marketplace Service Swagger](https://api.tukshopp.ng/docs/marketplace-service)

---

## 💡 Best Practices

### Performance

✅ **Use pagination** for list endpoints  
✅ **Cache responses** where appropriate  
✅ **Implement retry logic** with exponential backoff  
✅ **Batch requests** when possible  
✅ **Use webhooks** instead of polling  

### Security

✅ **Never expose API keys** in client-side code  
✅ **Use HTTPS** for all requests  
✅ **Validate webhook signatures**  
✅ **Implement rate limiting** on your end  
✅ **Rotate credentials** regularly  

### Error Handling

✅ **Handle all error codes** gracefully  
✅ **Log errors** for debugging  
✅ **Provide user-friendly messages**  
✅ **Implement fallback mechanisms**  

---

## 📞 Support

### Developer Support

- **Email**: developers@tukshopp.ng
- **Documentation**: [https://docs.tukshopp.ng](https://docs.tukshopp.ng)
- **Status Page**: [https://status.tukshopp.ng](https://status.tukshopp.ng)
- **Community Forum**: [Link to forum]

### Reporting Issues

Found a bug or issue? Report it:
- GitHub Issues: [tukshopp/api-issues](https://github.com/tukshopp/api-issues)
- Email: api-support@tukshopp.ng

---

## 📋 Changelog

Track API changes and updates:
[API Changelog →](./CHANGELOG.md)

---

## 📜 Terms & Policies

- [API Terms of Service](#)
- [Rate Limiting Policy](#)
- [Data Usage Policy](#)
- [SLA Agreement](#)

---

## 🚀 What's Next?

- Explore individual [service documentation](./auth-service.md)
- Check out [integration examples](./examples/)
- Join our [developer community](#)
- Subscribe to API updates

---

**Need help getting started?** Contact our developer support team at developers@tukshopp.ng

---

Last Updated: November 11, 2025

