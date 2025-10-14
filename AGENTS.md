# AI Agent Guide for Astro Modular Theme

This document contains essential information for AI agents working with this Astro-based blog theme. It combines development practices, folder-based content organization, technical implementation details, and the project's vision for seamless Obsidian-to-web publishing.

## 🚨 CRITICAL: Primary Information Sources

**⚠️ AI AGENTS MUST READ THIS SECTION FIRST ⚠️**

**The most up-to-date and accurate information is found in these blog posts:**

1. **[Getting Started Guide](src/content/posts/getting-started.md)** - Complete setup, configuration, and workflow information
2. **[Astro Suite Vault Guide](src/content/posts/astro-suite-vault-modular-guide.md)** - Obsidian vault configuration and usage

**These blog posts should be treated as the authoritative source of truth.** Use the information in this AGENTS.md file as supplementary technical documentation, but always reference the blog posts for the most current and accurate user-facing information.

**When in doubt, check the blog posts first.**

## 🚨 CRITICAL: Most Common AI Agent Mistakes

**⚠️ READ THESE FIRST - These are the #1 issues that keep coming up:**

1. **🚨 SWUP BREAKS JAVASCRIPT** - Interactive elements stop working after page transitions. [See detailed solution](#-critical-javascript-re-initialization-after-page-transitions)
2. **🚨 MATH RENDERING DUPLICATION** - Math appears twice due to wrong CSS. [See solution](#1--math-rendering-duplication-most-critical)
3. **🚨 PRODUCTION LOGGING** - Never use raw `console.log()` in production code
4. **🚨 IMAGE SYSTEM CONFUSION** - Post cards vs post content images are separate systems
5. **🚨 URL MAPPING SYSTEM CONFUSION** - URL mapping is for rendering only, doesn't affect linked mentions/graph view
6. **🚨 FOLDER-BASED CONTENT ASSUMPTIONS** - ALL content types support folder-based organization, not just posts

**These issues are documented in detail in the [Common AI Agent Mistakes](#common-ai-agent-mistakes) section.**

## Table of Contents

1. [Project Vision & Philosophy](#project-vision--philosophy)
2. [Development Environment](#development-environment)
3. [Content Organization](#content-organization)
4. [Obsidian Integration](#obsidian-integration)
5. [Image Handling](#image-handling)
6. [Mathematical Notation Support](#mathematical-notation-support)
7. [Mermaid Diagram Support](#mermaid-diagram-support)
8. [Build Process](#build-process)
   - [RSS and Atom Feeds](#rss-and-atom-feeds)
9. [Theme Updates](#theme-updates)
10. [Version Management](#version-management)
11. [Configuration & Customization](#configuration--customization)
   - [Typography Configuration](#typography-configuration)
12. [Troubleshooting](#troubleshooting)
13. [Best Practices](#best-practices)
14. [Common AI Agent Mistakes](#common-ai-agent-mistakes)

## Project Vision & Philosophy

### Core Mission
Astro Modular is an Astro blog theme template designed for **Obsidian users**, created by [David V. Kimball](https://davidvkimball.com). This theme turns Obsidian into a powerful CMS for instant blog publishing, bridging the gap between your Obsidian vault and a production-ready blog.

The theme is built on four core principles:

#### **Clarity First**
Content should be the star, not the design. Every element is carefully crafted to enhance readability and focus attention on what matters most - your ideas and writing.

#### **Highly Performant**
Fast loading times and smooth interactions are not optional. Every aspect of this theme has been optimized to deliver exceptional performance across all devices.

#### **Works with Obsidian**
If you use Obsidian, this theme is a natural extension of your workflow. All rich markdown and extended markdown features, including Wikilinks, callouts, and other embedded features, display seamlessly between your vault and published site.

#### **Modular Design**
Every feature can be toggled on or off through a single configuration file. Enable only what you need, keeping your site fast and focused on your specific use case.

### Key Design Principles

#### 1. **Built for Obsidian Users**
- **Direct Obsidian integration** - Write in a dedicated vault, publish to your blog
- **Wikilinks support** - `[[Internal Links]]` and `[[Link|Custom Text]]` work seamlessly
- **Obsidian callouts** - Full support for `> [!note]`, `> [!tip]`, `> [!warning]` and more
- **Tag compatibility** - Your Obsidian tags become blog tags automatically
- **Frontmatter sync** - Compatible metadata structure between Obsidian and Astro
- **Folder-based organization** - Keep content and assets together in dedicated folders
- **Obsidian bracket syntax** - Support for `[[image.jpg]]` syntax in image references
- **[Astro Suite Obsidian Vault](https://github.com/davidvkimball/obsidian-astro-suite) built-in** - Includes Obsidian vault configuration for streamlined publishing

#### 2. **Flexible & Customizable**
- **Modular design** - Each feature can be enabled/disabled independently
- **Multiple color options** - Select from a variety of prebuilt themes (Oxygen, Minimal, Atom, Ayu, Catppuccin, Charcoal, Dracula, Everforest, Flexoki, Gruvbox, macOS, Nord, Obsidian, Rosé Pine, Sky, Solarized, and Things)
- **Custom typography** - Separate font configuration for headings and body text with 20+ supported fonts
- **TypeScript throughout** for type safety and better development experience
- **Command palette** - Press `Ctrl+K` (or custom hotkey) for instant navigation and search
- **Responsive image grids** - Automatic layouts for multiple consecutive images
- **Dark/light themes** - System preference detection with manual toggle
- **SEO ready** - Automatic sitemaps, RSS feeds, and Open Graph images

#### 3. **Content Management Excellence**
- **Markdown-first** with enhanced processing and reading time estimation
- **Folder-based posts** - Organize content and assets in dedicated folders (like Fuwari)
- **Draft support** - Show drafts in development, hide in production
- **Image optimization** with WebP format priority and responsive layouts
- **Table of contents** auto-generation from headings

#### 4. **Navigation & Discovery**
- **Fuzzy search** through all content via command palette
- **Linked mentions** - See which posts reference each other
- **Tag filtering** and next/previous navigation between posts

### Target Audience
- **Obsidian power users** who want to publish their notes
- **Content creators** who prefer markdown-first workflows
- **Developers** who want a customizable, performant blog
- **Writers** who value seamless editing and publishing experience

## Development Environment

### Package Management
- **Package Manager**: This project uses `pnpm` instead of `npm` for all package and script commands
- **Scripts**: All commands should use `pnpm run <script-name>`

### Development Server
- **Command**: `pnpm run dev`
- **Port**: 5000 (with fallback to 5001 if occupied)
- **Host**: localhost
- **Hot Reload**: Enabled with file watching

### 🚨 CRITICAL: Vite File System Security

**⚠️ AI AGENTS MUST READ THIS SECTION CAREFULLY ⚠️**

**NEVER disable `vite.server.fs.strict`** in the Astro configuration. This is a **CRITICAL security requirement**.

#### **Why This Matters**
- **Security**: `fs.strict: false` allows access to files outside the project directory
- **Network Exposure**: Files on your machine become accessible on your network
- **Best Practice**: Vite's strict mode is the default for good security reasons
- **Professional Standards**: Production sites should maintain proper file system boundaries

#### **What NOT to Do**
```javascript
// ❌ WRONG - Never disable Vite strict mode
vite: {
  server: {
    fs: {
      strict: false,  // NEVER DO THIS
      allow: ['..']   // NEVER DO THIS
    }
  }
}
```

#### **What to Do Instead**
- **Keep Vite strict mode enabled** (default behavior)
- **Use proper file paths** within the project directory
- **Handle file system errors gracefully** without compromising security
- **Use environment variables** for development-specific configurations

**This is CRITICAL for maintaining security and professional development standards.**

### 🚨 CRITICAL: Production Logging Guidelines

**⚠️ AI AGENTS MUST READ THIS SECTION CAREFULLY ⚠️**

**NEVER use raw `console.log()` statements in production code.** This project maintains clean console output for professional deployments.

#### **Why This Matters**
- **Production Performance**: Console logs slow down production sites
- **User Experience**: Console spam degrades user experience
- **Professional Standards**: Production sites should have clean console output

#### **Simple Rule**
- **Development**: Console logs are acceptable for debugging
- **Production**: No console output should appear in the final build
- **Use the project's logger utility** (`src/utils/logger.ts`) for any logging needs

### 🎨 CRITICAL: Color Usage Guidelines

**⚠️ AI AGENTS MUST READ THIS SECTION CAREFULLY ⚠️**

This project uses a **dynamic theming system** where colors are defined in theme variables, not hardcoded values. This is **CRITICAL** for maintaining theme consistency and allowing users to switch between different color schemes.

#### **Why This Matters**
- **Theme Consistency**: All colors should work across all 17+ available themes
- **User Experience**: Users can switch themes and colors should adapt automatically
- **Maintainability**: Color changes only need to be made in one place (theme definitions)
- **Professional Standards**: Hardcoded colors break the theming system

#### **How to Use Theme Colors**

**✅ CORRECT - Use theme variables from Tailwind config**
```typescript
// Use Tailwind classes that reference theme variables
@apply bg-primary-50 dark:bg-primary-800
@apply text-primary-900 dark:text-primary-100
@apply border-primary-200 dark:border-primary-600
@apply text-highlight-600 dark:text-highlight-400
```

**❌ WRONG - Never use hardcoded color values**
```typescript
// BAD - Hardcoded colors break theming
background: white;
color: #1f2937;
border: 1px solid #e5e7eb;
box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
```

#### **Available Theme Color Variables**

**Primary Colors** (defined in `src/themes/index.ts`):
- `primary-50` through `primary-950` - Main theme colors
- `primary-100` - Light backgrounds, subtle elements
- `primary-200` - Borders, dividers, table headers
- `primary-300` - Muted text, secondary elements
- `primary-400` - Medium emphasis text
- `primary-500` - Default text color
- `primary-600` - High emphasis text
- `primary-700` - Dark backgrounds, strong elements
- `primary-800` - Darker backgrounds, headers
- `primary-900` - Darkest backgrounds, high contrast text

**Highlight Colors** (accent colors):
- `highlight-50` through `highlight-950` - Accent colors
- `highlight-400` - Links, interactive elements (light mode)
- `highlight-600` - Links, interactive elements (dark mode)
- `highlight-500` - Default highlight color

#### **Theme Color Usage Patterns**

**Backgrounds:**
```typescript
// Light backgrounds
@apply bg-primary-50 dark:bg-primary-800
@apply bg-primary-100 dark:bg-primary-700

// Dark backgrounds  
@apply bg-primary-900 dark:bg-primary-50
@apply bg-primary-800 dark:bg-primary-100
```

**Text Colors:**
```typescript
// High contrast text
@apply text-primary-900 dark:text-primary-50
@apply text-primary-800 dark:text-primary-100

// Medium emphasis text
@apply text-primary-600 dark:text-primary-300
@apply text-primary-500 dark:text-primary-400

// Muted text
@apply text-primary-400 dark:text-primary-500
```

**Borders and Dividers:**
```typescript
// Subtle borders
@apply border-primary-200 dark:border-primary-600
@apply border-primary-300 dark:border-primary-500

// Strong borders
@apply border-primary-400 dark:border-primary-400
```

**Interactive Elements:**
```typescript
// Links and buttons
@apply text-highlight-600 dark:text-highlight-400
@apply hover:text-highlight-700 dark:hover:text-highlight-300

// Focus states
@apply focus:ring-highlight-500 dark:focus:ring-highlight-400
```

#### **Where to Find Theme Definitions**

1. **Main Theme Config**: `src/themes/index.ts` - Contains all theme definitions
2. **Tailwind Config**: `tailwind.config.mjs` - Maps theme variables to Tailwind classes
3. **Current Theme**: `src/config.ts` - Shows which theme is currently active

#### **Action Required for AI Agents**

1. **ALWAYS** use Tailwind classes that reference theme variables
2. **NEVER** use hardcoded color values like `#ffffff`, `rgb(255, 255, 255)`, or `white`
3. **CHECK** existing code for hardcoded colors and replace them
4. **USE** the dark mode variants (`dark:`) for all color definitions
5. **REFERENCE** the theme files to understand available color scales

#### **Example Conversion**

```typescript
// Before (BAD - hardcoded colors)
.table-wrapper {
  background: white;
  border: 1px solid #e5e7eb;
  color: #1f2937;
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}

.dark .table-wrapper {
  background: #1e293b;
  border-color: #475569;
  color: #f1f5f9;
}

// After (GOOD - theme variables)
.table-wrapper {
  @apply bg-white dark:bg-primary-800;
  @apply border border-primary-200 dark:border-primary-600;
  @apply text-primary-900 dark:text-primary-100;
  @apply shadow-sm;
}
```

**This is CRITICAL for maintaining the theming system and user experience across all themes.**

### Key Scripts
```bash
pnpm run dev              # Start development server
pnpm run build            # Build for production
pnpm run check-images     # Check for missing images
pnpm run sync-images      # Sync images from content to public
pnpm run process-aliases  # Process content aliases
pnpm run generate-redirects # Generate redirects
```


## Content Organization

### Folder-Based Posts Structure

The theme supports both traditional single-file posts and folder-based posts for better asset organization.

#### Traditional Posts
```
src/content/posts/
├── traditional-post.md
└── another-post.md
```

#### Folder-Based Posts
```
src/content/posts/
├── traditional-post.md
├── folder-based-post/
│   ├── index.md          # Main content file
│   ├── hero-image.jpg    # Assets co-located
│   ├── attachments/      # Optional: Obsidian subfolder setting
│   │   ├── cover.png
│   │   └── diagram.svg
│   ├── gallery-1.jpg
│   ├── diagram.png
│   └── document.pdf
└── another-folder-post/
    ├── index.md
    └── cover.jpg
```

### URL Generation
- **Folder name becomes the slug**: `folder-based-post` → `/posts/folder-based-post/`
- **index.md contains the content**: Main content goes in the `index.md` file
- **Assets are co-located**: All related files stay in the same folder

### Folder-Based Content Support

**All content types support folder-based organization:**

#### **Posts** (`src/content/posts/`)
- **Folder-based**: `folder-based-post/index.md` → `/posts/folder-based-post/`
- **Single-file**: `traditional-post.md` → `/posts/traditional-post/`

#### **Pages** (`src/content/pages/`)
- **Folder-based**: `folder-based-page/index.md` → `/folder-based-page/`
- **Single-file**: `about.md` → `/about/`

#### **Projects** (`src/content/projects/`)
- **Folder-based**: `project-name/index.md` → `/projects/project-name/`
- **Single-file**: `project.md` → `/projects/project/`

#### **Documentation** (`src/content/docs/`)
- **Folder-based**: `guide-name/index.md` → `/docs/guide-name/`
- **Single-file**: `guide.md` → `/docs/guide/`

**Key Benefits:**
- **Asset Co-location**: Images, PDFs, and other files stay with content
- **Obsidian Compatibility**: Works seamlessly with Obsidian's folder-based organization
- **Clean URLs**: Folder name becomes the URL slug automatically
- **Flexible Organization**: Mix single-file and folder-based content as needed

**Technical Implementation:**
- **Astro Content Collections**: Automatically handles folder-based content
- **Asset Syncing**: `scripts/sync-images.js` copies assets to public directory
- **URL Generation**: Folder name becomes slug, `index.md` provides content
- **Image Processing**: Co-located images work with both `[[image.jpg]]` and `![image](image.jpg)` syntax

### Obsidian Subfolder Support
The theme supports Obsidian's "subfolder" setting where attachments are stored in an `attachments/` subfolder within folder-based content:

```
src/content/posts/my-post/
├── index.md
├── attachments/      # Obsidian subfolder setting
│   ├── cover.png
│   └── diagram.svg
└── other-file.jpg    # Co-located assets still work
```

**How it works:**
- Images in `attachments/` subfolders are automatically processed
- They're copied to the public directory without the `attachments/` prefix
- Both `attachments/image.png` and `image.png` references work seamlessly
- This prevents breaking when users switch between Obsidian's subfolder settings

### Content Types

The theme supports multiple content types for different purposes:

#### Posts (`src/content/posts/`)
- **Purpose**: Blog posts and articles
- **URL Structure**: `/posts/post-slug`
- **Features**: Tags, reading time, linked mentions, comments
- **Organization**: Single files or folder-based with co-located assets

#### Pages (`src/content/pages/`)
- **Purpose**: Static pages (About, Contact, etc.)
- **URL Structure**: `/page-slug`
- **Features**: Simple content without blog-specific features
- **Organization**: Single files or folder-based with co-located assets

#### Projects (`src/content/projects/`)
- **Purpose**: Portfolio items, side projects, work showcases
- **URL Structure**: `/projects/project-slug`
- **Features**: Categories, status, repository/demo links, featured flag
- **Organization**: Single files or folder-based with co-located assets
- **Frontmatter**: `title`, `description`, `date`, `categories`, `repositoryUrl`, `demoUrl`, `status`, `image`, `imageAlt`, `hideCoverImage`, `draft`, `noIndex`, `featured`

#### Documentation (`src/content/docs/`)
- **Purpose**: Technical documentation, guides, API references
- **URL Structure**: `/docs/doc-slug`
- **Features**: Categories, version control, table of contents, featured flag
- **Organization**: Single files or folder-based with co-located assets
- **Frontmatter**: `title`, `description`, `category`, `order`, `lastModified`, `version`, `image`, `imageAlt`, `hideCoverImage`, `hideTOC`, `draft`, `noIndex`, `showTOC`, `featured`

### Category Logic

The theme includes intelligent category handling that adapts based on your content:

#### Projects Categories
- **If NO projects have categories**: Hide the entire category system
  - Remove category display from project cards
  - Remove category display from individual project pages  
  - Remove the category filtering block from the projects index page
- **If SOME projects have categories**: Show categories normally

#### Documentation Categories
- **If NO docs have categories**: Remove category system entirely
  - Remove category headings and TOC-like structure on the docs index page
  - Just list docs by their `order` number
- **If SOME docs have categories**: 
  - Docs with categories go to their assigned category
  - Docs without categories go to a new "Unsorted" category
  - Keep the category structure

#### Individual Documentation TOC Control
- **`hideTOC` field**: Each doc can hide its table of contents with `hideTOC: true` in frontmatter
- **Separate from global setting**: This is independent of the global posts TOC setting
- **Default behavior**: TOC shows unless explicitly hidden

### Content Schema

#### Posts Collection
```typescript
{
  title: string;
  description: string;
  date: Date;
  tags?: string[];
  draft?: boolean;
  image?: string;
  imageOG?: boolean;
  imageAlt?: string;
  hideCoverImage?: boolean;
  targetKeyword?: string;
  // Note: author is global via siteConfig.author, not per-post
  // Note: noIndex is available but typically used for pages only
}
```

#### Pages Collection
```typescript
{
  title: string;
  description: string;
  draft?: boolean;
  lastModified?: Date;
  image?: string;
  imageAlt?: string;
  hideCoverImage?: boolean;
  hideTOC?: boolean;
  noIndex?: boolean;  // Commonly used for pages
}
```

#### Projects Collection
```typescript
{
  title: string;
  description: string;
  date: Date;
  categories?: string[];
  repositoryUrl?: string;
  demoUrl?: string;
  status?: 'in-progress' | 'completed';
  image?: string;
  imageAlt?: string;
  hideCoverImage?: boolean;
  hideTOC?: boolean;
  draft?: boolean;
  noIndex?: boolean;
  featured?: boolean;
}
```

#### Documentation Collection
```typescript
{
  title: string;
  description: string;
  category: string;
  order: number;
  lastModified?: Date;
  version?: string;
  image?: string;
  imageAlt?: string;
  hideCoverImage?: boolean;
  hideTOC?: boolean;
  draft?: boolean;
  noIndex?: boolean;
  showTOC?: boolean;
  featured?: boolean;
}
```

### H1 Title Handling - CRITICAL DISTINCTION

**IMPORTANT**: Both Posts and Pages now handle H1 titles the same way:

#### Posts (PostLayout)
- **H1 is HARDCODED** in the layout using `{post.data.title}` from frontmatter
- **NO H1 in markdown content** - posts should NOT have `# Title` in their markdown
- **Layout controls styling** - H1 styling is handled by the PostLayout component
- **Example**: Post frontmatter has `title: "My Post"` → Layout renders `<h1>My Post</h1>`

#### Pages (PageLayout)  
- **H1 is HARDCODED** in the layout using `{page.data.title}` from frontmatter
- **NO H1 in markdown content** - pages should NOT have `# Title` in their markdown
- **Layout controls styling** - H1 styling is handled by the PageLayout component
- **Example**: Page frontmatter has `title: "About"` → Layout renders `<h1>About</h1>`

#### Why This Matters
- **Both Posts and Pages**: Title comes from frontmatter, layout handles presentation
- **Content starts with H2**: Since H1 is hardcoded in the layout, all content should start with `##` headings
- **Consistency**: Both use the same approach for titles and content structure
- **AI Agents**: NEVER add H1 to any markdown content - both posts and pages have hardcoded H1s from frontmatter

### Table of Contents (TOC) Behavior - CRITICAL DISTINCTION

**IMPORTANT**: Posts have different TOC behavior than other content types:

#### Posts (PostLayout)
- **Uses global setting**: Controlled by `siteConfig.postOptions.tableOfContents` in `config.ts`
- **Can be overridden**: Use `hideTOC: true` in post frontmatter to hide TOC
- **Global control**: All posts respect the global setting unless individually overridden
- **Example**: If `tableOfContents: false` in config, all posts hide TOC unless `hideTOC: false` in frontmatter

#### Pages, Projects, Documentation (PageLayout, ProjectLayout, DocumentationLayout)
- **Independent TOC logic**: NOT affected by global `postOptions.tableOfContents` setting
- **Default behavior**: TOC shows by default (equivalent to `true`)
- **Frontmatter control**: Use `hideTOC: true` to hide TOC, `showTOC: false` to hide TOC
- **Per-content control**: Each content type has its own TOC behavior independent of posts

#### Why This Matters
- **Posts**: Global setting controls all posts, individual posts can override
- **Other content types**: Each has independent TOC behavior, not affected by posts setting
- **AI Agents**: Don't assume all content types use the same TOC logic - posts are different
- **Configuration**: Only `postOptions.tableOfContents` affects posts, not other content types

## Obsidian Integration

For complete Obsidian setup and usage instructions, see the [Astro Suite Vault Guide](src/content/posts/astro-suite-vault-modular-guide.md).

### Astro Suite Vault Philosophy
The included Obsidian vault follows three core principles:
1. **Plug-and-play Astro blogging experience**
2. **Emphasis on modularity**
3. **Customize your experience to get it just right**

### Vault Setup & Configuration

#### Theme & Visual Experience
- **Minimal Theme** - Understated color scheme with high contrast options
- **Minimal Theme Settings** - Complete control over your experience
- **Hider Plugin** - Remove distracting UI elements
- **Style Settings** - Fine-tune visual appearance
- **Custom CSS Snippets** - Optional enhancements for window management and mobile interface
  - `astro-modular-styling` - Gives embedded HTML elements similar look to frontend
  - `custom-draggable-top-area.css` - Makes window moving easier when frame is hidden
  - `hide-properties-heading-and-add-properties-button.css` - Alternative to Style Settings plugin
  - `hide-tabs-icon-mobile.css` - Removes tabs icon in mobile version
  - `hide-longpress-flair-mobile.css` - Simplifies mobile interface
  - `hide-header-title-mobile.css` - Simplifies mobile interface

#### Essential Hotkeys for AI Agents
When working with the Obsidian vault, these hotkeys are crucial:
- **Toggle left side panel**: `CTRL + ALT + Z`
- **Toggle right side panel**: `CTRL + ALT + X`
- **Toggle tab bar**: `CTRL + ALT + S`
- **Navigate back**: `ALT + ←`
- **Navigate forward**: `ALT + →`
- **Open homepage**: `CTRL + M`
- **Add property**: `CTRL + ;`
- **Toggle reading view**: `CTRL + E`
- **Toggle Zen mode**: `CTRL + SHIFT + Z`
- **Insert image**: `CTRL + '`
- **Insert callout**: `CTRL + SHIFT + C`
- **Rename note**: `CTRL + R`
- **Start Terminal**: `CTRL + SHIFT + D`
- **Open config file**: `CTRL + SHIFT + ,`
- **Git Commit and Sync**: `CTRL + SHIFT + S`

*Note: On Mac, `CTRL` = `CMD`*

### Key Plugins for Content Creation

#### Astro Composer
- **Purpose**: Easily create new notes as Astro blog posts
- **Functionality**: 
  - Creates kebab-case filenames from titles automatically
  - Supports `CTRL + R` for easy renaming
  - Generates default properties automatically
  - Works with both wikilinks and standard markdown links
  - Supports "Standardize properties" command for organizing properties
  - Allows copying heading links by right-clicking headings
- **Critical**: Unlike other themes, this theme supports **any internal link that works with Obsidian** without conversion

#### Homepage and Default New Tab Page
- **Home Base**: Default screen shows a `.base` file with all blog posts in reverse-chronological order
- **Location**: Nested in `_bases` folder (underscore prefix prevents Astro processing)
- **Customization**: Note properties in views can be customized

#### Content Management Plugins
- **Paste Image Rename**: Quickly rename images with kebab-case, SEO-friendly names
- **Image Inserter**: Pull images from Unsplash with `CTRL + '`
- **Title-Only Tab**: Uses `title` property instead of filename for tabs
- **Property Over Filename**: Use `title` property as primary identifier for linking/searching
- **Alias Filename History**: Stores old filenames as aliases for URL redirects
  - Automatically stores old filenames when renaming posts/pages
  - Configurable regex for ignoring names (like `Untitled` or `_` prefix)
  - Adjustable timeout and parent folder name tracking

#### Focus & Productivity
- **ProZen**: Full-screen writing mode with `CTRL + SHIFT + Z`
- **Disable Tabs**: Optional - new tabs replace current ones (great with hidden tab bar)
- **Shell Commands**: Quick access to terminal and config file
- **BRAT (Temporary)**: Used temporarily to load plugins before they're available in the official directory

### Git Integration
- **Git Plugin**: Publish to Astro blog without leaving Obsidian
- **Command**: `CTRL + SHIFT + S` for commit and sync
- **Configuration**: Requires git setup to enable

### Content Workflow
1. **Write in Obsidian** using the configured vault
2. **Use wikilinks** for internal connections (`[[Post Title]]` or `[[Post Title|Custom Text]]`)
3. **Add images** with `CTRL + '` and automatic SEO-friendly naming
4. **Create callouts** with `CTRL + SHIFT + C`
5. **Publish** with `CTRL + SHIFT + S` (git commit and sync)
6. **Content appears** on your Astro blog automatically

### Linking Behavior: Wikilinks vs Standard Links

This theme supports two distinct linking behaviors, each with specific use cases:

#### **Wikilinks (Obsidian-style) - Posts Only**
- **Syntax**: `[[Post Title]]` or `[[Post Title|Custom Text]]`
- **Purpose**: Obsidian's special linking syntax for seamless vault-to-blog publishing
- **Scope**: **ONLY works with posts collection**
- **Examples**:
  - `[[Getting Started]]` → `/posts/getting-started`
  - `[[My Post|Custom Link Text]]` → `/posts/my-post` with custom display text
  - `![[image.jpg]]` → Image reference (works in any content type)
- **Why posts only**: Maintains simplicity and matches Obsidian's primary use case for blog content

#### **Standard Markdown Links - All Content Types**
- **Syntax**: `[text](url)` 
- **Purpose**: Standard markdown linking that works everywhere
- **Scope**: **Works with ALL content types** (posts, pages, projects, docs)
- **Examples**:
  - `[Post Title](posts/post-slug)` or `[Post Title](post-slug)` → Posts
  - `[Page Title](pages/page-slug)` or `[Page Title](page-slug)` → Pages  
  - `[Project Title](projects/project-slug)` → Projects
  - `[Doc Title](docs/doc-slug)` → Documentation
  - `[Home](special/home)` or `[Home](homepage)` → Special pages

#### **When to Use Which**
- **Use Wikilinks** when writing in Obsidian for posts - they feel natural and work seamlessly
- **Use Standard Links** when linking between different content types or when you need explicit control
- **Both work together** - you can mix wikilinks and standard links in the same document

#### **Technical Implementation**
- **File**: `src/utils/internallinks.ts` (renamed from `wikilinks.ts` for clarity)
- **Wikilink Processing**: `remarkWikilinks()` - handles `[[...]]` syntax (posts only)
- **Standard Link Processing**: `remarkStandardLinks()` - handles `[text](url)` syntax (all content types)
- **Combined Processing**: `remarkInternalLinks()` - combines both for Astro configuration

### URL Mapping System for Obsidian Compatibility

The theme includes a sophisticated URL mapping system that ensures Obsidian-style relative links work seamlessly on the live website while maintaining full Obsidian compatibility.

#### **Purpose**
- **Obsidian Compatibility**: Links written in Obsidian work naturally without conversion
- **Live Site Functionality**: Relative links resolve correctly on the published website
- **Seamless Publishing**: Write in Obsidian, publish to blog with identical link behavior

#### **Supported URL Mappings**

**Pages Collection Mapping:**
- `[About](/pages/about)` → `/about` (removes `/pages` prefix)
- `[About](/pages/about/)` → `/about` (handles trailing slashes)
- `[Contact](pages/contact)` → `/contact` (handles both leading slash variants)

**Special Pages Mapping:**
- `[Home](/special/home)` → `/` (homepage)
- `[Home](/special/home/)` → `/` (handles trailing slashes)
- `[404 Page](/special/404)` → `/404` (404 error page)
- `[Projects](/special/projects)` → `/projects` (projects index)
- `[Docs](/special/docs)` → `/docs` (documentation index)

**Direct Index Mapping:**
- `[Home](/index)` → `/` (homepage)
- `[Home](/index/)` → `/` (handles trailing slashes)

#### **Technical Implementation**

**Core Functions:**
- **`mapRelativeUrlToSiteUrl()`**: Main URL mapping function in `src/utils/internallinks.ts`
- **`extractLinkTextFromUrlWithAnchor()`**: Processes URLs and extracts link text
- **`remarkStandardLinks()`**: Applies URL mapping during markdown processing

**Processing Flow:**
1. **Link Detection**: `isInternalLink()` identifies relative links
2. **URL Mapping**: `mapRelativeUrlToSiteUrl()` transforms Obsidian paths to site URLs
3. **Rendering**: `remarkStandardLinks()` applies mappings during HTML generation
4. **Anchor Handling**: Anchors (`#section`) are preserved and properly formatted

#### **Linked Mentions & Graph View Integration**

**Important Distinction**: URL mapping is applied for **rendering only**. The linked mentions and graph view features maintain their **posts-only** filtering:

- **URL Mapping**: Applied to all content types for proper rendering
- **Linked Mentions**: Only includes posts (filtered by `isPostLink` logic)
- **Graph View**: Only includes posts (filtered by `isPostLink` logic)

**Why This Design:**
- **Rendering**: All links work correctly on the live site
- **Features**: Linked mentions and graph view remain focused on blog content
- **Performance**: No unnecessary processing of non-post content for these features

#### **Edge Cases Handled**

**Trailing Slashes:**
- `/pages/about/` → `/about`
- `/special/home/` → `/`
- `/index/` → `/`

**Mixed Formats:**
- Both `/pages/about` and `pages/about` work identically
- Both `/special/home` and `special/home` work identically
- Obsidian's flexible linking is fully supported

**Anchor Preservation:**
- `[About](/pages/about#section)` → `/about#section`
- `[Home](/special/home#top)` → `/#top`
- Anchors are properly slugified for web compatibility

#### **Best Practices for AI Agents**

**URL Mapping Implementation:**
- **Always use `mapRelativeUrlToSiteUrl()`** for URL transformations
- **Test both leading slash variants** (`/pages/about` and `pages/about`)
- **Handle trailing slashes** in all mapping logic
- **Preserve anchors** during URL transformation

**Content Creation:**
- **Use Obsidian-style links** - they work seamlessly on the live site
- **Test URL mappings** with various link formats
- **Don't manually convert links** - the system handles it automatically
- **Include examples** in documentation for all supported formats

**Performance Considerations:**
- **URL mapping is lightweight** - minimal performance impact
- **Caching friendly** - mappings are applied during build time
- **No runtime overhead** - all processing happens during markdown compilation

### Automatic Aliases & Redirects
When you rename a post or page in Obsidian, the old filename is automatically stored as an alias. Astro processes these aliases and creates redirect rules, so old URLs continue to work. You don't need to add aliases manually - they appear automatically when you use Obsidian's rename functionality.

### Content Collections Structure

The theme uses Astro's content collections system with predefined collections:

#### **Standard Collections**
```
src/content/
├── posts/                   # Blog posts collection
│   ├── attachments/           # Shared post images
│   ├── getting-started.md   # File-based post
│   ├── sample-folder-post/  # Folder-based post
│   │   ├── index.md         # Main content file
│   │   ├── hero-image.jpg   # Post-specific assets
│   │   ├── diagram.png
│   │   └── document.pdf
│   └── another-post/        # Another folder-based post
│       ├── index.md
│       └── cover.jpg
├── pages/                   # Static pages collection
│   ├── attachments/           # Shared page images
│   ├── about.md
│   ├── contact.md
│   └── privacy.md
├── projects/                # Projects collection
│   ├── attachments/           # Shared project images
│   ├── project-1.md
│   └── project-2/
│       ├── index.md
│       └── screenshot.png
├── docs/                    # Documentation collection
│   ├── attachments/           # Shared doc images
│   ├── guide-1.md
│   └── guide-2/
│       ├── index.md
│       └── diagram.png
└── special/                 # Special pages collection
    ├── home.md             # Homepage blurb content
    ├── 404.md               # 404 page content
    ├── projects.md          # Projects index page content
    └── docs.md              # Docs index page content
```

#### **Special Collection**

The `special` collection contains content for specific pages that have special routing logic:

**Purpose**: Provides content for pages that need custom behavior or fixed URLs

**Files and Their Uses**:
- **`home.md`** - Homepage blurb content (controlled by `homeOptions.blurb.placement`)
- **`404.md`** - 404 error page content
- **`projects.md`** - Projects index page content (if `homeOptions.projects.enabled: true`)
- **`docs.md`** - Documentation index page content (if `homeOptions.docs.enabled: true`)

**URL Mapping**:
- `special/home.md` → `/` (homepage blurb)
- `special/404.md` → `/404` (404 error page)
- `special/projects.md` → `/projects` (projects index)
- `special/docs.md` → `/docs` (documentation index)

**Important Notes**:
- These files have **fixed URLs** determined by their filename, not frontmatter
- They use a simplified schema (title, description, hideTOC only)
- They're processed by the `[...slug].astro` catch-all route
- They're excluded from the main pages collection to avoid conflicts

#### **Custom Collections**

**Recommended Approach: Use Subfolders Within Pages Collection**

Instead of creating custom collections at the content level (which triggers Astro's auto-generation warnings), use subfolders within the existing `pages` collection:

**Example Structure**:
```
src/content/pages/
├── about.md
├── contact.md
├── services/
│   ├── web-development.md
│   ├── consulting.md
│   └── design.md
└── products/
    ├── software-solutions.md
    └── consulting-services.md
```

**URL Generation**:
- `src/content/pages/services/web-development.md` → `/services/web-development`
- `src/content/pages/services/consulting.md` → `/services/consulting`
- `src/content/pages/products/software-solutions.md` → `/products/software-solutions`

**Alternative: Nested Folders with index.md**:
```
src/content/pages/services/
├── web-development/
│   └── index.md
└── consulting/
    └── index.md
```

**Same URL Results**:
- `src/content/pages/services/web-development/index.md` → `/services/web-development`
- `src/content/pages/services/consulting/index.md` → `/services/consulting`

**Benefits of This Approach**:
- **No Astro warnings** - works within existing pages collection
- **No configuration needed** - uses standard Astro folder-based routing
- **Same URL structure** - creates the same `/services/...` URLs you want
- **Easy linking** - all internal links work automatically
- **Standard frontmatter** - uses the same schema as regular pages

**Linking to Custom Pages**:
- **From other pages**: `[Web Development](/services/web-development)`
- **From posts**: `[Our Services](/services/consulting)`
- **From any content**: Standard markdown links work out of the box

**Why This Design**:
- **Avoids Astro auto-generation** - no deprecation warnings
- **Maximum flexibility** - create any URL structure you want
- **Zero configuration** - works out of the box
- **Clean and simple** - leverages Astro's built-in folder routing

### Vault Structure
```
src/content/
├── posts/                   # Blog posts
├── pages/                   # Static pages  
├── projects/                # Projects
├── docs/                    # Documentation
├── special/                 # Special pages
└── .obsidian/               # Obsidian vault setup
    ├── plugins/             # Configured plugins
    ├── themes/              # Minimal theme
    └── snippets/            # Custom CSS snippets
```

## Page Transitions with Swup

### Swup Integration

This project uses **Swup** for client-side page transitions to provide a smooth, app-like navigation experience.

#### Configuration
- **Location**: `astro.config.mjs` - Swup is configured as an Astro integration
- **Accessibility**: Currently disabled (`accessibility: false`) to prevent invalid `tabindex` attributes on body elements
- **Containers**: Uses `#swup-container` as the main content container
- **Smooth Scrolling**: Disabled (handled by custom implementation)
- **Caching**: Enabled for better performance
- **Preloading**: Enabled for faster navigation
- **Back/Forward Navigation**: Completely disabled for Swup (`skipPopStateHandling: true`) to let browser handle naturally

#### 🚨 CRITICAL: Scroll Behavior Issues

**⚠️ AI AGENTS MUST READ THIS SECTION CAREFULLY ⚠️**

**The most common and frustrating issue with Swup is scroll behavior during back/forward navigation.** This section documents the exact cause and solution.

#### **The Problem: "Dumb Little Scroll Thing"**

When users navigate back/forward using browser buttons, they experience:
- Page jumps to top, then slowly scrolls down
- Unnatural scroll behavior that feels broken
- Loss of scroll position restoration

#### **Root Cause: `handleInitialHashScroll()` Function**

The issue is caused by the `handleInitialHashScroll()` function in `BaseLayout.astro` being called in the `visit:end` hook:

```javascript
// ❌ WRONG - This causes scroll issues
swup.hooks.on('visit:end', () => {
  // ... other code ...
  handleInitialHashScroll(); // ← THIS IS THE CULPRIT
});
```

**What `handleInitialHashScroll()` does:**
1. Calls `window.scrollTo(0, 0)` - Forces scroll to top
2. Performs custom smooth scroll animation
3. Interferes with browser's natural scroll restoration

#### **The Solution: Remove from `visit:end` Hook**

**✅ CORRECT - Fixed version:**
```javascript
swup.hooks.on('visit:end', () => {
  // Update navigation highlighting after page transition is complete
  updateNavigationHighlighting();
  // Initialize linked mentions after page transition is complete
  if (window.initializeLinkedMentions) {
    window.initializeLinkedMentions();
  }
  // Don't call handleInitialHashScroll() here - it interferes with back/forward navigation
});
```

#### **Why This Works**

1. **Browser handles scroll naturally** - No JavaScript interference
2. **Back/forward navigation preserved** - Browser's scroll restoration works
3. **No forced scrolling** - No `window.scrollTo(0, 0)` calls
4. **No custom animations** - No interference with natural behavior

#### **Additional Swup Scroll Configuration**

The project also uses these Swup configurations to prevent scroll issues:

```javascript
// In astro.config.mjs
swup({
  smoothScrolling: false,           // Disable Swup's smooth scrolling
  plugins: [],                      // Disable all plugins including scroll
  skipPopStateHandling: (event) => {
    // ALWAYS skip Swup handling for back/forward navigation
    return true;
  }
})
```

#### **🚨 CRITICAL: JavaScript Re-initialization After Page Transitions**

**⚠️ AI AGENTS MUST READ THIS SECTION CAREFULLY ⚠️**

**The #1 most common issue with Swup is that JavaScript stops working after page transitions.** This is because Swup replaces DOM content without triggering `DOMContentLoaded`, so event listeners and component initialization are lost.

#### **The Problem: "It works on first load but not after navigation"**

When users navigate between pages, they experience:
- Interactive elements stop responding to clicks
- Event listeners are lost after page transitions
- Components appear but don't function
- "It works on first load but not after navigation"

#### **Root Cause: DOM Content Replacement**

Swup replaces the content inside `#swup-container` without triggering `DOMContentLoaded`, so:
1. **Event listeners are lost** - They're attached to DOM elements that no longer exist
2. **Component initialization is skipped** - `DOMContentLoaded` doesn't fire again
3. **JavaScript state is lost** - Any component state is reset

#### **The Solution: Re-initialize After Every Page Transition**

**✅ CORRECT - Component Pattern:**
```javascript
// In component files (e.g., TableOfContents.astro, CommandPalette.astro)
function initializeMyComponent() {
  // Remove existing event listeners to prevent duplicates
  const existingElement = document.querySelector('.my-component');
  if (existingElement) {
    const newElement = existingElement.cloneNode(true);
    existingElement.parentNode?.replaceChild(newElement, existingElement);
  }
  
  // Get fresh references after cloning
  const element = document.querySelector('.my-component');
  if (element) {
    element.addEventListener('click', handleClick);
    // ... other initialization
  }
}

// Make function globally available
window.initializeMyComponent = initializeMyComponent;

// Initialize on DOMContentLoaded
document.addEventListener('DOMContentLoaded', initializeMyComponent);
```

**✅ CORRECT - BaseLayout.astro Integration:**
```javascript
// In BaseLayout.astro - add to BOTH hooks
window.swup.hooks.on('page:view', () => {
  // ... other code ...
  if (window.initializeMyComponent) {
    window.initializeMyComponent();
  }
});

window.swup.hooks.on('visit:end', () => {
  // ... other code ...
  if (window.initializeMyComponent) {
    window.initializeMyComponent();
  }
});
```

#### **Why This Works**

1. **Global functions** - Available to Swup hooks from any component
2. **Event listener cleanup** - Prevents duplicate listeners by cloning elements
3. **Fresh DOM references** - Gets new elements after content replacement
4. **Both hooks** - Ensures initialization happens regardless of transition timing

#### **Common Components That Need This Pattern**

- **Table of Contents** - Collapse/expand functionality
- **Command Palette** - Search and navigation
- **Theme Toggle** - Dark/light mode switching
- **Mobile Menu** - Navigation menu interactions
- **Image Galleries** - Lightbox and gallery controls
- **Mermaid Diagrams** - Diagram rendering and interactions
- **Linked Mentions** - Post connection highlighting

#### **Important Notes for AI Agents**
- **NEVER call `handleInitialHashScroll()` in Swup hooks** - It breaks back/forward navigation
- **ALWAYS re-initialize JavaScript after page transitions** - Use the pattern above
- **Let browser handle scroll restoration** - Don't interfere with natural behavior
- **Accessibility Warnings**: Swup can add `tabindex` attributes to body elements, causing accessibility warnings. This is why `accessibility: false` is set in the config.
- **Container Structure**: All page content must be wrapped in `#swup-container` for transitions to work
- **Image Loading**: Swup doesn't interfere with image loading attributes - these are handled by the PostCard and ImageWrapper components
- **Navigation**: Internal links automatically use Swup transitions when available

#### **Swup Hooks and Custom Behavior**
The project includes custom Swup behavior in `BaseLayout.astro`:
- **Scroll Management**: Custom scroll behavior to prevent unwanted scrolling during transitions
- **Content Replacement**: Handles content updates after page transitions
- **Mobile Menu**: Closes mobile menu on navigation
- **NO scroll interference**: Back/forward navigation is handled entirely by the browser

## Image Handling

### Critical Distinction: Post Cards vs Post Content Images

**IMPORTANT**: There are two completely separate image systems in this project:

#### 1. Post Card Images (Listings, Homepage, Tag Pages)
- **Controlled by**: `siteConfig.postOptions.showPostCardCoverImages` in `config.ts`
- **Options**: `"all"`, `"latest"`, `"home"`, `"posts"`, `"latest-and-posts"`, `"none"`
- **Current Setting**: `"latest-and-posts"` (shows on latest posts and posts/tags pages)
- **Frontmatter**: The `image` field in post frontmatter is used for card images
- **NOT affected by**: `hideCoverImage` frontmatter field
- **Loading**: Uses `eager` loading for first post on pages, `lazy` for others

#### 2. Post Content Images (Inside Individual Posts)
- **Controlled by**: `hideCoverImage` frontmatter field
- **Purpose**: Controls whether the main post image appears in the post content
- **Loading**: Always uses `eager` loading and `fetchpriority="high"`
- **Location**: Rendered by `PostContent.astro` component

#### Key Rules for AI Agents
- **Never confuse these two systems** - they are completely independent
- **Post card visibility** is controlled by `showPostCardCoverImages` config, not frontmatter
- **Post content visibility** is controlled by `hideCoverImage` frontmatter
- **Performance warnings** about "unoptimized loading" typically refer to post card images
- **Accessibility warnings** about "redundant alt text" can affect both systems

### Development Mode Graceful Error Handling

When working with Obsidian and actively editing content, missing image errors are handled gracefully:

1. **Graceful Fallbacks**: Missing images are automatically replaced with placeholder images
2. **Development Warnings**: Console warnings help identify missing images (only in dev mode)
3. **Continued Development**: The build process continues even with missing images
4. **Visual Indicators**: Placeholder images clearly indicate missing assets

### Placeholder Images

The system automatically uses placeholder images when assets are missing:
- **Posts**: `/posts/attachments/placeholder.jpg`
- **Pages**: `/pages/attachments/placeholder.jpg`
- **Default**: `/posts/attachments/placeholder.jpg`

### Image Reference Formats

#### Relative Paths (Recommended)
```markdown
![Image](image.jpg)
![Another Image](subfolder/image.png)
```

#### Obsidian Bracket Syntax
```markdown
![Image]([[image.jpg]])
![Another Image]([[subfolder/image.png]])
```

#### Frontmatter Images
```yaml
---
image: cover.jpg
imageAlt: Cover image description
---
```

Or with Obsidian bracket syntax:
```yaml
---
image: "[[cover.jpg]]"
imageAlt: Cover image description
---
```

### Image Resolution Logic

The theme automatically handles image resolution for folder-based posts:
- **Relative paths** (`image.jpg`) → `/posts/post-slug/image.jpg`
- **Absolute paths** (`/attachments/image.jpg`) → `/attachments/image.jpg`
- **External URLs** (`https://...`) → Used as-is

### Development Tools

#### Check Missing Images
```bash
pnpm run check-images
```

This script will:
- Scan all markdown files for image references
- Check if referenced images exist
- Report missing images with file locations and line numbers
- Provide helpful tips for fixing issues

#### Development Configuration

Located in `src/config/dev.ts`:

```typescript
export const devConfig = {
  images: {
    showPlaceholders: true,        // Show placeholder images
    logMissingImages: true,        // Log missing images to console
    fallbacks: {
      posts: '/posts/attachments/placeholder.jpg',
      pages: '/pages/attachments/placeholder.jpg',
      default: '/posts/attachments/placeholder.jpg'
    }
  },
  content: {
    continueOnMissingAssets: true, // Continue processing with missing assets
    logWarnings: true              // Log content processing warnings
  }
};
```

## Build Process

### Asset Sync

The build process automatically syncs folder-based assets to the public directory:

```
src/content/posts/my-post/
├── index.md
├── image.jpg
├── audio.wav
├── video.mp4
└── document.pdf

↓ (build process) ↓

public/posts/my-post/
├── image.jpg
├── audio.wav
├── video.mp4
└── document.pdf
```

**Supported Media Types:**
- **Images**: JPG, JPEG, PNG, GIF, WEBP, SVG, BMP, TIFF, TIF, ICO
- **Audio**: MP3, WAV, OGG, M4A, 3GP, FLAC, AAC
- **Video**: MP4, WEBM, OGV, MOV, MKV, AVI
- **Documents**: PDF

### Build Scripts

The build process includes several pre-build steps:
1. **Sync Media Files**: Copy images, audio, video, and PDF files from content to public directory
2. **Process Aliases**: Convert content aliases to redirects
3. **Generate Redirects**: Create redirect rules for deployment platforms
4. **Build Astro**: Compile the site

### RSS and Atom Feeds

The theme automatically generates both RSS and Atom feeds for content syndication:

#### Feed Generation
- **RSS Feed**: Available at `/rss.xml` - Uses `@astrojs/rss` integration
- **Atom Feed**: Available at `/feed.xml` - Custom implementation in `src/pages/feed.xml.ts`
- **Auto-generated**: Both feeds are automatically generated during the build process
- **Content Source**: Feeds include all published posts (excludes drafts in production)

#### Feed Features
- **Post Filtering**: Only includes non-draft posts in production builds
- **Sorting**: Posts are sorted by date (newest first)
- **Metadata**: Includes post title, description, publication date, and tags
- **Image Support**: RSS feed includes post images when `imageOG: true` is set
- **Categories**: Post tags are included as RSS categories and Atom categories
- **Author Information**: Uses global author from `siteConfig.author`

#### Feed Buttons
Both RSS and Atom feed buttons are available on all post listing pages:
- **Posts Index** (`/posts`) - Main posts listing
- **Paginated Posts** (`/posts/[page]`) - Paginated post listings  
- **Tag Pages** (`/posts/tag/[...tag]`) - Posts filtered by tag
- **Paginated Tag Pages** (`/posts/tag/[...tag]/[page]`) - Paginated tag listings

#### Feed Button Styling
- **Consistent Design**: Both buttons use identical styling with theme-aware colors
- **Icons**: RSS uses Lucide RSS icon, Atom uses Lucide atom icon
- **Accessibility**: Proper `title` attributes and `data-no-swup` for Swup compatibility
- **Responsive**: Buttons adapt to different screen sizes and themes

#### Technical Implementation
- **RSS**: Uses `@astrojs/rss` with custom data processing for images and metadata
- **Atom**: Custom XML generation with proper Atom 1.0 specification compliance
- **Caching**: Both feeds include appropriate cache headers (1 hour cache, 1 day s-maxage)
- **Content Type**: RSS serves as `application/rss+xml`, Atom as `application/xml`

### Deployment Platforms

The theme supports deployment to all major platforms with an elegant configuration system:

#### Platform Selection
Set your deployment platform once in `src/config.ts`:
```typescript
deployment: {
  platform: "netlify", // "netlify" | "vercel" | "github-pages"
}
```

#### Supported Platforms
- **Netlify**: Generates `netlify.toml` with redirects and build settings
- **Vercel**: Generates `vercel.json` with redirects and headers
- **GitHub Pages**: Generates `public/redirects.txt` for GitHub Pages redirects

#### Build Process
The build process automatically detects your chosen platform and generates the correct configuration files:
```bash
pnpm run build  # Works for all platforms - no environment variables needed!
```

#### Platform-Specific Features
- **Netlify**: Includes `netlify.toml` with redirects, build settings, and 404 handling
- **Vercel**: Generates `vercel.json` with redirects and cache headers for assets
- **GitHub Pages**: Creates `public/redirects.txt` in the format required by GitHub Pages

#### Migration from Environment Variables
The old environment variable approach is still supported for backward compatibility:
```bash
# Old way (still works)
DEPLOYMENT_PLATFORM=vercel pnpm run build

# New way (recommended)
# Just set platform in config.ts and run:
pnpm run build
```

## Theme Updates

### Update Theme Command

The theme includes a built-in update command that helps users keep their Astro Modular theme installation up to date with the latest features and fixes. The update process is designed to be **safe and conservative**, protecting your content while updating theme files.

#### Quick Start

**Safe Update (Recommended):**
```bash
pnpm run update-theme
```

**Content Update (Use with caution):**
```bash
pnpm run update-theme:content
# or
pnpm run update-theme --update-content
```

#### What the Update Command Does

The `update-theme` command will:

1. **Check your git repository** - Ensures you're in a valid git repository
2. **Check for uncommitted changes** - Warns you if you have uncommitted changes
3. **Setup upstream remote** - Adds the upstream repository if not already configured
4. **Create a backup** - Saves your current commit hash for reference
5. **Fetch latest changes** - Downloads the latest changes from the upstream repository
6. **Check for updates** - Compares your local version with the latest upstream version
7. **Check critical files** - Ensures all required scripts and files are present
8. **Ensure content directories** - Creates missing content directories (projects, docs)
9. **Update theme files** - Updates components, layouts, styles, and scripts
10. **Smart merge configurations** - Preserves your settings while adding new options
11. **Handle content files** - Skips content by default (opt-in only with `--update-content`)
12. **Update Obsidian configuration** - Updates plugins and core configs (with user confirmation)
13. **Update dependencies** - Runs `pnpm install` to update any changed dependencies
14. **Rebuild project** - Runs `pnpm run build` to ensure everything works

#### Content Protection (Critical)

**By default, your content files are NEVER updated:**
- Posts (`src/content/posts/`)
- Pages (`src/content/pages/`)
- Projects (`src/content/projects/`)
- Documentation (`src/content/docs/`)
- Public assets (`public/`)

**This prevents accidental loss of your blog posts and customizations.**

To update content files, you must explicitly use the `--update-content` flag, which will:
- Show clear warnings about content overwrites
- Require explicit confirmation before proceeding
- Only update content when you explicitly choose to do so

#### Obsidian Plugin Updates

The update process handles Obsidian plugins intelligently:

**Always Updated:**
- Plugin core files (`main.js`, `manifest.json`, `styles.css`)
- Obsidian core configuration files
- New plugins (when introduced)

**Never Updated:**
- User preference files (`data.json`)
- User appearance settings
- Custom snippets and themes
- User-specific configurations

**New Plugin Strategy:**
- New plugins are detected automatically
- You're asked if you want to install them
- Existing plugins are updated to latest versions
- Your plugin settings and preferences are preserved

**Plugin Update Process:**
1. Update core plugin files (main.js, manifest.json, styles.css)
2. Preserve user data files (data.json)
3. Detect new plugins from upstream
4. Ask for confirmation before installing new plugins
5. Maintain your existing plugin configurations

#### Prerequisites

- Your project must be a git repository
- You should have committed or stashed any local changes before running the update
- You need internet access to fetch updates from the upstream repository

#### First-Time Setup

If this is your first time using the update command, it will automatically:

- Add the upstream repository as a remote named `upstream`
- Configure it to point to the official Astro Modular theme repository

#### Handling Merge Conflicts

If you've made custom changes to theme files and there are conflicts during the merge:

1. The update command will stop and show you which files have conflicts
2. You'll need to manually resolve the conflicts in your code editor
3. After resolving conflicts, run:
   ```bash
   git add .
   git commit
   ```
4. Then run the update command again:
   ```bash
   pnpm run update-theme
   ```

#### Command Options

```bash
# Show help information
pnpm run update-theme --help

# Show version information
pnpm run update-theme --version
```

#### Best Practices for AI Agents

**Before Updating:**
1. **Commit your changes** - Always commit or stash your local changes before updating
2. **Test your site** - Make sure your current site is working properly
3. **Backup important files** - If you've made custom modifications, back them up

**After Updating:**
1. **Test your site** - Run `pnpm run dev` to make sure everything still works
2. **Check for conflicts** - Review any files that had conflicts during the merge
3. **Update documentation** - Update any custom documentation if needed

**Custom Modifications:**
- **Keep a changelog** - Document what you've changed and why
- **Use feature branches** - Consider keeping custom changes in separate branches
- **Test thoroughly** - Always test after updates to ensure your customizations still work

#### Troubleshooting Theme Updates

**"Not in a git repository"**
If you get this error, you need to initialize git in your project:

```bash
git init
git add .
git commit -m "Initial commit"
```

**"Merge failed - there may be conflicts"**
This means there are conflicts between your changes and the upstream changes. You'll need to:

1. Open the conflicted files in your editor
2. Look for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Resolve the conflicts by choosing which changes to keep
4. Remove the conflict markers
5. Add and commit the resolved files

**"Failed to fetch from upstream"**
This usually means there's a network issue or the upstream repository is unavailable. Check:

- Your internet connection
- The upstream repository URL is correct
- You have access to the repository

**"Dependency update failed"**
If dependency updates fail, try running manually:

```bash
pnpm install
```

#### Advanced Usage

**Checking for Updates Without Updating**
To see if updates are available without actually updating:

```bash
git fetch upstream
git log HEAD..upstream/main --oneline
```

**Updating from a Specific Branch**
If you want to update from a specific branch (e.g., a beta version):

```bash
git fetch upstream
git merge upstream/beta-branch
```

**Rolling Back Updates**
If an update causes issues, you can roll back to the previous version:

```bash
# Find the commit hash before the update
git log --oneline

# Reset to that commit (replace COMMIT_HASH with the actual hash)
git reset --hard COMMIT_HASH

# Rebuild the project
pnpm run build
```

## Version Management

### Theme Version System

The Astro Modular theme includes a comprehensive version management system that tracks the current theme version and displays it consistently across all build and development commands.

#### Version Display

**All pnpm commands now show the theme version:**
```bash
> astro-modular@0.1.0 dev C:\Users\david\Development\astro-modular
> astro-modular@0.1.0 build C:\Users\david\Development\astro-modular
> astro-modular@0.1.0 version C:\Users\david\Development\astro-modular
```

**Instead of the generic workspace version:**
```bash
# Old (generic)
> workspace@1.0.0 dev

# New (theme-specific)
> astro-modular@0.1.0 dev
```

#### Version Files

The version system uses multiple files to maintain consistency:

**Primary Version File:**
- **`VERSION`** - Contains the current version number (e.g., `0.1.0`)

**Package Configuration:**
- **`package.json`** - Updated with theme name (`astro-modular`) and version (`0.1.0`)

**Version Utility:**
- **`scripts/get-version.js`** - Provides version management functions

#### Version Commands

**Get Current Version:**
```bash
pnpm run version
# Output: 0.1.0
```

**Get Theme Identifier:**
```bash
node scripts/get-version.js
# Output: 0.1.0
```

#### Version Management Functions

The `scripts/get-version.js` utility provides several functions:

**`getThemeVersion()`** - Get current version from VERSION file or package.json
**`getThemeName()`** - Get theme name from package.json
**`getThemeIdentifier()`** - Get full identifier (name@version)
**`updateVersion(newVersion)`** - Update both VERSION file and package.json

#### Release Process

**For Maintainers (Creating Releases):**

1. **Update Version Files (Manual Process):**
   ```bash
   # Step 1: Update VERSION file
   echo "0.2.0" > VERSION
   
   # Step 2: Update package.json manually
   # Change "version": "0.1.0" to "version": "0.2.0"
   ```
   
   **Important**: Both files must be updated manually. The VERSION file is the source of truth, but package.json controls the terminal display.

2. **Create GitHub Release:**
   - Go to GitHub repository
   - Click "Releases" → "Create a new release"
   - Tag version: `v0.2.0` (with 'v' prefix)
   - Release title: `v0.2.0`
   - Description: List of changes
   - **Important**: Use "Source code (zip)" as the release asset (automatic)

3. **Publish Release:**
   - Click "Publish release"
   - GitHub automatically creates the source code zip
   - No additional files needed

**For Users (Updating Theme):**

1. **Check for Updates:**
   ```bash
   pnpm run update-theme --version
   # Shows current version and available updates
   ```

2. **Update Theme:**
   ```bash
   # Safe update (recommended)
   pnpm run update-theme
   
   # With content updates (use with caution)
   pnpm run update-theme:content
   ```

3. **Version Updates Automatically:**
   - Update script fetches latest release from GitHub
   - Downloads source code zip from release
   - Updates theme files while preserving content
   - Updates version information automatically
   - Creates single commit with changes

#### Version Update Process

**When users run `pnpm run update-theme`:**

1. **Fetch Latest Release** - Gets release info from GitHub API
2. **Download Source Code** - Downloads zip from release assets
3. **Extract Theme Files** - Extracts to temporary directory
4. **Update Theme Files** - Copies theme files to project
5. **Update Version** - Updates VERSION file and package.json
6. **Create Commit** - Single commit with all changes
7. **Cleanup** - Removes temporary files

**Version Sync:**
- Release tag: `v0.2.0` → Version: `0.2.0` (removes 'v' prefix)
- Both VERSION file and package.json are updated
- All commands show new version immediately

#### Template vs Fork Detection

**Template Users:**
- See `astro-modular@0.1.0` in all commands
- No update functionality available
- Version stays at template version

**Fork Users:**
- See `astro-modular@0.1.0` in all commands
- Full update functionality available
- Version updates with theme updates

#### Version Display Examples

**Development Server:**
```bash
> astro-modular@0.1.0 dev C:\Users\david\Development\astro-modular
> cross-env ASTRO_CONTENT_COLLECTION_CACHE=false node scripts/setup-dev.mjs && node scripts/sync-images.js && node scripts/process-aliases.js && node scripts/generate-redirects.js && astro dev --host localhost --port 5000
```

**Build Process:**
```bash
> astro-modular@0.1.0 build C:\Users\david\Development\astro-modular
> node scripts/sync-images.js && node scripts/process-aliases.js && node scripts/generate-redirects.js && astro build
```

**Version Check:**
```bash
> astro-modular@0.1.0 version C:\Users\david\Development\astro-modular
> node scripts/get-version.js
0.1.0
```

#### Best Practices for AI Agents

**Version Management:**
- **Always check current version** before making changes
- **Use version commands** to verify theme version
- **Don't hardcode version numbers** - use the utility functions
- **Test version display** after making changes

**Release Process:**
- **Update VERSION file first** - this is the source of truth
- **Update package.json manually** - both files must be updated separately
- **Use semantic versioning** - follow semver conventions
- **Test version commands** before creating releases
- **No automatic sync** - version files don't sync automatically during dev/build

**User Experience:**
- **Version display is consistent** across all commands
- **Theme identification is clear** - users know which version they're using
- **Update process is transparent** - users see version changes
- **No confusion** between workspace and theme versions

## Configuration & Customization

### Core Configuration (`src/config.ts`)

The configuration is organized in logical sections for easy customization. For detailed setup instructions, see the [Getting Started Guide](src/content/posts/getting-started.md).

#### Site Information
```typescript
export const siteConfig = {
  site: 'https://yourdomain.com',
  title: 'Your Blog Title',
  description: 'Your blog description',
  author: 'Your Name',
  language: 'en',
}
```

#### Theme & Layout Options
```typescript
theme: "oxygen",  // Available: Oxygen, Minimal, Atom, Ayu, Catppuccin, Charcoal, Dracula, Everforest, Flexoki, Gruvbox, macOS, Nord, Obsidian, Rosé Pine, Sky, Solarized, Things
layout: {
  contentWidth: "45rem",
},
postsPerPage: 5,
```

#### Deployment Platform Configuration
```typescript
deployment: {
  platform: "netlify", // "netlify" | "vercel" | "github-pages" - set once and forget!
}
```

**Deployment Platform Options:**
- **`"netlify"`** (default) - Generates `netlify.toml` with redirects and build settings
- **`"vercel"`** - Generates `vercel.json` with redirects and cache headers
- **`"github-pages"`** - Generates `public/redirects.txt` for GitHub Pages redirects

**Important:** Set this once in your config and the build process automatically generates the correct platform-specific configuration files. No environment variables needed!

#### Modular Features Configuration
```typescript
features: {
  readingTime: true,
  wordCount: true,
  tableOfContents: true,
  tags: true,
  linkedMentions: true,
  linkedMentionsCompact: false,
  scrollToTop: true,
  darkModeToggleButton: true,
  commandPalette: true,
  postNavigation: true,
  showSocialIconsInFooter: true,
  showPostCardCoverImages: "latest-and-posts", // See cover image options below
}
```

**Linked Mentions Features:**
- `linkedMentions: true` - Enable linked mentions section at the end of the page showing which posts reference the current post
- `linkedMentionsCompact: false` - Choose between detailed view (default) or compact view for linked mentions

#### Cover Image Options
- `"all"` - Show cover images everywhere
- `"latest"` - Show only on the latest post section and featured posts
- `"home"` - Show on homepage sections (latest and recent)
- `"posts"` - Show only on posts pages, tag pages, and post listings
- `"latest-and-posts"` - Show on latest post section AND posts pages/tags (but not recent posts section)
- `"none"` - Never show cover images

#### Homepage Content Options
The homepage content is now organized under the `homeOptions` section for better clarity and control:

```typescript
homeOptions: {
  featuredPost: {
    enabled: true,           // Show featured post on homepage
    type: "latest",          // "latest" or "featured"
    slug: "getting-started", // Only used when type is "featured"
  },
  recentPosts: {
    enabled: true,           // Show recent posts on homepage
    count: 7,                // Number of recent posts to show
  },
  projects: {
    enabled: false,          // Show featured projects on homepage
    count: 2,                // Number of projects to show
  },
  docs: {
    enabled: false,          // Show featured docs on homepage
    count: 3,                // Number of docs to show
  },
  blurb: {
    placement: "below",      // "above", "below", or "none"
  },
}
```

**Featured Post Configuration:**
- `enabled: true` - Show featured post section
- `type: "latest"` - Show the most recent post
- `type: "featured"` - Show a specific post by slug
- `slug: "post-name"` - Post slug (without `/posts/` prefix) when type is "featured"
- **Flexible Setup**: Slug can be present even when `type: "latest"` - it will be ignored until you switch to "featured"

**Recent Posts Configuration:**
- `enabled: true` - Show recent posts section
- `count: 7` - Number of recent posts to display
- **Smart Logic**: If featured post is shown, recent posts start from the second post

**Projects Configuration:**
- `enabled: true` - Show featured projects on homepage
- `count: 2` - Number of projects to display
- **Priority**: First tries to show projects with `featured: true` in frontmatter (up to count)
- **Fallback**: If no featured projects exist, shows the most recent projects (up to count)
- **No Content**: If no projects exist at all, shows nothing
- Includes "View all projects →" link

**Documentation Configuration:**
- `enabled: true` - Show featured docs on homepage  
- `count: 3` - Number of docs to display
- **Priority**: First tries to show docs with `featured: true` in frontmatter (up to count)
- **Fallback**: If no featured docs exist, shows the most recent docs (up to count)
- **No Content**: If no docs exist at all, shows nothing
- Includes "View all docs →" link

**Home Blurb Configuration:**
- `placement: "above"` - Show at top of homepage
- `placement: "below"` - Show after content (default)
- `placement: "none"` - Disable completely
- **Blurb-Only Mode**: When no other content is enabled, blurb becomes the main page content with H1 title and page-like styling

**Homepage Order:**
1. Featured Post (if enabled)
2. Recent Posts (if enabled)
3. Featured Projects (if enabled)
4. Featured Documentation (if enabled)
5. Home Blurb (if enabled)

**Special Layout Logic:**
- **Single Content Type**: When only one content type is displayed, it gets special treatment with centered "View all" links instead of section titles
- **Blurb-Only Mode**: When only blurb content is shown, it displays as a proper page with H1 title and rounded container styling

**Homepage Layout Examples:**

**Show only recent posts:**
```typescript
homeOptions: {
  featuredPost: { enabled: false, type: "latest", slug: undefined },
  recentPosts: { enabled: true, count: 7 },
  projects: { enabled: false },
  docs: { enabled: false },
  blurb: { placement: "none" },
}
// Result: Special treatment - no "Recent Posts" title, centered "View all posts →"
```

**Show only projects:**
```typescript
homeOptions: {
  featuredPost: { enabled: false, type: "latest", slug: undefined },
  recentPosts: { enabled: false, count: 7 },
  projects: { enabled: true },
  docs: { enabled: false },
  blurb: { placement: "none" },
}
// Result: Special treatment - no "Projects" title, centered "View all projects →"
```

**Show only blurb content:**
```typescript
homeOptions: {
  featuredPost: { enabled: false, type: "latest", slug: undefined },
  recentPosts: { enabled: false, count: 7 },
  projects: { enabled: false },
  docs: { enabled: false },
  blurb: { placement: "below" },
}
// Result: Page-like layout with H1 title and rounded container
```

**Show multiple content types:**
```typescript
homeOptions: {
  featuredPost: { enabled: true, type: "latest", slug: undefined },
  recentPosts: { enabled: true, count: 7 },
  projects: { enabled: true },
  docs: { enabled: false },
  blurb: { placement: "below" },
}
// Result: Normal layout with section titles and right-aligned "View all" links
```

#### Post Card Aspect Ratio Configuration
Configure the aspect ratio for post card cover images:

```typescript
features: {
  postCardAspectRatio: "og", // Default: OpenGraph standard
  customAspectRatio: undefined, // For custom ratios
}
```

**Aspect Ratio Options:**
- `"og"` (1.91:1) - OpenGraph standard (default)
- `"16:9"` (1.78:1) - Standard widescreen
- `"4:3"` (1.33:1) - Traditional
- `"3:2"` (1.5:1) - Classic photography
- `"square"` (1:1) - Square
- `"golden"` (1.618:1) - Golden ratio
- `"custom"` - Use your own ratio

**Custom Aspect Ratio Example:**
```typescript
postCardAspectRatio: "custom",
customAspectRatio: "2.5/1" // Custom 2.5:1 ratio
```

**Important Notes for AI Agents:**
- This **only affects post cards** (listings, homepage, tag pages)
- **Individual post cover images** maintain their original aspect ratio
- The aspect ratio is applied via CSS `aspect-ratio` property
- Use the `getPostCardAspectRatio()` utility function to get the current ratio value

#### Navigation Configuration
```typescript
navigation: {
  showNavigation: true,
  style: 'traditional', // or 'minimal'
  showMobileMenu: true,
  pages: [
    { title: 'Posts', url: '/posts' },
    { title: 'About', url: '/about' }
  ],
  social: [
    { title: 'GitHub', url: 'https://github.com/username', icon: 'github' }
  ],
}
```

#### SEO Configuration
```typescript
seo: {
  defaultOgImageAlt: "Astro Modular logo.",
},
footer: {
  enabled: true,  // Set to false for minimal footer
  content: `© 2025 {author}. Built with Astro Modular.`,
}
```

#### Profile Picture Configuration
```typescript
profilePicture: {
  enabled: true,
  image: "/profile.jpg",        // Path to your image (place in public/ directory)
  alt: "Profile picture",       // Alt text for accessibility
  size: "md",                   // "sm" (32px), "md" (48px), or "lg" (64px)
  url: "/about",                // Optional URL to link to when clicked
  placement: "footer",          // "footer" or "header"
  style: "circle",              // "circle", "square", or "none"
}
```

**Profile Picture Features:**
- **Placement Options**: Header (replaces text logo) or Footer (above or aligned with content)
- **Style Options**: Circle (profile photos), Square (logo squares), None (horizontal logos/banners)
- **Footer Control**: When `footer.enabled: false`, profile picture aligns with social icons
- **Responsive**: Different layouts for mobile and desktop
- **Theme-Aware**: Styling adapts to all available themes

### Content Frontmatter Schemas

#### Posts Frontmatter
```yaml
---
title: "{{title}}"
date: {{date}}
description: ""
tags: []
image: ""
imageAlt: ""
imageOG: false
hideCoverImage: false
targetKeyword: ""
draft: true
---
```

#### Pages Frontmatter
```yaml
---
title: "{{title}}"
description: ""
hideCoverImage: false
hideTOC: false
noIndex: false
---
```

#### Projects Frontmatter
```yaml
---
title: "{{title}}"
description: "Project description"
date: {{date}}
categories: ["Web Development", "Open Source"]
repositoryUrl: "https://github.com/username/repo"
demoUrl: "https://your-demo.com"
status: "completed"  # Any string value - "completed", "in-progress", "On Hold", etc.
image: "cover.jpg"
imageAlt: "Project screenshot"
hideCoverImage: false
hideTOC: false
draft: false
featured: true
---
```

#### Documentation Frontmatter
```yaml
---
title: "{{title}}"
description: "Documentation description"
category: "Setup"  # Optional - creates "Unsorted" if missing
order: 1
version: "1.0.0"
lastModified: 2024-01-15
image: "hero.jpg"
imageAlt: "Documentation screenshot"
hideCoverImage: false
hideTOC: false
draft: false
featured: true
---
```

### Development Configuration (`src/config/dev.ts`)

#### Image Handling
```typescript
images: {
  showPlaceholders: true,        // Show placeholder images for missing assets
  logMissingImages: true,        // Log missing images to console
  fallbacks: {
    posts: '/posts/attachments/placeholder.jpg',
    pages: '/pages/attachments/placeholder.jpg',
    default: '/posts/attachments/placeholder.jpg'
  }
}
```

#### Content Processing
```typescript
content: {
  continueOnMissingAssets: true, // Continue processing with missing assets
  logWarnings: true              // Log content processing warnings
}
```

#### Error Handling
```typescript
errors: {
  showDetails: true,             // Show detailed error information
  continueOnNonCriticalErrors: true
}
```

#### Tag Handling (Recently Added)
```typescript
tags: {
  handleMissingTags: true,       // Gracefully handle missing/deleted tags
  logTagWarnings: true,          // Log tag-related warnings
  continueOnMissingTags: true    // Continue processing with missing tags
}
```

### Theme Customization

#### Available Themes
- **Oxygen** (default) - Modern, clean design
- **Minimal** - Understated with high contrast
- **Atom** - Dark theme with vibrant accents
- **Ayu** - Three variants (light, mirage, dark)
- **Catppuccin** - Pastel color palette
- **Charcoal** - Dark, professional look
- **Dracula** - Dark theme with purple accents
- **Everforest** - Soft, warm colors
- **Flexoki** - Based on Material Design 3
- **Gruvbox** - Retro groove color scheme
- **macOS** - Native macOS appearance
- **Nord** - Arctic-inspired color palette
- **Obsidian** - Matches Obsidian's default theme
- **Rosé Pine** - All natural pine, faux fir, and winter
- **Sky** - Light, airy design
- **Solarized** - Precision colors for machines and people
- **Things** - Clean, minimal design
- **Custom** - Create your own theme using the template system


#### Theme Switching
- Use the command palette (`Ctrl+K`) for instant theme switching
- Theme changes are visible in real-time with `pnpm dev`
- All themes are optimized for performance and accessibility

#### Custom Themes
- **Template system**: Use `src/themes/custom/custom.ts` as a starting point
- **Configuration**: Set `theme: "custom"` and `customThemeFile: "filename"` in `src/config.ts`
- **Multiple themes**: Copy and rename the template file, then change `customThemeFile` in config
- **Documentation**: See `src/themes/custom/README.md` for detailed instructions
- **Real-time preview**: Changes are visible immediately with `pnpm dev`

### Typography Configuration

The theme includes a comprehensive typography system that allows separate font configuration for headings and body text.

#### Font Configuration
```typescript
typography: {
  headingFont: "Inter", // Font for headings (h1, h2, h3, h4, h5, h6)
  proseFont: "Inter",   // Font for body text and prose content
}
```

#### Supported Fonts

**Sans-Serif Fonts (Recommended for UI and headings):**
- **Inter** - Modern, highly readable (default)
- **Roboto** - Google's signature font
- **Open Sans** - Humanist, friendly
- **Lato** - Semi-rounded, warm
- **Poppins** - Geometric, modern
- **Source Sans Pro** - Adobe's open source font
- **Nunito** - Rounded, friendly
- **Montserrat** - Urban, geometric

**Serif Fonts (Great for headings and elegant typography):**
- **Playfair Display** - High contrast, elegant
- **Merriweather** - Highly readable, designed for screens
- **Lora** - Well-balanced, contemporary
- **Crimson Text** - Book typeface, readable
- **PT Serif** - Professional, clean
- **Libre Baskerville** - Classic, web-optimized

**Monospace Fonts (For code and technical content):**
- **Fira Code** - Programming ligatures
- **JetBrains Mono** - Developer-focused
- **Source Code Pro** - Adobe's monospace
- **IBM Plex Mono** - Corporate, clean
- **Cascadia Code** - Microsoft's coding font

#### Popular Font Combinations

**Modern & Professional:**
```typescript
typography: {
  headingFont: "Montserrat",
  proseFont: "Source Sans Pro",
}
```

**Elegant & Readable:**
```typescript
typography: {
  headingFont: "Playfair Display",
  proseFont: "Lato",
}
```

**Clean & Minimal:**
```typescript
typography: {
  headingFont: "Inter",
  proseFont: "Inter",
}
```

**Serif Typography:**
```typescript
typography: {
  headingFont: "Merriweather",
  proseFont: "Merriweather",
}
```

**Tech/Developer Blog:**
```typescript
typography: {
  headingFont: "JetBrains Mono",
  proseFont: "Source Sans Pro",
}
```

#### Technical Implementation

**Font Loading:**
- Automatically generates Google Fonts URLs based on configuration
- Only loads fonts that are actually used
- Provides fallbacks to system fonts for performance

**CSS Custom Properties:**
- `--font-heading` - Applied to all headings
- `--font-prose` - Applied to body text and prose content
- Fallback chain: Custom font → System fonts → Generic families

**Tailwind Classes:**
- `font-heading` - Apply heading font to any element
- `font-prose` - Apply prose font to any element
- `font-sans` - Apply prose font (alias)

**Performance Considerations:**
- Fonts are loaded asynchronously
- System font fallbacks prevent layout shift
- Google Fonts are optimized for web delivery
- Custom fonts gracefully degrade to system fonts

#### Best Practices for AI Agents

**Font Selection:**
- **Headings**: Choose fonts with good contrast and readability at various sizes
- **Body Text**: Prioritize readability and legibility for long-form content
- **Consistency**: Maintain visual hierarchy between heading and body fonts
- **Performance**: Stick to Google Fonts for automatic optimization

**Common Mistakes to Avoid:**
- Don't use too many different fonts (stick to 1-2 maximum)
- Avoid fonts that are too similar (defeats the purpose of separation)
- Don't forget to test readability in both light and dark themes
- Avoid fonts that don't have good fallbacks

**Testing Font Combinations:**
- Test at different screen sizes
- Verify readability in both light and dark modes
- Check that fonts load properly on slow connections
- Ensure proper fallbacks work when custom fonts fail

### Content Structure Customization

#### Folder-Based Posts
- Use descriptive kebab-case folder names
- Keep assets co-located with `index.md`
- Folder name becomes the URL slug automatically

#### Traditional Posts
- Single markdown files in `src/content/posts/`
- Images in `src/content/posts/attachments/`
- Use relative paths for images

#### Pages
- Static pages in `src/content/pages/`
- Images in `src/content/pages/attachments/`
- NO H1 headings in markdown content - title comes from frontmatter

#### Projects
- Showcase work, side projects, and portfolio items
- Support both folder-based and single-file organization
- Images in `src/content/projects/attachments/` or co-located with content
- Frontmatter includes: title, description, date, categories, repositoryUrl, demoUrl, status, image, imageAlt, hideCoverImage, draft, noIndex, featured
- Status: Any string value with intelligent handling:
  - **Recognized values**: "completed", "in-progress" (case-insensitive)
  - **Case normalization**: "In Progress" → "in-progress", "Completed" → "completed"
  - **Custom values**: "On Hold", "Paused", etc. display as-is with neutral theme colors
  - **Styling**: Recognized values get green/yellow colors, custom values use theme colors
- Featured: `true` to show on homepage (requires `homeProjects: true` in config)
- URL structure: `/projects/project-slug`

#### Documentation
- Technical documentation and guides
- Support both folder-based and single-file organization  
- Images in `src/content/docs/attachments/` or co-located with content
- Frontmatter includes: title, description, category, order, lastModified (optional), version, image, imageAlt, hideCoverImage, draft, noIndex, showTOC, featured
- Featured: `true` to show on homepage (requires `homeDocs: true` in config)
- URL structure: `/docs/doc-slug`

**Documentation Structure:**
Documentation files are organized by category and can be single files:

```
src/content/docs/
├── getting-started.md
├── advanced-configuration.md
├── api-reference/
│   ├── index.md
│   └── code.png
├── attachments/
│   └── sample-guide-hero.png
└── sample-guide.md
```

**Frontmatter Schema:**
```yaml
---
title: "Documentation Title"             # Required
description: "Documentation description" # Required
category: "Setup"                        # Required - for organization
order: 1                                 # Required - for sorting within category
lastModified: 2024-01-15                # Optional - if missing, no date is shown
version: "1.0.0"                         # Optional
image: "hero-image.jpg"                  # Optional
imageAlt: "Documentation screenshot"     # Optional
hideCoverImage: false                    # Optional
draft: false                             # Optional
noIndex: false                           # Optional
showTOC: true                            # Optional - override global TOC setting
featured: false                          # Optional - show on homepage (requires homeDocs: true)
---
```

**Features:**
- **Category Organization**: Group related documentation
- **Version Control**: Track documentation versions
- **Ordered Display**: Control display order within categories
- **Table of Contents**: Automatic TOC generation
- **Search Integration**: Included in command palette search
- **SEO Optimized**: Proper meta tags and structured data

**Categories:**
Common documentation categories:
- **Setup**: Installation and initial configuration
- **Configuration**: Advanced settings and customization
- **Reference**: API documentation and technical specs
- **Tutorials**: Step-by-step guides
- **Troubleshooting**: Common issues and solutions
- **Examples**: Code samples and use cases

**Best Practices:**
1. Use clear, descriptive titles
2. Organize by logical categories
3. Set appropriate order values
4. Include version information for API docs
5. Write for your target audience
6. Keep content up-to-date
7. Use consistent formatting

**URL Structure:**
- Individual docs: `/docs/documentation-slug`
- Documentation index: `/docs`
- Documentation is automatically included in search and command palette
- Categories are displayed as sections on the index page

## Troubleshooting

### Common Image Issues

#### Images Not Loading
- Check that image paths use relative paths
- Ensure images are in the same folder as `index.md`
- Verify the build process synced the assets
- Run `pnpm run check-images` to identify missing files

#### Build Errors
- Check for typos in image paths
- Ensure all referenced assets exist
- Verify folder structure is correct
- Check console for specific error messages

#### Development Mode Issues
- Missing images show placeholder images (this is normal)
- Console warnings indicate missing assets
- Use the check script to identify all missing images
- Fix images gradually - development continues with placeholders

### Quick Fixes

1. **Move Images**: Ensure images are in the correct folder
2. **Update Paths**: Fix image paths in markdown files
3. **Check Case**: File paths are case-sensitive
4. **Verify Extensions**: Ensure file extensions match exactly
5. **Run Check Script**: Use `pnpm run check-images` to identify issues

## Best Practices

### Content Organization

#### Naming Conventions
- Use kebab-case for folder names: `my-awesome-post`
- Keep folder names descriptive but concise
- Avoid special characters and spaces

#### Asset Organization
- Use descriptive filenames: `hero-image.jpg` not `img1.jpg`
- Group related assets in subfolders if needed
- Keep file sizes reasonable for web use

#### Content Structure
- Always use `index.md` for the main content in folder-based posts
- Use relative paths for all local assets
- Test image references before publishing

### Development Workflow

1. **Use the Check Script**: Run `pnpm run check-images` regularly
2. **Monitor Console**: Watch for development warnings about missing images
3. **Fix Images Gradually**: You don't need to fix all missing images immediately
4. **Use Placeholders**: The placeholder system lets you continue development

### Version Control

- Commit the entire folder as a unit for folder-based posts
- Use meaningful commit messages
- Consider using Git LFS for large assets
- Test builds before committing

### Production Deployment

- Missing images will cause build failures in production
- Always run `pnpm run check-images` before deploying
- Fix all missing images before production builds
- Test the build process locally before deployment

## Technical Implementation Details

### Image Processing Components

- **ImageWrapper.astro**: Handles image rendering with fallbacks
- **PostLayout.astro**: Manages post-specific image handling
- **internallinks.ts**: Processes both Obsidian-style wikilinks and standard markdown links
- **images.ts**: Utility functions for image path optimization

### Content Processing

- **remarkInternalLinks**: Processes both Obsidian wikilinks (posts only) and standard markdown links (all content types)
- **remarkFolderImages**: Handles folder-based post image paths
- **Content Collections**: Astro's content management system

### SEO and Meta Tags

All SEO features work with folder-based posts:
- Open Graph images are properly resolved
- Meta tags are generated correctly
- Sitemaps include folder-based posts
- RSS feeds include folder-based posts

## Common Issues and Solutions for AI Agents

### Accessibility Warnings
1. **"Invalid tabindex on non-interactive element"**: Usually caused by Swup. Check `astro.config.mjs` - `accessibility: false` should be set.
2. **"Missing content" warnings**: Check for proper `aria-label` attributes on interactive elements.
3. **"Redundant text in alt attribute"**: Alt text should describe the image, not repeat visible text. Use descriptive alt text instead of post titles.

### Performance Warnings
1. **"Unoptimized loading attribute"**: Above-the-fold images should use `loading="eager"`. First post on pages should have `eager={true}` prop.
2. **Image loading issues**: Check if `eager` prop is being passed correctly to PostCard components.

### Image System Confusion (Common AI Agent Mistake)
- **Post cards** show images based on `showPostCardCoverImages` config, NOT `hideCoverImage` frontmatter
- **Post content** shows images based on `hideCoverImage` frontmatter, NOT config
- **Post card aspect ratio** is controlled by `postCardAspectRatio` config, NOT individual post settings
- These are completely separate systems - don't mix them up!

### Development vs Production
- **Development**: Missing images show placeholders, warnings are logged
- **Production**: Missing images cause build failures
- Always run `pnpm run check-images` before deploying

### File Structure Patterns
- **Folder-based posts**: Use `index.md` as the main content file
- **Assets**: Co-locate with `index.md` in the same folder
- **URLs**: Folder name becomes the slug automatically

### Component Hierarchy
- **BaseLayout.astro**: Main layout with Swup container
- **PostLayout.astro**: Individual post layout
- **PostCard.astro**: Post cards for listings (controlled by config)
- **PostContent.astro**: Post content rendering (controlled by frontmatter)
- **ImageWrapper.astro**: Handles image optimization and fallbacks

### Key Configuration Files
- **`src/config.ts`**: Main site configuration including `showPostCardCoverImages`
- **`astro.config.mjs`**: Astro and Swup configuration
- **`src/config/dev.ts`**: Development-specific settings

## Mathematical Notation Support

### Overview

The theme includes comprehensive LaTeX math support using KaTeX for fast, client-side rendering. All math works seamlessly in both light and dark themes and maintains full compatibility with Obsidian workflows.

### Implementation Details

#### Dependencies
- **`remark-math`**: ^6.0.0 - Parses LaTeX math syntax in markdown
- **`rehype-katex`**: ^7.0.0 - Renders math using KaTeX
- **`katex`**: ^0.16.9 - Core KaTeX library for math rendering

#### Plugin Chain Integration
The math processing is integrated into the existing markdown pipeline:

**Remark Plugins (Processing Order):**
1. `remarkInternalLinks` - Process wikilinks and standard links
2. `remarkFolderImages` - Handle folder-based images
3. `remarkImageCaptions` - Process image captions
4. `remarkCallouts` - Process Obsidian-style callouts
5. `remarkImageGrids` - Handle image grid layouts
6. **`remarkMath`** - Parse LaTeX math syntax
7. `remarkReadingTime` - Calculate reading time
8. `remarkToc` - Generate table of contents

**Rehype Plugins (Rendering Order):**
1. **`rehypeKatex`** - Render math with KaTeX (first in chain)
2. `rehypeMark` - Process mark highlighting
3. `rehypeSlug` - Generate heading slugs
4. `rehypeAutolinkHeadings` - Add heading links

#### Styling Integration
- **KaTeX CSS**: Imported in `src/styles/global.css`
- **Theme Compatibility**: Works with all 17+ available themes
- **Light/Dark Mode**: Automatic color adaptation
- **Responsive Design**: Math scales properly on all devices
- **Callout Integration**: Math works within all callout types

### Supported Math Syntax

#### Inline Math
- **Syntax**: `$...$` (single dollar signs)
- **Example**: `$E = mc^2$` → $E = mc^2$
- **Use Cases**: Mathematical expressions within text

#### Display Math
- **Syntax**: `$$...$$` (double dollar signs)
- **Example**: 
  ```markdown
  $$
  \int_0^{2\pi} d\theta x+e^{-i\theta}
  $$
  ```
- **Use Cases**: Centered mathematical equations

#### Common Mathematical Notation

**Fractions and Superscripts:**
- Fractions: `\frac{a}{b}`, `\frac{x^2 + 1}{x - 1}`
- Superscripts: `x^2`, `e^{i\pi} + 1 = 0`
- Subscripts: `x_1`, `H_2O`

**Greek Letters:**
- Lowercase: `\alpha`, `\beta`, `\gamma`, `\delta`, `\epsilon`, `\theta`, `\lambda`, `\mu`, `\pi`, `\sigma`, `\phi`, `\omega`
- Uppercase: `\Gamma`, `\Delta`, `\Theta`, `\Lambda`, `\Pi`, `\Sigma`, `\Phi`, `\Omega`

**Mathematical Symbols:**
- Summation: `\sum_{i=1}^{n} x_i`
- Product: `\prod_{i=1}^{n} x_i`
- Integral: `\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}`
- Limit: `\lim_{x \to 0} \frac{\sin x}{x} = 1`

**Matrices:**
```markdown
$$
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
\begin{pmatrix}
x \\
y
\end{pmatrix}
=
\begin{pmatrix}
ax + by \\
cx + dy
\end{pmatrix}
$$
```

**Aligned Equations:**
```markdown
$$
\begin{align}
f(x) &= ax^2 + bx + c \\
f'(x) &= 2ax + b \\
f''(x) &= 2a
\end{align}
$$
```

**Piecewise Functions:**
```markdown
$$
f(x) = \begin{cases}
x^2 & \text{if } x \geq 0 \\
-x^2 & \text{if } x < 0
\end{cases}
$$
```

### Integration Features

#### Math in Callouts
Math works perfectly within all callout types:

```markdown
> [!note] Pythagorean Theorem
> For a right triangle: $$a^2 + b^2 = c^2$$
```

#### Math in Tables
Mathematical expressions render correctly in table cells:

| Feature | Formula | Example |
|---------|---------|---------|
| Quadratic | $ax^2 + bx + c = 0$ | $x^2 - 5x + 6 = 0$ |
| Derivative | $\frac{d}{dx}f(x)$ | $\frac{d}{dx}x^2 = 2x$ |

#### Math in Lists
- Mathematical expressions work in both ordered and unordered lists
- Complex equations can be included in list items
- Proper spacing and alignment maintained

### Obsidian Compatibility

The math implementation maintains full compatibility with Obsidian:

- **Identical Syntax**: Same `$...$` and `$$...$$` syntax
- **LaTeX Commands**: All standard LaTeX math commands supported
- **Greek Letters**: `\alpha`, `\beta`, `\gamma`, etc.
- **Mathematical Symbols**: `\sum`, `\int`, `\infty`, etc.
- **Complex Notation**: Matrices, aligned equations, piecewise functions
- **Seamless Workflow**: Write in Obsidian, publish to blog with identical rendering

### Performance Considerations

- **Client-side Rendering**: KaTeX renders math in the browser for fast loading
- **No Server Dependencies**: No need for server-side math processing
- **Optimized CSS**: Only necessary KaTeX styles included
- **Responsive Design**: Math scales properly on all devices
- **Theme Integration**: Math colors adapt to light/dark themes automatically

### Browser Support

- **Modern Browsers**: Full support in Chrome, Firefox, Safari, Edge
- **Mobile Devices**: Responsive math rendering on all screen sizes
- **Accessibility**: Proper ARIA labels and screen reader support
- **Print Styles**: Math renders correctly in print layouts

### Common Math Examples

#### Basic Physics
```markdown
Einstein's mass-energy equivalence: $E = mc^2$

The Schrödinger equation:
$$
i\hbar\frac{\partial}{\partial t}\Psi(\vec{r},t) = \hat{H}\Psi(\vec{r},t)
$$
```

#### Calculus
```markdown
The derivative of $x^2$ is $\frac{d}{dx}x^2 = 2x$

Integration by parts:
$$\int u \, dv = uv - \int v \, du$$
```

#### Linear Algebra
```markdown
Matrix multiplication:
$$
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
\begin{pmatrix}
x \\
y
\end{pmatrix}
=
\begin{pmatrix}
ax + by \\
cx + dy
\end{pmatrix}
$$
```

### Troubleshooting Math Rendering

#### Common Issues
1. **Math not rendering**: Check that `remarkMath` and `rehypeKatex` are properly configured
2. **Styling issues**: Verify KaTeX CSS is imported in `src/styles/global.css`
3. **Theme compatibility**: Math should adapt to all themes automatically
4. **Mobile rendering**: Math should be responsive on all devices

#### Development Tools
- **Test Post**: Use `src/content/posts/math-test.md` for testing
- **Formatting Reference**: See `src/content/posts/formatting-reference.md` for examples
- **Browser DevTools**: Check for KaTeX CSS loading and math element rendering

### Best Practices for AI Agents

#### Math Implementation
- **Always use KaTeX**: Client-side rendering for performance
- **Maintain plugin order**: `remarkMath` before reading time, `rehypeKatex` first in rehype chain
- **Test with themes**: Verify math works in both light and dark modes
- **Check responsiveness**: Ensure math scales on mobile devices

#### Content Creation
- **Use standard LaTeX**: Stick to common LaTeX math commands
- **Test in Obsidian**: Verify math renders correctly in Obsidian
- **Include examples**: Provide comprehensive math examples in documentation
- **Error handling**: Graceful fallbacks for malformed math

#### Performance Optimization
- **Client-side rendering**: No server-side math processing needed
- **CSS optimization**: Only include necessary KaTeX styles
- **Responsive design**: Math should work on all screen sizes
- **Theme integration**: Math colors should adapt automatically

### Technical Implementation Notes

#### CSS Custom Properties
```css
/* KaTeX Theme Integration */
.katex {
  font-size: 1.1em;
  color: rgb(var(--color-foreground));
}

.katex-display {
  margin: 1.5rem 0;
  text-align: center;
}

/* KaTeX in dark mode */
.dark .katex {
  color: rgb(var(--color-foreground));
}
```

#### Plugin Configuration
```javascript
// astro.config.mjs
remarkPlugins: [
  // ... other plugins
  remarkMath,  // Parse LaTeX math syntax
  // ... other plugins
],
rehypePlugins: [
  rehypeKatex,  // Render math with KaTeX (first)
  // ... other plugins
]
```

#### Dependencies
```json
{
  "remark-math": "^6.0.0",
  "rehype-katex": "^7.0.0", 
  "katex": "^0.16.9"
}
```

This comprehensive math support maintains the theme's core principles of clarity, performance, and Obsidian compatibility while adding powerful mathematical typesetting capabilities.

## Mermaid Diagram Support

### Overview

The theme includes comprehensive Mermaid diagram support with optimized performance, lazy loading, and seamless theme integration. All diagrams work seamlessly in both light and dark themes and maintain full compatibility with Obsidian workflows.

### Performance Optimizations

The Mermaid implementation includes several performance optimizations to address the heavy resource usage of the Mermaid.js library:

#### Lazy Loading
- **Intersection Observer**: Diagrams only render when entering the viewport
- **50px margin**: Starts loading 50px before becoming visible
- **Automatic cleanup**: Observer is removed after rendering

#### Diagram Caching
- **Theme-aware caching**: Rendered SVGs are cached by source + theme
- **Instant theme switching**: Cached diagrams switch themes instantly
- **Memory efficient**: Cache prevents re-rendering on theme changes

#### Progressive Loading
- **Skeleton states**: Loading animations with shimmer effect
- **Smooth transitions**: Fade-in animations for rendered diagrams
- **Visual feedback**: Clear loading states during rendering

### Implementation Details

#### Dependencies
- **`mermaid`**: ^11.1.0 - Core Mermaid library for diagram rendering

#### Plugin Chain Integration
The Mermaid processing is integrated into the existing markdown pipeline:

**Remark Plugins (Processing Order):**
1. `remarkInternalLinks` - Process wikilinks and standard links
2. `remarkFolderImages` - Handle folder-based images
3. `remarkImageCaptions` - Process image captions
4. `remarkCallouts` - Process Obsidian-style callouts
5. `remarkImageGrids` - Handle image grid layouts
6. **`remarkMermaid`** - Parse Mermaid code blocks
7. `remarkReadingTime` - Calculate reading time
8. `remarkToc` - Generate table of contents

#### Performance Features
- **Lazy Loading**: Uses Intersection Observer API for viewport-based rendering
- **Diagram Caching**: Prevents re-rendering on theme changes
- **Bundle Optimization**: Tree-shaking friendly imports
- **Memory Management**: Automatic cache cleanup and observer management

### Supported Diagram Types

#### Flowcharts
```mermaid
graph TD
    A[Start] --> B{Is it?}
    B -->|Yes| C[OK]
    C --> D[Rethink]
    D --> B
    B ---->|No| E[End]
```

#### Sequence Diagrams
```mermaid
sequenceDiagram
    participant Alice
    participant Bob
    Alice->>Bob: Hello Bob, how are you?
    Bob-->>Alice: Great!
```

#### Class Diagrams
```mermaid
classDiagram
    class Animal {
        +String name
        +int age
        +makeSound()
    }
    class Dog {
        +String breed
        +bark()
    }
    Animal <|-- Dog
```

#### State Diagrams
```mermaid
stateDiagram-v2
    [*] --> Still
    Still --> [*]
    Still --> Moving
    Moving --> Still
    Moving --> Crash
    Crash --> [*]
```

#### Entity Relationship Diagrams
```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE-ITEM : contains
    CUSTOMER }|..|{ DELIVERY-ADDRESS : uses
```

#### Gantt Charts
```mermaid
gantt
    title A Gantt Diagram
    dateFormat  YYYY-MM-DD
    section Section
    A task           :a1, 2024-01-01, 30d
    Another task     :after a1  , 20d
```

#### Pie Charts
```mermaid
pie title Pets adopted by volunteers
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15
```

#### Git Graphs
```mermaid
gitGraph
   commit
   commit
   branch develop
   checkout develop
   commit
   commit
   checkout main
   merge develop
```

### Theme Integration

#### Automatic Theme Detection
- **System preference**: Detects browser dark/light mode preference
- **Manual toggle**: Responds to theme toggle button
- **Cache optimization**: Uses cached diagrams when available

#### Theme Switching Performance
- **Instant switching**: Cached diagrams switch themes immediately
- **Fallback rendering**: Re-renders only if no cache available
- **Smooth transitions**: Fade animations during theme changes

### Performance Metrics

#### Before Optimization
- **Bundle size**: ~500KB Mermaid library
- **Initial load**: All diagrams render immediately
- **Theme changes**: All diagrams re-render
- **Memory usage**: High due to repeated processing

#### After Optimization
- **Bundle size**: ~300KB (40% reduction)
- **Initial load**: 60-70% faster with lazy loading
- **Theme changes**: 90% faster with caching
- **Memory usage**: 50% reduction with caching

### Error Handling

#### Graceful Error States
- **Syntax errors**: Clear error messages with source display
- **Rendering failures**: Fallback error states
- **Loading states**: Skeleton animations during processing

#### Development Tools
- **Console logging**: Helpful debug information
- **Cache management**: `clearMermaidCache()` function for development
- **Performance monitoring**: Bundle size and render time tracking

### Obsidian Compatibility

The Mermaid implementation maintains full compatibility with Obsidian:

- **Identical Syntax**: Same `mermaid` code block syntax
- **Theme Integration**: Diagrams adapt to Obsidian themes
- **Seamless Workflow**: Write in Obsidian, publish to blog with identical rendering
- **Performance**: Optimized for web while maintaining Obsidian compatibility

### Technical Implementation

#### Core Files
- **`src/utils/mermaid.ts`**: Main Mermaid utility with optimizations
- **`src/scripts/mermaid-client.ts`**: Client-side initialization
- **`src/components/MermaidDiagram.astro`**: Diagram component with styling
- **`src/utils/remark-mermaid.ts`**: Markdown processing plugin

#### Key Functions
- **`renderAllDiagrams()`**: Sets up lazy loading for all diagrams
- **`handleThemeChange()`**: Manages theme switching with caching
- **`clearCache()`**: Development utility for cache management
- **`createIntersectionObserver()`**: Lazy loading implementation

#### CSS Optimizations
```css
/* Performance optimizations */
.mermaid-diagram {
  contain: layout style paint;
}

.mermaid-diagram svg {
  will-change: auto;
}

/* Loading skeleton with shimmer */
.mermaid-loading-skeleton::before {
  content: '';
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  animation: shimmer 2s infinite;
}
```

### Best Practices for AI Agents

#### Mermaid Implementation
- **Always use lazy loading**: Prevents performance issues with multiple diagrams
- **Maintain caching**: Don't disable diagram caching without good reason
- **Test theme switching**: Verify diagrams work in both light and dark modes
- **Monitor performance**: Check bundle size and render times

#### Content Creation
- **Use standard syntax**: Stick to common Mermaid diagram types
- **Test in Obsidian**: Verify diagrams render correctly in Obsidian
- **Include examples**: Provide comprehensive diagram examples in documentation
- **Error handling**: Graceful fallbacks for malformed diagrams

#### Performance Optimization
- **Lazy loading**: Essential for pages with multiple diagrams
- **Caching**: Critical for theme switching performance
- **Bundle size**: Monitor Mermaid library size in builds
- **Memory management**: Clear cache when needed for development

This comprehensive Mermaid support maintains the theme's core principles of clarity, performance, and Obsidian compatibility while providing powerful diagram capabilities with optimized performance.

## Native Obsidian Embed Support

### Overview

The theme includes comprehensive native Obsidian embed support for audio, video, YouTube, PDF, and Twitter/X content. All embeds use Obsidian's native syntax and automatically convert to appropriate HTML5 players or embedded content, maintaining full compatibility with Obsidian workflows.

### Implementation Details

#### Dependencies
- **`remark-obsidian-embeds`**: Custom remark plugin for processing Obsidian embed syntax
- **HTML5 Audio/Video**: Native browser support for media playback
- **YouTube API**: Embedded iframe player
- **Twitter Widget**: Twitter's official embed script

#### Plugin Chain Integration
The embed processing is integrated into the existing markdown pipeline:

**Remark Plugins (Processing Order):**
1. `remarkInternalLinks` - Process wikilinks and standard links
2. `remarkFolderImages` - Handle folder-based images
3. `remarkImageCaptions` - Process image captions
4. `remarkMath` - Parse LaTeX math syntax
5. `remarkCallouts` - Process Obsidian-style callouts
6. `remarkImageGrids` - Handle image grid layouts
7. **`remarkObsidianEmbeds`** - Process Obsidian embed syntax
8. `remarkMermaid` - Process Mermaid diagrams
9. `remarkReadingTime` - Calculate reading time
10. `remarkToc` - Generate table of contents

### Supported Embed Types

#### Audio Files
- **Syntax**: `![[audio.mp3]]`, `![[audio.wav]]`, `![[audio.ogg]]`
- **Supported Formats**: MP3, WAV, OGG, M4A, 3GP, FLAC, AAC
- **Output**: HTML5 audio player with controls and filename display
- **Styling**: Theme-aware audio player with rounded corners and borders

#### Video Files
- **Syntax**: `![[video.mp4]]`, `![[video.webm]]`, `![[video.mov]]`
- **Supported Formats**: MP4, WEBM, OGV, MOV, MKV, AVI
- **Output**: HTML5 video player with controls
- **Styling**: Responsive video player with 16:9 aspect ratio

#### YouTube Videos
- **Syntax**: `![](https://www.youtube.com/watch?v=VIDEO_ID)`
- **Supported URL Formats**:
  - `https://www.youtube.com/watch?v=VIDEO_ID`
  - `https://youtu.be/VIDEO_ID`
  - `https://www.youtube.com/embed/VIDEO_ID`
- **Output**: Responsive iframe embed with YouTube's player
- **Features**: Modest branding, no related videos, lazy loading


#### PDF Documents
- **Syntax**: `![[document.pdf]]`, `![[document.pdf#page=3]]`
- **Output**: Inline PDF viewer with download link
- **Features**: Page-specific linking, download functionality
- **Styling**: Fixed height viewer with theme-aware borders

#### Twitter/X Posts
- **Syntax**: `![](https://twitter.com/user/status/ID)`
- **Supported URL Formats**:
  - `https://twitter.com/user/status/ID`
  - `https://x.com/user/status/ID`
- **Output**: Embedded tweet using Twitter's widget script
- **Features**: Automatic tweet loading, responsive design

### Technical Implementation

#### Core Files
- **`src/utils/remark-obsidian-embeds.ts`**: Main embed processing plugin
- **`src/styles/global.css`**: Embed-specific styling
- **`astro.config.mjs`**: Plugin configuration

#### Key Functions
- **Audio Processing**: Detects audio file extensions and generates HTML5 audio elements
- **Video Processing**: Detects video file extensions and generates HTML5 video elements
- **YouTube Processing**: Extracts video IDs and generates embed iframes
- **PDF Processing**: Generates iframe viewers with download links
- **Twitter Processing**: Generates Twitter widget embeds

#### CSS Styling
```css
/* Audio embeds */
.audio-embed {
  @apply my-6 rounded-lg overflow-hidden;
  @apply bg-primary-100 dark:bg-primary-800;
  @apply border border-primary-200 dark:border-primary-600;
}

/* Video embeds */
.video-embed {
  @apply my-8 rounded-xl overflow-hidden;
  @apply aspect-video;
}

/* YouTube embeds */
.youtube-embed {
  @apply aspect-video overflow-hidden rounded-xl my-8;
}

/* PDF embeds */
.pdf-embed iframe {
  @apply w-full h-[600px] rounded-lg;
  @apply border border-primary-200 dark:border-primary-600;
}
```

### Obsidian Compatibility

The embed implementation maintains full compatibility with Obsidian:

- **Identical Syntax**: Same `![[file]]` and `![](url)` syntax as Obsidian
- **File Format Support**: All supported formats work in both Obsidian and the blog
- **Seamless Workflow**: Write in Obsidian, publish to blog with identical rendering
- **Theme Integration**: Embeds adapt to all available themes automatically

### Performance Considerations

- **Lazy Loading**: YouTube embeds use lazy loading for better performance
- **Responsive Design**: All embeds scale properly on mobile devices
- **Theme Integration**: Embeds use theme-aware colors and styling
- **Fallback Support**: Graceful degradation for unsupported browsers

### Browser Support

- **Modern Browsers**: Full support in Chrome, Firefox, Safari, Edge
- **Mobile Devices**: Responsive embed rendering on all screen sizes
- **Accessibility**: Proper ARIA labels and keyboard navigation
- **Print Styles**: Embeds render correctly in print layouts

### Best Practices for AI Agents

#### Embed Implementation
- **Always use Obsidian syntax**: Maintain compatibility with Obsidian workflows
- **Test all embed types**: Verify audio, video, YouTube, PDF, and Twitter embeds
- **Check responsive behavior**: Ensure embeds work on mobile devices
- **Verify theme compatibility**: Test embeds in both light and dark themes

#### Content Creation
- **Use standard formats**: Stick to widely supported audio/video formats
- **Test in Obsidian**: Verify embeds render correctly in Obsidian
- **Include examples**: Provide comprehensive embed examples in documentation
- **Error handling**: Graceful fallbacks for malformed embeds

#### Performance Optimization
- **Lazy loading**: Essential for pages with multiple embeds
- **Responsive design**: Critical for mobile user experience
- **Theme integration**: Embeds should adapt to all themes
- **Fallback support**: Handle cases where embeds fail to load

This comprehensive embed support maintains the theme's core principles of clarity, performance, and Obsidian compatibility while providing powerful media embedding capabilities.

## Comments System (Giscus Integration)

### Overview
The theme includes a Giscus-powered commenting system that uses GitHub Discussions for storing and managing comments.

### Configuration
Comments are controlled by a single setting in `src/config.ts`:

```typescript
features: {
  comments: true,  // Enable/disable comments (ONLY setting that matters)
}
```

### Giscus Setup Process

#### 1. Enable GitHub Discussions
1. Go to your GitHub repository
2. Click **Settings** → **General**
3. Scroll to **"Features"** section
4. Check **"Discussions"** and click **"Set up discussions"**

#### 2. Create Discussion Category
1. Go to **Discussions** tab in your repository
2. Click **"New category"**
3. Name it **"General"**
4. Set format to **"Announcement"** (prevents random users from creating discussions)
5. Description: "Comments on blog posts"

#### 3. Get Giscus Configuration
1. Visit [giscus.app](https://giscus.app)
2. Enter your repository: `username/repo-name`
3. Select **"General"** as the discussion category
4. Copy the generated **Repository ID** and **Category ID**

#### 4. Update Configuration
```typescript
comments: {
  provider: "giscus",               // Currently only Giscus supported
  repo: "username/repo-name",       // Your GitHub repository
  repoId: "R_kgDO...",             // Repository ID from Giscus
  category: "General",              // Discussion category
  categoryId: "DIC_kwDO...",       // Category ID from Giscus
  mapping: "pathname",              // How posts map to discussions
  strict: "0",                      // Allow comments on any post (auto-creates discussions)
  reactions: "1",                   // Enable reactions (1) or disable (0)
  metadata: "0",                    // Hide discussion metadata (1) or show (0)
  inputPosition: "bottom",          // Comment input position
  theme: "preferred_color_scheme",  // Theme (light/dark/preferred_color_scheme)
  lang: "en",                       // Language code
  loading: "lazy",                  // Loading strategy
}
```

### How It Works
- **Each blog post** automatically creates a GitHub discussion when someone first tries to comment
- **Visitors need GitHub accounts** to comment
- **Comments appear** both on your blog and in GitHub Discussions
- **You moderate** through GitHub's interface
- **"Announcement" format** prevents random discussion creation
- **No manual setup required** - discussions are created on-demand

### Troubleshooting

#### Comments Not Appearing
1. **Check configuration**: Verify all Giscus IDs are set correctly
2. **Verify discussions enabled**: Repository must have discussions enabled
3. **Check repository visibility**: Repository must be public
4. **Verify Giscus app**: Make sure the Giscus app is installed

#### Redirect to Homepage After Sign-in
This usually means the Giscus configuration is incorrect. Check:
- Repository ID is correct
- Category ID is correct
- Discussions are enabled on your repository

#### Styling Issues
The comments are styled to match your theme automatically. If you see styling issues:
1. Check your theme configuration
2. Verify the `theme` setting matches your site theme
3. Clear browser cache and reload

### Security & Performance
- **No server-side code** required
- **GitHub handles authentication** and authorization
- **Comments are stored** in your GitHub repository
- **Lazy loading**: Comments only load when scrolled into view
- **Minimal JavaScript**: Uses the lightweight Giscus script
- **No database**: Comments are stored as GitHub discussions

## Common AI Agent Mistakes

### Critical Distinctions to Remember

#### 1. **🚨 MATH RENDERING DUPLICATION (MOST CRITICAL)**
- **NEVER hide MathML output** - MathML is the properly formatted version
- **ALWAYS hide HTML output** - HTML output is the broken, plain text version
- **The correct CSS is:**
  ```css
  .katex-mathml { display: inline-block !important; }
  .katex-html { display: none !important; }
  ```
- **WRONG approach (causes duplication):**
  ```css
  .katex-mathml { display: none !important; }  /* DON'T DO THIS */
  .katex-html { display: inline-block !important; }  /* DON'T DO THIS */
  ```
- **This mistake causes "E=mc2E=mc2" duplication where math appears twice**

#### 2. **🚨 SWUP PAGE TRANSITIONS BREAK JAVASCRIPT (CRITICAL)**
- **NEVER assume JavaScript works after page transitions** - Swup replaces DOM content without triggering `DOMContentLoaded`
- **ALWAYS re-initialize JavaScript after Swup transitions** - Use Swup hooks in `BaseLayout.astro`
- **The correct approach:**
  ```javascript
  // In component files (e.g., TableOfContents.astro)
  function initializeMyComponent() {
    // Your initialization code here
  }
  window.initializeMyComponent = initializeMyComponent;
  document.addEventListener('DOMContentLoaded', initializeMyComponent);
  ```
  ```javascript
  // In BaseLayout.astro - add to BOTH hooks
  window.swup.hooks.on('page:view', () => {
    // ... other code ...
    if (window.initializeMyComponent) {
      window.initializeMyComponent();
    }
  });
  
  window.swup.hooks.on('visit:end', () => {
    // ... other code ...
    if (window.initializeMyComponent) {
      window.initializeMyComponent();
    }
  });
  ```
- **Common symptoms of this mistake:**
  - Interactive elements stop working after navigation
  - Event listeners are lost after page transitions
  - Components appear but don't respond to clicks
  - "It works on first load but not after navigation"
- **This is the #1 most common issue** - affects ToC collapse, command palette, theme toggles, etc.

#### 3. **🚨 PRODUCTION LOGGING (CRITICAL)**
- **NEVER use raw `console.log()`** in production code
- **Use the project's logger utility** (`src/utils/logger.ts`) for any logging needs
- **Keep console output clean** for professional deployments

#### 3. **Image System Confusion (Most Common)**
- **Post cards** show images based on `showPostCardCoverImages` config, NOT `hideCoverImage` frontmatter
- **Post content** shows images based on `hideCoverImage` frontmatter, NOT config
- These are completely separate systems - don't mix them up!

#### 3. **Linking Behavior Confusion (Important)**
- **Wikilinks (`[[...]]`) only work with posts** - this is intentional and matches Obsidian's primary use case
- **Standard markdown links (`[text](url)`) work with all content types** - use these for pages, projects, docs
- **Don't try to extend wikilinks to other collections** - use standard links instead: `[Page Title](page-slug)`, `[Project](projects/project-slug)`, `[Doc](docs/doc-slug)`
- **Linked mentions only track posts** - pages, projects, and docs are not included in linked mentions
- **File renamed for clarity**: `wikilinks.ts` → `internallinks.ts` to distinguish between the two behaviors

#### 4. **🚨 URL MAPPING SYSTEM CONFUSION (CRITICAL)**
- **URL mapping is for RENDERING ONLY** - it doesn't affect linked mentions or graph view filtering
- **Linked mentions and graph view remain posts-only** - URL mapping doesn't change this behavior
- **Two separate systems**:
  - **URL Mapping**: Applied to all content types for proper rendering (`/pages/about` → `/about`)
  - **Linked Mentions/Graph**: Only includes posts (filtered by `isPostLink` logic)
- **Don't confuse the systems** - URL mapping makes links work, but doesn't change feature scope
- **Test both systems independently** - URL mapping and linked mentions are separate concerns

#### 5. **🚨 FOLDER-BASED CONTENT ASSUMPTIONS (CRITICAL)**
- **ALL content types support folder-based organization** - not just posts
- **Pages, projects, and docs work identically to posts** for folder-based content
- **Don't assume folder-based is posts-only** - all collections handle `folder-name/index.md` structure
- **Astro Content Collections handle this automatically** - no special configuration needed
- **Asset syncing works for all content types** - images, PDFs, etc. are copied to public directory
- **URL generation is consistent** - folder name becomes slug for all content types

#### 6. **H1 Title Handling**
- **Both Posts and Pages**: NO H1 in markdown content - title comes from frontmatter, content starts with H2
- **H1 is hardcoded** in both PostLayout and PageLayout using frontmatter title
- **NEVER add H1** to any markdown content - both posts and pages have hardcoded H1s from frontmatter

#### 7. **Custom Collections Approach**
- **Use subfolders within pages collection** - avoid creating custom collections at content level
- **No Astro warnings** - subfolders within pages don't trigger auto-generation warnings
- **Same URL structure** - `/services/web-development` works the same way
- **No configuration needed** - leverages Astro's built-in folder routing
- **Two approaches work identically**:
  - `pages/services/web-development.md` → `/services/web-development`
  - `pages/services/web-development/index.md` → `/services/web-development`

#### 8. **🚨 FAVICON THEME BEHAVIOR (CRITICAL)**
- **Favicon should NOT change with manual theme toggle** - it should only change with browser system theme
- **System theme detection**: Use `window.matchMedia('(prefers-color-scheme: dark)')` to detect browser preference
- **Favicon logic**: 
  - `prefers-color-scheme: dark` → use `favicon-dark.png`
  - `prefers-color-scheme: light` → use `favicon-light.png`
  - Unknown/unsupported → use default `favicon.ico`
- **Swup compatibility**: Reinitialize favicon after page transitions based on SYSTEM theme, not user's manual theme choice
- **NEVER update favicon** when user manually toggles theme - only when system theme changes
- **Implementation**: Use CSS media queries + JavaScript system theme detection, not manual theme state

#### 9. **🎨 COLOR USAGE (CRITICAL)**
- **NEVER use hardcoded colors** - Always use theme variables from `src/themes/index.ts`
- **Use Tailwind classes** that reference theme variables (`primary-*`, `highlight-*`)
- **Include dark mode variants** for all color definitions (`dark:bg-primary-800`)
- **Check existing code** for hardcoded colors and replace them
- **Reference theme files** to understand available color scales

#### 10. **Package Manager**
- Always use `pnpm` instead of `npm` for all commands
- Scripts: `pnpm run <script-name>`, not `npm run <script-name>`

#### 11. **Deployment Platform Configuration**
- **Set platform once in config** - Use `deployment.platform` in `src/config.ts`, not environment variables
- **No environment variables needed** - The build process automatically detects the platform from config
- **Platform options**: "netlify", "vercel", "github-pages" (all lowercase with hyphens)
- **Backward compatibility**: Environment variables still work but are not recommended
- **Configuration files**: Automatically generated based on platform choice

#### 12. **Homepage Configuration Structure**
- **Use `homeOptions`** - All homepage content is now under `homeOptions`, not `features` or `homeBlurb`
- **Featured Post**: Use `homeOptions.featuredPost` with `type: "latest"` or `type: "featured"`
- **Slug Flexibility**: Slug can be present even when `type: "latest"` - it will be ignored until switched to "featured"
- **Recent Posts**: Use `homeOptions.recentPosts` with `enabled` and `count` properties
- **Projects/Docs**: Use `homeOptions.projects` and `homeOptions.docs` with `enabled` property
- **Blurb**: Use `homeOptions.blurb` with `placement: "above" | "below" | "none"`
- **Old References**: `showLatestPost`, `recentPostsCount`, and `homeBlurb` are deprecated

#### 11. **Development vs Production Behavior**
- **Development**: Missing images show placeholders, warnings are logged
- **Production**: Missing images cause build failures
- Always run `pnpm run check-images` before deploying

### Accessibility Warnings

#### 1. **"Invalid tabindex on non-interactive element"**
- Usually caused by Swup
- Check `astro.config.mjs` - `accessibility: false` should be set
- This is intentional to prevent Swup from adding invalid tabindex attributes

#### 2. **"Missing content" warnings**
- Check for proper `aria-label` attributes on interactive elements
- Ensure all interactive elements have accessible labels

#### 3. **"Redundant text in alt attribute"**
- Alt text should describe the image, not repeat visible text
- Use descriptive alt text instead of post titles
- Avoid generic descriptions like "image" or "photo"

### Performance Warnings

#### 1. **"Unoptimized loading attribute"**
- Above-the-fold images should use `loading="eager"`
- First post on pages should have `eager={true}` prop
- Check if `eager` prop is being passed correctly to PostCard components

#### 2. **Image Loading Issues**
- Verify `eager` prop is being passed correctly
- Check image loading attributes in PostCard components
- Ensure proper `fetchpriority` attributes for critical images

### Content Structure Mistakes

#### 1. **File Structure Patterns**
- **Folder-based posts**: Use `index.md` as the main content file
- **Assets**: Co-locate with `index.md` in the same folder
- **URLs**: Folder name becomes the slug automatically

#### 2. **Obsidian Integration**
- Wikilinks work without conversion - don't convert them
- Use `[[image.jpg]]` syntax for Obsidian compatibility
- Tags sync automatically between Obsidian and blog

#### 3. **Frontmatter Issues**
- Use proper YAML syntax with correct indentation
- Include all required fields for posts and pages
- Validate frontmatter before committing changes

### Development Workflow Mistakes

#### 1. **Not Using Development Tools**
- Run `pnpm run check-images` regularly
- Monitor console for development warnings
- Use placeholder system for missing assets

#### 2. **Ignoring Graceful Error Handling**
- The system handles missing tags gracefully in development
- Don't panic about console warnings - they're helpful
- Continue development with placeholders when needed

#### 3. **Build Process Issues**
- Always test builds locally before deploying
- Fix all missing images before production builds
- Use the correct build commands for your deployment platform

### Component Hierarchy Understanding

#### 1. **Layout Components**
- **BaseLayout.astro**: Main layout with Swup container
- **PostLayout.astro**: Individual post layout (hardcoded H1 from frontmatter)
- **PageLayout.astro**: Page layout (hardcoded H1 from frontmatter)

#### 2. **Content Components**
- **PostCard.astro**: Post cards for listings (controlled by config)
- **PostContent.astro**: Post content rendering (controlled by frontmatter)
- **ImageWrapper.astro**: Handles image optimization and fallbacks

#### 3. **Key Configuration Files**
- **`src/config.ts`**: Main site configuration
- **`astro.config.mjs`**: Astro and Swup configuration
- **`src/config/dev.ts`**: Development-specific settings

### Best Practices for AI Agents

#### 1. **Always Check Context**
- Read the existing code before making changes
- Understand the component hierarchy
- Check configuration files for relevant settings

#### 2. **Test Changes Incrementally**
- Make small, focused changes
- Test each change before proceeding
- Use the development server to verify changes

#### 3. **Follow the Project's Philosophy**
- Respect the Obsidian-first approach
- Maintain the modular design principles
- Preserve the seamless publishing workflow

#### 4. **Document Changes**
- Add comments for complex logic
- Update configuration documentation
- Explain non-obvious decisions

This comprehensive guide should help AI agents understand the project structure, development practices, content organization patterns, and common pitfalls when working with Astro Modular.
