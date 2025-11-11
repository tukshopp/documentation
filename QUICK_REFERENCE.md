# TukShopp Documentation Quick Reference

A quick reference guide for navigating TukShopp's documentation structure.

## 🎯 I Want To...

### As an Engineer

#### Start Working on a New Feature
1. Read the [feature spec](./product-specs/features/)
2. Create a [technical design](./design-docs/templates/design-doc-template.md)
3. If making architecture changes, create an [ADR](./design-docs/templates/adr-template.md)
4. Implement and test
5. Update [user docs](./user-docs/) and [API docs](./api-docs/)
6. Update [CHANGELOG](./changelogs/CHANGELOG.md)

#### Set Up My Development Environment
→ [Development Setup Guide](./internal-docs/engineering/development-setup.md)

#### Understand the System Architecture
→ [Technical Specs - Architecture](./technical-specs/architecture/)

#### Understand a Specific Service
→ [Technical Specs - Services](./technical-specs/services/)

#### Deploy to Production
→ [Deployment Guide](./internal-docs/deployment/deployment-guide.md)

#### Handle an Incident
→ [Incident Response Guide](./internal-docs/operations/incident-response.md)

#### Review Code
→ [Code Review Guide](./internal-docs/engineering/code-review-guide.md)

#### Write Tests
→ [Testing Guidelines](./internal-docs/engineering/testing-guidelines.md)

---

### As a Product Manager

#### Submit a Feature Request
1. Use [feature request template](./feature-management/feature-requests/template.md)
2. Add to [backlog](./feature-management/feature-requests/backlog.md)

#### Create a Feature Specification
→ [Feature Template](./product-specs/features/feature-template.md)

#### Plan a Sprint
→ [Implementation Planning](./feature-management/implementation/)

#### Plan a Release
→ [Release Management](./feature-management/releases/)

#### Update the Roadmap
→ [Roadmap](./roadmap/ROADMAP.md)

---

### As a New Team Member

#### Get Onboarded
1. Read [Onboarding Guide](./internal-docs/team/onboarding.md)
2. Set up [development environment](./internal-docs/engineering/development-setup.md)
3. Review [coding standards](./internal-docs/engineering/coding-standards.md)
4. Learn [git workflow](./internal-docs/engineering/git-workflow.md)
5. Understand [system architecture](./technical-specs/architecture/)

---

### As an External Developer

#### Integrate with TukShopp API
1. Read [API Overview](./api-docs/README.md)
2. Review specific service docs:
   - [Auth Service](./api-docs/auth-service.md)
   - [Account Service](./api-docs/account-service.md)
   - [Vendor Service](./api-docs/vendor-service.md)
   - [Marketplace Service](./api-docs/marketplace-service.md)
3. Get API credentials
4. Start building!

---

## 📂 Where Is...?

### Documentation Types

| I'm Looking For... | It's In... |
|-------------------|------------|
| User guides (customers, vendors, riders) | [`user-docs/`](./user-docs/) |
| API documentation | [`api-docs/`](./api-docs/) |
| System architecture | [`technical-specs/architecture/`](./technical-specs/architecture/) |
| Service specifications | [`technical-specs/services/`](./technical-specs/services/) |
| Database schemas | [`technical-specs/data/`](./technical-specs/data/) |
| Integration details | [`technical-specs/integrations/`](./technical-specs/integrations/) |
| Development setup | [`internal-docs/engineering/`](./internal-docs/engineering/) |
| Deployment procedures | [`internal-docs/deployment/`](./internal-docs/deployment/) |
| Incident response | [`internal-docs/operations/`](./internal-docs/operations/) |
| Team information | [`internal-docs/team/`](./internal-docs/team/) |
| Feature specifications | [`product-specs/features/`](./product-specs/features/) |
| Business requirements | [`product-specs/requirements/`](./product-specs/requirements/) |
| Business logic | [`product-specs/business-logic/`](./product-specs/business-logic/) |
| Feature requests | [`feature-management/feature-requests/`](./feature-management/feature-requests/) |
| RFCs | [`feature-management/rfcs/`](./feature-management/rfcs/) |
| Sprint planning | [`feature-management/implementation/`](./feature-management/implementation/) |
| Release notes | [`feature-management/releases/`](./feature-management/releases/) |
| Technical designs | [`design-docs/system-design/`](./design-docs/system-design/) |
| Architecture decisions (ADRs) | [`design-docs/architecture-decisions/`](./design-docs/architecture-decisions/) |
| Changelog (public) | [`changelogs/CHANGELOG.md`](./changelogs/CHANGELOG.md) |
| Roadmap | [`roadmap/ROADMAP.md`](./roadmap/ROADMAP.md) |
| Compliance docs | [`compliance/`](./compliance/) |

---

## 📋 Templates

### Commonly Used Templates

| Template | Location | Use For |
|----------|----------|---------|
| Feature Specification | [`product-specs/features/feature-template.md`](./product-specs/features/feature-template.md) | New feature specs |
| Technical Design Doc | [`design-docs/templates/design-doc-template.md`](./design-docs/templates/design-doc-template.md) | Technical designs |
| ADR (Architecture Decision Record) | [`design-docs/templates/adr-template.md`](./design-docs/templates/adr-template.md) | Architecture decisions |
| Feature Request | [`feature-management/feature-requests/template.md`](./feature-management/feature-requests/template.md) | Submitting feature requests |

---

## 🔄 Common Workflows

### Feature Development Workflow
```
1. Feature Request → feature-management/feature-requests/
2. Product Spec → product-specs/features/planned/
3. RFC (if major) → feature-management/rfcs/
4. Technical Design → design-docs/system-design/
5. ADR (if architecture) → design-docs/architecture-decisions/
6. Implementation → Code + Tests
7. Move spec → product-specs/features/in-progress/
8. Update Docs → user-docs/, api-docs/, technical-specs/
9. Release → feature-management/releases/
10. Move spec → product-specs/features/implemented/
11. Update Changelog → changelogs/CHANGELOG.md
```

### Bug Fix Workflow
```
1. Identify bug → Create ticket
2. Debug → internal-docs/engineering/debugging-guide.md
3. Fix → Code + Tests
4. Deploy → internal-docs/deployment/
5. Update CHANGELOG-INTERNAL → changelogs/
```

### Incident Response Workflow
```
1. Incident detected → Follow internal-docs/operations/incident-response.md
2. Resolve → Document steps taken
3. Postmortem → Add to internal-docs/operations/
4. Prevent → Update monitoring, alerts, runbooks
```

---

## 🚨 Emergency Contacts

### On-Call
→ See [`internal-docs/operations/on-call-guide.md`](./internal-docs/operations/on-call-guide.md)

### Incident Response
→ See [`internal-docs/operations/incident-response.md`](./internal-docs/operations/incident-response.md)

---

## 📞 Who to Ask

| Question About... | Ask... | Find Info In... |
|------------------|--------|-----------------|
| Product requirements | Product Manager | `product-specs/` |
| Technical implementation | Engineering Lead | `technical-specs/` |
| System architecture | Technical Architect | `technical-specs/architecture/` |
| Deployment | DevOps/SRE | `internal-docs/deployment/` |
| API usage | API Team / Docs | `api-docs/` |
| User experience | Product Team | `user-docs/` |
| Business logic | Product Team | `product-specs/business-logic/` |
| Security/Compliance | Security Team | `compliance/` |

---

## 🔍 Search Tips

### Finding Information Quickly

1. **Use your IDE's search** - Search across all markdown files
2. **Check the relevant README** - Each section has a README with navigation
3. **Use the main README** - Organized by audience
4. **Check DOCUMENTATION_STRUCTURE.md** - Complete directory layout
5. **Look at templates** - Show what info should be documented

### Common Search Terms

- **"template"** - Find all templates
- **"TODO" or "TBD"** - Find incomplete sections
- **Service names** - Find service-specific docs
- **"architecture"** - Find design docs
- **"API"** - Find API documentation

---

## ⚡ Shortcuts

### Most Frequently Used Docs

- 📖 [Main README](./README.md)
- 🏗️ [System Architecture](./technical-specs/architecture/)
- 🔌 [API Overview](./api-docs/README.md)
- 📝 [Feature Template](./product-specs/features/feature-template.md)
- 🚀 [Deployment Guide](./internal-docs/deployment/deployment-guide.md)
- 🆘 [Incident Response](./internal-docs/operations/incident-response.md)
- 📅 [Roadmap](./roadmap/ROADMAP.md)
- 📋 [Changelog](./changelogs/CHANGELOG.md)

---

## 💡 Best Practices

### Documentation
- ✅ Write as you code
- ✅ Update docs when changing features
- ✅ Use templates
- ✅ Include diagrams for complex concepts
- ✅ Keep docs in sync with reality
- ✅ Review docs during code review

### File Organization
- ✅ Put docs in the right category
- ✅ Use descriptive file names
- ✅ Follow existing naming conventions
- ✅ Link related documents
- ✅ Keep README files updated

---

## 🆘 Need Help?

- **Documentation questions**: documentation@tukshopp.ng
- **Can't find something**: Ask in team chat or check [DOCUMENTATION_STRUCTURE.md](./DOCUMENTATION_STRUCTURE.md)
- **Broken links**: Report and fix them
- **Unclear docs**: Submit improvements

---

**Quick Tip:** Bookmark this page for fast access to documentation!

---

Last Updated: November 11, 2025

