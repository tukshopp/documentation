# Vendor Service API

Vendor management and operations service for TukShopp platform.

## Overview

The Vendor Service handles all vendor-related operations including outlet management, menu/catalog management, order processing, inventory tracking, and vendor analytics.

**Base URL:** `https://api.tukshopp.ng`

**Swagger Documentation:** [https://api.tukshopp.ng/docs/vendor-service](https://api.tukshopp.ng/docs/vendor-service)

---

## Authentication

All endpoints require vendor authentication:

```http
Authorization: Bearer {vendor_access_token}
```

---

## Endpoints

### Outlet Management

#### List Outlets

Get all outlets for vendor.

**Endpoint:** `GET /vendor/outlets`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "outlet_1234",
      "name": "Tasty Bites - Downtown",
      "address": "123 Main St, Lagos",
      "phone": "+1234567890",
      "zones": ["zone_a", "zone_b"],
      "status": "open",
      "operating_hours": {
        "monday": { "open": "09:00", "close": "22:00" },
        "tuesday": { "open": "09:00", "close": "22:00" }
      },
      "rating": 4.8,
      "total_orders": 1250,
      "created_at": "2025-01-15T10:00:00Z"
    }
  ]
}
```

---

#### Create Outlet

Add a new outlet.

**Endpoint:** `POST /vendor/outlets`

**Request Body:**
```json
{
  "name": "Tasty Bites - Ikeja",
  "street_address": "456 Market Road",
  "city": "Lagos",
  "state": "Lagos State",
  "postal_code": "100001",
  "country": "Nigeria",
  "phone": "+1234567890",
  "email": "ikeja@tastybites.com",
  "zones": ["zone_c"],
  "latitude": 6.5966,
  "longitude": 3.3408,
  "operating_hours": {
    "monday": { "open": "09:00", "close": "22:00" },
    "tuesday": { "open": "09:00", "close": "22:00" },
    "wednesday": { "open": "09:00", "close": "22:00" },
    "thursday": { "open": "09:00", "close": "22:00" },
    "friday": { "open": "09:00", "close": "23:00" },
    "saturday": { "open": "10:00", "close": "23:00" },
    "sunday": { "open": "10:00", "close": "21:00" }
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "outlet_5678",
    "name": "Tasty Bites - Ikeja",
    "status": "pending_approval",
    "created_at": "2025-11-11T10:00:00Z"
  },
  "message": "Outlet created successfully. Pending approval."
}
```

---

#### Update Outlet

Update outlet information.

**Endpoint:** `PUT /vendor/outlets/{outlet_id}`

**Request Body:**
```json
{
  "phone": "+1234567891",
  "zones": ["zone_c", "zone_d"],
  "operating_hours": {
    "friday": { "open": "09:00", "close": "00:00" }
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Outlet updated successfully"
}
```

---

#### Toggle Outlet Status

Open/close outlet or enable busy mode.

**Endpoint:** `PATCH /vendor/outlets/{outlet_id}/status`

**Request Body:**
```json
{
  "status": "busy"
}
```

**Status options:** `open`, `closed`, `busy`

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "busy"
  },
  "message": "Outlet status updated"
}
```

---

### Menu/Catalog Management

#### Get Menu

Retrieve outlet menu.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/menu`

**Query Parameters:**
- `category` - Filter by category
- `available` - Filter by availability (true/false)

**Response:**
```json
{
  "success": true,
  "data": {
    "categories": [
      {
        "id": "cat_1234",
        "name": "Main Courses",
        "display_order": 1,
        "items": [
          {
            "id": "item_1234",
            "name": "Grilled Chicken",
            "description": "Tender grilled chicken with herbs",
            "price": 2500,
            "currency": "NGN",
            "image_url": "https://cdn.tukshopp.ng/items/...",
            "category_id": "cat_1234",
            "available": true,
            "preparation_time": 15,
            "options": [
              {
                "name": "Size",
                "required": true,
                "choices": [
                  { "name": "Regular", "price": 0 },
                  { "name": "Large", "price": 500 }
                ]
              }
            ],
            "dietary_info": ["gluten-free"],
            "created_at": "2025-01-15T10:00:00Z"
          }
        ]
      }
    ]
  }
}
```

---

#### Add Menu Item

Add a new item to menu.

**Endpoint:** `POST /vendor/outlets/{outlet_id}/menu/items`

**Request Body:**
```json
{
  "name": "Jollof Rice",
  "description": "Traditional Nigerian jollof rice",
  "price": 1500,
  "currency": "NGN",
  "category_id": "cat_1234",
  "image_url": "https://cdn.tukshopp.ng/items/jollof.jpg",
  "preparation_time": 20,
  "available": true,
  "options": [
    {
      "name": "Protein",
      "required": false,
      "choices": [
        { "name": "Chicken", "price": 500 },
        { "name": "Beef", "price": 600 },
        { "name": "Fish", "price": 700 }
      ]
    }
  ],
  "dietary_info": ["vegetarian-option"]
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "item_5678",
    "name": "Jollof Rice",
    "created_at": "2025-11-11T10:30:00Z"
  },
  "message": "Menu item added successfully"
}
```

---

#### Update Menu Item

Update an existing menu item.

**Endpoint:** `PUT /vendor/outlets/{outlet_id}/menu/items/{item_id}`

**Request Body:**
```json
{
  "price": 1800,
  "available": true,
  "description": "Updated description"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Menu item updated successfully"
}
```

---

#### Delete Menu Item

Remove an item from menu.

**Endpoint:** `DELETE /vendor/outlets/{outlet_id}/menu/items/{item_id}`

**Response:**
```json
{
  "success": true,
  "message": "Menu item deleted successfully"
}
```

---

#### Bulk Update Availability

Update availability for multiple items.

**Endpoint:** `PATCH /vendor/outlets/{outlet_id}/menu/items/availability`

**Request Body:**
```json
{
  "items": [
    { "id": "item_1234", "available": false },
    { "id": "item_5678", "available": true }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "message": "Availability updated for 2 items"
}
```

---

#### Manage Categories

Create/update menu categories.

**Endpoint:** `POST /vendor/outlets/{outlet_id}/menu/categories`

**Request Body:**
```json
{
  "name": "Desserts",
  "description": "Sweet treats",
  "display_order": 5
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "cat_9999",
    "name": "Desserts"
  },
  "message": "Category created successfully"
}
```

---

### Order Management

#### Get Orders

List orders for outlet.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/orders`

**Query Parameters:**
- `status` - Filter by status (pending, accepted, preparing, ready, picked_up, completed, cancelled)
- `date_from` - Start date (ISO 8601)
- `date_to` - End date (ISO 8601)
- `page` - Page number
- `limit` - Items per page

**Response:**
```json
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": "ord_1234",
        "order_number": "TS-20251111-0001",
        "customer": {
          "name": "John Doe",
          "phone": "+1234567890"
        },
        "items": [
          {
            "name": "Grilled Chicken",
            "quantity": 2,
            "price": 2500,
            "options": [{ "name": "Size", "choice": "Large" }]
          }
        ],
        "subtotal": 5000,
        "status": "pending",
        "payment_status": "paid",
        "created_at": "2025-11-11T12:30:00Z",
        "scheduled_for": null
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 10,
      "total_items": 95
    }
  }
}
```

---

#### Get Order Details

Get detailed information about an order.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/orders/{order_id}`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "ord_1234",
    "order_number": "TS-20251111-0001",
    "status": "pending",
    "customer": {
      "id": "usr_1234",
      "name": "John Doe",
      "phone": "+1234567890"
    },
    "items": [
      {
        "id": "item_1234",
        "name": "Grilled Chicken",
        "quantity": 2,
        "unit_price": 2500,
        "total_price": 5000,
        "options": [
          { "name": "Size", "choice": "Large", "price": 500 }
        ],
        "special_instructions": "Extra spicy"
      }
    ],
    "subtotal": 5000,
    "delivery_fee": 500,
    "total": 5500,
    "payment_method": "card",
    "payment_status": "paid",
    "delivery_address": {
      "street": "789 Customer St",
      "city": "Lagos",
      "instructions": "Ring doorbell"
    },
    "created_at": "2025-11-11T12:30:00Z",
    "estimated_ready_time": "2025-11-11T12:50:00Z"
  }
}
```

---

#### Accept Order

Accept a pending order.

**Endpoint:** `POST /vendor/outlets/{outlet_id}/orders/{order_id}/accept`

**Request Body:**
```json
{
  "preparation_time": 20
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "accepted",
    "estimated_ready_time": "2025-11-11T12:50:00Z"
  },
  "message": "Order accepted successfully"
}
```

---

#### Reject Order

Reject an order.

**Endpoint:** `POST /vendor/outlets/{outlet_id}/orders/{order_id}/reject`

**Request Body:**
```json
{
  "reason": "Item not available"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Order rejected"
}
```

---

#### Mark Order Ready

Mark order as ready for pickup.

**Endpoint:** `POST /vendor/outlets/{outlet_id}/orders/{order_id}/ready`

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "ready"
  },
  "message": "Order marked as ready. Rider will be notified."
}
```

---

#### Update Preparation Time

Update estimated preparation time.

**Endpoint:** `PATCH /vendor/outlets/{outlet_id}/orders/{order_id}/preparation-time`

**Request Body:**
```json
{
  "additional_minutes": 10,
  "reason": "High order volume"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "estimated_ready_time": "2025-11-11T13:00:00Z"
  },
  "message": "Preparation time updated. Customer notified."
}
```

---

### Analytics & Reports

#### Get Dashboard Stats

Get outlet performance overview.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/analytics/dashboard`

**Query Parameters:**
- `period` - today, week, month, year

**Response:**
```json
{
  "success": true,
  "data": {
    "period": "today",
    "revenue": 125000,
    "orders": {
      "total": 45,
      "completed": 40,
      "cancelled": 2,
      "pending": 3
    },
    "average_order_value": 2778,
    "rating": 4.8,
    "top_items": [
      {
        "name": "Jollof Rice",
        "orders": 25,
        "revenue": 37500
      }
    ],
    "peak_hours": ["12:00-14:00", "19:00-21:00"]
  }
}
```

---

#### Get Sales Report

Detailed sales analytics.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/analytics/sales`

**Query Parameters:**
- `date_from` - Start date
- `date_to` - End date
- `group_by` - day, week, month

**Response:**
```json
{
  "success": true,
  "data": {
    "total_revenue": 450000,
    "total_orders": 180,
    "average_order_value": 2500,
    "breakdown": [
      {
        "date": "2025-11-01",
        "revenue": 45000,
        "orders": 18
      }
    ],
    "by_category": [
      {
        "category": "Main Courses",
        "revenue": 300000,
        "orders": 120
      }
    ]
  }
}
```

---

#### Get Performance Metrics

Get vendor performance metrics.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/analytics/performance`

**Response:**
```json
{
  "success": true,
  "data": {
    "acceptance_rate": 98.5,
    "average_preparation_time": 18,
    "on_time_rate": 95.2,
    "rating": 4.8,
    "total_ratings": 450,
    "rating_breakdown": {
      "5_star": 380,
      "4_star": 50,
      "3_star": 15,
      "2_star": 3,
      "1_star": 2
    }
  }
}
```

---

### Inventory Management

#### Track Stock Levels

**Endpoint:** `GET /vendor/outlets/{outlet_id}/inventory`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "item_id": "item_1234",
      "name": "Grilled Chicken",
      "stock_level": "in_stock",
      "quantity": 50,
      "low_stock_threshold": 10,
      "last_updated": "2025-11-11T10:00:00Z"
    }
  ]
}
```

---

#### Update Stock

**Endpoint:** `PATCH /vendor/outlets/{outlet_id}/inventory/{item_id}`

**Request Body:**
```json
{
  "stock_level": "low_stock",
  "quantity": 8
}
```

**Response:**
```json
{
  "success": true,
  "message": "Stock updated successfully"
}
```

---

### Reviews & Ratings

#### Get Reviews

Get customer reviews.

**Endpoint:** `GET /vendor/outlets/{outlet_id}/reviews`

**Query Parameters:**
- `rating` - Filter by rating (1-5)
- `page` - Page number

**Response:**
```json
{
  "success": true,
  "data": {
    "reviews": [
      {
        "id": "rev_1234",
        "customer_name": "John D.",
        "rating": 5,
        "comment": "Excellent food and service!",
        "order_id": "ord_1234",
        "created_at": "2025-11-10T15:30:00Z",
        "response": null
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 20
    }
  }
}
```

---

#### Respond to Review

Respond to a customer review.

**Endpoint:** `POST /vendor/outlets/{outlet_id}/reviews/{review_id}/respond`

**Request Body:**
```json
{
  "response": "Thank you for your feedback! We're glad you enjoyed your meal."
}
```

**Response:**
```json
{
  "success": true,
  "message": "Response posted successfully"
}
```

---

## Error Codes

| Code | Description |
|------|-------------|
| `VND_001` | Outlet not found |
| `VND_002` | Unauthorized outlet access |
| `VND_003` | Invalid menu item |
| `VND_004` | Order not found |
| `VND_005` | Cannot modify completed order |
| `VND_006` | Invalid category |
| `VND_007` | Duplicate item name |

---

## Examples

### Complete Order Processing Flow

```javascript
const token = 'vendor_access_token';
const outletId = 'outlet_1234';

// 1. Get pending orders
const ordersResponse = await fetch(
  `https://api.tukshopp.ng/vendor/outlets/${outletId}/orders?status=pending`,
  {
    headers: { 'Authorization': `Bearer ${token}` }
  }
);
const { data: { orders } } = await ordersResponse.json();

// 2. Accept order
const orderId = orders[0].id;
await fetch(
  `https://api.tukshopp.ng/vendor/outlets/${outletId}/orders/${orderId}/accept`,
  {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ preparation_time: 20 })
  }
);

// 3. Mark order ready
await fetch(
  `https://api.tukshopp.ng/vendor/outlets/${outletId}/orders/${orderId}/ready`,
  {
    method: 'POST',
    headers: { 'Authorization': `Bearer ${token}` }
  }
);
```

---

## Related Documentation

- [Account Service](./account-service.md) - Vendor account management
- [Marketplace Service](./marketplace-service.md) - Order and delivery flow
- [Vendor Guide](../user-guides/VENDORS.md) - Complete vendor guide

---

**Official Swagger Documentation:** [https://api.tukshopp.ng/docs/vendor-service](https://api.tukshopp.ng/docs/vendor-service)

Last Updated: November 11, 2025

