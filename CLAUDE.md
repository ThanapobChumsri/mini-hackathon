# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Nuxt 3 recruitment management application built for screening resumes and managing job applicants. The application uses @nuxt/ui for the component library, Tailwind CSS for styling, and Yup for form validation. The core functionality revolves around creating job postings, uploading candidate resumes (PDF), and reviewing applicants against custom criteria.

## Key Architecture Patterns

### Multi-View Page Pattern
The Recruitment page (`pages/Recruitment/index.vue`) demonstrates a three-view workflow pattern using a single page component:
- **Form View**: Job creation with criteria definition (mandatory/preferred toggles)
- **Upload View**: Resume upload with drag-and-drop via the `dnd.vue` component
- **Applicants View**: Candidate comparison table with scoring and filtering

This pattern uses `activeView` ref to toggle between views and maintains state across all three views in the same component. When working with similar multi-step workflows, follow this pattern rather than creating separate route components.

### Authentication Flow
- Cookie-based authentication using `useCookie('token')` from Nuxt
- Middleware guard at `middleware/auth.ts` protects all routes except `/login`
- Login page uses two-schema Yup validation (login vs register) with conditional rendering
- Logout clears cookie and redirects to login page

### File Upload Architecture
The `components/dnd.vue` component provides two UI types:
- Type "1": Single file preview with delete button (for forms)
- Type "2": Bulk upload area used in Recruitment flow (PDF resumes)

File uploads emit `@upload` events with `{ file, preview, name }` payload. Parent components maintain uploaded file lists and pass them to FormData when ready for API submission.

### Layout Structure
- `app.vue`: Root component that wraps `<NuxtLayout>` and `<NuxtPage>`
- `layouts/default.vue`: Includes header component and main content area
- `layouts/login.vue`: Minimal layout for authentication pages
- Header component (`components/header.vue`) provides navigation between "Screen Resume" (index) and "TOR" pages

### Component Composition
The app uses a flat component structure without deep nesting. Shared components include:
- `dnd.vue`: Drag-and-drop file upload with file type/size validation
- `header.vue`: Main navigation bar with logout
- `dashboard.vue`: Dashboard view accessible from index page
- Various table components for displaying data

## Development Commands

```bash
# Install dependencies (runs nuxt prepare automatically)
npm install

# Start development server at http://localhost:3000
npm run dev

# Build for production (outputs to .output/)
npm run build

# Preview production build
npm run preview

# Generate static site
npm run generate
```

## Important Implementation Notes

### Working with Forms
- Use Yup schemas for validation (imported from 'yup')
- Leverage @nuxt/ui form components (`UForm`, `UFormGroup`, `UInput`, etc.)
- Form state is managed with Vue `reactive()` for multi-field forms

### State Management
- No Pinia or Vuex stores are configured
- State is component-local using Vue 3 Composition API (`ref`, `reactive`, `computed`)
- Route parameters and navigation handled via `useRouter()` and `useRoute()`

### Styling Conventions
- Tailwind CSS utility classes throughout (inline in templates)
- Custom design system uses specific color palette:
  - Primary blue: `#4b7ee8`
  - Background: `#eef2f9`
  - Border colors: `#e0e6f4`, `#dbe3f6`, `#e6ebf8`
  - Text colors: `#1f2a44` (headings), `#4b5c7c` (body), `#7a87a6` (muted)
- Rounded corners use large values: `rounded-3xl`, `rounded-[28px]`, `rounded-full`
- Box shadows use soft, layered approach: `shadow-[0_24px_60px_rgba(15,23,42,0.05)]`

### API Integration (Pending)
Multiple TODO comments indicate API integration points:
- `pages/login.vue:132-136`: Login/register API calls
- `pages/Recruitment/index.vue:777`: Resume upload and criteria submission
- `components/header.vue:21`: Logout API call

When implementing APIs, use Nuxt's `$fetch` composable and handle FormData for file uploads.

### Navigation Structure
- `/` - Main jobs dashboard with list and create button
- `/login` - Authentication page (protected by middleware)
- `/Recruitment` - Job creation and applicant management workflow
- `/Tor` - TOR page (Terms of Reference, currently empty placeholder)

## Common Patterns to Follow

### Creating New Pages
1. Add `.vue` file to `pages/` directory (file-based routing)
2. Use `definePageMeta()` to set layout and middleware
3. Import required composables (`useRouter`, `useRoute`, etc.)
4. Follow existing color scheme and spacing patterns

### Adding New Components
1. Place in `components/` directory with descriptive lowercase names
2. Use `<script setup>` with TypeScript hints where needed
3. Emit events for parent communication (avoid direct state mutation)
4. Document props with JSDoc-style comments or TypeScript interfaces

### Working with Tables
The codebase uses custom table markup rather than a table component library. Follow the pattern in `pages/index.vue` and `pages/Recruitment/index.vue`:
- Wrap in rounded container with border
- Use thead with uppercase, tracked headings
- Apply hover states on tbody rows
- Include action buttons in first column

## File Organization

```
.
├── app.vue                    # Root app component
├── nuxt.config.ts             # Nuxt configuration
├── package.json               # Dependencies and scripts
├── assets/
│   ├── css/main.css          # Global styles
│   └── logo.png              # App logo
├── components/
│   ├── dnd.vue               # Drag-and-drop upload
│   ├── header.vue            # Main navigation
│   ├── dashboard.vue         # Dashboard component
│   └── ...                   # Other reusable components
├── layouts/
│   ├── default.vue           # Main layout with header
│   └── login.vue             # Auth layout
├── middleware/
│   └── auth.ts               # Route authentication guard
├── pages/
│   ├── index.vue             # Jobs dashboard
│   ├── login.vue             # Login/register page
│   ├── Recruitment/
│   │   └── index.vue         # Multi-step recruitment workflow
│   └── Tor/
│       └── index.vue         # TOR page (placeholder)
└── public/                   # Static assets served as-is
```

## Known Limitations & TODOs

- No backend API implemented (mock data in components)
- No automated testing configured (consider Vitest + @nuxt/test-utils)
- Filter and Sort buttons in tables are not functional
- Resume analysis/scoring is simulated with hardcoded data
- No error boundary or global error handling
- No loading states for async operations
