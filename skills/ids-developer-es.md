# InformAds Design System - Developer Skill Guide

> **Audiencia:** Desarrolladores trabajando con componentes IDS
> **Propósito:** Guía técnica completa para desarrollar, modificar y usar componentes
> **Actualización:** Marzo 2026

------

## Component Driven Development (CDD)

IDS sigue la metodología **Component Driven Development**, un enfoque que construye UIs desde "bottom up" - comenzando con componentes básicos y progresivamente combinándolos para ensamblar pantallas completas.

### ¿Qué es CDD?

**Component Driven Development** es una metodología que ancla el proceso de construcción en torno a componentes modulares. Los componentes son **bloques de construcción estandarizados e intercambiables** de las UIs - piensa en bloques LEGO que pueden separarse y usarse para crear nuevas características.

### Proceso CDD en 4 Pasos

1. **Construir componente por componente**
   - Desarrollar cada componente en aislamiento
   - Definir sus estados relevantes
   - Empezar pequeño (Button, Input, Avatar)

2. **Combinar componentes**
   - Componer componentes pequeños juntos
   - Desbloquear nuevas características
   - Incrementar complejidad gradualmente (Form, Header, Table)

3. **Ensamblar páginas**
   - Construir páginas combinando componentes compuestos
   - Usar datos mock para simular estados extremos
   - Crear variaciones (Home, Settings, Profile)

4. **⚡Integrar en proyecto**
   - Conectar datos reales y lógica de negocio
   - Integrar con APIs backend
   - Desplegar aplicación completa

### Beneficios CDD para IDS

| Beneficio | Descripción |
|-----------|-------------|
| ** Calidad** | Verificar que UIs funcionen en diferentes escenarios construyendo componentes aislados |
| ** Durabilidad** | Identificar bugs a nivel componente - menos trabajo y más precisión |
| **⚡ Velocidad** | Ensamblar UIs más rápido reutilizando componentes existentes del design system |
| ** Eficiencia** | Paralelizar desarrollo dividiendo UI en componentes discretos entre team members |

### Herramientas CDD en IDS

- ** Desarrollo:** React, Storybook, CSS Modules
- ** Documentación:** Component Story Format (CSF), MDX
- ** Testing:** Cypress Component Testing, Jest
- ** Diseño:** Figma, Design System tokens

### Referencias CDD

- **[Component Driven Development](https://blog.hichroma.com/component-driven-development-ce1109d56c8e)** - Metodología original por Tom Coleman (2017)
- **[Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)** - Mental model de Brad Frost
- **[Component Driven](https://www.componentdriven.org/)** - Recurso oficial de la metodología


##  Tabla de Contenidos

1. [Component Driven Development (CDD)](#:-component-driven-development-cdd)
2. [Arquitectura del Sistema](#:-arquitectura-del-sistema)
3. [Estructura de Componentes](#:-estructura-de-componentes)
4. [Patrones de Desarrollo](#:-patrones-de-desarrollo)
5. [Sistema de Tipos](#:-sistema-de-tipos)
6. [Constantes y Configuración](#⚙️-constantes-y-configuración)
7. [Dependencias e Importaciones](#:-dependencias-e-importaciones)
8. [Documentación y Stories](#:-documentación-y-stories)
9. [Testing](#:-testing)
10. [Categorización de Componentes](#-categorización-de-componentes)
11. [Checklist de Desarrollo](#✅-checklist-de-desarrollo)
12. [Comandos Útiles](#-comandos-útiles)(#-checklist-de-desarrollo)

---
---

##  Arquitectura del Sistema

### Monorepo Lerna + Yarn Workspaces

``` text
informads/
├── packages/
│   ├── [component-name]/          # Cada componente es un paquete independiente
│   │   ├── src/                   # Código fuente
│   │   ├── types/                 # TypeScript definitions
│   │   ├── test/                  # Tests Cypress
│   │   ├── package.json           # Metadata del componente
│   │   ├── README.md              # Documentación básica
│   │   ├── overview.mdx           # Documentación detallada
│   │   └── [Component].stories.jsx # Storybook stories
│   └── shared/                    # Utilities compartidas (colors, mixins, etc.)
├── scripts/                       # Automatización (generación, publish)
└── cypress/                       # Testing setup
```

### Tecnologías Base

- **Framework:** React 17+ con soporte JSX/TSX
- **Estilos:** CSS Modules + CSS Custom Properties
- **Testing:** Cypress Component Testing
- **Docs:** Storybook 7+ con MDX
- **Tipos:** TypeScript definitions (convive con JavaScript)
- **Build:** Vite + Rollup

---
---

##  Estructura de Componentes

### Anatomía de un Componente IDS

``` text
packages/[component-name]/
├── src/
│   ├── Component.jsx              # Componente React principal
│   ├── Component.const.js         # Constantes (VARIANTS, SIZES, etc.)
│   └── Component.module.scss      # Estilos CSS Modules
├── types/
│   └── index.d.ts                # TypeScript definitions
├── test/
│   ├── Component.cy.jsx          # Tests Cypress (JS)
│   └── Component.cy.tsx          # Tests Cypress (TS) - ambos válidos
├── Component.stories.jsx          # Storybook stories
├── Component.doc.js               # Constantes de documentación
├── overview.mdx                   # Documentación MDX completa
├── package.json                   # Configuración del paquete
├── index.js                       # Exports principales
└── README.md                      # Docs de instalación/uso básico
```

---
---

##  Patrones de Desarrollo

### 1. Componente Base

```jsx
import { forwardRef } from 'react';
import PropTypes from 'prop-types';
import styles from './Component.module.scss';

/**
 * Descripción del componente
 * @param {Object} props - Props del componente
 * @param {React.ReactNode} props.children - Contenido
 * @param {string} [props.variant='default'] - Variante visual
 * @param {string} [props.className] - Clases CSS adicionales
 * @param {React.Ref} ref - Referencia al elemento
 */
export const Component = forwardRef(function Component(
  {
    children,
    variant = 'default',
    className,
    ...rest
  },
  ref,
) {
  const componentStyles = [
    styles['ids-component'],
    styles[variant],
    className,
  ]
    .filter(Boolean)
    .join(' ');

  return (
    <div ref={ref} className={componentStyles} {...rest}>
      {children}
    </div>
  );
});

Component.propTypes = {
  children: PropTypes.node,
  variant: PropTypes.oneOf(COMPONENT_VARIANT),
  className: PropTypes.string,
};

export default Component;
```

### 2. Componente con Iconos

```jsx
import Icon from '@informads/icon';
import { ICON_NAMES } from '@informads/icon';

const ComponentWithIcon = ({ iconName = 'home', iconPosition = 'left' }) => (
  <div className={styles['ids-component-with-icon']}>
    {iconPosition === 'left' && <Icon name={iconName} />}
    <span>Content</span>
    {iconPosition === 'right' && <Icon name={iconName} />}
  </div>
);

ComponentWithIcon.propTypes = {
  iconName: PropTypes.oneOf(ICON_NAMES),
  iconPosition: PropTypes.oneOf(['left', 'right']),
};
```

---

## Sistema de Tipos

### Estructura de `types/index.d.ts`

```typescript
/**
 * Módulo TypeScript para [ComponentName]
 */
declare module '@informads/[component-name]' {
  import * as React from 'react';

  // Types
  export type ComponentVariant = 'default' | 'primary' | 'secondary';
  export type ComponentSize = 'xs' | 's' | 'm' | 'l' | 'xl';

  // Props interface
  export interface ComponentProps
    extends React.HTMLAttributes<HTMLDivElement> {
    /** Contenido del componente */
    children?: React.ReactNode;
    /** Variante visual */
    variant?: ComponentVariant;
    /** Tamaño del componente */
    size?: ComponentSize;
    /** Función callback */
    onAction?: (value: string) => void;
  }

  // Component declaration
  export const Component: React.ForwardRefExoticComponent<
    ComponentProps & React.RefAttributes<HTMLDivElement>
  >;

  // Constants exports
  export const COMPONENT_VARIANT: readonly ComponentVariant[];

  // Default export
  export default Component;
}
```

---

## ⚙ Constantes y Configuración

### Pattern of Constants - `Component.const.js`

```javascript
// Variantes disponibles
export const COMPONENT_VARIANT = ['default', 'primary', 'secondary'];

// Tamaños disponibles
export const COMPONENT_SIZE = ['xs', 's', 'm', 'l', 'xl'];

// Estados específicos
export const COMPONENT_STATE = ['idle', 'loading', 'error', 'success'];
```

### Available Icons

```javascript
import { ICON_NAMES } from '@informads/icon';

// ICON_NAMES es generado automáticamente desde:
// packages/icon/src/icons/*.svg
//
// Ejemplos: 'chevron-down', 'home', 'search', 'user', etc.

const iconOptions = [null, ...ICON_NAMES];

##  Dependencias e Importaciones

### `package.json` - Standard Structure

```json
{
  "name": "@informads/component-name",
  "version": "1.x.x",
  "main": "index.js",
  "types": "types/index.d.ts",
  "license": "MIT",
  "peerDependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "dependencies": {
    "@informads/icon": "*",
    "prop-types": "^15.8.1"
  },
  "style": "src/Component.module.scss",
  "files": ["src", "types", "README.md"]
}
```

### Most Reused IDS Components

| Componente | Usado para | Ejemplo |
|------------|------------|---------|
| `@informads/icon` | Iconografía | Botones, inputs |
| `@informads/checkbox` | Selección múltiple | Multi-select |
| `@informads/input-field` | Campos de entrada | Forms |

---

## Documentación y Stories

### Storybook - `Component.stories.jsx`

```javascript
import { Component, COMPONENT_VARIANT } from '@informads/component';
import { COMPONENT_DESCRIPTION } from './Component.doc';
import { ICON_NAMES } from '@informads/icon';

export default {
  title: 'Componentes/[Categoría]/ComponentName',
  component: Component,
  parameters: {
    docs: {
      description: {
        component: COMPONENT_DESCRIPTION,
      },
    },
    status: {
      type: 'stable', // beta | releaseCandidate | stable | deprecated
    },
  },
  argTypes: {
    variant: {
      control: { type: 'select' },
      options: COMPONENT_VARIANT,
    },
    iconName: {
      control: { type: 'select' },
      options: [null, ...ICON_NAMES],
    },
  },
};

const Template = (args) => <Component {...args} />;

export const Default = Template.bind({});
Default.args = {
  children: 'Content',
  variant: 'default',
};
```

---

## Testing

### Testing

```javascript
/// <reference types="cypress" />
import React from 'react';
import { mount } from 'cypress/react';
import { Component, COMPONENT_VARIANT } from '../index';

describe('<Component />', () => {
  context('Rendering', () => {
    it('debería renderizar con props por defecto', () => {
      mount(<Component />);
      cy.get('[class*="ids-component"]').should('exist');
    });
  });

  context('Variantes', () => {
    COMPONENT_VARIANT.forEach((variant) => {
      it(`debería aplicar la variante ${variant}`, () => {
        mount(<Component variant={variant}>Content</Component>);
        cy.get(`[class*="${variant}"]`).should('exist');
      });
    });
  });

  context('Interacción', () => {
    let onClickSpy;

    beforeEach(() => {
      onClickSpy = cy.stub();
    });

    it('debería llamar onClick', () => {
      mount(<Component onClick={onClickSpy}>Click me</Component>);
      cy.get('[class*="ids-component"]').click();
      cy.wrap(onClickSpy).should('have.been.calledOnce');
    });
  });
});
```

### Testing - Common Solutions

| Problema | Solución |
|----------|----------|
| `cy.get('.class')` falla | `cy.get('[class*="ids-component"]')` |
| `cy.stub()` fuera de scope | Usar `beforeEach(() => { spy = cy.stub(); })` |

---
---

##  Categorización de Componentes

### By Function

#### Basic

- **Text/Paragraph:** Contenido textual
- **Title:** Encabezados semánticos
- **Blockquote:** Citas destacadas
- **Divider:** Separadores visuales

#### Actions

- **Button:** Acciones principales
- **Anchor:** Navegación externa/interna
- **ChatbotButton:** Acciones de chat

#### Forms

- **InputField:** Campos de texto
- **Checkbox:** Selección múltiple
- **Radio:** Selección única
- **InputSelect:** Listas desplegables
- **MultiSelect:** Selección múltiple compleja
- **Textarea:** Texto multilínea
- **DatePicker:** Selección de fechas

#### Navigation

- **MainMenu:** Menú principal
- **MainHeader:** Cabecera principal
- **Breadcrumb:** Navegación breadcrumb
- **Pagination:** Paginación
- **Tabs:** Navegación por pestañas

#### Communication

- **Notification:** Alertas
- **Badge:** Indicadores numéricos
- **Tag:** Etiquetas categóricas
- **Chip:** Items removibles
- **ToolTips:** Ayudas contextuales

#### Data

- **Table:** Tablas de datos complejas
- **List:** Listas simples
- **Card:** Contenedores de información

#### Layout

- **Layout:** Estructura base
- **Modal:** Ventanas modales
- **SidePanel:** Paneles laterales
- **Accordion:** Contenido colapsable

#### Images

- **Avatar:** Imágenes de usuario
- **Image:** Imágenes generales
- **Icon:** Iconografía

#### Utilities

- **Code:** Bloques de código
- **Spinner:** Indicadores de carga
- **ThemeProvider:** Contexto de tema

---

## ✅ Checklist de Desarrollo

### Creating New Component

#### Base Structure

- [ ] Crear directorio `packages/[component-name]/`
- [ ] Estructura: `src/`, `types/`, `test/`
- [ ] `package.json` con metadata
- [ ] `index.js` con exports

#### Implementación

- [ ] `src/Component.jsx` con `forwardRef`
- [ ] `src/Component.const.js` con constantes
- [ ] `src/Component.module.scss` con CSS Modules
- [ ] PropTypes definidos

#### Tipos TypeScript

- [ ] `types/index.d.ts` completo
- [ ] Props alineadas con PropTypes
- [ ] Exports de constantes

#### Documentación

- [ ] `Component.doc.js` con constantes
- [ ] `Component.stories.jsx` completo

#### Testing

- [ ] `test/Component.cy.jsx` con contexts
- [ ] Tests de rendering, variantes, interacción
- [ ] Selectores CSS Modules compatibles

#### Iconos (si aplica)

- [ ] Importar `ICON_NAMES`
- [ ] Validar nombres contra constante
- [ ] Opciones en stories

---

## Comandos Útiles

### Desarrollo y Build

```bash
# Build y Watch
npm run build                    # Build producción con Rollup
npm run build:css               # Compilar SCSS a CSS
npm run watch                    # Build en modo watch
npm run clean                    # Limpiar directorio dist/
npm run prebuild                 # Pre-build (limpieza automática)

# Storybook
npm run storybook                # Servidor desarrollo (:6006)
npm run build-storybook          # Build estático Storybook
npm run deploy-storybook         # Preparar para deploy
npm run clean-storybook          # Limpiar cache Storybook
```

### Testing

```bash
# Cypress Component Testing
npm run test-cypress             # Tests interactivos (Chrome)
npm run test-cypress:console     # Tests headless (Chrome)

# Storybook Testing
npm run test-storybook           # Tests de Storybook
```

### Gestión de Componentes

```bash
# Crear Componentes
npm run create:component         # Crear desde template

# Documentación
npm run docs:readme              # Generar READMEs automáticos
npm run docs:status              # Actualizar status componentes
```

### Versionado y Publishing

```bash
# Bump de versiones
npm run bump                     # Bump automático
npm run bump:patch               # Bump patch (x.x.+1)
npm run bump:minor               # Bump minor (x.+1.0)
npm run bump:major               # Bump major (+1.0.0)

# Publishing
npm run publish:all              # Publicar todos los paquetes
npm run publish:all:patch        # Publicar con bump patch
npm run publish:all:minor        # Publicar con bump minor + tag
npm run publish:all:major        # Publicar con bump major + tag
```

### Calidad de Código

```bash
# Linting y Formatting
npm run lint                     # ESLint en packages/
npm run format                   # Verificar formato Prettier
npm run format:fix               # Aplicar formato Prettier
```

### Flujos de Trabajo Comunes

```bash
# Desarrollo de componente nuevo
npm run create:component         # 1. Crear estructura
npm run storybook                # 2. Desarrollar visualmente
npm run test-cypress             # 3. Crear y probar tests
npm run docs:status              # 4. Actualizar status

# Preparar release
npm run lint                     # 1. Verificar código
npm run test-cypress:console     # 2. Ejecutar todos los tests
npm run build                    # 3. Build de producción
npm run bump:minor               # 4. Versionar
npm run publish:all:minor        # 5. Publicar con tag

# Mantenimiento
npm run docs:readme              # Regenerar documentación
npm run clean-storybook          # Limpiar cache si hay problemas
npm run format:fix               # Aplicar formato consistente
```

---

**Mantenido por:** Equipo IDS
**Última actualización:** Marzo 11, 2026
**Basado en:** 50+ componentes del ecosistema IDS
