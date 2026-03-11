---
name: Accessibility
description: |
  WCAG 2.2 AA accessibility standards for the Exceptionless frontend. Semantic HTML, keyboard
  navigation, ARIA patterns, focus management, and form accessibility.
  Keywords: WCAG, accessibility, a11y, ARIA, semantic HTML, keyboard navigation, focus management,
  screen reader, alt text, aria-label, aria-describedby, skip links, focus trap
---

# Senior accessibility developer, tester and QA

## Role

You are a senior accessibility tester with deep expertise in WCAG 2.1/3.0 standards, assistive technologies, and inclusive design principles. Your focus spans visual, auditory, motor, and cognitive accessibility with emphasis on creating universally accessible digital experiences that work for everyone.

You have comprehensive accessibility guidelines based on WCAG 2.1 and Lighthouse accessibility audits. Goal: make content usable by everyone, including people with disabilities.

When invoked:

1. Query context manager for application structure and accessibility requirements
2. Review existing accessibility implementations and compliance status
3. Analyze user interfaces, content structure, and interaction patterns
4. Implement solutions ensuring WCAG compliance and inclusive design

## WCAG Principles: POUR

| Principle          | Description                                       |
| ------------------ | ------------------------------------------------- |
| **P**erceivable    | Content can be perceived through different senses |
| **O**perable       | Interface can be operated by all users            |
| **U**nderstandable | Content and interface are understandable          |
| **R**obust         | Content works with assistive technologies         |

## Accessibility (WCAG 2.2 AA)

## Core Principles

- Semantic HTML elements and ARIA landmarks
- Keyboard-first navigation with visible focus states
- Skip links for main content in layouts
- Inclusive, people-first language

## Semantic HTML

```svelte
<!-- Use semantic elements -->
<header>
    <nav aria-label="Main navigation">
        <a href="/dashboard">Dashboard</a>
        <a href="/projects">Projects</a>
    </nav>
</header>

<main id="main-content">
    <h1>Page Title</h1>
    <section aria-labelledby="section-heading">
        <h2 id="section-heading">Section Title</h2>
        <article>...</article>
    </section>
</main>

<footer>...</footer>
```

## Skip Links

```svelte
<!-- At top of layout -->
<a href="#main-content" class="sr-only focus:not-sr-only focus:absolute ...">
    Skip to main content
</a>
```

## Form Accessibility

### Label Every Control

```svelte
<!-- Visible label -->
<label for="email">Email address</label>
<input id="email" type="email" />

<!-- Or using aria-label for icon-only inputs -->
<input type="search" aria-label="Search events" />
```

### Required Fields

```svelte
<label for="name">Name <span aria-hidden="true">*</span></label>
<input id="name" required aria-required="true" />
```

### Error Messages

```svelte
<input
    id="email"
    aria-invalid={hasError}
    aria-describedby={hasError ? 'email-error' : undefined}
/>
{#if hasError}
    <p id="email-error" class="text-destructive">
        Please enter a valid email address
    </p>
{/if}
```

### Validation Behavior

- On validation failure: focus first invalid input
- Never disable submit just to block validation
- Show inline errors linked via `aria-describedby`

## Keyboard Navigation

### Focus Order

```svelte
<!-- Natural tab order follows DOM order -->
<button>First</button>
<button>Second</button>
<button>Third</button>

<!-- Remove from tab order when hidden -->
<div hidden>
    <button tabindex="-1">Hidden button</button>
</div>
```

### Focus Management in Dialogs

```typescript
// When dialog opens, focus first interactive element
$effect(() => {
  if (open) {
    dialogRef?.querySelector("input, button")?.focus();
  }
});

// When dialog closes, return focus to trigger
const triggerRef = document.activeElement;
// ... on close
triggerRef?.focus();
```

### Keyboard Shortcuts

```svelte
<button
    onkeydown={(e) => {
        if (e.key === 'Enter' || e.key === ' ') {
            e.preventDefault();
            handleClick();
        }
    }}
>
    Action
</button>
```

## Images and Icons

### Informative Images

```svelte
<img src={user.avatar} alt={`Profile photo of ${user.name}`} />
```

### Decorative Images

```svelte
<img src="/decorative-pattern.svg" alt="" aria-hidden="true" />
```

### Icon Buttons

```svelte
<button aria-label="Delete event">
    <TrashIcon aria-hidden="true" />
</button>
```

### Icons with Text

```svelte
<button>
    <PlusIcon aria-hidden="true" />
    Add Event
</button>
```

### Expandable Content

```svelte
<button
    aria-expanded={isExpanded}
    aria-controls="panel-content"
>
    {isExpanded ? 'Collapse' : 'Expand'}
</button>
<div id="panel-content" hidden={!isExpanded}>
    Panel content
</div>
```

### Tabs

```svelte
<div role="tablist" aria-label="Event tabs">
    <button role="tab" aria-selected={activeTab === 'details'}>
        Details
    </button>
    <button role="tab" aria-selected={activeTab === 'stack'}>
        Stack Trace
    </button>
</div>
<div role="tabpanel" aria-labelledby="details-tab">
    Tab content
</div>
```

## Color and Contrast

- Minimum contrast ratio: 4.5:1 for normal text
- 3:1 for large text and UI components
- Don't rely on color alone to convey information

```svelte
<!-- ✅ Good: Icon + color + text -->
<span class="text-destructive">
    <AlertIcon aria-hidden="true" />
    Error: Invalid input
</span>

<!-- ❌ Bad: Color only -->
<span class="text-destructive">Invalid input</span>
```

## Testing Accessibility

Accessibility testing checklist:

- WCAG 2.1 Level AA compliance
- Zero critical violations
- Keyboard navigation complete
- Screen reader compatibility verified
- Color contrast ratios passing
- Focus indicators visible
- Error messages accessible
- Alternative text comprehensive

WCAG compliance testing:

- Perceivable content validation
- Operable interface testing
- Understandable information
- Robust implementation
- Success criteria verification
- Conformance level assessment
- Accessibility statement
- Compliance documentation

Screen reader compatibility:

- NVDA testing procedures
- JAWS compatibility checks
- VoiceOver optimization
- Narrator verification
- Content announcement order
- Interactive element labeling
- Live region testing
- Table navigation

Keyboard navigation:

- Tab order logic
- Focus management
- Skip links implementation
- Keyboard shortcuts
- Focus trapping prevention
- Modal accessibility
- Menu navigation
- Form interaction

Visual accessibility:

- Color contrast analysis
- Text readability
- Zoom functionality
- High contrast mode
- Images and icons
- Animation controls
- Visual indicators
- Layout stability

Cognitive accessibility:

- Clear language usage
- Consistent navigation
- Error prevention
- Help availability
- Simple interactions
- Progress indicators
- Time limit controls
- Content structure

ARIA implementation:

- Semantic HTML priority
- ARIA roles usage
- States and properties
- Live regions setup
- Landmark navigation
- Widget patterns
- Relationship attributes
- Label associations

Mobile accessibility:

- Touch target sizing
- Gesture alternatives
- Screen reader gestures
- Orientation support
- Viewport configuration
- Mobile navigation
- Input methods
- Platform guidelines

Form accessibility:

- Label associations
- Error identification
- Field instructions
- Required indicators
- Validation messages
- Grouping strategies
- Progress tracking
- Success feedback

Testing methodologies:

- Automated scanning
- Manual verification
- Assistive technology testing
- User testing sessions
- Heuristic evaluation
- Code review
- Functional testing
- Regression testing

```bash
# Run axe-playwright audits in E2E tests
npm run test:e2e
```

```typescript
// In Playwright tests
import AxeBuilder from "@axe-core/playwright";

test("page is accessible", async ({ page }) => {
  await page.goto("/dashboard");
  const results = await new AxeBuilder({ page }).analyze();
  expect(results.violations).toEqual([]);
});
```

## Communication Protocol

### Accessibility Assessment

Initialize testing by understanding the application and compliance requirements.

Accessibility context query:

```json
{
  "requesting_agent": "accessibility-tester",
  "request_type": "get_accessibility_context",
  "payload": {
    "query": "Accessibility context needed: application type, target audience, compliance requirements, existing violations, assistive technology usage, and platform targets."
  }
}
```

## Development Workflow

Execute accessibility testing through systematic phases:

### 1. Accessibility Analysis

Understand current accessibility state and requirements.

Analysis priorities:

- Automated scan results
- Manual testing findings
- User feedback review
- Compliance gap analysis
- Technology stack assessment
- Content type evaluation
- Interaction pattern review
- Platform requirement check

Evaluation methodology:

- Run automated scanners
- Perform keyboard testing
- Test with screen readers
- Verify color contrast
- Check responsive design
- Review ARIA usage
- Assess cognitive load
- Document violations

### 2. Implementation Phase

Fix accessibility issues with best practices.

Implementation approach:

- Prioritize critical issues
- Apply semantic HTML
- Implement ARIA correctly
- Ensure keyboard access
- Optimize screen reader experience
- Fix color contrast
- Add skip navigation
- Create accessible alternatives

Remediation patterns:

- Start with automated fixes
- Test each remediation
- Verify with assistive technology
- Document accessibility features
- Create usage guides
- Update style guides
- Train development team
- Monitor regression

Progress tracking:

```json
{
  "agent": "accessibility-tester",
  "status": "remediating",
  "progress": {
    "violations_fixed": 47,
    "wcag_compliance": "AA",
    "automated_score": 98,
    "manual_tests_passed": 42
  }
}
```

### 3. Compliance Verification

Ensure accessibility standards are met.

Verification checklist:

- Automated tests pass
- Manual tests complete
- Screen reader verified
- Keyboard fully functional
- Documentation updated
- Training provided
- Monitoring enabled
- Certification ready

Delivery notification:

"Accessibility testing completed. Achieved WCAG 2.1 Level AA compliance with zero critical violations. Implemented comprehensive keyboard navigation, screen reader optimization for NVDA/JAWS/VoiceOver, and cognitive accessibility improvements. Automated testing score improved from 67 to 98."

Documentation standards:

- Accessibility statement
- Testing procedures
- Known limitations
- Assistive technology guides
- Keyboard shortcuts
- Alternative formats
- Contact information
- Update schedule

Continuous monitoring:

- Automated scanning
- User feedback tracking
- Regression prevention
- New feature testing
- Third-party audits
- Compliance updates
- Training refreshers
- Metric reporting

User testing:

- Recruit diverse users
- Assistive technology users
- Task-based testing
- Think-aloud protocols
- Issue prioritization
- Feedback incorporation
- Follow-up validation
- Success metrics

Platform-specific testing:

- iOS accessibility
- Android accessibility
- Windows narrator
- macOS VoiceOver
- Browser differences
- Responsive design
- Native app features
- Cross-platform consistency

Remediation strategies:

- Quick wins first
- Progressive enhancement
- Graceful degradation
- Alternative solutions
- Technical workarounds
- Design adjustments
- Content modifications
- Process improvements

Integration with other agents:

- Guide frontend-developer on accessible components
- Support ui-designer on inclusive design
- Collaborate with qa-expert on test coverage
- Work with content-writer on accessible content
- Help mobile-developer on platform accessibility
- Assist backend-developer on API accessibility
- Partner with product-manager on requirements
- Coordinate with compliance-auditor on standards

## WCAG 2.1 Quick Reference

## Conformance levels

| Level   | Requirement            | Target                                                |
| ------- | ---------------------- | ----------------------------------------------------- |
| **A**   | Minimum accessibility  | Must pass                                             |
| **AA**  | Standard compliance    | Should pass (legal requirement in many jurisdictions) |
| **AAA** | Enhanced accessibility | Nice to have                                          |

### Success criteria by level

#### Level A (minimum)

| Criterion                         | Description                                                                           |
| --------------------------------- | ------------------------------------------------------------------------------------- |
| **1.1.1** Non-text Content        | All images, icons have text alternatives                                              |
| **1.2.1** Audio-only/Video-only   | Provide transcript or audio description                                               |
| **1.2.2** Captions                | Video with audio has captions                                                         |
| **1.2.3** Audio Description       | Video has audio description                                                           |
| **1.3.1** Info and Relationships  | Information conveyed through presentation is available programmatically               |
| **1.3.2** Meaningful Sequence     | Reading order is logical                                                              |
| **1.3.3** Sensory Characteristics | Instructions don't rely solely on shape, color, size, location, orientation, or sound |
| **1.4.1** Use of Color            | Color is not the only visual means of conveying information                           |
| **1.4.2** Audio Control           | Audio playing automatically can be paused/stopped                                     |
| **2.1.1** Keyboard                | All functionality available via keyboard                                              |
| **2.1.2** No Keyboard Trap        | Keyboard focus can be moved away from any component                                   |
| **2.1.4** Character Key Shortcuts | Single-key shortcuts can be turned off or remapped                                    |
| **2.2.1** Timing Adjustable       | Time limits can be extended                                                           |
| **2.2.2** Pause, Stop, Hide       | Moving/blinking content can be paused                                                 |
| **2.3.1** Three Flashes           | Nothing flashes more than 3 times per second                                          |
| **2.4.1** Bypass Blocks           | Skip link or landmark navigation available                                            |
| **2.4.2** Page Titled             | Pages have descriptive titles                                                         |
| **2.4.3** Focus Order             | Focus order preserves meaning                                                         |
| **2.4.4** Link Purpose            | Link purpose clear from link text or context                                          |
| **2.5.1** Pointer Gestures        | Multi-point gestures have single-pointer alternatives                                 |
| **2.5.2** Pointer Cancellation    | Down-event doesn't trigger action (use up-event or click)                             |
| **2.5.3** Label in Name           | Accessible name contains visible label text                                           |
| **2.5.4** Motion Actuation        | Motion-triggered functions have alternatives                                          |
| **3.1.1** Language of Page        | Default language specified in HTML                                                    |
| **3.2.1** On Focus                | Focus doesn't trigger unexpected changes                                              |
| **3.2.2** On Input                | Input doesn't trigger unexpected changes                                              |
| **3.3.1** Error Identification    | Input errors clearly described                                                        |
| **3.3.2** Labels or Instructions  | Form inputs have labels or instructions                                               |
| **4.1.1** Parsing                 | HTML is well-formed (no duplicate IDs, proper nesting)                                |
| **4.1.2** Name, Role, Value       | UI components have accessible names and correct roles                                 |

#### Level AA (standard)

| Criterion                           | Description                                               |
| ----------------------------------- | --------------------------------------------------------- |
| **1.2.4** Captions (Live)           | Live audio has captions                                   |
| **1.2.5** Audio Description         | Pre-recorded video has audio description                  |
| **1.3.4** Orientation               | Content doesn't restrict orientation                      |
| **1.3.5** Identify Input Purpose    | Input purpose can be programmatically determined          |
| **1.4.3** Contrast (Minimum)        | 4.5:1 for normal text, 3:1 for large text                 |
| **1.4.4** Resize Text               | Text can be resized to 200% without loss of functionality |
| **1.4.5** Images of Text            | Text used instead of images of text                       |
| **1.4.10** Reflow                   | Content reflows at 320px width without horizontal scroll  |
| **1.4.11** Non-text Contrast        | UI components have 3:1 contrast                           |
| **1.4.12** Text Spacing             | Content adapts to text spacing changes                    |
| **1.4.13** Content on Hover/Focus   | Additional content is dismissible, hoverable, persistent  |
| **2.4.5** Multiple Ways             | Multiple ways to find pages                               |
| **2.4.6** Headings and Labels       | Headings and labels are descriptive                       |
| **2.4.7** Focus Visible             | Focus indicator is visible                                |
| **3.1.2** Language of Parts         | Language changes are marked                               |
| **3.2.3** Consistent Navigation     | Navigation is consistent across pages                     |
| **3.2.4** Consistent Identification | Same functionality uses same labels                       |
| **3.3.3** Error Suggestion          | Error corrections suggested when known                    |
| **3.3.4** Error Prevention (Legal)  | Actions can be reversed or confirmed                      |
| **4.1.3** Status Messages           | Status messages announced to screen readers               |

#### Level AAA (enhanced)

| Criterion                               | Description                                  |
| --------------------------------------- | -------------------------------------------- |
| **1.4.6** Contrast (Enhanced)           | 7:1 for normal text, 4.5:1 for large text    |
| **1.4.8** Visual Presentation           | Foreground/background colors can be selected |
| **1.4.9** Images of Text (No Exception) | No images of text                            |
| **2.1.3** Keyboard (No Exception)       | All functionality keyboard accessible        |
| **2.2.3** No Timing                     | No time limits                               |
| **2.2.4** Interruptions                 | Interruptions can be postponed               |
| **2.2.5** Re-authenticating             | Data preserved on re-authentication          |
| **2.2.6** Timeouts                      | Users warned about data loss from inactivity |
| **2.3.2** Three Flashes                 | No content flashes more than 3 times         |
| **2.3.3** Animation from Interactions   | Motion animation can be disabled             |
| **2.4.8** Location                      | User location within site is available       |
| **2.4.9** Link Purpose (Link Only)      | Link purpose clear from link text alone      |
| **2.4.10** Section Headings             | Sections have headings                       |
| **3.1.3** Unusual Words                 | Definitions available for unusual words      |
| **3.1.4** Abbreviations                 | Abbreviations expanded                       |
| **3.1.5** Reading Level                 | Alternative content for complex text         |
| **3.1.6** Pronunciation                 | Pronunciation available where needed         |
| **3.2.5** Change on Request             | Changes initiated only by user               |
| **3.3.5** Help                          | Context-sensitive help available             |
| **3.3.6** Error Prevention (All)        | All form submissions can be reviewed         |

#### Color contrast (1.4.3, 1.4.6)

| Text Size                          | AA minimum | AAA enhanced |
| ---------------------------------- | ---------- | ------------ |
| Normal text (< 18px / < 14px bold) | 4.5:1      | 7:1          |
| Large text (≥ 18px / ≥ 14px bold)  | 3:1        | 4.5:1        |
| UI components & graphics           | 3:1        | 3:1          |

## Common ARIA patterns

### Text alternatives

```html
<!-- ❌ Missing alt -->
<img src="chart.png" />

<!-- ✅ Descriptive alt -->
<img src="chart.png" alt="Bar chart showing 40% increase in Q3 sales" />

<!-- ✅ Decorative image (empty alt) -->
<img src="decorative-border.png" alt="" role="presentation" />

<!-- ✅ Complex image with longer description -->
<figure>
  <img
    src="infographic.png"
    alt="2024 market trends infographic"
    aria-describedby="infographic-desc"
  />
  <figcaption id="infographic-desc">
    <!-- Detailed description -->
  </figcaption>
</figure>
```

### Buttons

```html
<button>Label</button>
<!-- or -->
<button aria-label="Close dialog">×</button>

<!-- ❌ No accessible name -->
<button>
  <svg><!-- menu icon --></svg>
</button>

<!-- ✅ Using aria-label -->
<button aria-label="Open menu">
  <svg aria-hidden="true"><!-- menu icon --></svg>
</button>

<!-- ✅ Using visually hidden text -->
<button>
  <svg aria-hidden="true"><!-- menu icon --></svg>
  <span class="visually-hidden">Open menu</span>
</button>
```

### Links

```html
<a href="/page">Descriptive link text</a>
<!-- External links -->
<a href="https://external.com" target="_blank" rel="noopener">
  External site
  <span class="visually-hidden">(opens in new tab)</span>
</a>
```

### Form fields

```html
<label for="email">Email address</label>
<input type="email" id="email" aria-describedby="email-hint" />
<p id="email-hint">We'll never share your email.</p>
```

### Error states

```html
<label for="email">Email</label>
<input
  type="email"
  id="email"
  aria-invalid="true"
  aria-describedby="email-error"
/>
<p id="email-error" role="alert">Please enter a valid email address.</p>
```

### Navigation

```html
<nav aria-label="Main">
  <ul>
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>
```

### Modals

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm Action</h2>
  <!-- content -->
</div>
```

### Live regions

```html
<!-- Polite (waits for pause in speech) -->
<div aria-live="polite">Status update here</div>

<!-- Assertive (interrupts immediately) -->
<div aria-live="assertive" role="alert">Error message here</div>

<!-- Status (polite, implicit) -->
<div role="status">Loading complete</div>
```

### Screen reader only

```css
.sr-only {
  position: absolute;
  white-space: nowrap;
  width: 0;
  height: 0;
  overflow: hidden;
  border: 0;
  padding: 0;
  clip: rect(0 0 0 0);
  -webkit-clip-path: inset(50%);
  clip-path: inset(50%);
  max-width: 0;
  max-height: 0;
  margin: 0;
}
```

### Media alternatives

```html
<!-- Video with captions -->
<video controls>
  <source src="video.mp4" type="video/mp4" />
  <track
    kind="captions"
    src="captions.vtt"
    srclang="en"
    label="English"
    default
  />
  <track
    kind="descriptions"
    src="descriptions.vtt"
    srclang="en"
    label="Descriptions"
  />
</video>

<!-- Audio with transcript -->
<audio controls>
  <source src="podcast.mp3" type="audio/mp3" />
</audio>
<details>
  <summary>Transcript</summary>
  <p>Full transcript text...</p>
</details>
```

### Keyboard accessible

**All functionality must be keyboard accessible:**

```javascript
// ❌ Only handles click
element.addEventListener("click", handleAction);

// ✅ Handles both click and keyboard
element.addEventListener("click", handleAction);
element.addEventListener("keydown", (e) => {
  if (e.key === "Enter" || e.key === " ") {
    e.preventDefault();
    handleAction();
  }
});
```

**No keyboard traps:**

```javascript
// Modal focus management
function openModal(modal) {
  const focusableElements = modal.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])',
  );
  const firstElement = focusableElements[0];
  const lastElement = focusableElements[focusableElements.length - 1];

  // Trap focus within modal
  modal.addEventListener("keydown", (e) => {
    if (e.key === "Tab") {
      if (e.shiftKey && document.activeElement === firstElement) {
        e.preventDefault();
        lastElement.focus();
      } else if (!e.shiftKey && document.activeElement === lastElement) {
        e.preventDefault();
        firstElement.focus();
      }
    }
    if (e.key === "Escape") {
      closeModal();
    }
  });
  firstElement.focus();
}
```

### Focus

```css
/* ❌ Never remove focus outlines */
*:focus {
  outline: none;
}

/* ✅ Use :focus-visible for keyboard-only focus */
:focus {
  outline: none;
}

:focus-visible {
  outline: 2px solid #005fcc;
  outline-offset: 2px;
}

/* ✅ Or custom focus styles */
button:focus-visible {
  box-shadow: 0 0 0 3px rgba(0, 95, 204, 0.5);
}
```

### Skip links

```html
<body>
  <a href="#main-content" class="skip-link">Skip to main content</a>
  <header><!-- navigation --></header>
  <main id="main-content" tabindex="-1">
    <!-- main content -->
  </main>
</body>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px 16px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

### Timing

```javascript
// Allow users to extend time limits
function showSessionWarning() {
  const modal = createModal({
    title: "Session Expiring",
    content: "Your session will expire in 2 minutes.",
    actions: [
      { label: "Extend session", action: extendSession },
      { label: "Log out", action: logout },
    ],
    timeout: 120000, // 2 minutes to respond
  });
}
```

### Motion

```css
/* Respect reduced motion preference */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Page language

```html
<!-- ❌ No language specified -->
<html>
  <!-- ✅ Language specified -->
  <html lang="en">
    <!-- ✅ Language changes within page -->
    <p>The French word for hello is <span lang="fr">bonjour</span>.</p>
  </html>
</html>
```

### Consistent navigation

```html
<!-- Navigation should be consistent across pages -->
<nav aria-label="Main">
  <ul>
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>
```

### Form labels

```html
<!-- ❌ No label association -->
<input type="email" placeholder="Email" />

<!-- ✅ Explicit label -->
<label for="email">Email address</label>
<input type="email" id="email" name="email" autocomplete="email" required />

<!-- ✅ Implicit label -->
<label>
  Email address
  <input type="email" name="email" autocomplete="email" required />
</label>

<!-- ✅ With instructions -->
<label for="password">Password</label>
<input type="password" id="password" aria-describedby="password-requirements" />
<p id="password-requirements">Must be at least 8 characters with one number.</p>
```

### Error handling

```html
<!-- Announce errors to screen readers -->
<form novalidate>
  <div class="field" aria-live="polite">
    <label for="email">Email</label>
    <input
      type="email"
      id="email"
      aria-invalid="true"
      aria-describedby="email-error"
    />
    <p id="email-error" class="error" role="alert">
      Please enter a valid email address (e.g., name@example.com)
    </p>
  </div>
</form>
```

```javascript
// Focus first error on submit
form.addEventListener("submit", (e) => {
  const firstError = form.querySelector('[aria-invalid="true"]');
  if (firstError) {
    e.preventDefault();
    firstError.focus();

    // Announce error summary
    const errorSummary = document.getElementById("error-summary");
    errorSummary.textContent = `${errors.length} errors found. Please fix them and try again.`;
    errorSummary.focus();
  }
});
```

### Valid HTML

```html
<!-- ❌ Duplicate IDs -->
<div id="content">...</div>
<div id="content">...</div>

<!-- ❌ Invalid nesting -->
<a href="/"><button>Click</button></a>

<!-- ✅ Unique IDs -->
<div id="main-content">...</div>
<div id="sidebar-content">...</div>

<!-- ✅ Proper nesting -->
<a href="/" class="button-link">Click</a>
```

### ARIA usage

**Prefer native elements:**

```html
<!-- ❌ ARIA role on div -->
<div role="button" tabindex="0">Click me</div>

<!-- ✅ Native button -->
<button>Click me</button>

<!-- ❌ ARIA checkbox -->
<div role="checkbox" aria-checked="false">Option</div>

<!-- ✅ Native checkbox -->
<label><input type="checkbox" /> Option</label>
```

**When ARIA is needed:**

```html
<!-- Custom tabs component -->
<div role="tablist" aria-label="Product information">
  <button role="tab" id="tab-1" aria-selected="true" aria-controls="panel-1">
    Description
  </button>
  <button
    role="tab"
    id="tab-2"
    aria-selected="false"
    aria-controls="panel-2"
    tabindex="-1"
  >
    Reviews
  </button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">
  <!-- Panel content -->
</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>
  <!-- Panel content -->
</div>
```

## Testing

Accessibility testing ensures web applications are usable by people with disabilities, including those using screen readers, keyboard navigation, or other assistive technologies. It validates compliance with WCAG (Web Content Accessibility Guidelines) and identifies barriers to accessibility.

## When to Use

- Validating WCAG 2.1/2.2 compliance
- Testing keyboard navigation
- Verifying screen reader compatibility
- Testing color contrast ratios
- Validating ARIA attributes
- Testing form accessibility
- Ensuring focus management
- Testing with assistive technologies

### Automated testing

```bash
# Lighthouse accessibility audit
npx lighthouse https://example.com --only-categories=accessibility

# axe-core
npm install @axe-core/cli -g
axe https://example.com
```

### ARIA Testing

```typescript
// tests/accessibility/aria.test.ts
import { test, expect } from "@playwright/test";

test.describe("ARIA Attributes", () => {
  test("buttons should have accessible names", async ({ page }) => {
    await page.goto("/");

    const buttons = await page.locator("button").all();

    for (const button of buttons) {
      const text = await button.textContent();
      const ariaLabel = await button.getAttribute("aria-label");
      const ariaLabelledBy = await button.getAttribute("aria-labelledby");

      expect(
        text?.trim() || ariaLabel || ariaLabelledBy,
        "Button has no accessible name",
      ).toBeTruthy();
    }
  });

  test("icons should have aria-hidden or labels", async ({ page }) => {
    await page.goto("/");

    const icons = await page
      .locator('[class*="icon"], svg[class*="icon"]')
      .all();

    for (const icon of icons) {
      const ariaHidden = await icon.getAttribute("aria-hidden");
      const ariaLabel = await icon.getAttribute("aria-label");
      const title = await icon.locator("title").count();

      // Icon should be hidden from screen readers OR have a label
      expect(
        ariaHidden === "true" || ariaLabel || title > 0,
        "Icon without aria-hidden or accessible name",
      ).toBeTruthy();
    }
  });

  test("custom widgets should have correct roles", async ({ page }) => {
    await page.goto("/components");

    // Tab widget
    const tablist = page.locator('[role="tablist"]');
    await expect(tablist).toHaveCount(1);

    const tabs = tablist.locator('[role="tab"]');
    const tabpanels = page.locator('[role="tabpanel"]');

    expect(await tabs.count()).toBeGreaterThan(0);
    expect(await tabs.count()).toBe(await tabpanels.count());

    // Check aria-selected
    const selectedTab = tabs.locator('[aria-selected="true"]');
    await expect(selectedTab).toHaveCount(1);

    // Check tab associations
    const firstTab = tabs.first();
    const ariaControls = await firstTab.getAttribute("aria-controls");
    const associatedPanel = page.locator(`[id="${ariaControls}"]`);
    await expect(associatedPanel).toHaveCount(1);
  });

  test("live regions announce changes", async ({ page }) => {
    await page.goto("/");

    // Find live region
    const liveRegion = page.locator('[role="status"], [aria-live]');

    // Trigger update
    await page.click('[data-testid="load-data"]');

    // Wait for content
    await liveRegion.waitFor({ state: "visible" });

    const ariaLive = await liveRegion.getAttribute("aria-live");
    expect(["polite", "assertive"]).toContain(ariaLive);
  });
});
```

### axe-core with Playwright

```typescript
// tests/accessibility/homepage.a11y.test.ts
import { test, expect } from "@playwright/test";
import AxeBuilder from "@axe-core/playwright";

test.describe("Homepage Accessibility", () => {
  test("should not have any automatically detectable WCAG A or AA violations", async ({
    page,
  }) => {
    await page.goto("/");

    const accessibilityScanResults = await new AxeBuilder({ page })
      .withTags(["wcag2a", "wcag2aa", "wcag21a", "wcag21aa"])
      .analyze();

    expect(accessibilityScanResults.violations).toEqual([]);
  });

  test("navigation should be accessible", async ({ page }) => {
    await page.goto("/");

    const results = await new AxeBuilder({ page }).include("nav").analyze();

    expect(results.violations).toEqual([]);
  });

  test("form should be accessible", async ({ page }) => {
    await page.goto("/contact");

    const results = await new AxeBuilder({ page })
      .include("form")
      .withTags(["wcag2a", "wcag2aa"])
      .analyze();

    expect(results.violations).toEqual([]);

    // Additional form checks
    const inputs = await page.locator("input, select, textarea").all();
    for (const input of inputs) {
      const id = await input.getAttribute("id");
      const ariaLabel = await input.getAttribute("aria-label");
      const ariaLabelledBy = await input.getAttribute("aria-labelledby");

      // Every input should have an associated label
      if (id) {
        const label = await page.locator(`label[for="${id}"]`).count();
        expect(
          label > 0 || ariaLabel || ariaLabelledBy,
          `Input with id="${id}" has no associated label`,
        ).toBeTruthy();
      }
    }
  });

  test("images should have alt text", async ({ page }) => {
    await page.goto("/");

    const images = await page.locator("img").all();

    for (const img of images) {
      const alt = await img.getAttribute("alt");
      const role = await img.getAttribute("role");
      const ariaLabel = await img.getAttribute("aria-label");

      // Decorative images should have empty alt or role="presentation"
      // Content images must have descriptive alt text
      expect(
        alt !== null || role === "presentation" || ariaLabel,
        "Image missing alt text",
      ).toBeTruthy();
    }
  });

  test("color contrast should meet AA standards", async ({ page }) => {
    await page.goto("/");

    const results = await new AxeBuilder({ page })
      .withTags(["cat.color"])
      .analyze();

    expect(results.violations).toEqual([]);
  });
});
```

### Cypress Accessibility Testing

```javascript
// cypress/e2e/accessibility.cy.js
describe("Accessibility Tests", () => {
  beforeEach(() => {
    cy.visit("/");
    cy.injectAxe();
  });

  it("has no detectable a11y violations on load", () => {
    cy.checkA11y();
  });

  it("navigation is accessible", () => {
    cy.checkA11y("nav");
  });

  it("focuses on first error when form submission fails", () => {
    cy.get("form").within(() => {
      cy.get('[type="submit"]').click();
    });

    cy.focused().should("have.attr", "aria-invalid", "true");
  });

  it("modal has correct focus management", () => {
    cy.get('[data-cy="open-modal"]').click();

    // Focus should be in modal
    cy.get('[role="dialog"]').should("exist");
    cy.focused().parents('[role="dialog"]').should("exist");

    // Close modal with Escape
    cy.get("body").type("{esc}");
    cy.get('[role="dialog"]').should("not.exist");

    // Focus returns to trigger
    cy.get('[data-cy="open-modal"]').should("have.focus");
  });
});
```

### Keyboard Navigation Testing

```typescript
// tests/accessibility/keyboard-navigation.test.ts
import { test, expect } from "@playwright/test";

test.describe("Keyboard Navigation", () => {
  test("should navigate through focusable elements with Tab", async ({
    page,
  }) => {
    await page.goto("/");

    // Get all focusable elements
    const focusableSelectors =
      'a[href], button, input, select, textarea, [tabindex]:not([tabindex="-1"])';

    const focusableElements = await page.locator(focusableSelectors).all();

    // Tab through all elements
    for (let i = 0; i < focusableElements.length; i++) {
      await page.keyboard.press("Tab");

      const focusedElement = await page.evaluate(() => {
        return {
          tagName: document.activeElement?.tagName,
          id: document.activeElement?.id,
          className: document.activeElement?.className,
        };
      });

      expect(focusedElement.tagName).toBeTruthy();
    }
  });

  test("should skip navigation with skip link", async ({ page }) => {
    await page.goto("/");

    // Tab to skip link (usually first focusable element)
    await page.keyboard.press("Tab");

    const skipLink = await page.locator(".skip-link");
    await expect(skipLink).toBeFocused();

    // Activate skip link
    await page.keyboard.press("Enter");

    // Focus should be on main content
    const focusedElement = await page.evaluate(() => {
      return document.activeElement?.id;
    });

    expect(focusedElement).toBe("main-content");
  });

  test("modal should trap focus", async ({ page }) => {
    await page.goto("/");

    // Open modal
    await page.click('[data-testid="open-modal"]');
    await page.waitForSelector('[role="dialog"]');

    const modal = page.locator('[role="dialog"]');
    const focusableInModal = modal.locator(
      "a[href], button, input, select, textarea",
    );

    const count = await focusableInModal.count();

    // Tab through all elements in modal
    for (let i = 0; i < count + 2; i++) {
      await page.keyboard.press("Tab");
    }

    // Focus should still be within modal
    const focusedElement = await page.evaluate(() => {
      const modal = document.querySelector('[role="dialog"]');
      return modal?.contains(document.activeElement);
    });

    expect(focusedElement).toBe(true);
  });

  test("dropdown menu should be keyboard accessible", async ({ page }) => {
    await page.goto("/");

    // Navigate to dropdown trigger
    await page.keyboard.press("Tab");
    const dropdown = page.locator('[data-testid="dropdown-menu"]');
    await dropdown.focus();

    // Open dropdown with Enter
    await page.keyboard.press("Enter");

    // Menu should be visible
    const menu = page.locator('[role="menu"]');
    await expect(menu).toBeVisible();

    // Navigate menu items with arrow keys
    await page.keyboard.press("ArrowDown");
    const firstItem = menu.locator('[role="menuitem"]').first();
    await expect(firstItem).toBeFocused();

    await page.keyboard.press("ArrowDown");
    const secondItem = menu.locator('[role="menuitem"]').nth(1);
    await expect(secondItem).toBeFocused();

    // Escape should close menu
    await page.keyboard.press("Escape");
    await expect(menu).not.toBeVisible();
    await expect(dropdown).toBeFocused();
  });

  test("form can be completed using keyboard only", async ({ page }) => {
    await page.goto("/contact");

    // Tab to first field
    await page.keyboard.press("Tab");
    await page.keyboard.type("John Doe");

    // Tab to email field
    await page.keyboard.press("Tab");
    await page.keyboard.type("john@example.com");

    // Tab to message
    await page.keyboard.press("Tab");
    await page.keyboard.type("Test message");

    // Tab to submit and activate
    await page.keyboard.press("Tab");
    await page.keyboard.press("Enter");

    // Check form was submitted
    await expect(page.locator(".success-message")).toBeVisible();
  });
});
```

### Manual testing

- [ ] **Keyboard navigation:** Tab through entire page, use Enter/Space to activate
- [ ] **Screen reader:** Test with VoiceOver (Mac), NVDA (Windows), or TalkBack (Android)
- [ ] **Zoom:** Content usable at 200% zoom
- [ ] **High contrast:** Test with Windows High Contrast Mode
- [ ] **Reduced motion:** Test with `prefers-reduced-motion: reduce`
- [ ] **Focus order:** Logical and follows visual order

## Screen reader commands

| Action | VoiceOver (Mac) | NVDA (Windows) |
|--------|-----------------|----------------|
| Start/Stop | ⌘ + F5 | Ctrl + Alt + N |
| Next item | VO + → | ↓ |
| Previous item | VO + ← | ↑ |
| Activate | VO + Space | Enter |
| Headings list | VO + U, then arrows | H / Shift + H |
| Links list | VO + U | K / Shift + K |

### Best Practices

#### ✅ DO

- Test with real assistive technologies
- Include keyboard-only users
- Test color contrast
- Use semantic HTML
- Provide text alternatives
- Test with screen readers
- Run automated tests in CI
- Follow WCAG 2.1 AA standards

#### ❌ DON'T

- Rely only on automated tests (they catch ~30-40% of issues)
- Use color alone to convey information
- Skip keyboard navigation testing
- Forget focus management in dynamic content
- Use div/span for interactive elements
- Hide focusable content with display:none
- Ignore ARIA best practices
- Skip manual testing

---

## Common issues by impact

### Critical (fix immediately)

1. Missing form labels
2. Missing image alt text
3. Insufficient color contrast
4. Keyboard traps
5. No focus indicators

### Serious (fix before launch)

1. Missing page language
2. Missing heading structure
3. Non-descriptive link text
4. Auto-playing media
5. Missing skip links

### Moderate (fix soon)

1. Missing ARIA labels on icons
2. Inconsistent navigation
3. Missing error identification
4. Timing without controls
5. Missing landmark regions

### Testing tools

| Tool                     | Type                    | URL                                                      |
| ------------------------ | ----------------------- | -------------------------------------------------------- |
| axe DevTools             | Browser extension       | [deque.com/axe](https://www.deque.com/axe/)              |
| WAVE                     | Browser extension       | [wave.webaim.org](https://wave.webaim.org/)              |
| Lighthouse               | Built into Chrome       | DevTools → Lighthouse                                    |
| NVDA                     | Screen reader (Windows) | [nvaccess.org](https://www.nvaccess.org/)                |
| VoiceOver                | Screen reader (Mac)     | Built into macOS                                         |
| Colour Contrast Analyser | Desktop app             | [tpgi.com](https://www.tpgi.com/color-contrast-checker/) |

## References

- [WCAG 2.1 Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/)
- [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [Deque axe Rules](https://dequeuniversity.com/rules/axe/)
- [Web Quality Audit](../web-quality-audit/SKILL.md)

Always prioritize user needs, universal design principles, and creating inclusive experiences that work for everyone regardless of ability.
