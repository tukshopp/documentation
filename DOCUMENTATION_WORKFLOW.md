# Documentation Workflow Guide

**Purpose:** Clear guidelines on where and when to document platform updates, new features, and architectural changes.

---

## 🎯 Quick Decision Tree

```
New Change/Update?
    │
    ├─ Is it a NEW FEATURE or APP? → prd/draft/ (PRD) ⭐ START HERE
    │
    ├─ Is it a NEW SERVICE (backend)? → design-docs/technical-proposals/
    │
    ├─ Is it a MAJOR ARCHITECTURE CHANGE? → design-docs/architecture-decisions/ (ADR)
    │
    └─ Is it a BUG FIX/SMALL CHANGE? → Update existing docs + changelogs/
```

---

## 📋 Where to Document First

### 1. 🆕 New App or Feature

**⭐ SIMPLIFIED: For all new apps and features, start with a PRD!**

**Start Here:** `prd/draft/[feature-or-app-name].md`

**Examples:**
- New feature: Vendor discounts, improved search, dedicated rider service
- New app: Admin dashboard, vendor mobile app, customer web app
- Feature enhancement: Wallet improvements, pickup & delivery enhancements

**Why PRD?**
- Single source of truth for product requirements
- Clear communication with product designer
- Combines product and technical requirements
- Streamlined workflow

**Workflow:**
```
1. PRD Draft → prd/draft/[name].md
   ↓
2. Share with Product Designer → prd/review/[name].md
   ↓
3. Get Approval → prd/approved/[name].md
   ↓
4. Technical Design → design-docs/system-design/[name]-design.md
   ↓
5. Implementation → prd/in-progress/[name].md
   ↓
6. Shipped → prd/completed/[name].md
```

---

### 2. 🔧 New Service (Backend Only)

**For backend services that don't need product design:**

**Start Here:** `design-docs/technical-proposals/[service-name].md`

**Examples:** Notification service, analytics service, payment service

**Note:** If the service has user-facing components, use PRD instead.

---

### 1A. 🎨 New Feature or App

**Start Here:** `prd/draft/[feature-or-app-name].md` ⭐

**Workflow:**
```
1. PRD Draft → prd/draft/[feature-name].md ⭐ START HERE
   ↓
2. Share with Product Designer → prd/review/[feature-name].md
   ↓
3. Get Approval → prd/approved/[feature-name].md
   ↓
4. Technical Design → design-docs/system-design/[feature-name]-design.md
   ↓
5. Implementation → prd/in-progress/[feature-name].md
   ↓
6. User Documentation → user-docs/[relevant-guide].md
   ↓
7. API Documentation → api-docs/[service].md (if API changes)
   ↓
8. Shipped → prd/completed/[feature-name].md
   ↓
9. Update Changelog → changelogs/CHANGELOG.md
```

**Example: Adding Vendor Discounts Feature**

**Step 1: Create PRD**
```markdown
File: prd/draft/vendor-discounts.md

# PRD: Vendor Discounts

## Problem Statement
Vendors need a way to offer promotions and discounts...

## Proposed Solution
Create a discount management system where vendors can...

## User Stories
- As a vendor, I want to create discount campaigns...
- As a customer, I want to see available discounts...

## Technical Requirements
- New API endpoints for discount management
- Database schema changes
- Integration with order processing
```

**Step 2: Share with Product Designer**
- Move PRD to `prd/review/`
- Request design feedback
- Iterate based on feedback

**Step 3: Get Approval**
- Move PRD to `prd/approved/`
- Product designer creates mockups
- Proceed with technical design

**Step 4: Create Technical Design**
```markdown
File: design-docs/system-design/vendor-discounts-design.md

[Use templates/design-doc-template.md]
Reference: prd/approved/vendor-discounts.md
```

---

### 1B. 🔧 New Service (Backend Only)

**Start Here:** `design-docs/technical-proposals/[service-name].md`

**Note:** If the service has any user-facing components, use PRD instead (see 1A above).

**Workflow:**
```
1. Feature Request → feature-management/feature-requests/feature-name.md
   ↓
2. Product Spec → product-specs/features/planned/feature-name.md
   ↓
3. RFC (if major) → feature-management/rfcs/rfc-XXX-feature-name.md
   ↓
4. Technical Design → design-docs/system-design/feature-name-design.md
   ↓
5. Implementation → Code
   ↓
6. User Documentation → user-docs/
   ↓
7. API Documentation → api-docs/ (if API changes)
   ↓
8. Update Changelog → changelogs/CHANGELOG.md
```

**Example: Adding Vendor Discounts Feature**

**Step 1: Feature Request**
```markdown
File: feature-management/feature-requests/vendor-discounts.md

[Use feature-requests/template.md]
```

**Step 2: Product Specification**
```markdown
File: product-specs/features/planned/vendor-discounts.md

[Use features/feature-template.md]
```

**Step 3: Technical Design**
```markdown
File: design-docs/system-design/vendor-discounts-design.md

[Use templates/design-doc-template.md]
```

---

### 3. 🏗️ Major Architecture Change

**Start Here:** `design-docs/architecture-decisions/`

**Workflow:**
```
1. Architecture Decision Record (ADR) → design-docs/architecture-decisions/adr-XXX-change-name.md
   ↓
2. Technical Design → design-docs/system-design/change-name-design.md
   ↓
3. Update Architecture Docs → technical-specs/architecture/
   ↓
4. Implementation → Code
   ↓
5. Update Changelog → changelogs/CHANGELOG-INTERNAL.md
```

**Example: Migrating to Kubernetes**

**Step 1: ADR**
```markdown
File: design-docs/architecture-decisions/adr-010-kubernetes-migration.md

# ADR-010: Migrate to Kubernetes

## Status: Proposed
## Context
Current EC2-based deployment has limitations...

## Decision
Migrate to Kubernetes for better orchestration...

## Consequences
- Better scalability
- More complex operations
- Learning curve
```

---

### 4. 🔧 Infrastructure Change

**Start Here:** `design-docs/technical-proposals/` or `design-docs/architecture-decisions/`

**Workflow:**
```
1. Technical Proposal/ADR → design-docs/technical-proposals/infrastructure-change.md
   ↓
2. Update Infrastructure Doc → technical-specs/architecture/infrastructure.md
   ↓
3. Update Deployment Guide → internal-docs/deployment/deployment-guide.md
   ↓
4. Implementation → Infrastructure as Code
   ↓
5. Update Changelog → changelogs/CHANGELOG-INTERNAL.md
```

**Example: Adding Redis Cache**

**Step 1: Technical Proposal**
```markdown
File: design-docs/technical-proposals/redis-cache.md

# Technical Proposal: Add Redis Caching Layer

## Problem
Database queries are slow...

## Solution
Add Redis for caching...

## Implementation Plan
[Details]
```

**Step 2: Update Infrastructure**
```markdown
File: technical-specs/architecture/infrastructure.md

[Add Redis section]
```

---

### 5. 🐛 Bug Fix / Small Change

**Start Here:** Update existing documentation

**Workflow:**
```
1. Fix Code → Update relevant service spec if needed
   ↓
2. Update Changelog → changelogs/CHANGELOG.md (if user-facing)
   ↓
3. Update CHANGELOG-INTERNAL.md (if internal)
```

**No new documents needed** - just update existing ones.

---

## 📝 Documentation Templates

### For New App or Feature

#### ⭐ PRD (Product Requirements Document) - START HERE
1. **PRD** (Start Here)
   - Use: `prd/prd-template.md`
   - Location: `prd/draft/[feature-or-app-name].md`
   - Purpose: Document requirements and communicate with product designer

2. **Technical Design Document** (After PRD approval)
   - Use: `design-docs/templates/design-doc-template.md`
   - Location: `design-docs/system-design/[feature-name]-design.md`
   - Reference: Link to approved PRD

#### For New Service (Backend Only)
1. **Technical Proposal** (Start Here)
   - Use: `design-docs/templates/design-doc-template.md` (simplified)
   - Location: `design-docs/technical-proposals/[service-name].md`

2. **Architecture Decision Record**
   - Use: `design-docs/templates/adr-template.md`
   - Location: `design-docs/architecture-decisions/adr-XXX-[service-name].md`

3. **Technical Design Document**
   - Use: `design-docs/templates/design-doc-template.md`
   - Location: `design-docs/system-design/[service-name]-design.md`

4. **Service Specification**
   - Use: Create based on existing service specs
   - Location: `technical-specs/services/[service-name]-spec.md`

---

## 🎯 Decision Matrix

| Change Type | Start Document | Template | Next Steps |
|------------|---------------|----------|------------|
| **New Feature** (user-facing) | `prd/draft/` ⭐ | PRD Template | Review → Approval → Design → Implementation |
| **New App** (frontend/mobile) | `prd/draft/` ⭐ | PRD Template | Review → Approval → Design → Implementation |
| **New Service** (backend only) | `design-docs/technical-proposals/` | Design Doc Template | ADR → Design → Spec → Implementation |
| **Architecture Change** | `design-docs/architecture-decisions/` | ADR Template | Design → Update Architecture Docs |
| **Infrastructure Change** | `design-docs/technical-proposals/` | Design Doc Template | Update Infrastructure Doc |
| **API Change** | `design-docs/technical-proposals/` | Design Doc Template | Update API Docs |
| **Database Change** | `design-docs/technical-proposals/` | Design Doc Template | Update Database Design |
| **Bug Fix** | Update existing docs | N/A | Update Changelog |

---

## 🔄 Complete Workflow Examples

### Example 1: Adding a New Feature (Using PRD)

### Scenario: Adding "Vendor Discounts" Feature

#### Phase 1: PRD Creation & Review

**1. Create PRD Draft**
```bash
File: prd/draft/vendor-discounts.md
```
- Copy `prd/prd-template.md`
- Fill in problem statement
- Define user stories
- Document requirements
- Add technical constraints

**2. Share with Product Designer**
```bash
Move to: prd/review/vendor-discounts.md
```
- Request design feedback
- Discuss user flows
- Clarify UI/UX requirements
- Iterate based on feedback

**3. Get Approval**
```bash
Move to: prd/approved/vendor-discounts.md
```
- Product designer approves
- Design mockups created
- Ready for technical design

#### Phase 2: Technical Design

**4. Create Technical Design Document**
```bash
File: design-docs/system-design/vendor-discounts-design.md
```
- Reference: `prd/approved/vendor-discounts.md`
- System design
- API specifications
- Database schema changes
- Integration points

**5. Review Design**
- Architecture review
- Security review
- Performance review
- Get approvals

#### Phase 3: Implementation

**6. Move PRD to In Progress**
```bash
Move to: prd/in-progress/vendor-discounts.md
```

**7. Implementation**
- Write code
- Write tests
- Code review
- Update design doc if needed

**8. Update API Documentation**
```bash
File: api-docs/vendor-service.md
```
- Add discount endpoints
- Update examples

#### Phase 4: Documentation Updates

**9. Update User Documentation**
```bash
File: user-docs/VENDORS.md
```
- Add discount management guide
- Update feature list

**10. Update Changelog**
```bash
File: changelogs/CHANGELOG.md
```
- Add to release notes
- Document new features

**11. Move PRD to Completed**
```bash
Move to: prd/completed/vendor-discounts.md
```
- Feature shipped
- Documentation complete

---

### Example 2: Adding a New Service (Backend)

### Scenario: Adding a "Notification Service"

#### Phase 1: Planning & Approval

**1. Create Technical Proposal**
```bash
File: design-docs/technical-proposals/notification-service.md
```
- Problem statement
- Proposed solution
- Benefits and trade-offs
- High-level architecture
- Timeline estimate

**2. Team Review**
- Share proposal in engineering channel
- Collect feedback
- Revise if needed

**3. Create ADR (if approved)**
```bash
File: design-docs/architecture-decisions/adr-005-notification-service.md
```
- Document the decision
- Record alternatives considered
- Capture rationale

#### Phase 2: Design

**4. Create Technical Design Document**
```bash
File: design-docs/system-design/notification-service-design.md
```
- Detailed system design
- API specifications
- Database schema
- Integration points
- Security considerations
- Performance requirements

**5. Review Design**
- Architecture review
- Security review
- Performance review
- Get approvals

#### Phase 3: Specification

**6. Create Service Specification**
```bash
File: technical-specs/services/notification-service-spec.md
```
- Complete technical specification
- API endpoints
- Data models
- Business logic
- Error handling

#### Phase 4: Implementation

**7. Implementation**
- Write code
- Write tests
- Code review
- Update design doc if needed

**8. API Documentation**
```bash
File: api-docs/notification-service.md
```
- Public API documentation
- Endpoints
- Examples
- Authentication

#### Phase 5: Documentation Updates

**9. Update System Architecture**
```bash
File: technical-specs/architecture/system-architecture.md
```
- Add service to architecture diagram
- Update service list
- Document dependencies

**10. Update Infrastructure**
```bash
File: technical-specs/architecture/infrastructure.md
```
- Add EC2 instance configuration
- Update load balancer config
- Add monitoring setup

**11. Update Database Design** (if needed)
```bash
File: technical-specs/architecture/database-design.md
```
- Add new collections
- Update schemas

**12. Update Integration Docs** (if needed)
```bash
File: technical-specs/integrations/third-party.md
```
- Document new integrations

#### Phase 6: Release

**13. Update Changelog**
```bash
File: changelogs/CHANGELOG.md
```
- Add to release notes
- Document new features

**14. Update Internal Changelog**
```bash
File: changelogs/CHANGELOG-INTERNAL.md
```
- Technical changes
- Infrastructure updates

**15. Update Current Implementation Doc**
```bash
File: technical-specs/CURRENT_V1_IMPLEMENTATION.md
```
- Add service to overview
- Update capabilities

---

## ✅ Documentation Checklists

### For New Feature or App (Using PRD)

#### Before Implementation
- [ ] PRD created in `prd/draft/`
- [ ] PRD shared with product designer (`prd/review/`)
- [ ] Product designer feedback incorporated
- [ ] PRD approved (`prd/approved/`)
- [ ] Technical design document created
- [ ] Design reviewed and approved

#### During Implementation
- [ ] PRD moved to `prd/in-progress/`
- [ ] Design doc updated if changes occur
- [ ] Code comments added
- [ ] Tests documented

#### After Implementation
- [ ] PRD moved to `prd/completed/`
- [ ] User documentation updated
- [ ] API documentation updated (if API changes)
- [ ] Changelog updated

### For New Service/App (Backend)

#### Before Implementation
- [ ] Technical proposal created
- [ ] ADR created (if architecture change)
- [ ] Technical design document created
- [ ] Design reviewed and approved
- [ ] Service specification created

#### During Implementation
- [ ] Design doc updated if changes occur
- [ ] Code comments added
- [ ] Tests documented

#### After Implementation
- [ ] API documentation created
- [ ] System architecture updated
- [ ] Infrastructure documentation updated
- [ ] Database design updated (if needed)
- [ ] Integration docs updated (if needed)
- [ ] Changelog updated
- [ ] Current implementation doc updated

---

## 📍 File Locations Summary

### New Feature or App Documentation Path (Using PRD)
```
1. prd/draft/[feature-name].md                              (START HERE) ⭐
2. prd/review/[feature-name].md                             (Share with designer)
3. prd/approved/[feature-name].md                           (Approved)
4. design-docs/system-design/[feature]-design.md            (Technical design)
5. prd/in-progress/[feature-name].md                        (Implementation)
6. Implementation → Code
7. user-docs/[relevant-guide].md                            (User docs)
8. api-docs/[service].md                                    (If API changes)
9. prd/completed/[feature-name].md                          (Shipped)
10. changelogs/CHANGELOG.md                                 (Update)
```

### New Service/App (Backend) Documentation Path
```
1. design-docs/technical-proposals/[service-name].md         (START HERE)
2. design-docs/architecture-decisions/adr-XXX-[service].md   (If architecture change)
3. design-docs/system-design/[service]-design.md            (Detailed design)
4. technical-specs/services/[service]-spec.md                (Technical spec)
5. Implementation → Code
6. api-docs/[service].md                                    (Public API docs)
7. technical-specs/architecture/system-architecture.md      (Update)
8. technical-specs/architecture/infrastructure.md            (Update)
9. technical-specs/architecture/database-design.md          (Update if needed)
10. changelogs/CHANGELOG.md                                 (Update)
11. technical-specs/CURRENT_V1_IMPLEMENTATION.md            (Update)
```

---

## 🚨 Important Rules

### Always Document First
1. **New Feature or App** → PRD BEFORE coding ⭐
2. **New Service** (backend only) → Technical Proposal BEFORE coding
3. **Architecture Change** → ADR BEFORE implementation
4. **Infrastructure Change** → Technical Proposal BEFORE changes

### PRD Workflow
- Create PRD draft
- Share with product designer
- Get approval
- Then proceed with technical design
- Keep PRD updated during development

### Update Existing Docs
- Always update related documentation when making changes
- Keep architecture diagrams current
- Update changelogs for all releases
- Maintain API documentation accuracy

### Review Process
- Technical proposals → Engineering lead review
- ADRs → Architecture team review
- Feature specs → Product + Engineering review
- Design docs → Peer review before implementation

---

## 💡 Best Practices

### Documentation First
✅ Write proposal/design BEFORE coding  
✅ Get approval BEFORE implementation  
✅ Update docs DURING development  
✅ Complete docs BEFORE release  

### Keep It Current
✅ Update docs when code changes  
✅ Remove outdated information  
✅ Link related documents  
✅ Version control all docs  

### Make It Accessible
✅ Use clear, simple language  
✅ Include diagrams where helpful  
✅ Provide code examples  
✅ Link to related docs  

---

## 📞 Questions?

### Where Should I Document X?

| Question | Answer |
|----------|--------|
| "I want to add a new feature" (user-facing) | Start with `prd/draft/[feature-name].md` ⭐ |
| "I want to add a new app" (frontend/mobile) | Start with `prd/draft/[app-name].md` ⭐ |
| "I want to add a new service" (backend only, no UI) | Start with `design-docs/technical-proposals/` |
| "I want to add a new microservice" | Start with `design-docs/technical-proposals/` |
| "I want to change our database" | Start with `design-docs/architecture-decisions/` (ADR) |
| "I want to add a new API endpoint" | Update `api-docs/[service].md` + create PRD if user-facing |
| "I fixed a bug" | Update relevant docs + `changelogs/CHANGELOG.md` |
| "I want to change infrastructure" | Start with `design-docs/technical-proposals/` |

---

## 🎯 Quick Reference

### ⭐ For New Feature or App (Simplified!)
**START HERE:** `prd/draft/[feature-or-app-name].md`

**Then:**
1. Share with Product Designer → `prd/review/`
2. Get Approval → `prd/approved/`
3. Technical Design → `design-docs/system-design/`
4. Implementation → `prd/in-progress/`
5. User Docs → `user-docs/`
6. API Docs → `api-docs/` (if API changes)
7. Shipped → `prd/completed/`
8. Changelog → `changelogs/CHANGELOG.md`

### For New Service (Backend Only)
**START HERE:** `design-docs/technical-proposals/[service-name].md`

**Then:**
1. ADR (if architecture change) → `design-docs/architecture-decisions/`
2. Technical Design → `design-docs/system-design/`
3. Service Spec → `technical-specs/services/`
4. Implementation → Code
5. API Docs → `api-docs/`
6. Update Architecture → `technical-specs/architecture/`
7. Update Infrastructure → `technical-specs/architecture/infrastructure.md`
8. Update Changelog → `changelogs/`

---

**Remember:** Documentation is part of the development process, not an afterthought!

---

**Last Updated:** November 11, 2025  
**Maintained By:** Engineering Leadership Team

