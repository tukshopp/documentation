# TukShopp Database Design

**Database:** MongoDB  
**Version:** 1.0  
**Last Updated:** November 11, 2025

---

## 📋 Overview

TukShopp uses MongoDB as the primary NoSQL database for all services. MongoDB was chosen for its flexibility, scalability, and ability to handle complex nested data structures common in our delivery platform.

### Database Strategy
- **Single Database:** All collections in one database
- **Service Access:** Each microservice accesses relevant collections
- **Document Model:** Denormalization where appropriate for performance
- **Referential Integrity:** Handled at application level

---

## 🗂️ Collections Overview

### Core Collections

| Collection | Purpose | Primary Service | Estimated Size |
|------------|---------|----------------|----------------|
| `users` | User authentication | Auth Service | Growing |
| `profiles` | User profiles (all types) | Account Service | Growing |
| `vendors` | Vendor business data | Vendor Service | Medium |
| `outlets` | Vendor outlet locations | Vendor Service | Medium |
| `menu_items` | Product catalogs | Vendor Service | Large |
| `orders` | Order records | Marketplace Service | Very Large |
| `order_items` | Items in orders | Marketplace Service | Very Large |
| `deliveries` | Delivery tracking | Marketplace Service | Very Large |
| `wallets` | User wallet balances | Account Service | Growing |
| `transactions` | Financial transactions | Account/Marketplace | Very Large |
| `addresses` | Saved addresses | Account Service | Growing |
| `payment_methods` | Saved payment info | Account Service | Growing |
| `reviews` | Ratings and reviews | Marketplace Service | Large |
| `zones` | Delivery zones | Marketplace Service | Static |
| `notifications` | Notification history | All Services | Very Large |
| `promo_codes` | Promotional codes | Marketplace Service | Medium |

---

## 📊 Collection Schemas

### users
**Purpose:** User authentication and basic identity  
**Service:** Auth Service

```javascript
{
  _id: ObjectId,
  email: String (unique, indexed),
  phone: String (unique, indexed),
  password_hash: String,
  user_type: String, // "customer", "vendor", "rider", "admin"
  email_verified: Boolean,
  phone_verified: Boolean,
  status: String, // "active", "suspended", "deleted"
  last_login: Date,
  created_at: Date,
  updated_at: Date,
  // Security
  failed_login_attempts: Number,
  locked_until: Date,
  refresh_tokens: [
    {
      token: String,
      expires_at: Date,
      device_info: String
    }
  ]
}
```

**Indexes:**
- `email: 1` (unique)
- `phone: 1` (unique)
- `user_type: 1`
- `status: 1`

---

### profiles
**Purpose:** Detailed user profile information  
**Service:** Account Service

```javascript
{
  _id: ObjectId,
  user_id: ObjectId (ref: users, indexed),
  user_type: String, // "customer", "vendor", "rider"
  
  // Common fields
  first_name: String,
  last_name: String,
  avatar_url: String,
  date_of_birth: Date,
  
  // Customer-specific
  customer_data: {
    favorite_vendors: [ObjectId],
    dietary_preferences: [String],
    default_address_id: ObjectId,
    default_payment_method_id: ObjectId
  },
  
  // Vendor-specific
  vendor_data: {
    business_name: String,
    business_registration: String,
    tax_id: String,
    bank_account: {
      account_number: String,
      bank_name: String,
      account_name: String
    },
    commission_rate: Number,
    verification_status: String,
    verification_documents: [String]
  },
  
  // Rider-specific
  rider_data: {
    vehicle_type: String, // "motorcycle", "bicycle", "car"
    vehicle_number: String,
    license_number: String,
    license_expiry: Date,
    zones: [ObjectId],
    status: String, // "online", "offline", "busy"
    rating: Number,
    total_deliveries: Number,
    verification_documents: [String]
  },
  
  // Preferences
  preferences: {
    language: String,
    currency: String,
    notifications: {
      email: Boolean,
      sms: Boolean,
      push: Boolean,
      marketing: Boolean
    }
  },
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `user_id: 1` (unique)
- `user_type: 1`
- `vendor_data.verification_status: 1`
- `rider_data.zones: 1`
- `rider_data.status: 1`

---

### vendors
**Purpose:** Vendor business information  
**Service:** Vendor Service

```javascript
{
  _id: ObjectId,
  user_id: ObjectId (ref: users),
  business_name: String (indexed),
  slug: String (unique, indexed),
  description: String,
  logo_url: String,
  banner_url: String,
  category: String, // "restaurant", "grocery", "pharmacy", etc.
  cuisine_types: [String],
  
  // Business details
  business_hours: {
    monday: { open: String, close: String, closed: Boolean },
    tuesday: { open: String, close: String, closed: Boolean },
    // ... other days
  },
  
  // Ratings & Reviews
  rating: Number,
  total_ratings: Number,
  total_orders: Number,
  
  // Financial
  commission_rate: Number,
  minimum_order: Number,
  delivery_fee: Number,
  
  // Status
  status: String, // "active", "inactive", "pending", "suspended"
  verification_status: String,
  featured: Boolean,
  
  // Metadata
  tags: [String],
  search_keywords: [String],
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `user_id: 1`
- `slug: 1` (unique)
- `business_name: text`
- `status: 1, featured: -1, rating: -1`
- `category: 1`
- `search_keywords: text`

---

### outlets
**Purpose:** Vendor outlet locations  
**Service:** Vendor Service

```javascript
{
  _id: ObjectId,
  vendor_id: ObjectId (ref: vendors, indexed),
  name: String,
  
  // Location
  address: {
    street: String,
    city: String,
    state: String,
    postal_code: String,
    country: String,
    landmark: String
  },
  location: {
    type: "Point",
    coordinates: [Number, Number] // [longitude, latitude]
  },
  
  // Contact
  phone: String,
  email: String,
  
  // Operations
  zones: [ObjectId], // Zones this outlet serves
  operating_hours: {
    monday: { open: String, close: String, closed: Boolean },
    // ... other days
  },
  
  // Status
  status: String, // "open", "closed", "busy"
  is_default: Boolean,
  
  // Stats
  rating: Number,
  total_orders: Number,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `vendor_id: 1`
- `location: 2dsphere` (geospatial)
- `zones: 1`
- `status: 1`

---

### menu_items
**Purpose:** Product/menu item catalog  
**Service:** Vendor Service

```javascript
{
  _id: ObjectId,
  vendor_id: ObjectId (ref: vendors, indexed),
  outlet_id: ObjectId (ref: outlets, indexed), // null for all outlets
  
  name: String (indexed),
  slug: String,
  description: String,
  
  // Pricing
  price: Number,
  currency: String,
  compare_at_price: Number, // Original price before discount
  
  // Media
  image_url: String,
  images: [String],
  
  // Classification
  category: String,
  subcategory: String,
  tags: [String],
  
  // Options/Variants
  has_options: Boolean,
  options: [
    {
      name: String,
      required: Boolean,
      choices: [
        {
          name: String,
          price: Number
        }
      ]
    }
  ],
  
  // Dietary & Info
  dietary_info: [String], // "vegetarian", "vegan", "gluten-free", etc.
  allergens: [String],
  ingredients: String,
  nutrition_info: Object,
  
  // Availability
  available: Boolean,
  stock_quantity: Number,
  preparation_time: Number, // minutes
  
  // Stats
  rating: Number,
  total_ratings: Number,
  total_orders: Number,
  
  // SEO
  search_keywords: [String],
  
  // Display
  display_order: Number,
  featured: Boolean,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `vendor_id: 1, outlet_id: 1`
- `name: text, description: text`
- `category: 1, available: 1`
- `featured: -1, display_order: 1`
- `search_keywords: text`

---

### orders
**Purpose:** Customer orders  
**Service:** Marketplace Service

```javascript
{
  _id: ObjectId,
  order_number: String (unique, indexed),
  
  // Parties
  customer_id: ObjectId (ref: users, indexed),
  vendors: [
    {
      vendor_id: ObjectId,
      outlet_id: ObjectId,
      status: String,
      preparation_time: Number,
      ready_at: Date
    }
  ],
  rider_id: ObjectId (ref: users, indexed),
  
  // Order Details
  items: [
    {
      menu_item_id: ObjectId,
      vendor_id: ObjectId,
      name: String,
      quantity: Number,
      unit_price: Number,
      selected_options: [
        {
          option_name: String,
          choice_name: String,
          price: Number
        }
      ],
      special_instructions: String,
      subtotal: Number
    }
  ],
  
  // Pricing
  items_subtotal: Number,
  delivery_fee: Number,
  service_fee: Number,
  discount_amount: Number,
  promo_code: String,
  tip_amount: Number,
  tax_amount: Number,
  total_amount: Number,
  
  // Delivery
  delivery_address: {
    street: String,
    city: String,
    state: String,
    postal_code: String,
    latitude: Number,
    longitude: Number,
    instructions: String
  },
  delivery_zone: ObjectId,
  
  // Type
  order_type: String, // "vendor_order", "pickup_delivery"
  delivery_type: String, // "standard", "express", "scheduled"
  scheduled_for: Date,
  
  // Pickup & Delivery specific (for package delivery)
  package_details: {
    pickup_location: Object,
    dropoff_locations: [Object],
    package_size: String,
    package_description: String,
    sender_name: String,
    sender_phone: String,
    recipient_name: String,
    recipient_phone: String,
    proof_of_delivery_required: Boolean
  },
  
  // Payment
  payment_method: String, // "card", "wallet", "cash"
  payment_method_id: ObjectId,
  payment_status: String, // "pending", "paid", "failed", "refunded"
  payment_intent_id: String,
  paid_at: Date,
  
  // Status
  status: String, // "pending", "accepted", "preparing", "ready", "picked_up", "on_the_way", "delivered", "cancelled"
  cancellation_reason: String,
  
  // Timeline
  placed_at: Date,
  accepted_at: Date,
  ready_at: Date,
  picked_up_at: Date,
  delivered_at: Date,
  cancelled_at: Date,
  
  // Ratings
  customer_rating: {
    vendors: [
      {
        vendor_id: ObjectId,
        rating: Number,
        comment: String
      }
    ],
    rider: {
      rating: Number,
      comment: String
    },
    rated_at: Date
  },
  
  // Metadata
  notes: String,
  internal_notes: String,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `order_number: 1` (unique)
- `customer_id: 1, created_at: -1`
- `rider_id: 1, status: 1`
- `vendors.vendor_id: 1, status: 1`
- `status: 1, created_at: -1`
- `delivery_zone: 1, status: 1`
- `placed_at: -1`

---

### deliveries
**Purpose:** Delivery tracking and rider operations  
**Service:** Marketplace Service

```javascript
{
  _id: ObjectId,
  order_id: ObjectId (ref: orders, indexed),
  rider_id: ObjectId (ref: users, indexed),
  
  // Locations
  pickup_locations: [
    {
      vendor_id: ObjectId,
      outlet_id: ObjectId,
      address: Object,
      location: {
        type: "Point",
        coordinates: [Number, Number]
      },
      arrived_at: Date,
      picked_up_at: Date
    }
  ],
  
  dropoff_location: {
    address: Object,
    location: {
      type: "Point",
      coordinates: [Number, Number]
    },
    arrived_at: Date,
    delivered_at: Date
  },
  
  // Route
  estimated_distance: Number, // km
  actual_distance: Number, // km
  estimated_duration: Number, // minutes
  actual_duration: Number, // minutes
  
  // Live Tracking
  current_location: {
    type: "Point",
    coordinates: [Number, Number]
  },
  location_history: [
    {
      location: {
        type: "Point",
        coordinates: [Number, Number]
      },
      timestamp: Date,
      speed: Number
    }
  ],
  
  // Status
  status: String, // "assigned", "en_route_pickup", "arrived_pickup", "picked_up", "en_route_dropoff", "arrived_dropoff", "delivered", "failed"
  
  // Proof of Delivery
  proof_of_delivery: {
    photo_url: String,
    signature_url: String,
    notes: String,
    timestamp: Date
  },
  
  // Earnings
  base_fee: Number,
  distance_fee: Number,
  bonus: Number,
  tip: Number,
  total_earnings: Number,
  
  // Timeline
  assigned_at: Date,
  accepted_at: Date,
  started_at: Date,
  completed_at: Date,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `order_id: 1`
- `rider_id: 1, status: 1`
- `status: 1, assigned_at: -1`
- `current_location: 2dsphere`

---

### wallets
**Purpose:** User wallet balances  
**Service:** Account Service

```javascript
{
  _id: ObjectId,
  user_id: ObjectId (ref: users, unique, indexed),
  
  // Balance
  balance: Number,
  currency: String,
  
  // Limits
  daily_limit: Number,
  monthly_limit: Number,
  
  // Auto top-up
  auto_topup_enabled: Boolean,
  auto_topup_threshold: Number,
  auto_topup_amount: Number,
  auto_topup_payment_method_id: ObjectId,
  
  // Security
  pin_hash: String,
  pin_attempts: Number,
  locked_until: Date,
  
  // Status
  status: String, // "active", "frozen", "closed"
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `user_id: 1` (unique)
- `status: 1`

---

### transactions
**Purpose:** Financial transaction history  
**Service:** Account/Marketplace Service

```javascript
{
  _id: ObjectId,
  
  // Parties
  user_id: ObjectId (ref: users, indexed),
  related_user_id: ObjectId, // For transfers
  
  // Transaction Details
  type: String, // "credit", "debit"
  category: String, // "order_payment", "wallet_topup", "refund", "payout", "transfer"
  
  // Amount
  amount: Number,
  currency: String,
  
  // Reference
  reference_type: String, // "order", "wallet_topup", "payout"
  reference_id: ObjectId,
  order_id: ObjectId (indexed),
  
  // Payment Details
  payment_method: String,
  payment_gateway: String,
  payment_gateway_reference: String,
  
  // Wallet
  wallet_balance_before: Number,
  wallet_balance_after: Number,
  
  // Status
  status: String, // "pending", "completed", "failed", "reversed"
  
  // Metadata
  description: String,
  notes: String,
  metadata: Object,
  
  // Timestamps
  initiated_at: Date,
  completed_at: Date,
  failed_at: Date,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `user_id: 1, created_at: -1`
- `order_id: 1`
- `reference_id: 1, reference_type: 1`
- `status: 1`
- `type: 1, category: 1`

---

### addresses
**Purpose:** Saved delivery addresses  
**Service:** Account Service

```javascript
{
  _id: ObjectId,
  user_id: ObjectId (ref: users, indexed),
  
  label: String, // "Home", "Work", etc.
  
  // Address Details
  street_address: String,
  apartment: String,
  city: String,
  state: String,
  postal_code: String,
  country: String,
  landmark: String,
  
  // Location
  location: {
    type: "Point",
    coordinates: [Number, Number]
  },
  
  // Instructions
  delivery_instructions: String,
  
  // Status
  is_default: Boolean,
  is_deleted: Boolean,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `user_id: 1, is_deleted: 1`
- `location: 2dsphere`

---

### zones
**Purpose:** Delivery zone definitions  
**Service:** Marketplace Service

```javascript
{
  _id: ObjectId,
  name: String,
  code: String (unique),
  
  // Geographic boundary
  boundary: {
    type: "Polygon",
    coordinates: [[[Number, Number]]] // GeoJSON polygon
  },
  
  // Center point
  center: {
    type: "Point",
    coordinates: [Number, Number]
  },
  
  // Pricing
  base_delivery_fee: Number,
  price_per_km: Number,
  
  // Status
  status: String, // "active", "inactive"
  
  // Stats
  total_vendors: Number,
  total_riders: Number,
  
  created_at: Date,
  updated_at: Date
}
```

**Indexes:**
- `code: 1` (unique)
- `boundary: 2dsphere`
- `status: 1`

---

## 🔗 Relationships

### One-to-One
- `users` ↔ `profiles`
- `users` ↔ `wallets`

### One-to-Many
- `users` → `addresses`
- `users` → `orders`
- `vendors` → `outlets`
- `outlets` → `menu_items`
- `orders` → `order_items`

### Many-to-Many
- `riders` ↔ `zones` (via rider_data.zones in profiles)
- `outlets` ↔ `zones`

---

## 📈 Indexing Strategy

### Query Patterns
Most common queries:
1. Find user by email/phone (auth)
2. Get vendor menus (browse)
3. Get user orders (order history)
4. Find nearby vendors (location-based)
5. Track delivery location (real-time)
6. Get rider active deliveries

### Index Types
- **Single Field:** Most common lookups
- **Compound:** Complex queries
- **Text:** Full-text search
- **Geospatial (2dsphere):** Location queries
- **TTL:** Auto-expire documents (notifications)

### Performance Considerations
- Indexes on foreign keys (user_id, vendor_id, etc.)
- Compound indexes for common query patterns
- Covered queries where possible
- Regular index usage analysis

---

## 🔄 Data Lifecycle

### Hot Data
- Active orders (last 30 days)
- Real-time location data
- Current wallet balances

### Warm Data
- Historical orders (30-90 days)
- Transaction history
- Past deliveries

### Cold Data
- Old orders (90+ days)
- Archived data
- Compliance records

### Archival Strategy
- Monthly archival of old orders
- S3 backup of archived data
- Compliance retention periods

---

## 🛡️ Data Security

### Encryption
- At Rest: MongoDB encryption
- In Transit: TLS/SSL connections
- Sensitive Fields: Application-level encryption (passwords, payment data)

### Access Control
- Database users per service
- Least privilege principle
- Connection string security

### PII Handling
- Minimal PII storage
- GDPR compliance
- Right to deletion support

---

## 📊 Database Sizing

### Current (Estimated)
- Total Collections: 15+
- Total Documents: [X million]
- Database Size: [Y GB]
- Index Size: [Z GB]

### Growth Projections
- Orders: ~X per day
- Users: ~Y per month
- Storage: ~Z GB per month

---

## 🔧 Database Operations

### Backup Strategy
- Daily automated backups
- Point-in-time recovery
- S3 backup storage
- Retention: 30 days

### Monitoring
- Query performance
- Index usage
- Connection pool
- Replication lag

### Maintenance
- Index optimization
- Collection stats
- Data validation
- Cleanup scripts

---

## 📚 Related Documentation

- [System Architecture](./system-architecture.md)
- [Data Models](../data/data-models.md)
- [API Contracts](../data/api-contracts.md)
- [Service Specifications](../services/)

---

**Maintained by:** Engineering Team  
**Review Frequency:** Monthly

