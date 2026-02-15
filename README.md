# Local Meadow Vendor Panel

Vendor admin panel for Local Meadow marketplace. Built on [MercurJS Vendor Panel](https://github.com/mercurjs/vendor-panel).

**Documentation**: In the **localmeadow-docs** repo under `backlog/docs/` and `backlog/decisions/`. From that repo: `mise exec -- backlog doc list` then `mise exec -- backlog doc view <id>`.

## Features

- **Product Management**: Add, edit, and organize products with ease
- **Order Tracking**: Monitor order statuses and manage fulfillment processes
- **Store Customization**: Update vendor store details
- **Analytics Dashboard**: Gain insights into sales performance and customer behavior
- **Customer Chat**: Communicate with customers via TalkJS integration

## Quick Start

### Prerequisites

- Node 20+ (managed via mise)
- Local Meadow API running at http://localhost:9000

### Installation

```bash
# Install dependencies
npm install

# Set up environment
cp .env.template .env.local
# Edit .env.local with your configuration
```

### Development

```bash
# Start development server
mise run dev
# Opens at http://localhost:5173
```

### Login Credentials

```
Email: contact@greenvalleyfarm.com
Password: farm123
```

## Available Commands

```bash
# Development
mise run dev              # Start dev server
mise run build            # Build for production
mise run preview          # Preview production build

# Code Quality
mise run lint             # Lint code
mise run format           # Format code
mise run typecheck        # Type check
mise run test             # Run tests

# Internationalization
mise run i18n:validate    # Validate translations
mise run i18n:schema      # Generate translation schema

# Backlog Management
mise run backlog-browser  # Open backlog UI
mise run backlog-overview # Show backlog overview
mise run backlog-tasks    # List all tasks
```

## Tech Stack

- **React 18** with TypeScript
- **Vite** for build tooling
- **TailwindCSS** for styling
- **MedusaJS SDK** for API integration
- **React Query** for data fetching
- **React Router** for navigation
- **Recharts** for analytics
- **TalkJS** for customer chat
- **i18next** for internationalization

## Environment Variables

```env
VITE_MEDUSA_BASE='/'
VITE_MEDUSA_STOREFRONT_URL=http://localhost:3000
VITE_MEDUSA_BACKEND_URL=http://localhost:9000
VITE_TALK_JS_APP_ID=<your-talkjs-app-id>
VITE_DISABLE_SELLERS_REGISTRATION=false
```

## Project Structure

```
localmeadow-vendor/
├── src/
│   ├── components/       # Reusable UI components
│   ├── routes/          # Route components and pages
│   ├── hooks/           # Custom React hooks
│   ├── lib/             # Utilities and helpers
│   ├── i18n/            # Internationalization
│   └── App.tsx          # Root component
├── public/              # Static assets
├── scripts/             # Build and utility scripts
├── dist/                # Production build output
└── package.json
```

## Documentation

For more information, see:

- [Agent Instructions](./AGENTS.md) - AI agent guidelines
- [MercurJS Documentation](https://mercurjs.com)
- [Medusa Documentation](https://docs.medusajs.com)

## License

Based on [MercurJS Vendor Panel](https://github.com/mercurjs/vendor-panel)
