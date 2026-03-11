---
name: storybook-story-writing
description: "Use this agent when you need to create or modify Storybook stories for components using Template pattern with detailed documentation and comprehensive argTypes."
tools: Read, Write, Edit, Bash, Glob, Grep
model: sonnet
---

# Storybook

## Story Writing

Write well-structured, maintainable Storybook stories using the Template pattern (CSF2) with comprehensive documentation, categorized controls, and rich metadata.

## Story Structure Pattern

Our stories follow a consistent pattern with these key elements:

1. **External documentation** imported from `.doc.ts` files
2. **Template function** with hooks and refs when needed
3. **Rich metadata** including status badges and Figma links
4. **Categorized argTypes** (Props, Attributes, Events)
5. **Conditional controls** using `if` conditions
6. **Story variants** extending base args with spread operator

## Complete Story Example

```typescript
import { useRef } from 'react';
import { fn } from '@storybook/test';
import { useShortcutHandler } from '@informads/theme-provider';
import {
  Button,
  BUTTON_HIERARCHY,
  BUTTON_ICON_POSITION,
  BUTTON_TARGET,
  BUTTON_TYPE,
  BUTTON_VARIANT,
} from '@informads/button';
import {
  BUTTON_DESCRIPTION,
  BUTTON_DEFAULT_STORY_DESCRIPTION,
  BUTTON_AS_LINK_STORY_DESCRIPTION,
} from './Button.doc';
import { ICON_NAMES } from '@informads/icon';

const BUTTON_ICON_OPTIONS = [null, ...ICON_NAMES];

export default {
  title: 'Componentes/Acciones/Button',
  component: Button,
  parameters: {
    docs: {
      description: {
        component: BUTTON_DESCRIPTION,
      },
    },
    status: {
      type: 'stable', // beta | releaseCandidate | stable | deprecated
      url: '?path=/docs/componentes-status-de-componentes--docs#leyenda-de-estados',
    },
    design: {
      type: 'figma',
      url: 'https://www.figma.com/design/...',
    },
  },
  argTypes: {
    // ... detailed argTypes
  },
};

const Template = (args) => {
  const buttonRef = useRef();
  useShortcutHandler(args, buttonRef, args.onClick);

  const keyshortcut =
    args.shortcutCustom ||
    (args.shortcut ? args.children.trim().charAt(0).toLowerCase() : undefined);

  return <Button ref={buttonRef} aria-keyshortcuts={keyshortcut} {...args} />;
};

export const Default = Template.bind({});
Default.args = {
  children: 'Clic aquí',
  icon: 'chevron-down',
  // ... more args
};

Default.parameters = {
  docs: {
    description: {
      story: BUTTON_DEFAULT_STORY_DESCRIPTION,
    },
  },
};
```

## Key write Concepts

### 1. External Documentation Pattern

Always separate documentation into `.doc.ts` files for maintainability:

```typescript
// Button.doc.ts
export const BUTTON_DESCRIPTION = `
El componente Button permite crear botones interactivos con diferentes estilos,
jerarquías y configuraciones, incluyendo soporte para íconos y atajos de teclado.
`;

export const BUTTON_DEFAULT_STORY_DESCRIPTION = `
Configuración por defecto del botón con todas las opciones disponibles.
`;

export const BUTTON_AS_LINK_STORY_DESCRIPTION = `
Botón que funciona como enlace, puede ser interno (to) o externo (href).
`;
```

```typescript
// Button.stories.tsx
import {
  BUTTON_DESCRIPTION,
  BUTTON_DEFAULT_STORY_DESCRIPTION,
  BUTTON_AS_LINK_STORY_DESCRIPTION,
} from './Button.doc';

export default {
  // ...
  parameters: {
    docs: {
      description: {
        component: BUTTON_DESCRIPTION,
      },
    },
  },
};
```

### 2. Metadata Parameters

#### Status Badge

Indicate component maturity level:

```typescript
parameters: {
  status: {
    type: 'stable', // beta | releaseCandidate | stable | deprecated
    url: '?path=/docs/componentes-status-de-componentes--docs#leyenda-de-estados',
  },
}
```

#### Design Links

Link to Figma designs:

```typescript
parameters: {
  design: {
    type: 'figma',
    url: 'https://www.figma.com/design/uwOPAnp7z55IS2ADjVOH1r/Informa-Design-System?node-id=1-8&m=dev',
  },
}
```

### 3. ArgTypes Categorization

Organize argTypes into logical categories:

```typescript
argTypes: {
  // Props Category
  children: {
    control: 'text',
    table: { category: 'Props' },
    description: 'Texto que se mostrará en el botón',
  },
  variant: {
    control: { type: 'select' },
    options: BUTTON_VARIANT,
    table: { category: 'Props' },
    description: 'Estilo visual del botón',
  },

  // Attributes Category
  className: {
    control: 'text',
    table: { category: 'Attributes' },
    description: 'Clases personalizadas',
  },
  disabled: {
    control: 'boolean',
    table: { category: 'Attributes' },
    description: 'Deshabilita el botón',
  },

  // Events Category
  onClick: {
    action: 'clicked',
    table: { category: 'Events' },
    description: 'Función al hacer clic',
  },
}
```

### 4. Conditional Controls

Use `if` conditions to show/hide controls based on other arg values:

```typescript
argTypes: {
  shortcut: {
    control: 'boolean',
    table: { category: 'Props' },
    description: 'Habilita marcador de atajo automático',
  },
  shortcutCustom: {
    control: 'text',
    table: { category: 'Props' },
    description: 'Marcador de atajo personalizado',
    if: { arg: 'shortcut', eq: false }, // Show only when shortcut is false
  },
  href: {
    name: 'href (Only when is a link)',
    control: 'text',
    table: { category: 'Attributes' },
    description: 'URL del enlace',
    if: { arg: 'to', eq: '' }, // Show only when 'to' is empty
  },
  target: {
    name: 'target (Only when is a link)',
    control: { type: 'select' },
    options: BUTTON_TARGET,
    table: { category: 'Attributes' },
    description: 'Target del enlace',
    if: { arg: 'href', neq: '' }, // Show only when 'href' is not empty
  },
}
```

**Conditional operators:**

- `eq` - equals
- `neq` - not equals
- `truthy` - truthy value
- `exists` - property exists

### 5. Template Pattern with Hooks

Use Template function when you need refs, hooks, or custom logic:

```typescript
const Template = (args) => {
  const buttonRef = useRef();
  useShortcutHandler(args, buttonRef, args.onClick);

  const keyshortcut =
    args.shortcutCustom ||
    (args.shortcut ? args.children.trim().charAt(0).toLowerCase() : undefined);

  return <Button ref={buttonRef} aria-keyshortcuts={keyshortcut} {...args} />;
};

export const Default = Template.bind({});
Default.args = {
  children: 'Clic aquí',
  onClick: fn(), // Use fn() from @storybook/test for actions
};
```

### 6. Story Variants Pattern

Extend base story with spread operator:

```typescript
export const Default = Template.bind({});
Default.args = {
  children: 'Clic aquí',
  icon: 'chevron-down',
  type: BUTTON_TYPE[0],
  variant: BUTTON_VARIANT[0],
  hierarchy: BUTTON_HIERARCHY[0],
  disabled: false,
  onClick: fn(),
};

Default.parameters = {
  docs: {
    description: {
      story: BUTTON_DEFAULT_STORY_DESCRIPTION,
    },
  },
};

// Extend Default story
export const AsLink = Template.bind({});
AsLink.args = {
  ...Default.args, // Inherit all default args
  icon: 'external-link',
  children: 'Enlace como botón',
  href: 'https://www.informa.es/',
  target: BUTTON_TARGET[1],
};

AsLink.parameters = {
  docs: {
    description: {
      story: BUTTON_AS_LINK_STORY_DESCRIPTION,
    },
  },
};
```

## Best Practices

### 1. Use Constants for Options

```typescript
import { ICON_NAMES } from '@informads/icon';

const BUTTON_ICON_OPTIONS = [null, ...ICON_NAMES];

argTypes: {
  icon: {
    control: 'select',
    options: BUTTON_ICON_OPTIONS,
    table: { category: 'Props' },
    description: 'Ícono del botón',
  },
}
```

### 2. Document Every ArgType

Always provide clear Spanish descriptions:

```typescript
argTypes: {
  loading: {
    control: 'boolean',
    table: { category: 'Props' },
    description: 'Mostrar spinner de carga',
  },
  onlyIcon: {
    control: 'boolean',
    table: { category: 'Props' },
    description: 'Solo ícono, sin texto',
  },
}
```

### 3. Use Descriptive Story Names

```typescript
export const Default = Template.bind({});
export const AsLink = Template.bind({});
export const WithShortcutCustom = Template.bind({});
export const Loading = Template.bind({});
export const Disabled = Template.bind({});
export const OnlyIcon = Template.bind({});
```

### 4. Group Related Components

Use hierarchical titles:

```typescript
export default {
  title: 'Componentes/Acciones/Button', // Category/Subcategory/Component
  // or
  title: 'Componentes/Formularios/Input',
  title: 'Componentes/Navegación/Menu',
}
```

### 5. Use fn() for Event Handlers

```typescript
import { fn } from '@storybook/test';

Default.args = {
  onClick: fn(),
  onSubmit: fn(),
  onChange: fn(),
};
```

## Common Write Patterns

### Form Components

```typescript
const Template = (args) => {
  const inputRef = useRef();
  return <Input ref={inputRef} {...args} />;
};

export const Default = Template.bind({});
Default.args = {
  label: 'Nombre',
  placeholder: 'Ingrese su nombre',
  type: 'text',
  required: false,
  disabled: false,
  error: '',
  onChange: fn(),
};

export const WithError = Template.bind({});
WithError.args = {
  ...Default.args,
  error: 'Este campo es requerido',
};

export const Disabled = Template.bind({});
Disabled.args = {
  ...Default.args,
  disabled: true,
  value: 'Valor deshabilitado',
};
```

### Link/Button Components

```typescript
argTypes: {
  href: {
    name: 'href (Only when is a link)',
    control: 'text',
    table: { category: 'Attributes' },
    description: 'URL del enlace',
    if: { arg: 'to', eq: '' },
  },
  to: {
    name: 'to (Only when is a link)',
    control: 'text',
    table: { category: 'Attributes' },
    description: 'Ruta interna para enlaces de React Router',
    placeholder: 'Usar para enlaces internos con React Router',
  },
  target: {
    name: 'target (Only when is a link)',
    control: { type: 'select' },
    options: ['_self', '_blank', '_parent', '_top'],
    table: { category: 'Attributes' },
    if: { arg: 'href', neq: '' },
  },
  rel: {
    name: 'rel (Only when is a link)',
    control: 'text',
    table: { category: 'Attributes' },
    if: { arg: 'href', neq: '' },
  },
}
```

### Components with Icons

```typescript
import { ICON_NAMES } from '@informads/icon';

const ICON_OPTIONS = [null, ...ICON_NAMES];

argTypes: {
  icon: {
    control: 'select',
    options: ICON_OPTIONS,
    table: { category: 'Props' },
    description: 'Ícono del botón',
  },
  iconPosition: {
    control: { type: 'select' },
    options: ['left', 'right'],
    table: { category: 'Props' },
    description: 'Posición del ícono',
  },
  onlyIcon: {
    control: 'boolean',
    table: { category: 'Props' },
    description: 'Solo ícono, sin texto',
  },
}
```

### Accessibility Props

```typescript
argTypes: {
  'aria-label': {
    control: 'text',
    table: { category: 'Attributes' },
    description: 'aria-label, texto para mejorar la accesibilidad',
  },
  'aria-describedby': {
    control: 'text',
    table: { category: 'Attributes' },
    description: 'ID del elemento que describe el componente',
  },
  'aria-keyshortcuts': {
    control: 'text',
    table: { category: 'Attributes' },
    description: 'Atajo de teclado asignado',
  },
}
```

## Complete Story File Structure

```typescript
// 1. Imports
import { useRef } from 'react';
import { fn } from '@storybook/test';
import { Component, CONSTANTS } from '@informads/component';
import { DOCUMENTATION } from './Component.doc';

// 2. Option Arrays
const OPTIONS = [null, ...CONSTANTS];

// 3. Default Export (Meta)
export default {
  title: 'Componentes/Category/Component',
  component: Component,
  parameters: {
    docs: {
      description: { component: DOCUMENTATION },
    },
    status: {
      type: 'stable',
      url: '?path=/docs/...',
    },
    design: {
      type: 'figma',
      url: 'https://www.figma.com/...',
    },
  },
  argTypes: {
    // Props
    children: {
      control: 'text',
      table: { category: 'Props' },
      description: '...',
    },
    // Attributes
    className: {
      control: 'text',
      table: { category: 'Attributes' },
      description: '...',
    },
    // Events
    onClick: {
      action: 'clicked',
      table: { category: 'Events' },
      description: '...',
    },
  },
};

// 4. Template Function
const Template = (args) => {
  const ref = useRef();
  // Custom logic here
  return <Component ref={ref} {...args} />;
};

// 5. Story Variants
export const Default = Template.bind({});
Default.args = {
  // Base args
  onClick: fn(),
};
Default.parameters = {
  docs: {
    description: { story: DESCRIPTION },
  },
};

export const Variant = Template.bind({});
Variant.args = {
  ...Default.args,
  // Override specific args
};
Variant.parameters = {
  docs: {
    description: { story: VARIANT_DESCRIPTION },
  },
};
```

## Anti-Patterns

### ❌ Don't Inline Documentation

```typescript
// Bad
parameters: {
  docs: {
    description: {
      component: 'Este es un botón muy largo...',
    },
  },
}
```

```typescript
// Good
import { BUTTON_DESCRIPTION } from './Button.doc';

parameters: {
  docs: {
    description: {
      component: BUTTON_DESCRIPTION,
    },
  },
}
```

### ❌ Don't Skip Categories

```typescript
// Bad
argTypes: {
  onClick: {
    action: 'clicked',
    description: 'Click handler',
  },
}
```

```typescript
// Good
argTypes: {
  onClick: {
    action: 'clicked',
    table: { category: 'Events' },
    description: 'Función al hacer clic',
  },
}
```

### ❌ Don't Duplicate Args

```typescript
// Bad
export const Variant1 = Template.bind({});
Variant1.args = {
  children: 'Button',
  icon: 'check',
  variant: 'primary',
  disabled: false,
  loading: false,
};

export const Variant2 = Template.bind({});
Variant2.args = {
  children: 'Submit',
  icon: 'check',
  variant: 'primary',
  disabled: false,
  loading: false, // Duplicate values
};
```

```typescript
// Good
export const Default = Template.bind({});
Default.args = {
  children: 'Button',
  icon: 'check',
  variant: 'primary',
  disabled: false,
  loading: false,
};

export const Submit = Template.bind({});
Submit.args = {
  ...Default.args, // Reuse base args
  children: 'Submit',
};
```

### ❌ Don't Mix Spanish and English

```typescript
// Bad
argTypes: {
  children: {
    description: 'Button text',
  },
  disabled: {
    description: 'Deshabilita el botón',
  },
}
```

```typescript
// Good - All in Spanish
argTypes: {
  children: {
    description: 'Texto del botón',
  },
  disabled: {
    description: 'Deshabilita el botón',
  },
}
```

## Component Documentation

Create comprehensive, auto-generated component documentation using Storybook's autodocs feature, MDX pages, and JSDoc comments.

## Key Documentation Concepts

### Autodocs

Automatically generate documentation pages from stories:

```typescript
const meta = {
  title: 'Components/Button',
  component: Button,
  tags: ['autodocs'],  // Enable auto-documentation
  parameters: {
    docs: {
      description: {
        component: 'A versatile button component with multiple variants and sizes.',
      },
    },
  },
} satisfies Meta<typeof Button>;
```

### MDX Documentation

Create custom documentation pages with full control:

```mdx
import { Meta, Canvas, Story, Controls } from '@storybook/blocks';
import * as ButtonStories from './Button.stories';

<Meta of={ButtonStories} />

# Button Component

A versatile button component for user interactions.

## Usage

<Canvas of={ButtonStories.Primary} />

## Props

<Controls of={ButtonStories.Primary} />
```

### JSDoc Comments

Document component props for auto-extraction:

```typescript
interface ButtonProps {
  /**
   * The button label text
   */
  label: string;

  /**
   * Is this the principal call to action?
   * @default false
   */
  primary?: boolean;

  /**
   * Button size variant
   * @default 'medium'
   */
  size?: 'small' | 'medium' | 'large';
}
```

## Best Practices to document

### 1. Enable Autodocs

Add the `autodocs` tag to generate documentation automatically:

```typescript
const meta = {
  component: Button,
  tags: ['autodocs'],
  parameters: {
    docs: {
      description: {
        component: 'Button component with primary and secondary variants.',
      },
    },
  },
} satisfies Meta<typeof Button>;
```

### 2. Document Component Descriptions

Provide clear, concise component descriptions:

```typescript
const meta = {
  component: Tooltip,
  tags: ['autodocs'],
  parameters: {
    docs: {
      description: {
        component: `
Tooltip displays helpful information when users hover over an element.
Supports multiple placements and can be triggered by hover, click, or focus.

## Features
- Multiple placement options
- Customizable delay
- Accessible (ARIA compliant)
- Keyboard navigation support
        `.trim(),
      },
    },
  },
} satisfies Meta<typeof Tooltip>;
```

### 3. Document Story Variations

Add descriptions to individual stories:

```typescript
export const WithIcon: Story = {
  args: {
    label: 'Download',
    icon: 'download',
  },
  parameters: {
    docs: {
      description: {
        story: 'Buttons can include icons to enhance visual communication.',
      },
    },
  },
};

export const Loading: Story = {
  args: {
    loading: true,
    label: 'Processing...',
  },
  parameters: {
    docs: {
      description: {
        story: 'Loading state disables interaction and shows a spinner.',
      },
    },
  },
};
```

### 4. Use JSDoc for Type Documentation

Document props with JSDoc for rich documentation:

```typescript
interface CardProps {
  /**
   * Card title displayed at the top
   */
  title: string;

  /**
   * Optional subtitle below the title
   */
  subtitle?: string;

  /**
   * Card variant affecting visual style
   * @default 'default'
   */
  variant?: 'default' | 'outlined' | 'elevated';

  /**
   * Card content
   */
  children: React.ReactNode;

  /**
   * Called when card is clicked
   * @param event - The click event
   */
  onClick?: (event: React.MouseEvent) => void;

  /**
   * Elevation level (shadow depth)
   * @minimum 0
   * @maximum 5
   * @default 1
   */
  elevation?: number;
}
```

### 5. Create Usage Examples

Show realistic usage examples in MDX:

```mdx
import { Meta, Canvas, Story } from '@storybook/blocks';
import * as FormStories from './Form.stories';

<Meta of={FormStories} />

# Form Component

## Basic Usage

<Canvas of={FormStories.Default} />

## With Validation

<Canvas of={FormStories.WithValidation} />

```tsx
import { Form } from './Form';

function MyForm() {
  return (
    <Form
      onSubmit={(data) => console.log(data)}
      validationSchema={schema}
    >
      <Input name="email" label="Email" />
      <Button type="submit">Submit</Button>
    </Form>
  );
}
```

## Common Documentation Patterns

### Component Overview Page

```mdx
import { Meta, Canvas, Controls } from '@storybook/blocks';
import * as ButtonStories from './Button.stories';

<Meta of={ButtonStories} />

# Button

Buttons trigger actions and events throughout the application.

## Variants

### Primary

Use primary buttons for the main call-to-action.

<Canvas of={ButtonStories.Primary} />

### Secondary

Use secondary buttons for less important actions.

<Canvas of={ButtonStories.Secondary} />

## Props

<Controls of={ButtonStories.Primary} />

## Accessibility

- Keyboard accessible (Enter/Space to activate)
- Screen reader friendly
- Focus visible indicator
- Proper ARIA labels

## Best Practices

- Use clear, action-oriented labels
- Provide sufficient click target size (min 44×44px)
- Don't use more than one primary button per section
- Include loading states for async actions
```

### API Documentation

```typescript
const meta = {
  component: DataGrid,
  tags: ['autodocs'],
  parameters: {
    docs: {
      description: {
        component: 'Advanced data grid with sorting, filtering, and pagination.',
      },
    },
  },
  argTypes: {
    data: {
      description: 'Array of data objects to display',
      table: {
        type: { summary: 'Array<Record<string, any>>' },
      },
    },
    columns: {
      description: 'Column configuration',
      table: {
        type: { summary: 'ColumnDef[]' },
      },
    },
    onRowClick: {
      description: 'Callback fired when a row is clicked',
      table: {
        type: { summary: '(row: any, index: number) => void' },
      },
    },
  },
} satisfies Meta<typeof DataGrid>;
```

### Migration Guides

```mdx
import { Meta } from '@storybook/blocks';

<Meta title="Guides/Migration/v2 to v3" />

# Migration Guide: v2 → v3

## Breaking Changes

### Button Component

**Before (v2):**
```tsx
<Button type="primary">Click me</Button>
```

**After (v3):**

```tsx
<Button variant="primary">Click me</Button>
```

The `type` prop has been renamed to `variant` for consistency.

### Input Component

**Before (v2):**

```tsx
<Input error="Invalid email" />
```

**After (v3):**

```tsx
<Input error={{ message: "Invalid email" }} />
```

Error handling now uses an object to support additional metadata.

## New Features

### Icon Support

Buttons now support icons:

```tsx
<Button variant="primary" icon="check">
  Save
</Button>
```

### Design Tokens

Document design tokens and theming:

```mdx
import { Meta, ColorPalette, ColorItem, Typeset } from '@storybook/blocks';

<Meta title="Design System/Colors" />

# Color Palette

## Primary Colors

<ColorPalette>
  <ColorItem
    title="Primary"
    subtitle="Main brand color"
    colors={{ Primary: '#007bff' }}
  />
  <ColorItem
    title="Secondary"
    subtitle="Supporting color"
    colors={{ Secondary: '#6c757d' }}
  />
</ColorPalette>

## Typography

<Typeset
  fontSizes={[12, 14, 16, 20, 24, 32, 40, 48]}
  fontWeight={400}
  sampleText="The quick brown fox jumps over the lazy dog"
/>
```

## Advanced Patterns

### Inline Stories in MDX

```mdx
import { Meta, Story } from '@storybook/blocks';
import { Button } from './Button';

<Meta title="Components/Button/Examples" component={Button} />

# Button Examples

## Inline Story

<Story name="Custom">
  {() => {
    const [count, setCount] = React.useState(0);
    return (
      <Button onClick={() => setCount(count + 1)}>
        Clicked {count} times
      </Button>
    );
  }}
</Story>
```

### Code Snippets

```mdx
  import { Source } from '@storybook/blocks';

  <Meta title="Guides/Setup" />

  # Installation

  Install the component library:

  <Source
    language="bash"
    code={`
  npm install @company/ui-components
  # or
  yarn add @company/ui-components
    `}
  />

  Then import components:

  <Source
    language="tsx"
    code={`
  import { Button, Input, Form } from '@company/ui-components';

  function App() {
    return (
      <Form>
        <Input label="Email" />
        <Button>Submit</Button>
      </Form>
    );
  }
  `}
/>
```

## Related Skills

- **storybook-args-controls**: Advanced arg configuration and interactive controls
- **storybook-play-functions**: Automated interaction testing within stories
- **storybook-component-documentation**: Auto-generating component documentation
- **storybook-story-writing**: Creating well-structured stories
- **storybook-args-controls**: Configuring interactive controls for props
- **storybook-configuration**: Setting up Storybook with proper documentation addons
