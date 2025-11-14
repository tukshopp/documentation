# PRD: Vendor Discounts

**Status:** Draft  
**Created:** 2025-11-11  
**Last Updated:** 2025-11-11  
**Owner:** [Your Name]  
**Product Designer:** [Designer Name]  
**Target Release:** v1.2.0

---

## 📋 Summary

Enable vendors to create and manage discount campaigns (time-based promotions, item discounts, flash sales) to attract customers and increase sales.

**Type:** Feature  
**Priority:** P1 - High

---

## 🎯 Problem Statement

### What problem are we solving?
Vendors currently have no way to offer promotions or discounts to attract customers. They need a tool to create campaigns, set discount rules, and track performance.

### Who experiences this problem?
- [x] Vendors
- [x] Customers (indirectly - want better deals)

### Current State
Vendors manually adjust prices in their menu, which is time-consuming and doesn't allow for time-based promotions or automatic application.

### Why Now?
Competitive feature that helps vendors grow and improves customer value proposition.

---

## 💡 Proposed Solution

### Overview
A discount management system where vendors can create various types of discounts (percentage, fixed amount, time-based, item-specific) that automatically apply at checkout.

### Key Features
- **Discount Creation:** Vendors can create discount campaigns
- **Time-Based Discounts:** Happy hours, lunch specials, weekend deals
- **Item Discounts:** Mark down specific items or categories
- **Automatic Application:** Discounts apply automatically at checkout
- **Performance Tracking:** Vendors see discount usage and impact

### User Experience Flow
```
Vendor Flow:
1. Vendor opens discount management
2. Clicks "Create Discount"
3. Selects discount type (time-based, item, etc.)
4. Sets discount rules and schedule
5. Saves and activates discount

Customer Flow:
1. Customer browses vendor
2. Sees discount badge on items/vendor
3. Adds items to cart
4. Discount automatically applies at checkout
5. Sees discount amount in order summary
```

---

## 👥 User Stories

### Primary User Stories

**As a vendor**,  
**I want to create time-based discount campaigns**,  
**So that I can attract customers during slow periods.**

**As a vendor**,  
**I want to offer discounts on specific items**,  
**So that I can clear inventory or promote new items.**

**As a customer**,  
**I want to see available discounts**,  
**So that I can save money on my orders.**

---

## ✅ Requirements

### Must Have (MVP)
1. Create percentage-based discounts (e.g., 20% off)
2. Create fixed amount discounts (e.g., ₦500 off)
3. Set discount schedule (start/end date/time)
4. Apply to specific items or entire order
5. Set minimum order value requirement
6. Automatic discount application at checkout
7. Display discount badge on vendor/items

### Should Have
1. Flash sales (limited-time surprise deals)
2. First-time customer discounts
3. Bundle deals ("Buy X, Get Y")
4. Discount usage analytics

### Nice to Have
1. Customer segment targeting
2. A/B testing for discounts
3. Recurring discount schedules

---

## 🎨 Design Requirements

### User Interface
- [x] Vendor dashboard (web/mobile)
- [x] Customer app (discount display)

### Key Screens/Pages
1. **Discount Management Page** - Vendor creates/manages discounts
2. **Discount List** - View all active/past discounts
3. **Create Discount Form** - Step-by-step discount creation
4. **Vendor Page** - Display discount badges
5. **Checkout Page** - Show applied discounts

### User Flows
**Primary Flow: Creating a Discount**
```
1. Vendor navigates to Discounts section
2. Clicks "Create New Discount"
3. Selects discount type
4. Fills in discount details (amount, schedule, items)
5. Reviews and saves
6. Discount becomes active
```

### Design Considerations
- Clear discount badges/icons
- Easy-to-use discount creation form
- Prominent discount display for customers
- Mobile-responsive design

### Mockups/Wireframes
- [Link to Figma/Design file when available]

---

## 🔧 Technical Requirements

### Services Affected
- **Vendor Service:** Discount CRUD operations
- **Marketplace Service:** Discount application at checkout

### API Changes
- [x] New endpoints required

**New Endpoints:**
- `POST /vendor/discounts` - Create discount
- `GET /vendor/discounts` - List discounts
- `PUT /vendor/discounts/{id}` - Update discount
- `DELETE /vendor/discounts/{id}` - Delete discount
- `POST /marketplace/orders/apply-discount` - Apply discount to order

### Database Changes
- [x] New collections/tables

**Changes:**
- Collection: `discounts` - Store discount campaigns
- Collection: `orders` - Add `applied_discount` field

### Third-Party Integrations
- None required

### Performance Requirements
- Discount lookup: < 50ms
- Checkout discount calculation: < 100ms

### Security Considerations
- Only vendor can create/manage their discounts
- Validate discount rules server-side
- Prevent discount abuse

---

## 📊 Success Metrics

### Key Performance Indicators (KPIs)
1. **Discount Usage Rate:** Target: 30% of orders use discounts
2. **Vendor Adoption:** Target: 60% of vendors create at least one discount
3. **Order Value Increase:** Target: 15% increase in average order value
4. **Customer Satisfaction:** Target: 4.5+ rating for discount feature

### Success Criteria
- [ ] 50+ vendors create discounts in first month
- [ ] Discounts used in 25%+ of orders
- [ ] Positive vendor feedback
- [ ] No performance degradation

---

## 🚫 Out of Scope

What we're explicitly NOT building in this version:
- **Loyalty Program Integration:** Will be separate feature
- **Advanced Analytics:** Basic analytics only for MVP
- **Customer Targeting:** All customers see all discounts

---

## 🔗 Dependencies

### Internal Dependencies
- Vendor Service: Discount management endpoints
- Marketplace Service: Discount application logic
- Order Processing: Discount calculation

### External Dependencies
- None

### Blockers
- None currently

---

## ⚠️ Risks & Mitigations

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| Discount abuse | Medium | Low | Server-side validation, rate limiting |
| Performance impact | Medium | Medium | Cache discount rules, optimize queries |
| Complex discount rules | Low | Medium | Start simple, iterate based on feedback |

---

## 📅 Timeline

### Milestones
- **PRD Approval:** 2025-11-15
- **Design Complete:** 2025-11-22
- **Development Start:** 2025-11-25
- **Beta Testing:** 2025-12-10
- **Production Release:** 2025-12-20

### Phases
- **Phase 1 (MVP):** Basic discount creation and application - 2025-12-20
- **Phase 2:** Advanced discount types and analytics - Q1 2026

---

## 💰 Cost Considerations

### Development Effort
- Engineering: 3 person-weeks
- Design: 1 person-week
- QA: 1 person-week

### Infrastructure Costs
- Minimal - uses existing infrastructure

---

## 🧪 Testing Strategy

### Test Scenarios
1. **Happy Path:** Vendor creates discount, customer uses it
2. **Edge Cases:** Expired discounts, minimum order not met, multiple discounts
3. **Error Cases:** Invalid discount rules, discount abuse attempts

### QA Requirements
- [x] Unit tests
- [x] Integration tests
- [x] E2E tests
- [x] Performance tests

---

## 🚀 Rollout Plan

### Release Strategy
- [x] Feature flag controlled
- [x] Gradual rollout (10% → 50% → 100%)
- [x] Beta program with select vendors

### Communication Plan
- **Internal:** Announce in team channel
- **External:** Email vendors about new feature
- **Support:** Train support team on discount management

---

## 📚 Related Documentation

- Technical Design: [To be created]
- API Documentation: [To be updated]
- Vendor Guide: [To be updated]

---

## 💬 Design Notes

### Questions for Product Designer
1. How should discount badges look on vendor pages?
2. Should we show discount countdown timers?
3. What's the best way to display multiple active discounts?

### Design Decisions Needed
- Discount badge style and placement
- Discount creation form layout
- Discount display in checkout

---

## ✅ Approval

### Product Designer Approval
- [ ] Reviewed
- [ ] Feedback provided
- [ ] Approved

**Designer:** [Name]  
**Approval Date:** [Date]

---

**This is an example PRD. Use `prd/prd-template.md` to create your own.**

