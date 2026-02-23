
> 
> You are a Senior Frontend Engineer expert in Next.js 15 and the modern React ecosystem. Your task is to design the UI architecture and build highly reusable components for my dashboard application.
> **Tech Stack & Environment:**
>
> - Framework: Next.js 15 (App Router strictly required)
> - Language: TypeScript (Strict mode)
> - Styling: Tailwind CSS, Shadcn UI, TailAdmin (for dashboard layout reference)
> - Data Management: TanStack Table (React Table v8) + TanStack Query (React Query v5)
> - Build Config: output: 'standalone' in next.config.ts
> - Architecture: Strictly Frontend-only. Data is fetched from an external API.
>
> **Task:**
> Create several core reusable components, specifically:
>
> 1. **Dashboard Wrapper/Layout:** A responsive layout with a left Sidebar Navigation (collapsible) and a dynamic main content area.
> 2. **Breadcrumb:** A dynamic breadcrumb component that reads the current route or accepts custom props.
> 3. **Data Table:** A smart, highly reusable table component using TanStack Table and React Query. It must support pagination, sorting, and handle external API states (loading, error, success) gracefully.
>
> **Strict Constraints:**
>
> 1. **Domain-Driven Structure:** Do NOT group everything into a global `components/` folder. Group code by domain (e.g., table-related logic and UI stay together in a shared domain, layout in a layout domain).
> 2. **API Separation:** Separate API call logic (fetcher functions) from UI components using custom React Query hooks.
> 3. **Clean Code:** Use clear TypeScript interfaces. Avoid over-engineering.
>
> **Action:**
> Please review the provided `plan.md` first. Once you understand the structure and plan, proceed to generate the complete implementation code for the requested components step-by-step according to the plan.

---

````markdown
# Frontend Dashboard Implementation Plan

## 1. Tech Stack Overview

- **Framework:** Next.js 15 (App Router)
- **Runtime & Package Manager:** Bun
- **Language:** TypeScript
- **Styling:** Tailwind CSS, Shadcn UI, TailAdmin inspiration
- **State & Data Fetching:** TanStack Query (v5)
- **Table Management:** TanStack Table (v8)
- **Build Output:** Standalone (`output: 'standalone'` in Next config)

## 2. Architectural Guidelines (Domain-Driven)

To maintain scalability and a clean developer experience (DX), the project will group files by domain rather than file type. Since this is a pure frontend application consuming an external API, API logic will be decoupled from UI components.

**Proposed Directory Structure:**

```text
src/
├── app/                      # Next.js App Router (Pages & Layouts)
│   ├── (dashboard)/          # Route group for dashboard
│   │   ├── layout.tsx        # Uses the DashboardWrapper
│   │   └── page.tsx
│   └── layout.tsx            # Root layout (QueryClientProvider goes here)
├── core/                     # Core application configurations
│   ├── api/                  # Axios/Fetch instances & interceptors for external API
│   └── query/                # React Query client setup
├── domains/                  # Domain-driven modules
│   ├── layout/               # Layout domain
│   │   ├── components/       # Sidebar, Header, DashboardWrapper
│   │   └── types.ts          # Layout-specific interfaces
│   ├── shared/               # Shared UI elements across domains
│   │   ├── breadcrumb/       # Breadcrumb component & logic
│   │   ├── datatable/        # Reusable TanStack Table wrapper
│   │   └── ui/               # Shadcn UI base components (buttons, inputs)
│   └── users/                # Example feature domain (e.g., User Management)
│       ├── components/       # User-specific UI
│       ├── hooks/            # React Query hooks (e.g., useUsers)
│       ├── services/         # API call functions (e.g., fetchUsers)
│       └── types.ts          # User entity interfaces
```
````

## 3. Component Specifications

### A. Dashboard Wrapper (`domains/layout/components/DashboardWrapper.tsx`)

- **Purpose:** Provide the main shell for the application.
- **Features:**
- Left Sidebar (Navigation) with active state highlighting.
- Top Header (User profile, notifications).
- Main content area (`children`).
- Fully responsive (collapsible sidebar on mobile/tablet).

- **Dependencies:** Shadcn UI (Sheet for mobile sidebar, ScrollArea).

### B. Breadcrumb (`domains/shared/breadcrumb/Breadcrumb.tsx`)

- **Purpose:** Show the user's current location within the app hierarchy.
- **Features:**
- Utilizes Next.js `usePathname` to auto-generate links.
- Accepts an optional `items` prop to override auto-generation for complex dynamic routes.

- **Dependencies:** Shadcn UI (Breadcrumb components), `lucide-react` for icons.

### C. Smart Data Table (`domains/shared/datatable/DataTable.tsx`)

- **Purpose:** A highly reusable, headless-UI based table to display external API data.
- **Features:**
- **Headless Power:** Built on `@tanstack/react-table`.
- **Data Fetching Integration:** Designed to easily plug in `@tanstack/react-query` hooks.
- **Capabilities:** Server-side pagination, sorting, and global/column filtering.
- **Loading/Error States:** Built-in skeletons or spinners for loading states, and error boundary fallbacks.

- **Props Interface:**
- `columns`: ColumnDef<TData, TValue>[]
- `data`: TData[]
- `pageCount`: number (for server-side pagination)
- `isLoading`: boolean
- `onPaginationChange`, `onSortingChange`: Functions to update query params.

## 4. Execution Steps (For AI Agent)

1. **Initialize Project & Dependencies:**

- Ensure `next.config.ts` has `output: 'standalone'`.
- Command: `bun add @tanstack/react-query @tanstack/react-table lucide-react`
- Initialize Shadcn UI and add required base components (table, button, input, breadcrumb, sheet).

2. **Setup Core Providers:**

- Create `QueryClientProvider` wrapper in `src/core/query/QueryProvider.tsx`.
- Wrap the root layout.

3. **Build Shared Components:**

- Implement `Breadcrumb.tsx`.
- Implement `DataTable.tsx` with generic TypeScript types (`<TData, TValue>`).

4. **Build Layout Domain:**

- Implement the Sidebar, Header, and `DashboardWrapper.tsx`.

5. **Integration Example:**

- Create a dummy domain (e.g., `posts` or `users`) fetching from a public placeholder API to demonstrate how the `DataTable`, `React Query`, and `DashboardWrapper` work together.
