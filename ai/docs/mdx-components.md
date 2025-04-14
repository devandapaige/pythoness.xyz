# MDX Components

## Overview
This document outlines the custom MDX components available for use in blog posts and portfolio project pages. These components provide enhanced functionality and consistent styling throughout the site.

## MDX Integration with 11ty

MDX allows us to use React components directly in our Markdown content. This is implemented using the `@jamshop/eleventy-plugin-mdx` plugin in our 11ty configuration:

```javascript
// .eleventy.js
const mdxPlugin = require('@jamshop/eleventy-plugin-mdx');

module.exports = function(eleventyConfig) {
  // MDX configuration
  eleventyConfig.addPlugin(mdxPlugin, {
    remarkPlugins: [
      require('remark-slug'),
      require('remark-autolink-headings')
    ],
    rehypePlugins: [
      require('rehype-highlight')
    ],
    components: {
      // Mapping of component names to their implementations
      CodeBlock: './src/_includes/components/CodeBlock.jsx',
      Callout: './src/_includes/components/Callout.jsx',
      ImageWithCaption: './src/_includes/components/ImageWithCaption.jsx',
      TableOfContents: './src/_includes/components/TableOfContents.jsx',
      TechStack: './src/_includes/components/TechStack.jsx',
      ProcessSteps: './src/_includes/components/ProcessSteps.jsx',
      ImageGallery: './src/_includes/components/ImageGallery.jsx',
      VideoDemo: './src/_includes/components/VideoDemo.jsx',
      ProjectResults: './src/_includes/components/ProjectResults.jsx'
    }
  });
  
  // Other configuration...
};
```

## Available Components

### Blog Components

#### CodeBlock

The `CodeBlock` component provides syntax-highlighted code snippets with optional line numbers and titles.

```jsx
// src/_includes/components/CodeBlock.jsx
import React from 'react';

export default function CodeBlock({ 
  children, 
  language = 'javascript', 
  title,
  showLineNumbers = false
}) {
  return (
    <div className="code-block">
      {title && <div className="code-block__title">{title}</div>}
      <pre className={`language-${language} ${showLineNumbers ? 'line-numbers' : ''}`}>
        <code>{children}</code>
      </pre>
    </div>
  );
}
```

Usage in MDX:

```mdx
<CodeBlock language="javascript" title="Example Function" showLineNumbers={true}>
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet('World'));
</CodeBlock>
```

#### Callout

The `Callout` component displays highlighted information boxes for notes, warnings, tips, or important information.

```jsx
// src/_includes/components/Callout.jsx
import React from 'react';

export default function Callout({ children, type = 'info', title }) {
  const types = {
    info: {
      icon: 'ℹ️',
      className: 'callout--info'
    },
    warning: {
      icon: '⚠️',
      className: 'callout--warning'
    },
    danger: {
      icon: '🚫',
      className: 'callout--danger'
    },
    tip: {
      icon: '💡',
      className: 'callout--tip'
    }
  };
  
  const { icon, className } = types[type] || types.info;
  
  return (
    <div className={`callout ${className}`}>
      <div className="callout__header">
        <span className="callout__icon">{icon}</span>
        {title && <span className="callout__title">{title}</span>}
      </div>
      <div className="callout__content">
        {children}
      </div>
    </div>
  );
}
```

Usage in MDX:

```mdx
<Callout type="warning" title="Important Note">
  This feature is currently in beta and may change in future updates.
</Callout>
```

#### ImageWithCaption

The `ImageWithCaption` component displays an image with an optional caption and attribution.

```jsx
// src/_includes/components/ImageWithCaption.jsx
import React from 'react';

export default function ImageWithCaption({ 
  src, 
  alt, 
  caption, 
  credit, 
  creditLink,
  width = '100%'
}) {
  return (
    <figure className="image-with-caption">
      <img 
        src={src} 
        alt={alt} 
        style={{ maxWidth: width }} 
        loading="lazy" 
      />
      {(caption || credit) && (
        <figcaption>
          {caption && <span className="image-caption">{caption}</span>}
          {credit && (
            <span className="image-credit">
              Credit: 
              {creditLink ? (
                <a href={creditLink} target="_blank" rel="noopener noreferrer">{credit}</a>
              ) : credit}
            </span>
          )}
        </figcaption>
      )}
    </figure>
  );
}
```

Usage in MDX:

```mdx
<ImageWithCaption 
  src="/assets/images/example.jpg" 
  alt="An example image" 
  caption="This is an example image showing a concept." 
  credit="Jane Doe" 
  creditLink="https://example.com" 
  width="75%" 
/>
```

#### TableOfContents

The `TableOfContents` component automatically generates a table of contents from the headings in the content.

```jsx
// src/_includes/components/TableOfContents.jsx
import React from 'react';

export default function TableOfContents({ headings, title = "Table of Contents" }) {
  // If headings aren't provided, they'll be extracted from the page in client-side JS
  return (
    <nav className="table-of-contents" aria-labelledby="toc-heading">
      <h2 id="toc-heading" className="table-of-contents__title">{title}</h2>
      <ul className="table-of-contents__list" id="toc-list">
        {headings && headings.map((heading, index) => (
          <li key={index} className={`table-of-contents__item table-of-contents__item--level-${heading.level}`}>
            <a href={`#${heading.id}`}>{heading.text}</a>
          </li>
        ))}
      </ul>
    </nav>
  );
}
```

Usage in MDX:

```mdx
<TableOfContents title="In This Article" />

## First Section
Content here...

## Second Section
More content...
```

### Portfolio Components

#### TechStack

The `TechStack` component displays categorized technology icons and names.

```jsx
// src/_includes/components/TechStack.jsx
import React from 'react';

export default function TechStack({ 
  frontend = [], 
  backend = [], 
  tools = [] 
}) {
  const renderTechList = (techList, title) => {
    if (!techList.length) return null;
    
    return (
      <div className="tech-stack__section">
        <h3 className="tech-stack__title">{title}</h3>
        <ul className="tech-stack__list">
          {techList.map((tech, index) => (
            <li key={index} className="tech-stack__item">
              <span className="tech-stack__icon">{getIconForTech(tech)}</span>
              <span className="tech-stack__name">{tech}</span>
            </li>
          ))}
        </ul>
      </div>
    );
  };
  
  // Helper function to map technology names to icons (simplified example)
  const getIconForTech = (tech) => {
    const iconMap = {
      'React': '⚛️',
      'Node.js': '🟢',
      // Add more mappings as needed
    };
    
    return iconMap[tech] || '🔧';
  };
  
  return (
    <div className="tech-stack">
      {renderTechList(frontend, 'Frontend')}
      {renderTechList(backend, 'Backend')}
      {renderTechList(tools, 'Tools & Infrastructure')}
    </div>
  );
}
```

Usage in MDX:

```mdx
<TechStack
  frontend={['React', 'SCSS', 'TypeScript']}
  backend={['Node.js', 'Express', 'MongoDB']}
  tools={['Webpack', 'Jest', 'GitHub Actions']}
/>
```

#### ProcessSteps

The `ProcessSteps` component displays a visual timeline of project phases or steps.

```jsx
// src/_includes/components/ProcessSteps.jsx
import React from 'react';

export default function ProcessSteps({ steps = [] }) {
  if (!steps.length) return null;
  
  return (
    <div className="process-steps">
      {steps.map((step, index) => (
        <div key={index} className="process-step">
          <div className="process-step__number">{index + 1}</div>
          <div className="process-step__content">
            <h3 className="process-step__title">{step.title}</h3>
            <p className="process-step__description">{step.description}</p>
          </div>
        </div>
      ))}
    </div>
  );
}
```

Usage in MDX:

```mdx
<ProcessSteps
  steps={[
    {
      title: "Research & Planning",
      description: "Market research and requirements gathering"
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
      title: "Testing & Launch",
      description: "Quality assurance and deployment"
    }
  ]}
/>
```

#### ImageGallery

The `ImageGallery` component displays multiple images in a grid or carousel.

```jsx
// src/_includes/components/ImageGallery.jsx
import React from 'react';

export default function ImageGallery({ 
  images = [], 
  layout = 'grid', // 'grid' or 'carousel'
  columns = 2 
}) {
  if (!images.length) return null;
  
  return (
    <div className={`image-gallery image-gallery--${layout}`}>
      <div 
        className="image-gallery__container" 
        style={layout === 'grid' ? { 
          display: 'grid',
          gridTemplateColumns: `repeat(${columns}, 1fr)`,
          gap: '1rem'
        } : {}}
      >
        {images.map((image, index) => (
          <figure key={index} className="image-gallery__item">
            <img 
              src={image.src} 
              alt={image.alt} 
              className="image-gallery__image" 
              loading="lazy" 
            />
            {image.caption && (
              <figcaption className="image-gallery__caption">
                {image.caption}
              </figcaption>
            )}
          </figure>
        ))}
      </div>
    </div>
  );
}
```

Usage in MDX:

```mdx
<ImageGallery
  layout="grid"
  columns={3}
  images={[
    {
      src: "./images/photo1.jpg",
      alt: "Project screenshot 1",
      caption: "Homepage design"
    },
    {
      src: "./images/photo2.jpg",
      alt: "Project screenshot 2",
      caption: "Mobile view"
    },
    {
      src: "./images/photo3.jpg",
      alt: "Project screenshot 3",
      caption: "Admin dashboard"
    }
  ]}
/>
```

#### VideoDemo

The `VideoDemo` component embeds a video player with a caption.

```jsx
// src/_includes/components/VideoDemo.jsx
import React from 'react';

export default function VideoDemo({ 
  src, 
  caption, 
  width = '100%',
  poster,
  autoplay = false
}) {
  // Determine if it's a local video or external (YouTube, Vimeo)
  const isExternal = src.includes('youtube.com') || src.includes('vimeo.com');
  
  return (
    <figure className="video-demo">
      {isExternal ? (
        <div className="video-demo__embed" style={{ width }}>
          <iframe 
            src={src}
            title={caption || "Video demonstration"}
            frameBorder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowFullScreen
          ></iframe>
        </div>
      ) : (
        <video 
          src={src}
          poster={poster}
          controls
          autoPlay={autoplay}
          playsInline
          className="video-demo__player"
          style={{ maxWidth: width }}
        >
          Your browser does not support the video tag.
        </video>
      )}
      {caption && (
        <figcaption className="video-demo__caption">
          {caption}
        </figcaption>
      )}
    </figure>
  );
}
```

Usage in MDX:

```mdx
<VideoDemo 
  src="./videos/demo.mp4" 
  caption="Walk-through of the application" 
  poster="./images/video-thumbnail.jpg" 
/>

<!-- YouTube Example -->
<VideoDemo 
  src="https://www.youtube.com/embed/dQw4w9WgXcQ" 
  caption="Project demonstration on YouTube" 
/>
```

#### ProjectResults

The `ProjectResults` component displays project metrics and outcomes visually.

```jsx
// src/_includes/components/ProjectResults.jsx
import React from 'react';

export default function ProjectResults({ 
  metrics = [], 
  layout = 'grid' // 'grid' or 'list'
}) {
  if (!metrics.length) return null;
  
  return (
    <div className={`project-results project-results--${layout}`}>
      {metrics.map((metric, index) => (
        <div key={index} className="project-result">
          <div className="project-result__value">{metric.value}</div>
          <div className="project-result__label">{metric.label}</div>
        </div>
      ))}
    </div>
  );
}
```

Usage in MDX:

```mdx
<ProjectResults
  metrics={[
    { label: "Conversion Rate", value: "3.2%" },
    { label: "Page Load Time", value: "1.5s" },
    { label: "Mobile Users", value: "65%" },
    { label: "Sales Increase", value: "25%" }
  ]}
/>
```

## Component Styling

All components have corresponding SCSS files in the `src/assets/scss/components/` directory. For example:

```scss
// src/assets/scss/components/_callout.scss

.callout {
  margin: 2rem 0;
  padding: 1.5rem;
  border-radius: 8px;
  border-left: 4px solid var(--color-primary);
  background-color: var(--color-background-alt);
  
  &__header {
    display: flex;
    align-items: center;
    margin-bottom: 0.75rem;
  }
  
  &__icon {
    margin-right: 0.5rem;
    font-size: 1.25rem;
  }
  
  &__title {
    font-weight: 600;
    font-size: var(--font-size-lg);
  }
  
  &__content {
    p:last-child {
      margin-bottom: 0;
    }
  }
  
  // Variants
  &--info {
    border-color: #3498db;
    background-color: rgba(52, 152, 219, 0.1);
  }
  
  &--warning {
    border-color: #f39c12;
    background-color: rgba(243, 156, 18, 0.1);
  }
  
  &--danger {
    border-color: #e74c3c;
    background-color: rgba(231, 76, 60, 0.1);
  }
  
  &--tip {
    border-color: var(--brand-green-accent);
    background-color: rgba(21, 127, 31, 0.1);
  }
}
```

## Accessibility Considerations

All components are designed with accessibility in mind:

1. Proper semantic HTML structure
2. ARIA attributes where needed
3. Keyboard navigation support
4. Sufficient color contrast
5. Alternative text for images
6. Proper heading hierarchy

For example, the `VideoDemo` component includes:
- Captions for context
- Controls for video playback
- Fallback text for browsers that don't support the video tag

## Adding New Components

To add a new component:

1. Create a JSX component file in `src/_includes/components/`
2. Create a corresponding SCSS file in `src/assets/scss/components/`
3. Register the component in the MDX plugin configuration in `.eleventy.js`
4. Import the SCSS file in `main.scss`

Example of registering a new component:

```javascript
// .eleventy.js
eleventyConfig.addPlugin(mdxPlugin, {
  // ...existing configuration
  components: {
    // ...existing components
    NewComponent: './src/_includes/components/NewComponent.jsx'
  }
});
```

## Using Components in Content

Components can be used in any MDX file (blog posts or portfolio projects) using the JSX syntax. For example:

```mdx
---
title: "My Blog Post"
date: 2023-08-15
tags:
  - javascript
  - tutorial
---

# My Blog Post

<Callout type="info" title="Quick Tip">
  You can use components anywhere in your MDX content.
</Callout>

## Introduction

Regular markdown content works as expected.

<ImageWithCaption 
  src="/assets/images/screenshot.jpg" 
  alt="Screenshot of the application" 
  caption="The application dashboard" 
/>

## Code Example

<CodeBlock language="javascript" showLineNumbers={true}>
const greeting = 'Hello, world!';
console.log(greeting);
</CodeBlock>
``` 