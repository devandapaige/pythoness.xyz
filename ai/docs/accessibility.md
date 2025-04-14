# Accessibility Guidelines

## Overview
This document outlines the accessibility standards and implementation guidelines for our 11ty portfolio and blog site. Ensuring our site is accessible to all users, including those with disabilities, is a core principle of our design philosophy.

## WCAG Compliance

We aim to meet or exceed [Web Content Accessibility Guidelines (WCAG) 2.1 Level AA](https://www.w3.org/TR/WCAG21/) standards throughout the site. This includes:

1. **Perceivable** - Information and user interface components must be presentable to users in ways they can perceive
2. **Operable** - User interface components and navigation must be operable
3. **Understandable** - Information and the operation of the user interface must be understandable
4. **Robust** - Content must be robust enough to be interpreted by a wide variety of user agents, including assistive technologies

## Core Accessibility Features

### Keyboard Navigation

All interactive elements must be accessible via keyboard navigation:

- Logical tab order that follows the visual layout
- Visible focus indicators for all interactive elements
- Skip links to bypass repetitive navigation
- No keyboard traps

Implementation example:

```html
<!-- Skip link at the top of each page -->
<a href="#main-content" class="skip-link">Skip to main content</a>

<!-- Focus visible styles in CSS -->
<style>
  a:focus,
  button:focus,
  input:focus,
  select:focus,
  textarea:focus,
  [tabindex]:focus {
    outline: 2px solid var(--brand-purple-light);
    outline-offset: 2px;
  }
</style>
```

### Screen Reader Support

All content must be accessible to screen readers:

- Semantic HTML structure
- Appropriate ARIA roles, states, and properties
- Descriptive alt text for images
- Form labels and instructions
- Meaningful link text

Implementation example:

```html
<!-- Semantic structure example -->
<article>
  <header>
    <h1>Article Title</h1>
    <p>Published on <time datetime="2023-08-15">August 15, 2023</time></p>
  </header>
  
  <div class="content">
    <!-- Content here -->
  </div>
  
  <footer>
    <!-- Article footer -->
  </footer>
</article>

<!-- Image with alt text -->
<img src="diagram.png" alt="Flowchart showing the data processing pipeline from user input to database storage">

<!-- ARIA example for a tab interface -->
<div role="tablist">
  <button id="tab1" role="tab" aria-selected="true" aria-controls="panel1">Tab 1</button>
  <button id="tab2" role="tab" aria-selected="false" aria-controls="panel2">Tab 2</button>
</div>
<div id="panel1" role="tabpanel" aria-labelledby="tab1">Panel 1 content</div>
<div id="panel2" role="tabpanel" aria-labelledby="tab2" hidden>Panel 2 content</div>
```

### Color Contrast

All text and interface elements must have sufficient color contrast:

- Normal text: 4.5:1 contrast ratio minimum
- Large text (18pt+): 3:1 contrast ratio minimum
- UI components and graphical objects: 3:1 contrast ratio minimum

Implementation:

```scss
// High contrast text colors
:root {
  --color-text: var(--gray-900); // #212529 on white background
  --color-text-light: var(--gray-700); // #495057 on white background
}

// Tools for checking contrast
// We use tools like WebAIM Contrast Checker during development:
// https://webaim.org/resources/contrastchecker/
```

### Focus Indicators

All interactive elements must have visible focus indicators:

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

// Non-visual focus styles are used only in addition to visible focus
.some-element:focus-visible {
  // Styles that enhance visual focus
}

// Never hide focus completely
.some-element:focus {
  outline: none; // ❌ NEVER do this without alternative visible focus styles
}
```

### Responsive Design

The site is designed to be fully accessible across all device sizes and orientations:

- Content scales appropriately on different screen sizes
- Touch targets are at least 44×44 pixels for mobile users
- No horizontal scrolling at widths of 320px or greater
- Supports both portrait and landscape orientations

```scss
// Ensuring touch targets are appropriately sized
.button {
  min-height: 44px;
  min-width: 44px;
  padding: 0.75rem 1.25rem;
}

// Responsive layout adjustments
@media (max-width: 768px) {
  .container {
    padding-left: 1rem;
    padding-right: 1rem;
  }
  
  // Adjust font sizes for readability
  h1 {
    font-size: var(--font-size-3xl);
  }
}
```

### Motion and Animation

Animations and motion are implemented with accessibility in mind:

- No content flashes more than 3 times per second
- Motion can be disabled via `prefers-reduced-motion` media query
- Essential animations are subtle and non-distracting

```scss
// Base animation
.animated-element {
  transition: transform 0.3s ease, opacity 0.3s ease;
  
  &:hover {
    transform: translateY(-5px);
  }
}

// Respect user preferences for reduced motion
@media (prefers-reduced-motion: reduce) {
  .animated-element {
    transition: none;
    
    &:hover {
      transform: none;
    }
  }
  
  .carousel {
    scroll-behavior: auto;
  }
}
```

## Implementation Checklists

### HTML Structure Checklist

- [ ] Use semantic HTML elements (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`, etc.)
- [ ] Ensure proper heading hierarchy (h1, h2, h3, etc.)
- [ ] Use lists (`<ul>`, `<ol>`, `<dl>`) for list content
- [ ] Use tables (`<table>`, `<th>`, `<td>`) only for tabular data with proper headers
- [ ] Implement proper form labels and structure
- [ ] Add lang attribute to HTML element (`<html lang="en">`)

### Images and Media Checklist

- [ ] Provide alt text for all images (`alt="descriptive text"` or `alt=""` for decorative images)
- [ ] Include captions and transcripts for audio and video content
- [ ] Ensure media controls are keyboard accessible
- [ ] Avoid auto-playing media with sound
- [ ] Provide text alternatives for complex visualizations or charts

### Interactive Elements Checklist

- [ ] Ensure all interactive elements are keyboard accessible
- [ ] Provide visible focus states for all interactive elements
- [ ] Use appropriate ARIA roles, states, and properties
- [ ] Maintain logical tab order
- [ ] Implement proper form validation with clear error messages
- [ ] Ensure sufficient touch target size (minimum 44×44 pixels)

### Content Checklist

- [ ] Write clear, simple content
- [ ] Avoid jargon and complex language when possible
- [ ] Structure content with headings, lists, and short paragraphs
- [ ] Use descriptive link text (avoid "click here" or "read more")
- [ ] Provide definitions for specialized terms
- [ ] Avoid all-caps text for readability

## Component-Specific Guidelines

### Navigation

- Include a skip link at the top of each page
- Clearly indicate current page in navigation
- Provide breadcrumbs for deep content hierarchies
- Use ARIA landmarks to identify navigation regions

```html
<a href="#main-content" class="skip-link">Skip to main content</a>

<nav aria-label="Main navigation">
  <ul>
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/blog/">Blog</a></li>
    <li><a href="/portfolio/">Portfolio</a></li>
    <li><a href="/about/">About</a></li>
    <li><a href="/contact/">Contact</a></li>
  </ul>
</nav>

<main id="main-content">
  <!-- Main content -->
</main>
```

### Forms

- Associate labels with form controls
- Group related form controls with fieldset and legend
- Provide clear instructions and error messages
- Indicate required fields
- Support keyboard navigation within complex form controls

```html
<form>
  <div class="form-group">
    <label for="name">Name <span class="required">*</span></label>
    <input 
      type="text" 
      id="name" 
      name="name" 
      required 
      aria-required="true"
      aria-describedby="name-help"
    >
    <div id="name-help" class="form-text">Enter your full name.</div>
  </div>
  
  <fieldset>
    <legend>Contact Preferences</legend>
    
    <div class="form-check">
      <input type="checkbox" id="contact-email" name="contact" value="email">
      <label for="contact-email">Email</label>
    </div>
    
    <div class="form-check">
      <input type="checkbox" id="contact-phone" name="contact" value="phone">
      <label for="contact-phone">Phone</label>
    </div>
  </fieldset>
  
  <button type="submit">Submit</button>
</form>
```

### Cards and Grids

- Ensure grid items are navigable in a logical order
- Make card links encompass the entire card area when possible
- Maintain adequate spacing between clickable elements
- Provide visual and programmatic indication of card relationships

```html
<div class="card-grid">
  <article class="card">
    <a href="/blog/post-1/" class="card__link">
      <h2 class="card__title">Blog Post Title</h2>
      <div class="card__image">
        <img src="thumbnail.jpg" alt="Brief description of the post content">
      </div>
      <div class="card__content">
        <p>Brief excerpt from the post...</p>
      </div>
      <span class="sr-only">Read more about Blog Post Title</span>
    </a>
  </article>
  <!-- Additional cards -->
</div>
```

### MDX Components

All custom MDX components must follow accessibility best practices:

- `CodeBlock`: Properly marked up with semantic elements and appropriate syntax highlighting contrast
- `ImageWithCaption`: Proper alt text and caption structure using `<figure>` and `<figcaption>`
- `TableOfContents`: Proper landmark role and navigable structure
- Interactive components: Full keyboard support and ARIA attributes

## Testing Guidelines

### Automated Testing

Integrate automated accessibility testing in the development workflow:

- Use axe-core or similar tools for automated checks
- Run Lighthouse accessibility audits
- Validate HTML to ensure proper structure

### Manual Testing

Perform regular manual accessibility testing:

- Keyboard navigation testing (tab through the entire site)
- Screen reader testing (NVDA, VoiceOver, JAWS)
- Test with different device sizes and orientations
- Test all interactive components without a mouse
- Verify color contrast meets requirements

### Testing Checklist

- [ ] Test with keyboard only (no mouse)
- [ ] Test with screen readers
- [ ] Test at different zoom levels (up to 200%)
- [ ] Test color contrast
- [ ] Test in high contrast mode
- [ ] Test with text-only browsers
- [ ] Test with browser plugins that disable CSS/JavaScript
- [ ] Test all form validation and error states

## Resources

- [WebAIM: Web Accessibility In Mind](https://webaim.org/)
- [A11Y Project Checklist](https://www.a11yproject.com/checklist/)
- [MDN Accessibility Documentation](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
- [Inclusive Components](https://inclusive-components.design/)
- [Deque University](https://dequeuniversity.com/)

## Implementation Plan

1. **Build-time checks**: Integrate automated accessibility testing in the build process
2. **Development patterns**: Use accessible components and patterns by default
3. **Documentation**: Document accessibility features of all components
4. **Regular testing**: Schedule regular accessibility audits
5. **Ongoing education**: Keep the team informed about accessibility best practices

By following these guidelines, we ensure our 11ty portfolio and blog site is accessible to all users, regardless of their abilities or the assistive technologies they use. 