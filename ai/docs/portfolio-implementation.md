# Portfolio Implementation

## Overview
This document describes the implementation details for the portfolio section of our 11ty site. The portfolio showcases projects separately from blog content, with rich media and detailed project descriptions.

## Features

- Separate from blog content
- Detailed project pages with images and descriptions
- Categorization of projects by type/technology
- Responsive grid layout
- Rich media support (images, videos, embedded demos)
- Accessibility-focused implementation

## Directory Structure

```
src/
├── portfolio/
│   ├── index.njk               # Portfolio landing/listing page
│   ├── [project-name]/         # Individual project directories
│   │   ├── index.mdx           # Project details and content
│   │   └── images/             # Project-specific images
│   └── categories/             # Project category pages
│       ├── index.njk           # All categories listing
│       └── [category].njk      # Individual category page (template)
```

## Data Model

### Portfolio Project Front Matter

```yaml
---
title: "Project Title"
date: 2023-07-15
featured: true  # Optional, for highlighting on homepage
excerpt: "Brief description of the project"
thumbnail: "./images/thumbnail.jpg"
coverImage: "./images/cover.jpg"  # Optional larger header image
alt: "Description of the cover image"
demo: "https://demo-link.com"  # Optional
repository: "https://github.com/username/repo"  # Optional
technologies:
  - JavaScript
  - CSS
  - HTML
categories:
  - Web Development
  - Design
---
```

## Collections

Portfolio projects are organized into collections defined in `.eleventy.js`:

```javascript
// Collection for all portfolio projects
eleventyConfig.addCollection("portfolio", function(collection) {
  return collection.getFilteredByGlob("src/portfolio/**/index.mdx")
    .sort((a, b) => b.date - a.date);
});

// Collection for featured portfolio projects
eleventyConfig.addCollection("featuredProjects", function(collection) {
  return collection.getFilteredByGlob("src/portfolio/**/index.mdx")
    .filter(project => project.data.featured)
    .sort((a, b) => b.date - a.date)
    .slice(0, 4);
});

// Collection for portfolio categories
eleventyConfig.addCollection("portfolioCategories", function(collection) {
  const categories = new Set();
  collection.getFilteredByGlob("src/portfolio/**/index.mdx").forEach(project => {
    if (project.data.categories) {
      project.data.categories.forEach(category => categories.add(category));
    }
  });
  return [...categories].sort();
});
```

## Page Templates

### Portfolio Index Page

The portfolio index page (`src/portfolio/index.njk`) displays a grid of portfolio projects:

```njk
---
layout: layouts/base.njk
title: Portfolio
description: A showcase of my projects and work
---

<h1>Portfolio</h1>

{# Category filter #}
<div class="category-filter">
  <a href="/portfolio/" {% if not category %}class="active"{% endif %}>All</a>
  {% for category in collections.portfolioCategories %}
    <a href="/portfolio/categories/{{ category | slug }}/" {% if category == page.fileSlug %}class="active"{% endif %}>{{ category }}</a>
  {% endfor %}
</div>

{# Project grid #}
<div class="project-grid">
  {% for project in collections.portfolio %}
    <div class="project-card">
      <a href="{{ project.url }}" class="project-card__link">
        <div class="project-card__image">
          <img src="{{ project.url }}{{ project.data.thumbnail }}" alt="{{ project.data.title }}" loading="lazy">
        </div>
        <div class="project-card__content">
          <h2 class="project-card__title">{{ project.data.title }}</h2>
          {% if project.data.excerpt %}
            <p>{{ project.data.excerpt }}</p>
          {% endif %}
          <div class="project-card__technologies">
            {% for tech in project.data.technologies %}
              <span class="tech-tag">{{ tech }}</span>
            {% endfor %}
          </div>
        </div>
      </a>
    </div>
  {% endfor %}
</div>
```

### Project Detail Template

The individual project template (`src/_includes/layouts/project.njk`) displays a single project:

```njk
---
layout: layouts/base.njk
---

<article class="project">
  <header class="project__header">
    {% if coverImage %}
      <div class="project__cover">
        <img src="{{ page.url }}{{ coverImage }}" alt="{{ alt }}" loading="eager">
      </div>
    {% endif %}
    
    <h1 class="project__title">{{ title }}</h1>
    
    <div class="project__meta">
      <time datetime="{{ date | dateIso }}">{{ date | dateReadable }}</time>
      
      <div class="project__links">
        {% if demo %}
          <a href="{{ demo }}" class="button" target="_blank" rel="noopener">View Demo</a>
        {% endif %}
        
        {% if repository %}
          <a href="{{ repository }}" class="button button--secondary" target="_blank" rel="noopener">Source Code</a>
        {% endif %}
      </div>
    </div>
    
    <div class="project__technologies">
      {% for tech in technologies %}
        <span class="tech-tag">{{ tech }}</span>
      {% endfor %}
    </div>
  </header>
  
  <div class="project__content">
    {{ content | safe }}
  </div>
  
  <footer class="project__footer">
    <div class="project__categories">
      {% for category in categories %}
        <a href="/portfolio/categories/{{ category | slug }}/" class="category-link">{{ category }}</a>
      {% endfor %}
    </div>
    
    <a href="/portfolio/" class="back-link">← Back to Portfolio</a>
  </footer>
</article>
```

## Custom Components for Project Pages

We've created several MDX components specifically for portfolio projects:

1. `ImageGallery` - For displaying multiple project images in a grid or carousel
2. `VideoDemo` - For embedding video demonstrations
3. `TechStack` - For showcasing the technology stack with icons
4. `ProcessSteps` - For highlighting the project development process
5. `ProjectResults` - For showcasing project outcomes and metrics

Example usage in a project file:

```mdx
---
title: "E-commerce Website"
date: 2023-06-20
featured: true
excerpt: "A responsive e-commerce website built with modern technologies"
thumbnail: "./images/thumbnail.jpg"
coverImage: "./images/cover.jpg"
alt: "E-commerce website homepage on multiple devices"
demo: "https://demo-store.example.com"
repository: "https://github.com/username/ecommerce"
technologies:
  - React
  - Node.js
  - MongoDB
categories:
  - Web Development
  - E-commerce
---

# E-commerce Website

This project is a fully functional e-commerce website built with modern technologies.

<TechStack
  frontend={["React", "Redux", "SCSS"]}
  backend={["Node.js", "Express", "MongoDB"]}
  tools={["Webpack", "Jest", "GitHub Actions"]}
/>

## Project Overview

The goal was to create a responsive, user-friendly e-commerce platform with advanced features.

<ProcessSteps
  steps={[
    {
      title: "Research & Planning",
      description: "Market research and requirement gathering"
    },
    {
      title: "Design",
      description: "Creating wireframes and visual design"
    },
    {
      title: "Development",
      description: "Frontend and backend implementation"
    },
    {
      title: "Testing & Deployment",
      description: "Quality assurance and launch"
    }
  ]}
/>

## Key Features

- Responsive design for all device sizes
- User authentication and account management
- Product search and filtering
- Shopping cart and checkout process
- Payment integration
- Order tracking

<ImageGallery
  images={[
    {
      src: "./images/homepage.jpg",
      alt: "Website homepage",
      caption: "Homepage with featured products"
    },
    {
      src: "./images/product-page.jpg",
      alt: "Product detail page",
      caption: "Product detail with related items"
    },
    {
      src: "./images/checkout.jpg",
      alt: "Checkout process",
      caption: "Streamlined checkout experience"
    }
  ]}
/>

## Development Process

The project was developed using Agile methodology over a period of 8 weeks.

<VideoDemo src="./videos/demo.mp4" caption="Product walkthrough" />

## Results

<ProjectResults
  metrics={[
    { label: "Conversion Rate", value: "3.2%" },
    { label: "Page Load Time", value: "1.5s" },
    { label: "Mobile Users", value: "65%" }
  ]}
/>

The project resulted in a 25% increase in online sales and improved user satisfaction.
```

## CSS Implementation

The portfolio uses SCSS for styling with a focus on a responsive grid layout:

```scss
// src/assets/scss/components/_project-grid.scss

.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
  
  @media (max-width: 768px) {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 1.5rem;
  }
  
  @media (max-width: 480px) {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
}

.project-card {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  
  &:hover, &:focus-within {
    transform: translateY(-5px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
  }
  
  &__link {
    display: block;
    text-decoration: none;
    color: inherit;
    
    &:focus {
      outline: 2px solid var(--brand-purple-light);
      outline-offset: 2px;
    }
  }
  
  &__image {
    aspect-ratio: 16 / 9;
    overflow: hidden;
    
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.5s ease;
      
      .project-card:hover & {
        transform: scale(1.05);
      }
    }
  }
  
  &__content {
    padding: 1.5rem;
    background-color: var(--white);
  }
  
  &__title {
    margin-top: 0;
    margin-bottom: 0.5rem;
    font-size: 1.25rem;
    color: var(--brand-purple-dark);
  }
  
  &__technologies {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 1rem;
  }
}

.tech-tag {
  display: inline-block;
  padding: 0.25rem 0.5rem;
  background-color: var(--white-opacity-20);
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 500;
}
```

## Accessibility Features

The portfolio implementation includes several accessibility features:

1. Semantic HTML structure for better screen reader navigation
2. Proper heading hierarchy
3. Sufficient color contrast meeting WCAG standards
4. Keyboard navigation with visible focus indicators
5. Alternative text for all images
6. ARIA attributes for custom components
7. Focus management for interactive elements

## Future Enhancements

Planned enhancements for the portfolio section:

1. Case study templates with more structured sections
2. Interactive project demos embedded directly in the page
3. Before/after image comparison sliders
4. Project filtering by multiple technologies/categories
5. Related projects suggestions 