---
name: prototype-analysis-template
description: InformaDS prototype analysis template for UI/UX analysis - structured template for analyzing prototypes, identifying InformaDS components, documenting functionality requirements, component installation guides, design tokens usage, interactive states, data structures, API integration, implementation checklist, accessibility considerations, and responsive design specifications.
model: haiku
---

# Prototype Analysis: [SCREEN NAME]

## 📸 Analyzed Prototype

**File:** `prototypes/[file-name].png`
**Analysis Date:** [DATE]
**Analyst:** [DEVELOPER NAME]

---

## 📝 General Description

**Screen purpose:**
[Briefly describe what function this screen serves in the application]

**Target users:**
[Who will use this screen and for what purpose]

**Navigation flow:**
[From where it's accessed and where the user can navigate]

---

## 🎨 Identified InformaDS Components

### 1. **[Component Name 1]**

- **Component:** `@informads/[component-name]`
- **Location:** [Where it's located on the screen]
- **Features:**
  - [Feature 1]
  - [Feature 2]
  - [Etc.]
- **Main props:**

  ```typescript
  {
    prop1: "value",
    prop2: true,
    // ...
  }
  ```

### 2. **[Component Name 2]**

- **Component:** `@informads/[component-name]`
- **Location:** [Where it's located on the screen]
- **Features:**
  - [Feature 1]
  - [Feature 2]

### 3. **[Continue with all identified components...]**

---

## 🏗️ View Architecture

```text
┌─────────────────────────────────────────────────┐
│  [Top Section]                                  │
│  ┌──────────────────┐  ┌──────────────────┐    │
│  │ [Element 1]      │  │ [Element 2]      │    │
│  └──────────────────┘  └──────────────────┘    │
├─────────────────────────────────────────────────┤
│  [Middle Section]                               │
│  ┌──────────────────────────────────────────┐  │
│  │ [Main content]                           │  │
│  └──────────────────────────────────────────┘  │
├─────────────────────────────────────────────────┤
│  [Bottom Section]                               │
│  ┌──────────────────────────────────────────┐  │
│  │ [Additional elements]                    │  │
│  └──────────────────────────────────────────┘  │
└─────────────────────────────────────────────────┘
```

**Layout notes:**

- [Describe the layout type: grid, flex, etc.]
- [Responsive considerations]
- [Spacing and measurements]

---

## 🎯 Required Functionalities

### Main Actions

1. **[Action 1]**
   - **Trigger:** [What triggers the action]
   - **Suggested component:** `@informads/[component]`
   - **Behavior:** [What should happen]
   - **Validations:** [If applicable]

2. **[Action 2]**
   - **Trigger:** [What triggers the action]
   - **Suggested component:** `@informads/[component]`
   - **Behavior:** [What should happen]

3. **[Continue with all actions...]**

### Recommended Additional Functionalities

- **[Functionality 1]:** [Description] - `@informads/[component]`
- **[Functionality 2]:** [Description] - `@informads/[component]`
- **[Functionality 3]:** [Description] - `@informads/[component]`

---

## 📦 Required InformaDS Components

### Minimum Viable Product (MVP) Installation

```bash
yarn add @informads/theme-provider  # Required
yarn add @informads/[component-1]
yarn add @informads/[component-2]
yarn add @informads/[component-3]
# ... Add all basic components
```

### Complete Installation (Recommended)

```bash
yarn add @informads/theme-provider
yarn add @informads/[component-1]
yarn add @informads/[component-2]
yarn add @informads/[component-3]
# ... Add all components including optional ones
```

### Complete Component List

| #   | Component                 | Version | Usage         | Priority       |
| --- | ------------------------- | ------- | ------------- | -------------- |
| 1   | @informads/theme-provider | ^1.5.16 | Global theme  | ⚠️ REQUIRED    |
| 2   | @informads/[comp-1]       | ^X.X.X  | [Description] | 🔴 High        |
| 3   | @informads/[comp-2]       | ^X.X.X  | [Description] | 🟡 Medium      |
| 4   | @informads/[comp-3]       | ^X.X.X  | [Description] | 🟢 Low         |

---

## 🎨 Applied Tokens and Styles

### Spacing

- **Container padding:** `var(--spacing-[size], [fallback]px)`
- **Between sections:** `var(--spacing-[size], [fallback]px)`
- **Internal:** `var(--spacing-[size], [fallback]px)`
- **Small:** `var(--spacing-[size], [fallback]px)`

### Colors

- **Primary:** `var(--color-primary, [fallback])`
- **Main text:** `var(--color-text-primary, [fallback])`
- **Secondary text:** `var(--color-text-secondary, [fallback])`
- **Borders:** `var(--color-border, [fallback])`
- **Backgrounds:** `var(--color-background-[variant], [fallback])`

### Typography

- **Main title:** `var(--font-size-[size], [fallback]px)` / `var(--font-weight-[weight], [fallback])`
- **Base text:** `var(--font-size-base, 16px)`
- **Small text:** `var(--font-size-sm, 14px)`

### Borders and Shadows

- **Border radius:** `var(--border-radius-[size], [fallback]px)`
- **Shadows:** `var(--shadow-[size], [fallback])`

---

## 🔄 Interactive States

### [Component 1]

- **Hover:** [Behavior description]
- **Active:** [Behavior description]
- **Disabled:** [Behavior description]
- **Focus:** [Behavior description]

### [Component 2]

- **Hover:** [Behavior description]
- **Selected:** [Behavior description]
- **Error:** [Behavior description]

### [Continue with other components...]

---

## 📊 Data Structure

### TypeScript Interfaces

```typescript
// Main interface
interface [MainName] {
  id: string
  [field1]: string
  [field2]: number
  [field3]: boolean
  // ... more fields
}

// Auxiliary interfaces
interface [AuxiliaryName] {
  // ...
}
```

### Component States

```typescript
// Required states
const [state1, setState1] = useState<Type>(initialValue);
const [state2, setState2] = useState<Type>(initialValue);
// ... more states
```

---

## 🌐 API Integration (if applicable)

### Required Endpoints

1. **GET /api/[resource]**
   - **Purpose:** [What it's used for]
   - **Params:** [Required parameters]
   - **Response:** [Response structure]

2. **POST /api/[resource]**
   - **Purpose:** [What it's used for]
   - **Body:** [Body structure]
   - **Response:** [Response structure]

3. **[Other endpoints...]**

---

## ✅ Implementation Checklist

### Base Structure

- [ ] Create page/component folder
- [ ] Create main component `.tsx` file
- [ ] Create `.module.scss` file for styles
- [ ] Create TypeScript interfaces in `types/`

### InformaDS Components

- [ ] Install required components
- [ ] Import components in the file
- [ ] Configure basic props
- [ ] Apply theme tokens

### Functionality

- [ ] Implement required states
- [ ] Create event handlers
- [ ] Implement validations
- [ ] Connect with APIs (if applicable)

### Styles

- [ ] Apply spacing tokens
- [ ] Apply color tokens
- [ ] Implement interactive states
- [ ] Verify responsive design

### Testing

- [ ] Unit tests for logic
- [ ] User interaction tests
- [ ] Accessibility tests

### Documentation

- [ ] Document component props
- [ ] Document main functions
- [ ] Add usage examples

---

## 🚨 Special Considerations

### Accessibility (WCAG 2.1)

- [Consideration 1]
- [Consideration 2]
- [Consideration 3]

### Performance

- [Optimization 1]
- [Optimization 2]

### Responsive Design

- **Mobile (< 768px):** [Behavior]
- **Tablet (768px - 1024px):** [Behavior]
- **Desktop (> 1024px):** [Behavior]

### Edge Cases

- [Edge case 1 and how to handle it]
- [Edge case 2 and how to handle it]

---

## 📝 Additional Notes

### Design Decisions

- [Note 1]
- [Note 2]

### Pending / To Define

- [ ] [Pending 1]
- [ ] [Pending 2]

### References

- **Figma:** [Link to Figma design]
- **Jira/Issue:** [Link to ticket]
- **Related documentation:** [Links]

---

## 🔄 Change History

| Date   | Version | Changes              | Author   |
| ------ | ------- | -------------------- | -------- |
| [DATE] | 1.0     | Initial analysis     | [NAME]   |
| [DATE] | 1.1     | [Change description] | [NAME]   |

---

**Last update:** [DATE]
**Status:** [ ] Draft | [ ] Under Review | [ ] Approved | [ ] Implemented
