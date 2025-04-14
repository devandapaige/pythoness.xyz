# Blog Implementation

## Overview
This document describes the implementation details for the blog section of our 11ty portfolio site. The blog is separate from the portfolio to maintain a clear distinction between written content and project showcases.

## Features

- MDX-powered content with support for React components
- Tag-based organization and filtering
- Search functionality
- Date-based filtering and archives
- Responsive design with accessibility features

## Directory Structure

```
src/
├── blog/
│   ├── index.njk            # Blog listing/landing page
│   ├── [YYYY-MM-DD]-post-slug.mdx  # Individual blog posts
│   ├── tags/                # Tag pages
│   │   ├── index.njk        # All tags listing
│   │   └── [tag].njk        # Individual tag page (template)
│   └── search.njk           # Search page for blog posts
```

## Data Model

### Blog Post Front Matter

```yaml
---
title: "Post Title"
date: 2023-08-15
updated: 2023-08-16  # Optional
tags: 
  - javascript
  - eleventy
excerpt: "A short description of the post for previews and SEO"
coverImage: "/assets/images/blog/post-cover.jpg"  # Optional
alt: "Description of the cover image"  # Required if coverImage is present
featured: true  # Optional, for highlighting posts
draft: false  # Optional, for draft posts
---
```

## Collections

Blog posts are organized into collections defined in `.eleventy.js`:

```javascript
// Collection for all published blog posts
eleventyConfig.addCollection("blog", function(collection) {
  return collection.getFilteredByGlob("src/blog/**/*.mdx")
    .filter(post => !post.data.draft)
    .sort((a, b) => b.date - a.date);
});

// Collection for featured blog posts
eleventyConfig.addCollection("featuredPosts", function(collection) {
  return collection.getFilteredByGlob("src/blog/**/*.mdx")
    .filter(post => post.data.featured && !post.data.draft)
    .sort((a, b) => b.date - a.date)
    .slice(0, 3);
});

// Collection for blog tags
eleventyConfig.addCollection("blogTags", function(collection) {
  const tags = new Set();
  collection.getFilteredByGlob("src/blog/**/*.mdx").forEach(post => {
    if (post.data.tags && !post.data.draft) {
      post.data.tags.forEach(tag => tags.add(tag));
    }
  });
  return [...tags].sort();
});
```

## Page Templates

### Blog Index Page

The blog index page (`src/blog/index.njk`) displays a paginated list of blog posts:

```njk
---
layout: layouts/base.njk
title: Blog
description: My thoughts, ideas, and tutorials
pagination:
  data: collections.blog
  size: 10
  alias: posts
---

<h1>Blog</h1>

{# Filter by tag #}
<div class="tag-filter">
  <a href="/blog/" {% if not tag %}class="active"{% endif %}>All</a>
  {% for tag in collections.blogTags %}
    <a href="/blog/tags/{{ tag }}/" {% if tag == page.fileSlug %}class="active"{% endif %}>{{ tag }}</a>
  {% endfor %}
</div>

{# Blog post list #}
<div class="post-list">
  {% for post in posts %}
    <article class="post-card">
      {% if post.data.coverImage %}
        <div class="post-card__image">
          <img src="{{ post.data.coverImage }}" alt="{{ post.data.alt }}" loading="lazy">
        </div>
      {% endif %}
      <div class="post-card__content">
        <h2 class="post-card__title">
          <a href="{{ post.url }}">{{ post.data.title }}</a>
        </h2>
        <time datetime="{{ post.date | dateIso }}">{{ post.date | dateReadable }}</time>
        {% if post.data.excerpt %}
          <p>{{ post.data.excerpt }}</p>
        {% endif %}
        <div class="post-card__tags">
          {% for tag in post.data.tags %}
            <a href="/blog/tags/{{ tag }}/">{{ tag }}</a>
          {% endfor %}
        </div>
      </div>
    </article>
  {% endfor %}
</div>

{# Pagination controls #}
{% include "partials/pagination.njk" %}
```

### Tag Pages

The tag page template (`src/blog/tags/[tag].njk`) displays all posts with a specific tag:

```njk
---
layout: layouts/base.njk
pagination:
  data: collections.blogTags
  size: 1
  alias: tag
permalink: /blog/tags/{{ tag }}/
eleventyComputed:
  title: "Posts tagged with '{{ tag }}'"
---

<h1>Posts tagged with '{{ tag }}'</h1>

<a href="/blog/tags/" class="back-link">← All tags</a>

<div class="post-list">
  {% set taglist = collections.blog | filterByTags(tag) %}
  {% for post in taglist %}
    <article class="post-card">
      <!-- Similar structure to blog index -->
    </article>
  {% endfor %}
</div>
```

## Search Implementation

The search functionality is implemented with JavaScript:

1. A JSON file is generated with all posts' metadata and content
2. Client-side JS searches through this data when the user submits a query
3. Results are displayed on the page without requiring a page reload

```javascript
// Script in src/assets/js/search.js
document.addEventListener('DOMContentLoaded', () => {
  const searchForm = document.getElementById('search-form');
  const searchInput = document.getElementById('search-input');
  const searchResults = document.getElementById('search-results');

  if (searchForm) {
    searchForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const query = searchInput.value.toLowerCase();
      
      // Fetch the search index JSON
      fetch('/search-index.json')
        .then(response => response.json())
        .then(posts => {
          // Filter posts based on search query
          const results = posts.filter(post => {
            return post.title.toLowerCase().includes(query) || 
                  post.excerpt.toLowerCase().includes(query) || 
                  post.content.toLowerCase().includes(query) ||
                  (post.tags && post.tags.some(tag => tag.toLowerCase().includes(query)));
          });
          
          displayResults(results);
        });
    });
  }

  function displayResults(results) {
    if (results.length === 0) {
      searchResults.innerHTML = '<p>No results found.</p>';
      return;
    }
    
    let html = '<div class="post-list">';
    results.forEach(post => {
      html += `
        <article class="post-card">
          <div class="post-card__content">
            <h2 class="post-card__title">
              <a href="${post.url}">${post.title}</a>
            </h2>
            <time datetime="${post.dateISO}">${post.date}</time>
            <p>${post.excerpt}</p>
            <div class="post-card__tags">
              ${post.tags ? post.tags.map(tag => `<a href="/blog/tags/${tag}/">${tag}</a>`).join('') : ''}
            </div>
          </div>
        </article>
      `;
    });
    html += '</div>';
    
    searchResults.innerHTML = html;
  }
});
```

## Accessibility Features

The blog implementation includes several accessibility features:

1. Semantic HTML structure for screen readers
2. Proper heading hierarchy (h1, h2, h3)
3. ARIA attributes for interactive elements
4. Keyboard navigation support
5. Focus indicators for interactive elements
6. Alt text for all images
7. Skip-to-content link at the top of each page

## Custom Components

We've created several reusable MDX components for blog posts:

1. `CodeBlock` - For syntax-highlighted code snippets
2. `Callout` - For highlighting important information
3. `ImageWithCaption` - For images with captions and attribution
4. `TableOfContents` - Automatically generates a table of contents

## Future Enhancements

Planned enhancements for the blog section:

1. Reading time estimation
2. Related posts suggestions
3. Newsletter subscription integration
4. Social sharing buttons
5. Comments system integration (possibly using Giscus or similar) 