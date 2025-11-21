# SEAL Template: Web Development Projects

Plug-and-play SEAL configuration for frontend/fullstack web development.

## Quick Start

```bash
# 1. Copy this template to your project
cp WEB-DEVELOPMENT-SEAL.md your-project/.claude/seal/

# 2. Initialize SEAL
/seal-start --template web-development

# 3. Start coding!
# SEAL will learn your React/Vue/Svelte patterns automatically
```

## Pre-Configured Task Types

### 1. Component Creation

**Pattern**: Creating new UI components

**Initial Strategy**:
```markdown
When creating a component:
1. Check existing components for patterns
   - File: src/components/**/*.{jsx,tsx,vue,svelte}
   - Note: naming conventions, file structure, prop patterns
2. Match styling approach
   - CSS Modules, Styled Components, Tailwind, etc.
   - Check imports in similar components
3. Create with TypeScript types (if project uses TS)
   - Look at existing prop types
   - Follow interface naming conventions
4. Include basic accessibility
   - ARIA labels, keyboard navigation, semantic HTML
5. Create co-located test file if pattern exists
   - Check if components have .test.js files
   - Match test structure
```

**Example prompts**:
- "Create a UserProfile component"
- "Add a SearchBar component to the header"
- "Build a Card component for the dashboard"

### 2. API Integration

**Pattern**: Integrating with backend APIs or third-party services

**Initial Strategy**:
```markdown
When integrating an API:
1. Find existing API client/service files
   - Common locations: src/api/, src/services/, src/lib/
   - Check how other APIs are called
2. Match data fetching approach
   - fetch, axios, SWR, React Query, etc.
   - Look for custom hooks like useApi
3. Handle loading and error states
   - Check existing patterns for loading spinners
   - See how errors are displayed (toasts, inline, etc.)
4. Add TypeScript types for responses (if TS project)
   - Check src/types/ or inline types
   - Match naming conventions
5. Update state management appropriately
   - Redux, Zustand, Jotai, Context, local state
   - Follow existing patterns
```

**Example prompts**:
- "Add Stripe payment integration"
- "Fetch user data from /api/users"
- "Integrate SendGrid for email notifications"

### 3. State Management

**Pattern**: Managing application state

**Initial Strategy**:
```markdown
When managing state:
1. Identify state management solution
   - Redux, Zustand, Jotai, Recoil, MobX, Context
   - Check package.json and existing stores
2. Match existing store structure
   - Slice patterns, action creators, selectors
   - Folder organization
3. Follow naming conventions
   - actionTypes, reducers, selectors naming
4. Add TypeScript types (if TS project)
   - State shape, action types, dispatch types
5. Consider local vs global state
   - When to use component state vs store
   - Check existing patterns
```

**Example prompts**:
- "Add cart state management"
- "Create a Redux slice for user preferences"
- "Manage form state with React Hook Form"

### 4. Routing & Navigation

**Pattern**: Adding routes and navigation

**Initial Strategy**:
```markdown
When adding routes:
1. Check routing library
   - React Router, Next.js, TanStack Router, Vue Router
2. Match route definition pattern
   - File-based (Next.js) or config-based
   - Look at existing routes
3. Add navigation links
   - Match existing Link/NavLink patterns
   - Update navigation components
4. Handle route guards if needed
   - Auth checks, role checks
   - See how existing protected routes work
5. Add page-level components
   - Match folder structure (pages/, views/, routes/)
   - Include layouts if pattern exists
```

**Example prompts**:
- "Add a /dashboard route"
- "Create a protected route for /admin"
- "Add navigation to the settings page"

### 5. Form Handling

**Pattern**: Building forms with validation

**Initial Strategy**:
```markdown
When creating forms:
1. Identify form library (if any)
   - React Hook Form, Formik, uncontrolled, controlled
   - Check existing forms
2. Match validation approach
   - Yup, Zod, built-in validation
   - Error display patterns
3. Handle submission
   - API calls, optimistic updates
   - Success/error feedback (toasts, inline messages)
4. Add accessibility
   - Labels, error announcements, keyboard support
5. Match styling
   - Input components, button styles, layout
```

**Example prompts**:
- "Create a login form with validation"
- "Add a user registration form"
- "Build a multi-step checkout form"

### 6. Styling & Theming

**Pattern**: Styling components and implementing themes

**Initial Strategy**:
```markdown
When styling:
1. Identify styling approach
   - CSS Modules, Styled Components, Tailwind, Emotion
   - Check existing components
2. Match design system
   - Colors, spacing, typography
   - Look for theme files or design tokens
3. Handle responsive design
   - Breakpoints, mobile-first vs desktop-first
   - Check existing responsive patterns
4. Follow component styling patterns
   - BEM, CSS-in-JS, utility classes
5. Consider dark mode if implemented
   - Check for theme switching logic
   - Use CSS variables or themed styles
```

**Example prompts**:
- "Style the header component"
- "Add dark mode support"
- "Make the dashboard responsive"

## Project Structure Detection

SEAL will auto-detect:

**Framework**:
- React: `react` in package.json, `.jsx/.tsx` files
- Vue: `vue` in package.json, `.vue` files
- Svelte: `svelte` in package.json, `.svelte` files
- Next.js: `next` in package.json, `pages/` or `app/` directory
- Vite: `vite` in package.json

**Styling**:
- Tailwind: `tailwindcss` in package.json
- Styled Components: `styled-components` in package.json
- CSS Modules: `*.module.css` files
- Sass: `*.scss` files

**State Management**:
- Redux: `@reduxjs/toolkit` or `redux` in package.json
- Zustand: `zustand` in package.json
- Jotai: `jotai` in package.json
- MobX: `mobx` in package.json

**Testing**:
- Jest: `jest` in package.json
- Vitest: `vitest` in package.json
- Testing Library: `@testing-library/react` in package.json
- Cypress: `cypress` in package.json

## Auto-Bootstrap Patterns

On initialization, SEAL will:

1. **Analyze component structure**:
   ```bash
   # Find common component patterns
   src/components/**/*.{jsx,tsx,vue,svelte}

   # Learn:
   # - Naming: PascalCase, kebab-case, etc.
   # - Structure: index files, co-location
   # - Props: destructuring, types, defaults
   ```

2. **Detect styling patterns**:
   ```bash
   # Check first 5 components
   # - Import statements for styles
   # - className patterns
   # - Inline styles vs external
   ```

3. **Learn data fetching**:
   ```bash
   # Look for API calls in src/
   # - fetch, axios, custom hooks
   # - Loading states
   # - Error handling
   ```

4. **Understand testing**:
   ```bash
   # Find test files
   # - Naming: .test.js, .spec.js
   # - Structure: describe/it, test()
   # - Mocking patterns
   ```

## Success Metrics for Web Development

SEAL tracks:

- **Component consistency**: % of new components matching patterns
- **TypeScript coverage**: % of new code with types (if TS project)
- **Test coverage**: % of components with tests
- **Accessibility score**: ARIA usage, semantic HTML
- **Performance**: Bundle size impact, lazy loading usage

View with:
```bash
/seal-metrics --detailed
```

## Common Learned Patterns (Examples)

After 10+ tasks, SEAL typically learns:

### Pattern: "Create React Component"
```markdown
**Confidence**: 0.92 (based on 12 successful tasks)

1. Check src/components/ for similar components
2. Use functional component with TypeScript
3. Match existing prop destructuring pattern
4. Import styles from component.module.css
5. Add PropTypes or TS interface
6. Create co-located test file
7. Export from components/index.ts
```

### Pattern: "Fix Console Error"
```markdown
**Confidence**: 0.87 (based on 8 successful tasks)

1. Read browser console output carefully
2. Check React DevTools for component tree
3. Look for key prop warnings → add unique keys
4. Check for missing dependencies in useEffect
5. Verify prop types match usage
6. Test fix in browser
```

### Pattern: "Add API Endpoint Call"
```markdown
**Confidence**: 0.94 (based on 15 successful tasks)

1. Find src/api/ or src/services/
2. Match existing API client pattern (axios instance)
3. Add TypeScript types for request/response
4. Create custom hook if pattern exists (useApi pattern)
5. Add error handling with toast notifications
6. Update relevant component to use hook
7. Test with mock data first
```

## Integration Examples

### Example 1: Component Creation

**User**: "Create a NotificationBadge component"

**SEAL applies learned pattern**:
```markdown
[Automatically reads src/components/Badge.tsx as reference]
[Checks styling approach → Tailwind CSS]
[Identifies pattern → functional component with TS]
[Creates component matching style]
```

**After 3+ similar tasks, SEAL learns**:
- Always check for similar components
- Use Tailwind utility classes
- Add hover states
- Include dark mode classes
- Export from index

### Example 2: API Integration

**User**: "Fetch product data from /api/products"

**SEAL applies learned pattern**:
```markdown
[Reads src/api/client.ts → sees axios instance]
[Reads src/hooks/useApi.ts → custom hook pattern]
[Creates useProducts hook following pattern]
[Adds TypeScript Product interface]
[Handles loading/error states like existing hooks]
```

**SEAL learned this because**:
- Previous API tasks followed same structure
- Success rate was 100% when matching pattern
- User rated highly when consistency maintained

## Troubleshooting

### "SEAL isn't detecting my framework"

Check package.json:
```bash
/seal-status

# If detection failed, manually set:
# Edit .claude/seal/config.json
{
  "project_metadata": {
    "type": "web-development",
    "frameworks": ["react", "next", "tailwind", "typescript"]
  }
}
```

### "Patterns aren't matching my style"

After 2-3 tasks with your preferred style:
```bash
/seal-review --rating 5

# SEAL will start learning your specific patterns
# It adapts to YOUR codebase, not generic patterns
```

### "I want to reset a bad pattern"

```bash
# Edit .claude/seal/learned-patterns.json
# Remove or modify the pattern
# Or use:
/seal-reset-pattern "component-creation"
```

## Advanced Configuration

### Custom Task Types

Add web-specific task types to `.claude/seal/task-types.json`:

```json
{
  "responsive-design": {
    "description": "Making layouts responsive",
    "trigger_keywords": ["responsive", "mobile", "breakpoint"],
    "learned_prompts": []
  },
  "performance-optimization": {
    "description": "Optimizing bundle size and runtime performance",
    "trigger_keywords": ["performance", "slow", "optimize", "bundle"],
    "learned_prompts": []
  },
  "accessibility-improvement": {
    "description": "Improving accessibility",
    "trigger_keywords": ["a11y", "accessibility", "aria", "keyboard"],
    "learned_prompts": []
  }
}
```

### Auto-Review on Git Commit

Add to `.git/hooks/post-commit`:
```bash
#!/bin/bash
echo "🤖 SEAL: Learning from this commit..."
claude-code /seal-review --auto --rating 4
```

## Best Practices

1. **Rate consistently**: Always use `/seal-review` after tasks
2. **Be specific in feedback**: Note what specifically worked/didn't
3. **Let it learn**: Need 3-5 tasks before patterns emerge
4. **Review patterns monthly**: Check `.claude/seal/learned-patterns.json`
5. **Share patterns**: Export successful patterns for team use

## Next Steps

1. Initialize SEAL: `/seal-start --template web-development`
2. Complete 3-5 tasks of each type
3. Review and rate each: `/seal-review`
4. Watch patterns emerge: `/seal-metrics`
5. Enjoy improving efficiency! 🚀

---

**Your web development workflow will improve with every task.**
