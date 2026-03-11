---
name: ids-consumer-developer
description: "A guide for frontend developers on how to use InformAds Design System (IDS) components productively in React (.tsx) applications with full IntelliSense and type safety."
technologies: React, TypeScript, JSX, CSS Modules, Storybook
version: 1.5.0
---

# InformAds Design System - Consumer Guide

> **Audience:** Frontend developers building React applications (.tsx)
> **Objective:** Use IDS components productively with full IntelliSense support
> **Goal:** AI and IDE automatically detect properties and documentation

## 🎯 What this guide includes

### ✅ Correct Focus

- **Audience:** Frontend developers building React applications with TypeScript
- **Objective:** Use IDS components productively with complete IntelliSense
- **Goal:** AI detects properties and documentation automatically

### 💡 Key Features

- **Full IntelliSense** - Props autocomplete in VS Code
- **Type Safety** - TypeScript catches errors before runtime
- **Inline Documentation** - Docs appear on hover
- **Rapid Development** - Less time reading docs, more coding

### 📋 Content Included

- ⚡ Quick setup (5 minutes)
- 🧩 Main component examples (Button, InputField, Layout)
- 📦 Complete list of available components
- 🎨 Theme configuration
- 🔍 Real-world examples (forms, tables, navigation)
- 🔧 Integration patterns (React Hook Form, React Router)
- 🚨 Common troubleshooting
- ⚡ VS Code productivity shortcuts

---

## Component-Driven Development (CDD)

IDS follows the **Component-Driven Development** methodology, an approach that builds UIs from the "bottom-up"—starting with basic components and progressively combining them to assemble complete screens.

### What is CDD?

**Component-Driven Development** is a methodology that anchors the building process around modular components. Components are **standardized and interchangeable building blocks** of UIs—think of LEGO bricks that can be separated and used to create new features.

### 4-Step CDD Process

1. **Build component by component**
    - Develop each component in isolation
    - Define its relevant states
    - Start small (Button, Input, Avatar)

2. **Combine components**
    - Compose small components together
    - Unlock new features
    - Gradually increase complexity (Form, Header, Table)

3. **Assemble pages**
    - Build pages by combining composite components
    - Use mock data to simulate edge cases
    - Create variations (Home, Settings, Profile)

4. **⚡ Integrate into the project**
    - Connect real data and business logic
    - Integrate with backend APIs
    - Deploy the full application

### CDD Benefits for IDS

| Benefit | Description |
|---|---|
| **Quality** | Verify that UIs work in different scenarios by building components in isolation |
| **Durability** | Identify bugs at the component level—less work and more precision |
| **⚡ Speed** | Assemble UIs faster by reusing existing components from the design system |
| **Efficiency** | Parallelize development by dividing the UI into discrete components among team members |

### CDD Tools in IDS

- **Development:** React, Storybook, CSS Modules
- **Documentation:** Component Story Format (CSF), MDX
- **Testing:** Cypress Component Testing, Jest
- **Design:** Figma, Design System tokens

### CDD References

- **[Component Driven Development](https://blog.hichroma.com/component-driven-development-ce1109d56c8e)** - Original methodology by Tom Coleman (2017)
- **[Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)** - Mental model by Brad Frost
- **[Component Driven](https://www.componentdriven.org/)** - Official resource for the methodology

## Table of Contents

1. [Component-Driven Development (CDD)](#-component-driven-development-cdd)
2. [System Architecture](#-system-architecture)
3. [Component Structure](#-component-structure)
4. [Development Patterns](#-development-patterns)
5. [Type System](#-type-system)
6. [Constants and Configuration](#⚙️-constants-and-configuration)
7. [Dependencies and Imports](#-dependencies-and-imports)
8. [Documentation and Stories](#-documentation-and-stories)
9. [Testing](#-testing)
10. [Component Categorization](#-component-categorization)
11. [Development Checklist](#✅-development-checklist)
12. [Useful Commands](#-useful-commands)

---

## ⚡ Quick Setup (5 minutes)

### 1. Install Dependencies

```bash
# Install the design system
npm install @informads/theme-provider @informads/button @informads/input-field

# Or install all components at once
npm install @informads/accordion @informads/anchor @informads/avatar @informads/badge @informads/blockquote @informads/breadcrumb @informads/bubble-text @informads/button @informads/button-agent @informads/card @informads/chatbot-button @informads/checkbox @informads/chip @informads/code @informads/copy-box @informads/datepicker @informads/divider @informads/icon @informads/image @informads/input-chat-ia @informads/input-field @informads/input-select @informads/label-form @informads/layout @informads/list @informads/list-definition @informads/list-nested-numeric @informads/main-header @informads/main-menu @informads/media-picker @informads/modal @informads/multi-select @informads/notification @informads/overlay @informads/pagination @informads/paragraph @informads/popover @informads/radio @informads/secondary-header @informads/side-panel @informads/spinner @informads/table @informads/tabs @informads/tag @informads/textarea @informads/think @informads/title @informads/tool-tips @informads/tools
```

### 2. Setup Theme Provider

```tsx
// App.tsx
import React from 'react';
import { ThemeProvider } from '@informads/theme-provider';

function App() {
  return (
    <ThemeProvider theme="Corporative" environment="Development">
      {/* Your application */}
    </ThemeProvider>
  );
}

export default App;
```

### 3. VS Code Extensions (Recommended)

Install these extensions for the best development experience:

- **TypeScript Importer** - Auto imports
- **Auto Rename Tag** - Sync tag renaming
- **IntelliSense for CSS class names** - CSS class autocomplete
- **Auto Close Tag** - Auto close JSX tags

---

### A Note on Compatibility

While IDS components are internally built using React with `.jsx`, they are **fully compatible** with your TypeScript projects (`.tsx`).

Each component includes a `index.d.ts` file that provides complete TypeScript definitions. This means you get all the benefits of a native TypeScript experience without any extra configuration:

- ✅ **Full Type Safety:** Catch errors at compile time.
- ✅ **Automatic IntelliSense:** Get property and documentation suggestions in your IDE.
- ✅ **Seamless Integration:** Use IDS components in your `.tsx` files as if they were native TypeScript components.

---

## 🧩 Component Examples

### Button Component

```tsx
import React from 'react';
import { Button } from '@informads/button';

// Basic usage with full IntelliSense
function MyComponent() {
  return (
    <Button
      variant="default"        // 🔍 Hover: Shows 'default' | 'danger' | 'neutral'
      hierarchy="primary"      // 🔍 Hover: Shows 'primary' | 'secondary' | 'tertiary'
      loading={false}          // 🔍 Hover: Shows boolean with description
      onClick={(e) => {        // 🔍 Full type safety on event
        console.log('Clicked!');
      }}
    >
      Click me
    </Button>
  );
}

// With icon
function ButtonWithIcon() {
  return (
    <Button
      icon="search"           // 🔍 IntelliSense shows available icons
      iconPosition="left"     // 🔍 'left' | 'right'
    >
      Search
    </Button>
  );
}

// Loading state
function LoadingButton() {
  const [loading, setLoading] = React.useState(false);

  const handleSubmit = async () => {
    setLoading(true);
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 2000));
    setLoading(false);
  };

  return (
    <Button
      loading={loading}       // 🔍 TypeScript ensures boolean
      disabled={loading}      // 🔍 Prevents double-clicks
      onClick={handleSubmit}
    >
      {loading ? 'Submitting...' : 'Submit'}
    </Button>
  );
}
```

### InputField Component

```tsx
import React from 'react';
import { InputField } from '@informads/input-field';

// Controlled input with validation
function FormExample() {
  const [email, setEmail] = React.useState('');
  const [error, setError] = React.useState('');

  const validateEmail = (value: string) => {
    const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
    setError(isValid ? '' : 'Please enter a valid email');
  };

  return (
    <InputField
      type="email"              // 🔍 Full HTML input type support
      value={email}             // 🔍 TypeScript ensures string
      onChange={(e) => {        // 🔍 Event typing automatic
        setEmail(e.target.value);
        validateEmail(e.target.value);
      }}
      placeholder="Enter your email"
      error={error}             // 🔍 Shows error styling when present
      helperText="We'll never share your email"
      required                  // 🔍 Adds asterisk automatically
    />
  );
}

// Password field with toggle
function PasswordField() {
  const [password, setPassword] = React.useState('');
  const [showPassword, setShowPassword] = React.useState(false);

  return (
    <InputField
      type={showPassword ? 'text' : 'password'}
      value={password}
      onChange={(e) => setPassword(e.target.value)}
      placeholder="Enter password"
      rightIcon={showPassword ? 'eye-off' : 'eye'}
      onRightIconClick={() => setShowPassword(!showPassword)}
    />
  );
}
```

### Layout Component

```tsx
import React from 'react';
import { Layout } from '@informads/layout';

// Responsive layout with IntelliSense
function AppLayout() {
  return (
    <Layout
      direction="column"        // 🔍 'row' | 'column' | 'row-reverse' | 'column-reverse'
      gap="medium"             // 🔍 'small' | 'medium' | 'large' | number
      padding="large"          // 🔍 Same options as gap
      align="stretch"          // 🔍 Full flexbox align-items options
      justify="space-between"  // 🔍 Full flexbox justify-content options
    >
      <header>Header content</header>
      <main style={{ flex: 1 }}>Main content</main>
      <footer>Footer content</footer>
    </Layout>
  );
}

// Card grid layout
function CardGrid({ items }: { items: any[] }) {
  return (
    <Layout
      direction="row"
      gap="medium"
      wrap                     // 🔍 Enables flex-wrap
      justify="flex-start"
    >
      {items.map((item, index) => (
        <div key={index} style={{ minWidth: '300px', flex: '1 1 300px' }}>
          {/* Card content */}
        </div>
      ))}
    </Layout>
  );
}
```

---

## 📦 Complete Component Library

### Navigation & Structure

- **Layout** - Flexbox layout container with responsive props
- **MainHeader** - Application header with navigation
- **SecondaryHeader** - Secondary navigation bar
- **MainMenu** - Sidebar navigation menu
- **Breadcrumb** - Navigation breadcrumb trail
- **Tabs** - Tab navigation component

### Form Controls

- **Button** - Primary action buttons with variants
- **InputField** - Text inputs with validation support
- **InputSelect** - Dropdown select component
- **MultiSelect** - Multi-option selection
- **Checkbox** - Single/group checkbox inputs
- **Radio** - Radio button groups
- **Textarea** - Multi-line text input
- **Datepicker** - Date selection component
- **LabelForm** - Form field labels

### Data Display

- **Table** - Data tables with sorting/pagination
- **List** - Simple and complex list displays
- **ListDefinition** - Definition/description lists
- **ListNestedNumeric** - Numbered hierarchical lists
- **Card** - Content cards container
- **Badge** - Status badges and labels
- **Tag** - Removable tags
- **Avatar** - User profile images
- **Icon** - Icon display component

### Feedback

- **Modal** - Dialog and modal windows
- **Notification** - Toast notifications
- **Overlay** - Full-screen overlays
- **SidePanel** - Sliding side panels
- **Popover** - Contextual popup content
- **ToolTips** - Hover help tooltips
- **Spinner** - Loading indicators

### Content

- **Title** - Heading text with hierarchy
- **Paragraph** - Body text content
- **Blockquote** - Quote text formatting
- **Code** - Code snippet display
- **Image** - Responsive image component
- **Divider** - Visual content separators

### Interactive

- **Accordion** - Collapsible content sections
- **Pagination** - Page navigation controls
- **Anchor** - Enhanced link component
- **CopyBox** - Copy-to-clipboard functionality
- **MediaPicker** - File/media selection

### Specialized

- **ChatbotButton** - AI chat interface trigger
- **InputChatIA** - AI chat input field
- **BubbleText** - Chat message bubbles
- **ButtonAgent** - Agent action buttons
- **Think** - Thinking/processing indicators
- **Tools** - Tool selection interfaces

---

## 🎨 Theme Configuration

### Available Themes

```tsx
import { ThemeProvider } from '@informads/theme-provider';

// Available theme options
type Theme = 'Corporative' | 'Light' | 'Dark' | 'Custom';
type Environment = 'Development' | 'Staging' | 'Production';

function App() {
  return (
    <ThemeProvider
      theme="Corporative"           // 🔍 IntelliSense shows all options
      environment="Development"     // 🔍 Changes debug indicators
    >
      <YourApp />
    </ThemeProvider>
  );
}
```

### Custom CSS Properties

IDS uses CSS custom properties that you can override:

```css
/* Custom theme variables */
:root {
  --color-primary: #1b4ad9;
  --color-danger: #d9124d;
  --bg-container: #ffffff;
  --bg-hover: #f1f4fd;
  --color-text-primary: #1a202c;
  --border-radius-default: 0.25rem;
  /* ... more variables available */
}
```

### Dark Mode Support

```tsx
function ThemeToggle() {
  const [isDark, setIsDark] = React.useState(false);

  return (
    <ThemeProvider theme={isDark ? 'Dark' : 'Light'}>
      <Button onClick={() => setIsDark(!isDark)}>
        Toggle {isDark ? 'Light' : 'Dark'} Mode
      </Button>
      <YourApp />
    </ThemeProvider>
  );
}
```

---

## 🔍 Real-World Examples

### Complete Form Example

```tsx
import React from 'react';
import { Button } from '@informads/button';
import { InputField } from '@informads/input-field';
import { InputSelect } from '@informads/input-select';
import { Checkbox } from '@informads/checkbox';
import { Layout } from '@informads/layout';
import { Card } from '@informads/card';

interface FormData {
  firstName: string;
  lastName: string;
  email: string;
  country: string;
  subscribe: boolean;
}

function ContactForm() {
  const [formData, setFormData] = React.useState<FormData>({
    firstName: '',
    lastName: '',
    email: '',
    country: '',
    subscribe: false,
  });

  const [errors, setErrors] = React.useState<Partial<FormData>>({});
  const [isSubmitting, setIsSubmitting] = React.useState(false);

  const countries = [
    { value: 'us', label: 'United States' },
    { value: 'ca', label: 'Canada' },
    { value: 'mx', label: 'Mexico' },
    // ... more countries
  ];

  const validateForm = (): boolean => {
    const newErrors: Partial<FormData> = {};

    if (!formData.firstName.trim()) {
      newErrors.firstName = 'First name is required';
    }

    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
      newErrors.email = 'Please enter a valid email';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) return;

    setIsSubmitting(true);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Form submitted:', formData);
      // Handle success
    } catch (error) {
      console.error('Submission error:', error);
      // Handle error
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <Card>
      <form onSubmit={handleSubmit}>
        <Layout direction="column" gap="large">

          {/* Name fields */}
          <Layout direction="row" gap="medium">
            <InputField
              label="First Name"
              value={formData.firstName}
              onChange={(e) => setFormData(prev => ({
                ...prev,
                firstName: e.target.value
              }))}
              error={errors.firstName}
              required
            />

            <InputField
              label="Last Name"
              value={formData.lastName}
              onChange={(e) => setFormData(prev => ({
                ...prev,
                lastName: e.target.value
              }))}
            />
          </Layout>

          {/* Email field */}
          <InputField
            type="email"
            label="Email Address"
            value={formData.email}
            onChange={(e) => setFormData(prev => ({
              ...prev,
              email: e.target.value
            }))}
            error={errors.email}
            helperText="We'll use this to send you updates"
            required
          />

          {/* Country select */}
          <InputSelect
            label="Country"
            value={formData.country}
            onChange={(value) => setFormData(prev => ({
              ...prev,
              country: value
            }))}
            options={countries}
            placeholder="Select your country"
          />

          {/* Newsletter checkbox */}
          <Checkbox
            checked={formData.subscribe}
            onChange={(checked) => setFormData(prev => ({
              ...prev,
              subscribe: checked
            }))}
            label="Subscribe to our newsletter"
          />

          {/* Submit button */}
          <Button
            type="submit"
            hierarchy="primary"
            loading={isSubmitting}
            disabled={isSubmitting}
          >
            {isSubmitting ? 'Submitting...' : 'Submit Form'}
          </Button>

        </Layout>
      </form>
    </Card>
  );
}
```

### Data Table with Actions

```tsx
import React from 'react';
import { Table } from '@informads/table';
import { Button } from '@informads/button';
import { Badge } from '@informads/badge';
import { Modal } from '@informads/modal';

interface User {
  id: number;
  name: string;
  email: string;
  role: string;
  status: 'active' | 'inactive' | 'pending';
}

function UserManagement() {
  const [users, setUsers] = React.useState<User[]>([
    { id: 1, name: 'John Doe', email: 'john@example.com', role: 'Admin', status: 'active' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'User', status: 'inactive' },
    // ... more users
  ]);

  const [selectedUser, setSelectedUser] = React.useState<User | null>(null);
  const [showDeleteModal, setShowDeleteModal] = React.useState(false);

  const columns = [
    {
      key: 'name',
      header: 'Name',
      render: (user: User) => user.name,
    },
    {
      key: 'email',
      header: 'Email',
      render: (user: User) => user.email,
    },
    {
      key: 'role',
      header: 'Role',
      render: (user: User) => user.role,
    },
    {
      key: 'status',
      header: 'Status',
      render: (user: User) => (
        <Badge
          variant={user.status === 'active' ? 'success' : 'warning'}
        >
          {user.status}
        </Badge>
      ),
    },
    {
      key: 'actions',
      header: 'Actions',
      render: (user: User) => (
        <div style={{ display: 'flex', gap: '8px' }}>
          <Button
            size="small"
            hierarchy="secondary"
            onClick={() => handleEdit(user)}
          >
            Edit
          </Button>
          <Button
            size="small"
            variant="danger"
            hierarchy="secondary"
            onClick={() => handleDelete(user)}
          >
            Delete
          </Button>
        </div>
      ),
    },
  ];

  const handleEdit = (user: User) => {
    // Navigate to edit page or open edit modal
    console.log('Edit user:', user);
  };

  const handleDelete = (user: User) => {
    setSelectedUser(user);
    setShowDeleteModal(true);
  };

  const confirmDelete = () => {
    if (selectedUser) {
      setUsers(users.filter(u => u.id !== selectedUser.id));
      setShowDeleteModal(false);
      setSelectedUser(null);
    }
  };

  return (
    <>
      <Table
        data={users}                    // 🔍 TypeScript infers User[] type
        columns={columns}               // 🔍 Column configuration with typing
        pagination={{                   // 🔍 Optional pagination config
          pageSize: 10,
          showSizeChanger: true,
        }}
        sortable                        // 🔍 Enable column sorting
        selectable                      // 🔍 Enable row selection
        onSelectionChange={(selected) => {
          console.log('Selected users:', selected);
        }}
      />

      {/* Delete confirmation modal */}
      <Modal
        open={showDeleteModal}
        onClose={() => setShowDeleteModal(false)}
        title="Confirm Delete"
      >
        <p>
          Are you sure you want to delete user "{selectedUser?.name}"?
          This action cannot be undone.
        </p>

        <div style={{ display: 'flex', gap: '12px', justifyContent: 'flex-end', marginTop: '24px' }}>
          <Button
            hierarchy="secondary"
            onClick={() => setShowDeleteModal(false)}
          >
            Cancel
          </Button>
          <Button
            variant="danger"
            onClick={confirmDelete}
          >
            Delete User
          </Button>
        </div>
      </Modal>
    </>
  );
}
```

### Navigation with React Router

```tsx
import React from 'react';
import { BrowserRouter, Routes, Route, useNavigate, useLocation } from 'react-router-dom';
import { MainHeader } from '@informads/main-header';
import { MainMenu } from '@informads/main-menu';
import { Breadcrumb } from '@informads/breadcrumb';
import { Layout } from '@informads/layout';

function AppNavigation() {
  const navigate = useNavigate();
  const location = useLocation();

  const menuItems = [
    {
      key: 'dashboard',
      label: 'Dashboard',
      icon: 'dashboard',
      path: '/dashboard',
    },
    {
      key: 'users',
      label: 'Users',
      icon: 'users',
      path: '/users',
      children: [
        { key: 'user-list', label: 'User List', path: '/users' },
        { key: 'user-roles', label: 'Roles', path: '/users/roles' },
      ],
    },
    {
      key: 'settings',
      label: 'Settings',
      icon: 'settings',
      path: '/settings',
    },
  ];

  const breadcrumbItems = React.useMemo(() => {
    const pathSegments = location.pathname.split('/').filter(Boolean);
    return pathSegments.map((segment, index) => ({
      label: segment.charAt(0).toUpperCase() + segment.slice(1),
      path: '/' + pathSegments.slice(0, index + 1).join('/'),
    }));
  }, [location.pathname]);

  return (
    <Layout direction="column" style={{ height: '100vh' }}>

      {/* Header */}
      <MainHeader
        logo="/logo.png"
        title="My Application"
        user={{
          name: "John Doe",
          avatar: "/avatar.jpg",
          role: "Admin"
        }}
        onLogout={() => {
          // Handle logout
          navigate('/login');
        }}
      />

      <Layout direction="row" style={{ flex: 1 }}>

        {/* Sidebar */}
        <MainMenu
          items={menuItems}
          activeKey={location.pathname}
          onItemClick={(item) => navigate(item.path)}
          collapsible
        />

        {/* Main content */}
        <Layout direction="column" style={{ flex: 1, padding: '24px' }}>

          {/* Breadcrumb */}
          <Breadcrumb
            items={breadcrumbItems}
            onItemClick={(item) => navigate(item.path)}
          />

          {/* Page content */}
          <main style={{ flex: 1, marginTop: '16px' }}>
            <Routes>
              <Route path="/dashboard" element={<DashboardPage />} />
              <Route path="/users" element={<UsersPage />} />
              <Route path="/users/roles" element={<RolesPage />} />
              <Route path="/settings" element={<SettingsPage />} />
            </Routes>
          </main>

        </Layout>
      </Layout>
    </Layout>
  );
}

// Root app with router
function App() {
  return (
    <BrowserRouter>
      <AppNavigation />
    </BrowserRouter>
  );
}
```

---

## 🔧 Integration Patterns

### React Hook Form Integration

```tsx
import React from 'react';
import { useForm, Controller } from 'react-hook-form';
import { InputField } from '@informads/input-field';
import { InputSelect } from '@informads/input-select';
import { Button } from '@informads/button';

interface FormData {
  firstName: string;
  email: string;
  country: string;
}

function ReactHookFormExample() {
  const {
    control,
    handleSubmit,
    formState: { errors, isSubmitting }
  } = useForm<FormData>();

  const onSubmit = async (data: FormData) => {
    console.log('Form data:', data);
    // Handle form submission
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>

      {/* Controlled InputField */}
      <Controller
        name="firstName"
        control={control}
        rules={{ required: 'First name is required' }}
        render={({ field, fieldState }) => (
          <InputField
            {...field}                    // 🔍 Spreads value, onChange, etc.
            label="First Name"
            error={fieldState.error?.message}
            required
          />
        )}
      />

      {/* Email with validation */}
      <Controller
        name="email"
        control={control}
        rules={{
          required: 'Email is required',
          pattern: {
            value: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
            message: 'Please enter a valid email',
          },
        }}
        render={({ field, fieldState }) => (
          <InputField
            {...field}
            type="email"
            label="Email Address"
            error={fieldState.error?.message}
            required
          />
        )}
      />

      {/* Select with options */}
      <Controller
        name="country"
        control={control}
        rules={{ required: 'Please select a country' }}
        render={({ field, fieldState }) => (
          <InputSelect
            {...field}
            label="Country"
            options={[
              { value: 'us', label: 'United States' },
              { value: 'ca', label: 'Canada' },
            ]}
            error={fieldState.error?.message}
            required
          />
        )}
      />

      <Button
        type="submit"
        loading={isSubmitting}
        disabled={isSubmitting}
      >
        Submit
      </Button>

    </form>
  );
}
```

### State Management with Zustand

```tsx
import { create } from 'zustand';
import { Notification } from '@informads/notification';

// Store definition
interface NotificationStore {
  notifications: Array<{
    id: string;
    type: 'success' | 'error' | 'warning' | 'info';
    message: string;
    duration?: number;
  }>;
  addNotification: (notification: Omit<NotificationStore['notifications'][0], 'id'>) => void;
  removeNotification: (id: string) => void;
}

const useNotificationStore = create<NotificationStore>((set) => ({
  notifications: [],
  addNotification: (notification) =>
    set((state) => ({
      notifications: [
        ...state.notifications,
        { ...notification, id: Date.now().toString() },
      ],
    })),
  removeNotification: (id) =>
    set((state) => ({
      notifications: state.notifications.filter((n) => n.id !== id),
    })),
));

// Notification manager component
function NotificationManager() {
  const { notifications, removeNotification } = useNotificationStore();

  return (
    <div style={{ position: 'fixed', top: '20px', right: '20px', zIndex: 1000 }}>
      {notifications.map((notification) => (
        <Notification
          key={notification.id}
          type={notification.type}          // 🔍 IntelliSense shows valid types
          message={notification.message}
          onClose={() => removeNotification(notification.id)}
          autoClose
          duration={notification.duration || 5000}
        />
      ))}
    </div>
  );
}

// Usage in components
function MyComponent() {
  const addNotification = useNotificationStore((state) => state.addNotification);

  const handleSuccess = () => {
    addNotification({
      type: 'success',
      message: 'Operation completed successfully!',
    });
  };

  const handleError = () => {
    addNotification({
      type: 'error',
      message: 'Something went wrong. Please try again.',
      duration: 8000, // Longer duration for errors
    });
  };

  return (
    <div>
      <Button onClick={handleSuccess}>Trigger Success</Button>
      <Button onClick={handleError}>Trigger Error</Button>
    </div>
  );
}
```

---

## 🚨 Common Troubleshooting

### TypeScript Issues

#### Problem: "Property does not exist on type"

```tsx
// ❌ Wrong - Missing import
<Button variant="primary">Click me</Button>

// ✅ Correct - Import the component
import { Button } from '@informads/button';
<Button variant="default">Click me</Button>  // Note: 'primary' is hierarchy, not variant
```

#### Problem: "Type 'string' is not assignable to type..."

```tsx
// ❌ Wrong - String instead of proper type
const variant = "primary";
<Button variant={variant}>Click me</Button>

// ✅ Correct - Use proper typing
const variant: ButtonVariant = "default";
<Button variant={variant}>Click me</Button>

// ✅ Or use as const
const variant = "default" as const;
<Button variant={variant}>Click me</Button>
```

### Style Issues

#### Problem: Styles not applying

```tsx
// ❌ Wrong - Missing ThemeProvider
function App() {
  return <Button>Click me</Button>;
}

// ✅ Correct - Wrap with ThemeProvider
import { ThemeProvider } from '@informads/theme-provider';

function App() {
  return (
    <ThemeProvider theme="Corporative">
      <Button>Click me</Button>
    </ThemeProvider>
  );
}
```

#### Problem: Custom CSS not overriding

```css
/* ❌ Wrong - Low specificity */
.my-button {
  background: red;
}

/* ✅ Correct - Use CSS custom properties */
.my-button {
  --ids-button-background: red;
}

/* ✅ Or increase specificity */
.my-app .my-button {
  background: red !important;
}
```

### Event Handling Issues

#### Problem: Event object type errors

```tsx
// ❌ Wrong - Generic event type
const handleClick = (e: Event) => {
  e.preventDefault(); // May cause type errors
};

// ✅ Correct - Specific React event type
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  e.preventDefault(); // Full type safety
};

// ✅ Or let TypeScript infer
<Button onClick={(e) => {
  e.preventDefault(); // TypeScript infers the correct type
}}>
  Click me
</Button>
```

### Performance Issues

#### Problem: Re-rendering on every prop change

```tsx
// ❌ Wrong - Object created on every render
<InputField
  style={{ width: '100%' }}  // New object each render
  onChange={(e) => setValue(e.target.value)}  // New function each render
/>

// ✅ Correct - Stable references
const inputStyle = { width: '100%' };
const handleChange = useCallback((e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
}, []);

<InputField
  style={inputStyle}
  onChange={handleChange}
/>
```

---

## ⚡ VS Code Productivity Shortcuts

### IntelliSense Tips

1. **Trigger IntelliSense manually:** `Ctrl + Space`
2. **Parameter hints:** `Ctrl + Shift + Space`
3. **Quick info (hover):** `Ctrl + K, Ctrl + I`
4. **Go to definition:** `F12`
5. **Show references:** `Shift + F12`

### Auto Import Configuration

Add to your VS Code settings.json:

```json
{
  "typescript.suggest.autoImports": true,
  "typescript.preferences.includePackageJsonAutoImports": "auto",
  "typescript.suggest.completeFunctionCalls": true
}
```

### Useful Code Snippets

Create these in VS Code (File → Preferences → Configure User Snippets → typescript/typescriptreact):

```json
{
  "IDS Component Import": {
    "prefix": "idsimport",
    "body": [
      "import { ${1:Component} } from '@informads/${2:component}';"
    ],
    "description": "Import IDS component"
  },

  "IDS Button": {
    "prefix": "idsbutton",
    "body": [
      "<Button",
      "  variant=\"${1|default,danger,neutral|}\"",
      "  hierarchy=\"${2|primary,secondary,tertiary|}\"",
      "  onClick={${3:handleClick}}",
      ">",
      "  ${4:Button Text}",
      "</Button>"
    ],
    "description": "IDS Button component"
  },

  "IDS InputField": {
    "prefix": "idsinput",
    "body": [
      "<InputField",
      "  type=\"${1|text,email,password,number|}\"",
      "  value={${2:value}}",
      "  onChange={${3:handleChange}}",
      "  placeholder=\"${4:Enter text}\"",
      "  ${5:required}",
      "/>"
    ],
    "description": "IDS InputField component"
  }
}
```

### Debugging Tips

1. **Component Props Inspection:**
   - Install React Developer Tools browser extension
   - Use `console.log` with component props for debugging
   - Enable TypeScript strict mode for better error catching

2. **Style Debugging:**
   - Use browser dev tools to inspect CSS custom properties
   - Check if ThemeProvider is properly wrapping components
   - Verify CSS import order

3. **Type Checking:**
   - Run `npx tsc --noEmit` to check types without compiling
   - Use `// @ts-expect-error` for intentional type violations
   - Enable `strict: true` in tsconfig.json

---

## 🎯 Goal Achieved

This guide is now focused on IDS component consumers, not creators. Frontend developers can now use components with:

- **Full TypeScript Support** - Complete type safety and IntelliSense
- **Automatic Documentation** - Hover hints and parameter suggestions
- **Rapid Development** - Less time reading docs, more time coding
- **Best Practices** - Real-world examples and integration patterns
- **Troubleshooting** - Common issues and solutions
- **Productivity** - VS Code shortcuts and configuration

The AI will now automatically detect properties, provide suggestions, and help with implementation using the InformAds Design System components.

---

## 📚 Additional Resources

- **Storybook Documentation:** [Component Examples & API](http://localhost:6006)
- **TypeScript Definitions:** Available in each component's `/types` folder
- **Source Code:** Available in `/packages/[component-name]/src`
- **Component Tests:** Examples in `/packages/[component-name]/test`

### Getting Help

1. **TypeScript Errors:** Check component type definitions in `/types/index.d.ts`
2. **Style Issues:** Verify ThemeProvider setup and CSS custom properties
3. **Integration Questions:** See real-world examples above
4. **Performance:** Use React DevTools and follow optimization patterns

---

**Last Updated:** March 10, 2026
**Version:** 1.5.0
**Maintained by:** InformAds Design System Team
