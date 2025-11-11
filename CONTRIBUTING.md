# Contributing to TukShopp Documentation

Thank you for your interest in contributing to TukShopp documentation! This guide will help you contribute effectively.

## 📋 Table of Contents

- [How to Contribute](#how-to-contribute)
- [Documentation Standards](#documentation-standards)
- [File Structure](#file-structure)
- [Writing Guidelines](#writing-guidelines)
- [Submitting Changes](#submitting-changes)
- [Review Process](#review-process)

---

## 🤝 How to Contribute

### Types of Contributions

We welcome the following contributions:

1. **Corrections**
   - Fix typos and grammatical errors
   - Correct inaccurate information
   - Update outdated content

2. **Improvements**
   - Clarify confusing sections
   - Add missing information
   - Improve examples and code snippets
   - Enhance formatting and readability

3. **New Content**
   - Add new guides or tutorials
   - Document new features
   - Create troubleshooting guides
   - Add FAQ entries

4. **Translations**
   - Translate documentation to other languages
   - Keep translations up-to-date

---

## 📐 Documentation Standards

### Markdown Style

We use standard Markdown with these conventions:

#### Headings

```markdown
# H1 - Document Title (one per document)
## H2 - Major Sections
### H3 - Subsections
#### H4 - Sub-subsections
```

#### Code Blocks

Use triple backticks with language identifier:

```markdown
```javascript
const example = "code";
```
```

#### Links

- Use descriptive link text
- Prefer relative links for internal documentation
- Check that links work

```markdown
[Good link text](./relative-path.md)
[External link](https://example.com)
```

#### Lists

- Use `-` for unordered lists
- Use `1.` for ordered lists
- Indent nested lists with 2 spaces

#### Emphasis

- **Bold** for emphasis and UI elements
- *Italic* for terms and light emphasis
- `Code` for code, commands, and file names

---

### Content Structure

#### Document Template

```markdown
# Document Title

Brief introduction (1-2 sentences about what this document covers).

## Overview

More detailed introduction with context.

---

## Main Section

Content here...

### Subsection

Content here...

---

## Related Documentation

- [Link to related doc](./related.md)

---

Last Updated: YYYY-MM-DD
```

#### Sections Order

1. Title and introduction
2. Overview/prerequisites
3. Main content (logical flow)
4. Examples (if applicable)
5. Troubleshooting (if applicable)
6. Related documentation
7. Last updated date

---

## 📁 File Structure

### Directory Organization

```
documentation/
├── README.md                    # Main documentation index
├── CHANGELOG.md                 # Feature updates
├── ROADMAP.md                   # Future plans
├── CONTRIBUTING.md             # This file
├── user-guides/                 # User-facing guides
│   ├── CUSTOMERS.md
│   ├── VENDORS.md
│   └── RIDERS.md
├── platform/                    # Platform documentation
│   ├── OVERVIEW.md
│   ├── FEATURES.md
│   └── ZONES.md
├── api/                         # API documentation
│   ├── README.md
│   ├── auth-service.md
│   ├── account-service.md
│   ├── vendor-service.md
│   └── marketplace-service.md
└── resources/                   # Support resources
    ├── FAQ.md
    └── SUPPORT.md
```

### Naming Conventions

- Use UPPERCASE for top-level files (README.md, CHANGELOG.md)
- Use kebab-case for multi-word files (auth-service.md)
- Use descriptive, clear names
- Keep file names concise

---

## ✍️ Writing Guidelines

### Voice and Tone

- **Clear and concise** - Get to the point
- **Friendly but professional** - Be approachable
- **Active voice** - "Click the button" not "The button should be clicked"
- **Second person** - Address reader as "you"
- **Present tense** - "The app displays" not "The app will display"

### Technical Writing Best Practices

#### Be Clear

❌ **Bad:** "You might want to consider doing this thing"  
✅ **Good:** "Do this to achieve X"

#### Be Specific

❌ **Bad:** "Wait a few moments"  
✅ **Good:** "Wait 1-3 business days"

#### Use Examples

Include practical examples:

```markdown
### Example: Creating an Order

```javascript
const order = await createOrder({
  items: [{ id: 'item_123', quantity: 2 }]
});
```
```

#### Include Visuals

When appropriate:
- Screenshots of UI
- Diagrams of processes
- Code examples
- Tables for comparisons

---

### Code Examples

#### Format Code Properly

```javascript
// Good: Well-formatted, commented code
async function placeOrder(items) {
  try {
    const response = await fetch('/api/orders', {
      method: 'POST',
      body: JSON.stringify({ items })
    });
    return await response.json();
  } catch (error) {
    console.error('Order failed:', error);
  }
}
```

#### Make Examples Complete

- Include necessary imports
- Show full context
- Provide realistic data
- Add comments for clarity

#### Test Your Code

- Verify code examples work
- Test API endpoints
- Check syntax
- Validate JSON

---

### API Documentation

When documenting APIs:

1. **Endpoint** - HTTP method and path
2. **Description** - What it does
3. **Authentication** - Requirements
4. **Parameters** - Required and optional
5. **Request body** - With example
6. **Response** - With example
7. **Error codes** - Possible errors
8. **Example** - Complete working example

---

## 🔄 Submitting Changes

### Before Submitting

✅ **Checklist:**

- [ ] Spell-check your content
- [ ] Verify all links work
- [ ] Test code examples
- [ ] Check formatting in preview
- [ ] Follow file naming conventions
- [ ] Update "Last Updated" date
- [ ] Review for clarity and accuracy

### Submission Process

1. **Fork the repository** (if external contributor)

2. **Create a branch**
   ```bash
   git checkout -b docs/improve-customer-guide
   ```

3. **Make your changes**
   - Edit existing files or create new ones
   - Follow the standards in this guide
   - Commit with clear messages

4. **Write a clear commit message**
   ```bash
   git commit -m "docs: Clarify order cancellation process in customer guide"
   ```

   **Commit message format:**
   - `docs:` - Documentation changes
   - `fix:` - Corrections/fixes
   - `feat:` - New content
   - `style:` - Formatting changes

5. **Push your changes**
   ```bash
   git push origin docs/improve-customer-guide
   ```

6. **Create a Pull Request**
   - Provide clear title and description
   - Reference related issues
   - Explain what changed and why

---

## 👀 Review Process

### What Happens Next

1. **Automated Checks**
   - Markdown linting
   - Link validation
   - Spell checking

2. **Team Review**
   - Content accuracy
   - Style consistency
   - Technical correctness

3. **Feedback**
   - Reviewers may request changes
   - Address feedback promptly
   - Discuss any disagreements respectfully

4. **Approval & Merge**
   - Once approved, changes are merged
   - Documentation is automatically deployed

### Review Criteria

Your contribution will be evaluated on:

- ✅ **Accuracy** - Information is correct
- ✅ **Clarity** - Easy to understand
- ✅ **Completeness** - Covers the topic adequately
- ✅ **Consistency** - Follows style guide
- ✅ **Formatting** - Properly formatted Markdown
- ✅ **Links** - All links work
- ✅ **Examples** - Code examples work

---

## 📝 Documentation Types

### User Guides

**Purpose:** Help users accomplish tasks

**Include:**
- Step-by-step instructions
- Screenshots
- Common issues
- Tips and best practices

**Example:** [Customer Guide](./user-guides/CUSTOMERS.md)

---

### API Documentation

**Purpose:** Enable developers to integrate with TukShopp

**Include:**
- Endpoint details
- Request/response examples
- Error codes
- Authentication
- Complete code examples

**Example:** [API Documentation](./api/README.md)

---

### Platform Documentation

**Purpose:** Explain how TukShopp works

**Include:**
- Architecture overview
- Feature descriptions
- Technical concepts
- System behavior

**Example:** [Platform Overview](./platform/OVERVIEW.md)

---

### Support Documentation

**Purpose:** Help users solve problems

**Include:**
- FAQ
- Troubleshooting guides
- Contact information
- Common issues and solutions

**Example:** [FAQ](./resources/FAQ.md)

---

## 🎨 Formatting Guidelines

### Emojis

Use emojis sparingly for visual organization:

- 📋 Lists and tables
- 🎯 Goals and objectives
- ✅ Checkmarks and good examples
- ❌ Cross marks and bad examples
- 💡 Tips and insights
- 🚨 Warnings and important notes
- 📞 Contact information

### Callouts

Use blockquotes for important notes:

```markdown
> **Note:** This is an important note.

> **Warning:** This is a warning.

> **Tip:** This is a helpful tip.
```

### Tables

Use tables for structured data:

```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
```

---

## 🌍 Localization

### Translation Guidelines

When translating:

1. **Maintain structure** - Keep heading hierarchy
2. **Adapt examples** - Use local currency, names
3. **Cultural sensitivity** - Consider local context
4. **Keep links** - Translate link text, not URLs
5. **File naming** - Use language codes (e.g., `README.es.md`)

---

## 🐛 Reporting Issues

### Found an Error?

Report documentation issues:

1. **Check existing issues** - Avoid duplicates
2. **Create detailed report**
   - What's wrong
   - Where it's located
   - What it should be
   - Screenshots if helpful

3. **Use issue template** (if available)

**Issue Title Format:**
```
[DOCS] Brief description of issue
```

---

## 💬 Getting Help

### Questions?

- **Email:** documentation@tukshopp.ng
- **Community Forum:** [Link to forum]
- **GitHub Discussions:** For contribution questions

### Need Clarification?

Don't hesitate to ask before making changes:
- Open a discussion
- Comment on related issues
- Contact documentation team

---

## 🏆 Recognition

### Contributors

We appreciate all contributions! Contributors will be:

- Listed in our contributors file
- Mentioned in release notes (for significant contributions)
- Eligible for contributor swag (coming soon)

---

## 📜 License

By contributing to TukShopp documentation, you agree that your contributions will be licensed under the same license as the project.

---

## 📚 Resources

### Helpful Links

- [Markdown Guide](https://www.markdownguide.org/)
- [Technical Writing Best Practices](https://developers.google.com/tech-writing)
- [API Documentation Best Practices](https://swagger.io/blog/api-documentation/best-practices-in-api-documentation/)

### Style Guides

- [Google Developer Documentation Style Guide](https://developers.google.com/style)
- [Microsoft Writing Style Guide](https://docs.microsoft.com/en-us/style-guide/welcome/)

---

## 🙏 Thank You!

Your contributions help make TukShopp better for everyone. We appreciate your time and effort!

**Questions?** Contact us at documentation@tukshopp.ng

---

Last Updated: November 11, 2025

