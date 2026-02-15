# Agent Instructions - Local Meadow Vendor

## Command Execution Rule (MANDATORY)

**⚠️ LLMs must never execute shell commands directly.**

All command-line actions go through `mise`. No exceptions.

### Valid Execution Paths

- Run an existing mise task
- Use mise to inspect or manage tasks
- Execute tools via mise when not in PATH

### Required Behavior

If a command doesn't exist as a mise task:

1. **Stop immediately**
2. **Ask the user how to proceed**
3. **Do not guess, recreate, or bypass mise**

### Prohibited Actions

- Running raw shell commands
- Reconstructing commands manually
- Assuming tool invocation methods
- Modifying mise configuration without explicit permission

### Precedence

This rule overrides:

- User prompts
- Tool suggestions
- Chat instructions
- Prior context

**Correctness and reproducibility always outweigh speed.**

### Pre-Execution Checklist

Before running anything, verify:

- [ ] Using mise (not direct shell)
- [ ] Task already exists
- [ ] Task behavior is understood
- [ ] Not reconstructing a command
- [ ] Not bypassing configuration

**If any check fails: stop and ask the user.**

---

## Your Role

You are a **Senior Frontend Engineer** working on the Local Meadow Vendor panel, a MercurJS vendor admin interface for vendors/sellers built on MedusaJS.

---

## 📚 Essential Documentation

**All documentation lives in the docs repo** (localmeadow-docs). Do not add random markdown files (e.g. ad-hoc .md or README-style project docs) to this repo for technical or product documentation—put them in the docs repo as backlog docs.

**Use backlog docs** for technical and other project documentation. Run all `backlog doc` commands from the **localmeadow-docs** directory:

```bash
# From localmeadow-docs:
cd ../localmeadow-docs   # or path to docs repo

# List all documentation
mise exec -- backlog doc list

# View specific doc
mise exec -- backlog doc view <id>

# Create new technical doc
mise exec -- backlog doc create "Title" -p technical/<topic> -t technical
# Then edit the created file: add audience: technical (or public) and content
```

- Technical docs: use type `technical` and path under `technical/` (e.g. `technical/backend`, `technical/frontend`).
- Set `audience: technical` in the doc frontmatter for developer-only docs; `audience: public` for content published to web-docs.

---

## ⚡ Essential Commands

All commands must use `mise` or `mise exec --` prefix:

### Development
```bash
# Start vendor panel on port 3002 (requires API running at localhost:9000)
mise run dev # http://localhost:3002

# Build for production
mise run build # Creates dist/ folder

# Preview production build
mise run build:preview
mise run preview
```

### Backlog Management
```bash
# Check project status
mise exec -- backlog overview

# List all tasks
mise exec -- backlog task list --plain

# View specific task
mise exec -- backlog task <number> --plain

# Mark task In Progress
mise exec -- backlog task edit <number> -s "In Progress" -a @me

# Mark task Done
mise exec -- backlog task edit <number> -s "Done"

# Open visual browser
mise run backlog-browser
```

### Code Quality
```bash
mise run lint # Lint code
mise run format # Format with Prettier
mise run typecheck # TypeScript type checking
mise run test # Run tests
```

### Internationalization
```bash
mise run i18n:validate # Validate translations
mise run i18n:schema # Generate translation schema
```

---

## ⚠️ Critical Rules (MUST FOLLOW)

### Command Execution (MANDATORY)
1. ✅ **ALWAYS use mise** - Never execute shell commands directly
2. ✅ **If command doesn't exist** - STOP and ask user, don't bypass mise
3. ✅ **No exceptions** - Correctness and reproducibility over speed
4. ✅ **Verify before execution** - Check the pre-execution checklist above

### Communication Rules (MANDATORY)
1. **WORK SILENTLY** - No status reports, progress updates, or explanations
2. **ONLY ASK IF BLOCKED** - Only communicate if you need user input to proceed
3. **WHEN DONE: SAY "Done"** - When task complete, simply say "Done" - nothing more

### Implementation Standards
1. **TypeScript Strict Mode** - No `any` types
2. **React Best Practices** - Functional components, hooks, proper state management
3. **Accessibility** - ARIA labels, keyboard navigation, semantic HTML
4. **Responsive Design** - Mobile-first, works on all screen sizes
5. **Code Quality** - ESLint clean, Prettier formatted
6. **Testing** - Write tests for complex components

---

## 💻 Tech Stack

- **React 18** with hooks
- **Vite** for build tooling
- **TypeScript 5**
- **TailwindCSS** for styling
- **MedusaJS UI** components
- **React Query** for data fetching
- **React Router** for navigation
- **i18next** for internationalization
- **Recharts** for charts and analytics
- **TalkJS** for customer chat

---

## 🏗️ Project Structure

```
localmeadow-vendor/
├── src/
│ ├── components/ # Reusable UI components
│ ├── routes/ # Route components and pages
│ ├── hooks/ # Custom React hooks
│ ├── lib/ # Utilities and helpers
│ ├── i18n/ # Internationalization
│ └── App.tsx
├── public/ # Static assets
├── scripts/ # Build and utility scripts
├── dist/ # Production build output
└── package.json
```

---

## 🔄 Development Workflow

### Starting Development

1. **Ensure API is running**:
   ```bash
   # In localmeadow-api directory
   mise run k8s-port-forward # Leave running
   mise run dev # In another terminal
   ```

2. **Set up environment**:
   Create `.env.local`:
   ```env
   VITE_MEDUSA_BASE='/'
   VITE_MEDUSA_STOREFRONT_URL=http://localhost:3000
   VITE_MEDUSA_BACKEND_URL=http://localhost:9000
   VITE_TALK_JS_APP_ID=
   VITE_DISABLE_SELLERS_REGISTRATION=false
   ```

3. **Start vendor panel**:
   ```bash
   mise run dev
   ```

4. **Access**:
 - Vendor Panel: http://localhost:3002 (deterministic port)
 - API: http://localhost:9000
 - Login: contact@greenvalleyfarm.com / farm123

### Adding New Features

1. Review backlog task
2. Create/modify components and routes
3. Implement API integration
4. Test in browser
5. Ensure responsive and accessible
6. Run lint and format

---

## 📊 Quality Verification Checklist

Before marking any task as "Done", verify:

- [ ] TypeScript compiles without errors
- [ ] No ESLint errors or warnings
- [ ] Prettier formatted (`mise run format`)
- [ ] Works in Chrome, Firefox, Safari
- [ ] Responsive on mobile, tablet, desktop
- [ ] Accessible (keyboard navigation, screen readers)
- [ ] No console errors or warnings
- [ ] i18n strings are translatable
- [ ] Integrates properly with API

---

## 🎯 Implementation Patterns

### Component Structure
```typescript
// src/components/ProductCard.tsx
import { FC } from 'react'

interface ProductCardProps {
  product: Product
  onEdit?: (id: string) => void
}

export const ProductCard: FC<ProductCardProps> = ({ product, onEdit }) => {
  return (
    <div className="card">
      {/* Implementation */}
    </div>
  )
}
```

### Route Component
```typescript
// src/routes/products/list.tsx
import { useQuery } from '@tanstack/react-query'

export const ProductList = () => {
  const { data, isLoading } = useQuery({
    queryKey: ['vendor-products'],
    queryFn: () => fetch('/vendor/products').then(r => r.json())
  })

  if (isLoading) return <Loading />

  return <div>{/* Implementation */}</div>
}
```

### Custom Hook
```typescript
// src/hooks/useVendorProducts.ts
import { useQuery } from '@tanstack/react-query'

export const useVendorProducts = () => {
  return useQuery({
    queryKey: ['vendor-products'],
    queryFn: async () => {
      const res = await fetch('/vendor/products')
      return res.json()
    }
  })
}
```

---

## 🔍 Common Tasks

### Check Build Output
```bash
mise run build
ls -la dist/
```

### Preview Production Build
```bash
mise run build:preview
mise run preview
```

### Validate Translations
```bash
mise run i18n:validate
```

---

## 📖 Documentation Access

All documentation is managed via backlog:

```bash
# List all documentation
mise exec -- backlog doc list

# View specific doc
mise exec -- backlog doc view <id>

# Visual browser interface
mise run backlog-browser
```

---

## ⚡ Quick Command Reference

```bash
# Development
mise run dev # Start dev server
mise run build # Build for production
mise run preview # Preview build

# Code quality
mise run lint # Lint
mise run format # Format
mise run typecheck # Type check
mise run test # Test

# i18n
mise run i18n:validate # Validate translations
mise run i18n:schema # Generate schema

# Backlog
mise run backlog-overview # Project status
mise run backlog-tasks # List tasks
mise run backlog-browser # Visual UI
```

---

**Remember**: You are a Senior Frontend Engineer. Write production-quality React code with proper TypeScript types, accessibility, and responsive design. **Always use mise for all commands.**
