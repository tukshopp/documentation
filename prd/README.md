# Product Requirements Documents (PRD)

**Purpose:** Centralized location for documenting new apps and features for product designer communication.

**Owner:** Engineering Team  
**Audience:** Product Designers, Product Managers, Engineering Team

---

## 📋 Overview

The PRD folder contains Product Requirements Documents that serve as the primary communication tool between engineering and product design teams. PRDs document what we're building, why we're building it, and how it should work.

### PRD Workflow

```
Draft → Review → Approved → In Progress → Completed
```

---

## 📁 Folder Structure

```
prd/
├── draft/          # Initial PRDs being written
├── review/         # PRDs under review with product designer
├── approved/       # PRDs approved and ready for design/development
├── in-progress/    # PRDs for features currently being built
└── completed/      # PRDs for shipped features
```

---

## 📝 Creating a PRD

### For New Features
1. Copy the [PRD Template](./prd-template.md)
2. Fill in all sections
3. Save to `prd/draft/[feature-name].md`
4. Share with product designer for review
5. Move to `review/` when ready for feedback
6. Move to `approved/` when approved
7. Move to `in-progress/` when development starts
8. Move to `completed/` when shipped

### For New Apps/Services
1. Copy the [PRD Template](./prd-template.md)
2. Focus on technical requirements section
3. Follow same workflow as features

---

## 🎯 PRD Status

### Draft
- Initial writing phase
- Engineering team working on requirements
- Not yet shared with product designer

### Review
- Shared with product designer
- Gathering feedback
- Iterating based on design input

### Approved
- Product designer approved
- Ready for design mockups
- Ready for technical design
- Ready for development

### In Progress
- Feature/app is being built
- Design mockups created
- Development in progress

### Completed
- Feature/app shipped to production
- Documentation updated
- Can be referenced for future work

---

## 📊 PRD vs Other Documentation

| Document Type | Purpose | Audience | Location |
|--------------|---------|----------|----------|
| **PRD** | Product requirements & specs | Product Designer, Product Manager | `prd/` |
| **Feature Request** | Initial idea/request | Product Team | `feature-management/feature-requests/` |
| **Product Spec** | Detailed product specification | Product Team | `product-specs/features/` |
| **Technical Design** | Technical implementation design | Engineering Team | `design-docs/system-design/` |
| **Service Spec** | Technical service specification | Engineering Team | `technical-specs/services/` |

### When to Use PRD vs Other Docs

**Use PRD when:**
- ✅ Communicating with product designer
- ✅ Documenting user-facing features
- ✅ Documenting new apps
- ✅ Need product requirements before design
- ✅ Need a single source of truth for what we're building

**Use Feature Request when:**
- Initial idea stage
- Need to prioritize
- Gathering requirements

**Use Technical Design when:**
- PRD is approved
- Ready for technical implementation
- Need detailed system design

---

## 🔄 PRD Workflow Integration

### Complete Flow

```
1. Feature Request (optional)
   ↓
2. PRD Draft → prd/draft/
   ↓
3. PRD Review → prd/review/ (with product designer)
   ↓
4. PRD Approved → prd/approved/
   ↓
5. Technical Design → design-docs/system-design/
   ↓
6. Implementation → prd/in-progress/
   ↓
7. Shipped → prd/completed/
```

---

## 📋 PRD Checklist

### Before Sharing with Product Designer
- [ ] Problem statement clearly defined
- [ ] User stories written
- [ ] Success metrics defined
- [ ] Technical constraints documented
- [ ] Dependencies identified
- [ ] Timeline estimate provided

### After Product Designer Review
- [ ] Design feedback incorporated
- [ ] User flows validated
- [ ] UI/UX requirements clarified
- [ ] Edge cases discussed
- [ ] Approval received

### Before Development
- [ ] PRD approved
- [ ] Technical design created
- [ ] Design mockups ready
- [ ] API contracts defined (if applicable)
- [ ] Dependencies resolved

---

## 👥 Roles & Responsibilities

### Engineering Lead (You)
- Create PRD drafts
- Define technical requirements
- Document constraints and dependencies
- Communicate with product designer
- Update PRD based on feedback
- Move PRD through workflow

### Product Designer
- Review PRD for design feasibility
- Provide UX/UI feedback
- Create design mockups
- Validate user flows
- Approve PRD

### Product Manager
- Prioritize PRDs
- Validate business requirements
- Approve features for development

---

## 📚 Related Documentation

- [PRD Template](./prd-template.md) - Use this template for new PRDs
- [Documentation Workflow](../DOCUMENTATION_WORKFLOW.md) - Complete documentation workflow
- [Feature Management](../feature-management/) - Feature lifecycle management
- [Product Specs](../product-specs/) - Detailed product specifications

---

## 💡 Best Practices

### Writing PRDs
- ✅ Be clear and concise
- ✅ Include user stories
- ✅ Define success metrics
- ✅ Document technical constraints
- ✅ Provide visual mockups/wireframes if available
- ✅ Link to related documents

### Communication
- ✅ Share PRD early for feedback
- ✅ Iterate based on designer input
- ✅ Keep PRD updated during development
- ✅ Document decisions and changes

### Maintenance
- ✅ Keep PRDs current
- ✅ Archive completed PRDs
- ✅ Reference PRDs in technical designs
- ✅ Link PRDs to related documentation

---

## 🎯 Quick Start

1. **Copy the template:**
   ```bash
   cp prd/prd-template.md prd/draft/my-feature.md
   ```

2. **Fill in the PRD:**
   - Problem statement
   - User stories
   - Requirements
   - Technical constraints

3. **Share with product designer:**
   - Move to `prd/review/`
   - Request feedback

4. **Iterate:**
   - Incorporate feedback
   - Update PRD

5. **Get approval:**
   - Move to `prd/approved/`
   - Proceed with technical design

---

**Last Updated:** November 11, 2025  
**Maintained By:** Engineering Team

