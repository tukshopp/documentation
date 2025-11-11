# Marketplace Service API

Marketplace operations including order placement, delivery management, and search for TukShopp platform.

## Overview

The Marketplace Service manages the core marketplace functionality including vendor discovery, order placement and management, delivery tracking, rider operations, and ratings/reviews.

**Base URL:** `https://api.tukshopp.ng`

**Swagger Documentation:** [https://api.tukshopp.ng/docs/marketplace-service](https://api.tukshopp.ng/docs/marketplace-service)

---

## Authentication

Most endpoints require authentication:

```http
Authorization: Bearer {access_token}
```

Some discovery endpoints (search, browse) may be accessible without authentication.

---

## Endpoints

### Vendor Discovery

#### Search Vendors

Search for vendors by name, cuisine, or category.

**Endpoint:** `GET /marketplace/vendors/search`

**Query Parameters:**
- `q` - Search query
- `zone` - Filter by zone
- `category` - Filter by category
- `cuisine` - Filter by cuisine type
- `min_rating` - Minimum rating
- `delivery_time_max` - Maximum delivery time
- `sort_by` - Sort by (rating, distance, delivery_time, popularity)
- `page` - Page number
- `limit` - Items per page

**Response:**
```json
{
  "success": true,
  "data": {
    "vendors": [
      {
        "id": "vnd_1234",
        "name": "Tasty Bites",
        "category": "Restaurant",
        "cuisine": ["Nigerian", "Continental"],
        "rating": 4.8,
        "total_ratings": 450,
        "logo_url": "https://cdn.tukshopp.ng/vendors/...",
        "banner_url": "https://cdn.tukshopp.ng/vendors/...",
        "distance": 2.5,
        "estimated_delivery_time": 30,
        "delivery_fee": 500,
        "is_open": true,
        "is_favorite": false,
        "zones": ["zone_a", "zone_b"]
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 48
    }
  }
}
```

---

#### Browse Vendors

Browse vendors by location/zone.

**Endpoint:** `GET /marketplace/vendors`

**Query Parameters:**
- `latitude` - User latitude
- `longitude` - User longitude
- `zone` - Zone ID
- `category` - Filter by category
- `featured` - Show only featured vendors (boolean)

**Response:**
```json
{
  "success": true,
  "data": {
    "featured": [
      {
        "id": "vnd_1234",
        "name": "Tasty Bites",
        "rating": 4.8,
        "banner_url": "https://cdn.tukshopp.ng/vendors/..."
      }
    ],
    "nearby": [
      {
        "id": "vnd_5678",
        "name": "Quick Eats",
        "distance": 1.2,
        "estimated_delivery_time": 25
      }
    ],
    "popular": [
      {
        "id": "vnd_9999",
        "name": "Top Restaurant",
        "total_orders": 5000
      }
    ]
  }
}
```

---

#### Get Vendor Details

Get detailed information about a vendor.

**Endpoint:** `GET /marketplace/vendors/{vendor_id}`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "vnd_1234",
    "name": "Tasty Bites",
    "description": "Authentic Nigerian and continental cuisine",
    "category": "Restaurant",
    "cuisine": ["Nigerian", "Continental"],
    "logo_url": "https://cdn.tukshopp.ng/vendors/logo.jpg",
    "banner_url": "https://cdn.tukshopp.ng/vendors/banner.jpg",
    "rating": 4.8,
    "total_ratings": 450,
    "outlets": [
      {
        "id": "outlet_1234",
        "name": "Downtown",
        "address": "123 Main St, Lagos",
        "distance": 2.5,
        "is_open": true,
        "operating_hours": {
          "monday": { "open": "09:00", "close": "22:00" }
        }
      }
    ],
    "delivery_info": {
      "estimated_time": 30,
      "fee": 500,
      "minimum_order": 1000
    },
    "payment_methods": ["card", "cash"],
    "is_favorite": false
  }
}
```

---

#### Get Vendor Menu

Get vendor's menu/catalog.

**Endpoint:** `GET /marketplace/vendors/{vendor_id}/menu`

**Query Parameters:**
- `outlet_id` - Specific outlet ID (optional)

**Response:**
```json
{
  "success": true,
  "data": {
    "outlet_id": "outlet_1234",
    "categories": [
      {
        "id": "cat_1234",
        "name": "Main Courses",
        "items": [
          {
            "id": "item_1234",
            "name": "Jollof Rice",
            "description": "Traditional Nigerian jollof rice",
            "price": 1500,
            "currency": "NGN",
            "image_url": "https://cdn.tukshopp.ng/items/jollof.jpg",
            "available": true,
            "preparation_time": 20,
            "rating": 4.9,
            "options": [
              {
                "name": "Protein",
                "required": false,
                "choices": [
                  { "name": "Chicken", "price": 500 },
                  { "name": "Beef", "price": 600 }
                ]
              }
            ],
            "dietary_info": ["gluten-free"],
            "tags": ["popular", "spicy"]
          }
        ]
      }
    ]
  }
}
```

---

### Order Management (Customer)

#### Create Order

Place a new order.

**Endpoint:** `POST /marketplace/orders`

**Request Body:**
```json
{
  "items": [
    {
      "vendor_id": "vnd_1234",
      "outlet_id": "outlet_1234",
      "item_id": "item_1234",
      "quantity": 2,
      "options": [
        {
          "name": "Protein",
          "choice": "Chicken"
        }
      ],
      "special_instructions": "Extra spicy"
    },
    {
      "vendor_id": "vnd_5678",
      "outlet_id": "outlet_5678",
      "item_id": "item_5678",
      "quantity": 1
    }
  ],
  "delivery_address_id": "addr_1234",
  "payment_method_id": "pm_1234",
  "delivery_instructions": "Ring doorbell twice",
  "scheduled_for": null,
  "promo_code": "SAVE10"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "order_id": "ord_1234",
    "order_number": "TS-20251111-0001",
    "status": "pending",
    "items": [
      {
        "vendor_name": "Tasty Bites",
        "item_name": "Jollof Rice",
        "quantity": 2,
        "price": 4000
      }
    ],
    "subtotal": 5500,
    "delivery_fee": 500,
    "discount": 600,
    "total": 5400,
    "payment_status": "pending",
    "estimated_delivery_time": "2025-11-11T13:30:00Z"
  },
  "message": "Order placed successfully"
}
```

---

#### Get Order Details

Get details of a specific order.

**Endpoint:** `GET /marketplace/orders/{order_id}`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "ord_1234",
    "order_number": "TS-20251111-0001",
    "status": "on_the_way",
    "created_at": "2025-11-11T12:30:00Z",
    "vendors": [
      {
        "vendor_id": "vnd_1234",
        "vendor_name": "Tasty Bites",
        "outlet_id": "outlet_1234",
        "items": [
          {
            "name": "Jollof Rice",
            "quantity": 2,
            "price": 4000,
            "options": [{ "name": "Protein", "choice": "Chicken" }]
          }
        ],
        "subtotal": 4000,
        "status": "ready"
      }
    ],
    "delivery": {
      "address": {
        "street": "789 Customer St",
        "city": "Lagos",
        "instructions": "Ring doorbell twice"
      },
      "rider": {
        "id": "rdr_1234",
        "name": "Mike Johnson",
        "phone": "+1234567890",
        "rating": 4.9,
        "vehicle_type": "motorcycle",
        "current_location": {
          "latitude": 6.5244,
          "longitude": 3.3792
        }
      },
      "estimated_arrival": "2025-11-11T13:30:00Z"
    },
    "payment": {
      "subtotal": 5500,
      "delivery_fee": 500,
      "discount": 600,
      "total": 5400,
      "method": "card",
      "status": "paid"
    },
    "timeline": [
      {
        "status": "placed",
        "timestamp": "2025-11-11T12:30:00Z"
      },
      {
        "status": "accepted",
        "timestamp": "2025-11-11T12:32:00Z"
      },
      {
        "status": "preparing",
        "timestamp": "2025-11-11T12:33:00Z"
      },
      {
        "status": "ready",
        "timestamp": "2025-11-11T12:50:00Z"
      },
      {
        "status": "picked_up",
        "timestamp": "2025-11-11T12:52:00Z"
      },
      {
        "status": "on_the_way",
        "timestamp": "2025-11-11T12:53:00Z"
      }
    ]
  }
}
```

---

#### Track Order

Get real-time tracking information.

**Endpoint:** `GET /marketplace/orders/{order_id}/track`

**Response:**
```json
{
  "success": true,
  "data": {
    "order_id": "ord_1234",
    "status": "on_the_way",
    "rider": {
      "name": "Mike Johnson",
      "phone": "+1234567890",
      "rating": 4.9,
      "current_location": {
        "latitude": 6.5244,
        "longitude": 3.3792
      }
    },
    "estimated_arrival": "2025-11-11T13:30:00Z",
    "delivery_address": {
      "latitude": 6.5298,
      "longitude": 3.3772
    },
    "distance_remaining": 1.2
  }
}
```

---

#### Get Order History

List customer's order history.

**Endpoint:** `GET /marketplace/orders`

**Query Parameters:**
- `status` - Filter by status
- `date_from` - Start date
- `date_to` - End date
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
        "vendors": ["Tasty Bites", "Quick Eats"],
        "status": "delivered",
        "total": 5400,
        "created_at": "2025-11-11T12:30:00Z",
        "delivered_at": "2025-11-11T13:28:00Z"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 12,
      "total_items": 115
    }
  }
}
```

---

#### Cancel Order

Cancel a pending order.

**Endpoint:** `POST /marketplace/orders/{order_id}/cancel`

**Request Body:**
```json
{
  "reason": "Changed my mind"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "cancelled",
    "refund_amount": 5400,
    "refund_status": "processing"
  },
  "message": "Order cancelled successfully. Refund will be processed in 5-7 business days."
}
```

---

### Rider Operations

#### Get Available Deliveries (Rider)

Get available delivery requests.

**Endpoint:** `GET /marketplace/deliveries/available`

**Query Parameters:**
- `zone` - Rider's current zone

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "delivery_id": "dlv_1234",
      "order_id": "ord_1234",
      "pickup_location": {
        "name": "Tasty Bites - Downtown",
        "address": "123 Main St",
        "latitude": 6.5244,
        "longitude": 3.3792
      },
      "dropoff_location": {
        "address": "789 Customer St",
        "latitude": 6.5298,
        "longitude": 3.3772
      },
      "distance": 2.5,
      "estimated_earnings": 800,
      "pickup_ready": true,
      "expires_at": "2025-11-11T13:05:00Z"
    }
  ]
}
```

---

#### Accept Delivery

Accept a delivery request.

**Endpoint:** `POST /marketplace/deliveries/{delivery_id}/accept`

**Response:**
```json
{
  "success": true,
  "data": {
    "delivery_id": "dlv_1234",
    "order_id": "ord_1234",
    "status": "assigned",
    "pickup_location": {
      "name": "Tasty Bites - Downtown",
      "address": "123 Main St",
      "phone": "+1234567890",
      "latitude": 6.5244,
      "longitude": 3.3792
    },
    "order_details": {
      "items_count": 3,
      "special_instructions": "Handle with care"
    }
  },
  "message": "Delivery accepted. Navigate to pickup location."
}
```

---

#### Update Delivery Status

Update delivery status as rider progresses.

**Endpoint:** `PATCH /marketplace/deliveries/{delivery_id}/status`

**Request Body:**
```json
{
  "status": "picked_up",
  "location": {
    "latitude": 6.5244,
    "longitude": 3.3792
  },
  "notes": "Order collected successfully"
}
```

**Status options:** `arrived_pickup`, `picked_up`, `on_the_way`, `arrived_dropoff`, `delivered`

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "picked_up",
    "next_step": "Navigate to customer location"
  },
  "message": "Status updated successfully"
}
```

---

#### Confirm Delivery

Mark delivery as completed.

**Endpoint:** `POST /marketplace/deliveries/{delivery_id}/complete`

**Request Body:**
```json
{
  "photo_url": "https://cdn.tukshopp.ng/delivery-proof/...",
  "location": {
    "latitude": 6.5298,
    "longitude": 3.3772
  },
  "notes": "Delivered to customer"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "delivery_id": "dlv_1234",
    "status": "completed",
    "earnings": 800,
    "rating_pending": true
  },
  "message": "Delivery completed successfully"
}
```

---

#### Get Rider Earnings

View rider earnings.

**Endpoint:** `GET /marketplace/rider/earnings`

**Query Parameters:**
- `period` - today, week, month
- `date_from` - Start date
- `date_to` - End date

**Response:**
```json
{
  "success": true,
  "data": {
    "period": "today",
    "total_earnings": 15600,
    "deliveries_completed": 22,
    "breakdown": {
      "base_fees": 11000,
      "tips": 3200,
      "bonuses": 1400
    },
    "pending_payout": 15600,
    "next_payout_date": "2025-11-18"
  }
}
```

---

### Ratings & Reviews

#### Rate Order

Rate vendors and rider after delivery.

**Endpoint:** `POST /marketplace/orders/{order_id}/rate`

**Request Body:**
```json
{
  "vendor_ratings": [
    {
      "vendor_id": "vnd_1234",
      "rating": 5,
      "comment": "Excellent food and packaging!"
    }
  ],
  "rider_rating": {
    "rating": 5,
    "comment": "Professional and fast delivery"
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Thank you for your feedback!"
}
```

---

#### Get Vendor Reviews

View reviews for a vendor.

**Endpoint:** `GET /marketplace/vendors/{vendor_id}/reviews`

**Query Parameters:**
- `rating` - Filter by rating
- `page` - Page number
- `limit` - Items per page

**Response:**
```json
{
  "success": true,
  "data": {
    "average_rating": 4.8,
    "total_reviews": 450,
    "rating_distribution": {
      "5": 380,
      "4": 50,
      "3": 15,
      "2": 3,
      "1": 2
    },
    "reviews": [
      {
        "id": "rev_1234",
        "customer_name": "John D.",
        "rating": 5,
        "comment": "Amazing food!",
        "created_at": "2025-11-10T15:30:00Z",
        "verified_purchase": true,
        "vendor_response": "Thank you!"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 45
    }
  }
}
```

---

### Promo Codes

#### Validate Promo Code

Check if promo code is valid.

**Endpoint:** `POST /marketplace/promo-codes/validate`

**Request Body:**
```json
{
  "code": "SAVE10",
  "order_total": 5000
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "code": "SAVE10",
    "valid": true,
    "discount_type": "percentage",
    "discount_value": 10,
    "discount_amount": 500,
    "minimum_order": 1000,
    "maximum_discount": 1000,
    "expires_at": "2025-12-31T23:59:59Z"
  }
}
```

---

## WebSocket Events

### Real-time Updates

Connect to WebSocket for real-time order updates:

**WebSocket URL:** `wss://api.tukshopp.ng/marketplace/ws`

**Authentication:**
```json
{
  "type": "auth",
  "token": "Bearer {access_token}"
}
```

**Events:**

#### Order Status Update
```json
{
  "event": "order.status_updated",
  "data": {
    "order_id": "ord_1234",
    "status": "on_the_way",
    "timestamp": "2025-11-11T12:53:00Z"
  }
}
```

#### Rider Location Update
```json
{
  "event": "rider.location_updated",
  "data": {
    "order_id": "ord_1234",
    "location": {
      "latitude": 6.5244,
      "longitude": 3.3792
    },
    "timestamp": "2025-11-11T12:54:00Z"
  }
}
```

---

## Error Codes

| Code | Description |
|------|-------------|
| `MKT_001` | Vendor not found |
| `MKT_002` | Item not available |
| `MKT_003` | Outside delivery zone |
| `MKT_004` | Minimum order not met |
| `MKT_005` | Invalid promo code |
| `MKT_006` | Order cannot be cancelled |
| `MKT_007` | Payment failed |
| `MKT_008` | No available riders |

---

## Examples

### Complete Order Flow

```javascript
const token = 'customer_access_token';

// 1. Search vendors
const vendors = await fetch(
  'https://api.tukshopp.ng/marketplace/vendors/search?q=jollof&zone=zone_a',
  { headers: { 'Authorization': `Bearer ${token}` } }
);

// 2. Get vendor menu
const menu = await fetch(
  'https://api.tukshopp.ng/marketplace/vendors/vnd_1234/menu',
  { headers: { 'Authorization': `Bearer ${token}` } }
);

// 3. Place order
const order = await fetch(
  'https://api.tukshopp.ng/marketplace/orders',
  {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      items: [
        {
          vendor_id: 'vnd_1234',
          outlet_id: 'outlet_1234',
          item_id: 'item_1234',
          quantity: 2
        }
      ],
      delivery_address_id: 'addr_1234',
      payment_method_id: 'pm_1234'
    })
  }
);

// 4. Track order
const tracking = await fetch(
  'https://api.tukshopp.ng/marketplace/orders/ord_1234/track',
  { headers: { 'Authorization': `Bearer ${token}` } }
);

// 5. Rate after delivery
await fetch(
  'https://api.tukshopp.ng/marketplace/orders/ord_1234/rate',
  {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      vendor_ratings: [{ vendor_id: 'vnd_1234', rating: 5 }],
      rider_rating: { rating: 5 }
    })
  }
);
```

---

## Related Documentation

- [Vendor Service](./vendor-service.md) - Vendor operations
- [Account Service](./account-service.md) - User account management
- [Customer Guide](../user-guides/CUSTOMERS.md) - Customer user guide
- [Rider Guide](../user-guides/RIDERS.md) - Rider user guide

---

**Official Swagger Documentation:** [https://api.tukshopp.ng/docs/marketplace-service](https://api.tukshopp.ng/docs/marketplace-service)

Last Updated: November 11, 2025

