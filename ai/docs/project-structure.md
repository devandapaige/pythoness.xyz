# 11ty Portfolio Project Structure

## Overview
This document outlines the structure and organization of our 11ty portfolio and blog site. The project uses 11ty (Eleventy) as the static site generator with MDX for enhanced content creation.

## Directory Structure

```
/
├── .eleventy.js             # Main 11ty configuration
├── package.json             # Project dependencies
├── .gitignore               # Git ignore file
├── README.md                # Project documentation
├── _data/                   # Global data files
│   ├── site.json            # Site-wide configuration
│   ├── metadata.json        # SEO and metadata
│   └── navigation.json      # Navigation structure
├── src/                     # Source files
│   ├── _includes/           # Templates and partials
│   │   ├── layouts/         # Page layouts
│   │   │   ├── base.njk     # Base layout
│   │   │   ├── post.njk     # Blog post layout
│   │   │   └── project.njk  # Portfolio project layout
│   │   ├── partials/        # Reusable page sections
│   │   │   ├── header.njk   # Site header
│   │   │   ├── footer.njk   # Site footer
│   │   │   └── navigation.njk # Navigation menu
│   │   └── components/      # Reusable UI components
│   ├── assets/              # Static assets
│   │   ├── images/          # Image files
│   │   ├── js/              # JavaScript files
│   │   └── scss/            # SCSS style files
│   │       ├── main.scss    # Main stylesheet
│   │       ├── _variables.scss # Variables including brand colors
│   │       ├── _typography.scss # Typography styles
│   │       └── components/  # Component-specific styles
│   ├── blog/                # Blog posts
│   │   ├── index.njk        # Blog listing page
│   │   ├── [post-slug].mdx  # Individual blog posts
│   │   └── tags/            # Tag pages
│   │       └── [tag].njk    # Individual tag page
│   ├── portfolio/           # Portfolio projects
│   │   ├── index.njk        # Portfolio listing page
│   │   └── [project-name]/  # Project directories
│   │       ├── index.mdx    # Project details
│   │       └── images/      # Project-specific images
│   └── pages/               # Static pages
│       ├── index.njk        # Homepage
│       ├── about.mdx        # About page
│       └── contact.njk      # Contact page
└── _site/                   # Generated site (not in repository)
```

## Key Files and Their Purposes

### Configuration Files

- `.eleventy.js`: Primary configuration for 11ty, including plugins, collections, filters, and shortcodes.
- `package.json`: Project dependencies and scripts.
- `_data/site.json`: Global site configuration (title, description, URL, etc.).

### Templates

- `src/_includes/layouts/base.njk`: The main layout template that all pages extend.
- `src/_includes/layouts/post.njk`: Template for blog posts.
- `src/_includes/layouts/project.njk`: Template for portfolio projects.

### Style Files

- `src/assets/scss/main.scss`: Main stylesheet that imports all other SCSS files.
- `src/assets/scss/_variables.scss`: Contains brand colors and other CSS variables.

### Content Files

- `src/blog/`: Contains all blog posts as MDX files with front matter.
- `src/portfolio/`: Contains portfolio projects, each in their own directory.
- `src/pages/`: Contains static pages like the homepage, about page, etc.

## Content Creation Workflow

### Adding a Blog Post

1. Create a new MDX file in `src/blog/` with the naming convention `YYYY-MM-DD-post-slug.mdx`.
2. Add front matter with title, date, tags, and excerpt.
3. Write the content in MDX format.

```markdown
---
title: My New Blog Post
date: 2023-08-15
tags:
  - javascript
  - eleventy
excerpt: A brief description of the post
---

# My New Blog Post

Content goes here...
```

### Adding a Portfolio Project

1. Create a new directory in `src/portfolio/` with the project name.
2. Add an `index.mdx` file with project details and front matter.
3. Add project-specific images to the project's directory.

```markdown
---
title: My Project
date: 2023-06-10
featured: true
technologies:
  - JavaScript
  - CSS
  - HTML
thumbnail: ./images/thumbnail.jpg
---

# My Project

Project description and details go here...
```

## Development Workflow

1. Run local development server: `npm run dev`
2. Build for production: `npm run build`
3. Deploy to Vercel (happens automatically when pushing to the main branch)

## Data Flow

1. Global data from `_data/` is available to all templates.
2. Front matter in content files provides page-specific data.
3. Collections (defined in `.eleventy.js`) group content like blog posts and portfolio projects.
4. Templates render content using the data from these sources. 