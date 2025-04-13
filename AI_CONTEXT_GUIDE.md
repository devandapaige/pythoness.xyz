# AI-Friendly Documentation Guide for Projects

## Why Document Your Project for AI Assistance?

Creating well-structured documentation for your project isn't just for human collaborators. Modern AI assistants can provide significantly better help when they have access to clear, organized documentation about your project's:

- Architecture and overall design
- Core components and their relationships
- Implementation decisions and reasoning
- User workflows and features
- Technical requirements and constraints

This guide will help you create documentation specifically optimized for AI assistance throughout your development process.

## Documentation Structure

For maximum AI effectiveness, consider creating the following structure in your project:

```
/project-root
  /ai
    /docs        # Detailed documentation for AI context
    /templates   # Templates for consistent documentation
    plan.md      # Overall project plan and phases
    prd.md       # Product requirements document
```

### Key Documentation Files

1. **Project Plan (`plan.md`)**
   - Overall roadmap and phased approach
   - Major milestones and dependencies
   - Team roles if applicable
   
2. **Product Requirements (`prd.md`)**
   - User stories and requirements
   - Feature specifications
   - Constraints and limitations
   
3. **Documentation by Phase/Feature**
   - Create separate markdown files for each project phase or major feature
   - Name files consistently (e.g., `1-foundation.md`, `2-core-features.md`)
   - Cross-reference between documents to establish relationships

## Documentation Principles

### 1. Single Source of Truth
- Reference code files rather than duplicating their contents
- Point to definitive sources (e.g., "The schema in schema.prisma is the definitive source for data models")
- Update documentation when code changes

### 2. Structure for Skimmability
- Use clear headings and subheadings (H1, H2, H3)
- Include bullet points and numbered lists
- Keep paragraphs short and focused
- Use code blocks with language specifications for code snippets

### 3. Provide Context and Rationale
- Explain _why_ decisions were made, not just what was done
- Document challenges and how they were resolved
- Note alternatives that were considered and rejected

### 4. Cross-Reference Related Information
- Link to other documentation files when referencing related concepts
- Create a network of connected information the AI can navigate

### 5. Keep Documentation Updated
- Update docs immediately after implementing features
- Revise documentation when refactoring or changing approaches
- Mark outdated sections clearly if they can't be updated immediately

## Documentation Templates

Consider creating templates for consistent documentation of:

1. **Phase Documentation** - Overall implementation of a project phase
2. **Feature Documentation** - Detailed documentation of a specific feature
3. **Technical Architecture** - System design and component relationships
4. **User Workflows** - Step-by-step description of how users accomplish tasks

### Phase Documentation Template Structure

```markdown
# Phase X: [Phase Name]

## Overview
Brief description of this phase and its goals.

## Related Documentation
- Links to other relevant documentation

## 1. Component/Feature One
- Objective
- Implementation details
- Challenges and solutions

## 2. Component/Feature Two
- Objective
- Implementation details
- Challenges and solutions

## Summary & Next Steps
What was accomplished and what's coming next.
```

### Feature Documentation Template Structure

```markdown
# Feature: [Feature Name]

## Overview
What this feature does and why it's important.

## Related Documentation
- Links to other relevant docs and code

## Operations/API
- Detailed description of related functions/methods
- Inputs, outputs, and permissions

## User Workflows
- Step-by-step descriptions of user interactions
- Error handling and edge cases

## Future Enhancements
- Planned improvements
- Known limitations
```

## How to Use Documentation with AI Assistants

1. **Reference Documentation in Queries**
   - "Based on the user workflow in `/ai/docs/feature-payments.md`, help me implement..."
   - "According to our architecture in `/ai/docs/technical-architecture.md`..."

2. **Keep Documentation Concise but Complete**
   - AI can process detailed information, but organization matters more than volume
   - Break up documentation into logical files rather than creating massive documents

3. **Update Before Major AI Assistance**
   - Before asking AI for help with complex features, ensure documentation is current
   - Add temporary notes about work-in-progress areas when necessary

4. **Document As You Go**
   - Create documentation templates at the start of new features
   - Fill in details as you implement, while decisions are fresh

## Documentation Anti-Patterns

Avoid these common documentation mistakes that reduce AI effectiveness:

1. **Outdated Information** - Contradictory documentation confuses AI models
2. **Duplicate Details** - Repeated information in multiple places leads to confusion when one copy is updated
3. **Missing Context** - Documents that assume too much background knowledge
4. **Vague Descriptions** - Imprecise language that doesn't clearly specify requirements or behavior
5. **Unexplained Code** - Code snippets without explanation of purpose or function

## Getting Started

1. Create the basic folder structure (`/ai/docs/`)
2. Start with a project plan (`/ai/plan.md`) and requirements (`/ai/prd.md`)
3. Create documentation templates for your specific project needs
4. Document your foundation/architecture before proceeding to specific features
5. Update documentation alongside code changes

Remember: Well-documented projects receive more precise, contextual AI assistance, ultimately saving you development time and reducing errors. 