# Styling Guide

## Overview
This document outlines the styling approach and standards for our 11ty portfolio and blog site. We use SCSS/SASS for styling with a focus on minimal design, accessibility, and responsive layouts.

## Brand Colors

```scss
:root {
  /* Brand Colors */
  --brand-green-dark: #1A472A;    /* Deep forest green */
  --brand-purple-dark: #2D1B69;   /* Rich purple */
  --brand-green-accent: #157F1F;  /* Vibrant green */
  --brand-cream: #F5F5DC;         /* Soft cream */
  --brand-purple-light: #9747FF;  /* Light purple accent */

  /* UI Colors */
  --white: #FFFFFF;               /* Pure white for contrast */
  --white-opacity-10: rgba(255, 255, 255, 0.1);
  --white-opacity-20: rgba(255, 255, 255, 0.2);
  --white-opacity-90: rgba(255, 255, 255, 0.9);
  
  /* Neutral Colors */
  --gray-100: #F8F9FA;
  --gray-200: #E9ECEF;
  --gray-300: #DEE2E6;
  --gray-400: #CED4DA;
  --gray-500: #ADB5BD;
  --gray-600: #6C757D;
  --gray-700: #495057;
  --gray-800: #343A40;
  --gray-900: #212529;
  
  /* Semantic Colors */
  --color-text: var(--gray-900);
  --color-text-light: var(--gray-700);
  --color-background: var(--white);
  --color-background-alt: var(--gray-100);
  --color-primary: var(--brand-purple-dark);
  --color-primary-light: var(--brand-purple-light);
  --color-secondary: var(--brand-green-dark);
  --color-accent: var(--brand-green-accent);
  --color-link: var(--brand-purple-light);
  --color-link-hover: var(--brand-purple-dark);
  --color-success: #28a745;
  --color-warning: #ffc107;
  --color-error: #dc3545;
}
```

## Color Usage Guidelines

### Primary Color Applications
- `--brand-purple-dark` (#2D1B69): Main headings, primary buttons, navigation
- `--brand-purple-light` (#9747FF): Links, accents, interactive elements, focus states

### Secondary Color Applications
- `--brand-green-dark` (#1A472A): Secondary buttons, section backgrounds, footer
- `--brand-green-accent` (#157F1F): Call-to-action highlights, success states

### Supporting Color Applications
- `--brand-cream` (#F5F5DC): Background for cards or content sections
- `--white` (#FFFFFF): Main background, text on dark backgrounds
- Opacity variants: Used for overlays, subtle backgrounds, and hover states

## Typography

```scss
:root {
  /* Typography */
  --font-primary: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  --font-secondary: 'Georgia', 'Times New Roman', serif;
  --font-mono: 'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, Courier, monospace;
  
  --font-size-base: 1rem;          /* 16px */
  --font-size-sm: 0.875rem;        /* 14px */
  --font-size-xs: 0.75rem;         /* 12px */
  --font-size-lg: 1.125rem;        /* 18px */
  --font-size-xl: 1.25rem;         /* 20px */
  --font-size-2xl: 1.5rem;         /* 24px */
  --font-size-3xl: 1.875rem;       /* 30px */
  --font-size-4xl: 2.25rem;        /* 36px */
  
  --line-height-tight: 1.2;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;
}

body {
  font-family: var(--font-primary);
  font-size: var(--font-size-base);
  line-height: var(--line-height-normal);
  color: var(--color-text);
  background-color: var(--color-background);
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-primary);
  line-height: var(--line-height-tight);
  color: var(--color-primary);
  margin-top: 2rem;
  margin-bottom: 1rem;
  font-weight: 700;
}

h1 {
  font-size: var(--font-size-4xl);
}

h2 {
  font-size: var(--font-size-3xl);
}

h3 {
  font-size: var(--font-size-2xl);
}

p {
  margin-bottom: 1.5rem;
}

a {
  color: var(--color-link);
  text-decoration: underline;
  text-underline-offset: 0.2em;
  transition: color 0.2s ease;
  
  &:hover, &:focus {
    color: var(--color-link-hover);
  }
  
  &:focus {
    outline: 2px solid var(--color-primary-light);
    outline-offset: 2px;
  }
}

code, pre {
  font-family: var(--font-mono);
  font-size: var(--font-size-sm);
}
```

## Layout System

We use a combination of CSS Grid and Flexbox for layouts:

```scss
.container {
  width: 100%;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 1rem;
  padding-right: 1rem;
  
  @media (min-width: 768px) {
    padding-left: 2rem;
    padding-right: 2rem;
  }
}

.grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 1.5rem;
  
  @media (max-width: 768px) {
    grid-template-columns: repeat(6, 1fr);
    gap: 1rem;
  }
  
  @media (max-width: 480px) {
    grid-template-columns: 1fr;
  }
}

.flex {
  display: flex;
  
  &--wrap {
    flex-wrap: wrap;
  }
  
  &--column {
    flex-direction: column;
  }
  
  &--center {
    align-items: center;
    justify-content: center;
  }
  
  &--between {
    justify-content: space-between;
  }
}
```

## Component Styling

We follow the BEM (Block, Element, Modifier) methodology for component naming:

```scss
.card {
  background-color: var(--white);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  
  &__header {
    padding: 1.5rem;
    border-bottom: 1px solid var(--gray-200);
  }
  
  &__title {
    margin-top: 0;
    margin-bottom: 0.5rem;
    font-size: var(--font-size-xl);
  }
  
  &__content {
    padding: 1.5rem;
  }
  
  &__footer {
    padding: 1.5rem;
    border-top: 1px solid var(--gray-200);
  }
  
  &--featured {
    border-left: 4px solid var(--brand-purple-light);
  }
}
```

## Buttons

```scss
.button {
  display: inline-block;
  padding: 0.75rem 1.5rem;
  font-size: var(--font-size-base);
  font-weight: 500;
  line-height: 1;
  text-align: center;
  text-decoration: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  transition: background-color 0.2s ease, transform 0.1s ease;
  
  // Primary button
  background-color: var(--brand-purple-dark);
  color: var(--white);
  
  &:hover, &:focus {
    background-color: darken(#2D1B69, 5%);
    color: var(--white);
  }
  
  &:active {
    transform: translateY(1px);
  }
  
  &:focus {
    outline: 2px solid var(--brand-purple-light);
    outline-offset: 2px;
  }
  
  // Secondary button
  &--secondary {
    background-color: var(--brand-green-dark);
    color: var(--white);
    
    &:hover, &:focus {
      background-color: darken(#1A472A, 5%);
    }
  }
  
  // Outline button
  &--outline {
    background-color: transparent;
    color: var(--brand-purple-dark);
    border: 2px solid var(--brand-purple-dark);
    
    &:hover, &:focus {
      background-color: var(--brand-purple-dark);
      color: var(--white);
    }
  }
  
  // Small button
  &--small {
    padding: 0.5rem 1rem;
    font-size: var(--font-size-sm);
  }
  
  // Large button
  &--large {
    padding: 1rem 2rem;
    font-size: var(--font-size-lg);
  }
  
  &--full {
    display: block;
    width: 100%;
  }
  
  &[disabled] {
    opacity: 0.6;
    cursor: not-allowed;
  }
}
```

## Form Elements

```scss
.form-group {
  margin-bottom: 1.5rem;
  
  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: var(--color-text);
  }
  
  .form-control {
    display: block;
    width: 100%;
    padding: 0.75rem;
    font-size: var(--font-size-base);
    line-height: 1.5;
    color: var(--color-text);
    background-color: var(--white);
    border: 2px solid var(--gray-300);
    border-radius: 4px;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
    
    &:focus {
      border-color: var(--brand-purple-light);
      outline: none;
      box-shadow: 0 0 0 3px rgba(151, 71, 255, 0.2);
    }
    
    &::placeholder {
      color: var(--gray-500);
    }
    
    &--error {
      border-color: var(--color-error);
      
      &:focus {
        box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.2);
      }
    }
  }
  
  .form-text {
    display: block;
    margin-top: 0.25rem;
    font-size: var(--font-size-sm);
    color: var(--color-text-light);
  }
  
  .error-message {
    display: block;
    margin-top: 0.25rem;
    font-size: var(--font-size-sm);
    color: var(--color-error);
  }
}
```

## Accessibility Features

### Focus States

All interactive elements have clearly visible focus states:

```scss
a:focus,
button:focus,
input:focus,
select:focus,
textarea:focus,
[tabindex]:focus {
  outline: 2px solid var(--brand-purple-light);
  outline-offset: 2px;
}
```

### Skip Link

A skip link is provided for keyboard users to bypass navigation:

```scss
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background-color: var(--brand-purple-dark);
  color: var(--white);
  padding: 0.5rem 1rem;
  z-index: 100;
  
  &:focus {
    top: 0;
  }
}
```

### Contrast Ratios

All color combinations meet WCAG AA standards for contrast:

- Text on background: Minimum 4.5:1 contrast ratio
- Large text on background: Minimum 3:1 contrast ratio
- UI components and graphical objects: Minimum 3:1 contrast ratio

### ARIA Attributes

ARIA attributes are used appropriately to enhance screen reader experience:

```html
<nav aria-label="Main navigation">
  <button 
    class="menu-toggle" 
    aria-expanded="false" 
    aria-controls="main-menu">
    Menu
  </button>
  <ul id="main-menu" class="menu">
    <!-- Menu items -->
  </ul>
</nav>
```

### Responsive Design

The design is responsive and accessible across all device sizes:

```scss
@media (max-width: 768px) {
  h1 {
    font-size: var(--font-size-3xl);
  }
  
  h2 {
    font-size: var(--font-size-2xl);
  }
  
  .container {
    padding-left: 1rem;
    padding-right: 1rem;
  }
}

@media (min-width: 1024px) {
  // Larger viewport styles
}
```

## Dark Mode Support

The site supports dark mode through CSS variables:

```scss
@media (prefers-color-scheme: dark) {
  :root {
    --color-text: var(--gray-100);
    --color-text-light: var(--gray-300);
    --color-background: var(--gray-900);
    --color-background-alt: var(--gray-800);
    --color-link: var(--brand-purple-light);
    --color-link-hover: lighten(#9747FF, 15%);
  }
  
  .card {
    background-color: var(--gray-800);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  }
  
  .button--outline {
    border-color: var(--brand-purple-light);
    color: var(--brand-purple-light);
  }
}
```

## Browser Support

The site is designed to work on all modern browsers:

- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

Fallbacks are provided for older browsers where necessary using feature queries:

```scss
@supports (display: grid) {
  .grid-layout {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 1rem;
  }
}

@supports not (display: grid) {
  .grid-layout {
    display: flex;
    flex-wrap: wrap;
    
    > * {
      flex: 0 0 calc(33.33% - 1rem);
      margin: 0.5rem;
    }
  }
}
```

## SCSS Organization

The SCSS files are organized following the 7-1 pattern:

```
src/assets/scss/
├── main.scss           # Main file that imports all other files
├── _variables.scss     # Variables including colors, typography, etc.
├── _functions.scss     # SCSS functions
├── _mixins.scss        # SCSS mixins
├── base/               # Base styles
│   ├── _reset.scss     # CSS reset/normalize
│   ├── _typography.scss # Typography rules
│   └── _utilities.scss  # Utility classes
├── components/         # Component-specific styles
│   ├── _buttons.scss
│   ├── _forms.scss
│   ├── _cards.scss
│   ├── _navigation.scss
│   └── ...
├── layout/             # Layout components
│   ├── _header.scss
│   ├── _footer.scss
│   ├── _grid.scss
│   └── ...
├── pages/              # Page-specific styles
│   ├── _home.scss
│   ├── _blog.scss
│   ├── _portfolio.scss
│   └── ...
└── themes/             # Theme variations
    └── _dark-mode.scss
```

## Usage Guidelines

### Adding Component Styles

1. Create a new SCSS file in the appropriate directory
2. Follow BEM naming conventions
3. Import the file in `main.scss`

### Extending Existing Styles

Use modifiers to extend existing components:

```scss
.card {
  // Base card styles
  
  &--featured {
    // Featured card variations
  }
}
```

### Using the Grid System

The grid system is used to create responsive layouts:

```html
<div class="grid">
  <div class="span-4">Column 1</div>
  <div class="span-4">Column 2</div>
  <div class="span-4">Column 3</div>
</div>
```

```scss
.span-4 {
  grid-column: span 4;
  
  @media (max-width: 768px) {
    grid-column: span 6;
  }
  
  @media (max-width: 480px) {
    grid-column: span 12;
  }
}
```

## Accessibility Testing

All components are tested for accessibility using:

1. Automated tools (Lighthouse, axe)
2. Keyboard navigation
3. Screen reader testing
4. Color contrast checking

## Animation Guidelines

Animations are kept subtle and considerate of users who prefer reduced motion:

```scss
.animated-element {
  transition: transform 0.3s ease, opacity 0.3s ease;
  
  &:hover {
    transform: translateY(-5px);
  }
}

@media (prefers-reduced-motion: reduce) {
  .animated-element {
    transition: none;
    
    &:hover {
      transform: none;
    }
  }
}
``` 