# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- `npm run dev` - Start development server with Turbopack
- `npm run build` - Build the application for production
- `npm run start` - Start production server
- `npm run lint` - Run ESLint
- `registry:build` - Build the shadcn registry

## Project Architecture

This is a Next.js 15 application built as part of a UI experiments workspace. Originally a calendar/event management system demo, it's being transformed into a comprehensive **Salon SAAS Appointment Management MVP** with full-stack capabilities.

### Key Technologies

#### Frontend (Existing + Enhanced)
- **Next.js 15** with React 19 and App Router
- **shadcn/ui** components with customized registry configuration
- **Tailwind CSS v4** with custom color scheme
- **@dnd-kit** for drag and drop functionality
- **date-fns** for date manipulation
- **next-themes** for light/dark mode support

#### New Full-Stack Additions (MVP Implementation)
- **Zustand** for client-side state management (salon operations)
- **TanStack Query v5** for server state and real-time synchronization
- **Express.js + TypeScript** backend API server
- **Prisma ORM** with PostgreSQL database
- **NextAuth.js v5** for authentication and authorization
- **Socket.io** for real-time appointment updates
- **Zod** for schema validation and type safety

### Application Structure

#### Calendar System Architecture

The core feature is a comprehensive event calendar with multiple view modes:

- **Main Entry Point**: `app/page.tsx` renders the main dashboard with sidebar navigation
- **Calendar Component**: `components/big-calendar.tsx` manages sample data and event state
- **Event Calendar**: `components/event-calendar/event-calendar.tsx` is the main calendar component

#### Context Architecture

- **CalendarProvider** (`components/event-calendar/calendar-context.tsx`): Manages shared calendar state including current date and color-based event visibility
- **ThemeProvider** (`providers/theme-provider.tsx`): Handles light/dark theme switching

#### Event Calendar Features

1. **Multiple Views**: Month, Week, Day, and Agenda views with keyboard shortcuts (M/W/D/A)
2. **Drag & Drop**: Events can be dragged and dropped between time slots using @dnd-kit
3. **Event Management**: Create, edit, and delete events with toast notifications
4. **Color Filtering**: Events are categorized by color with toggleable visibility
5. **Responsive Design**: Optimized for mobile and desktop layouts

#### Component Organization

- **UI Components**: Located in `components/ui/` - shadcn components
- **Event Calendar**: Modular system in `components/event-calendar/` with separate views, context, and utilities
- **Layout Components**: Sidebar navigation (`app-sidebar.tsx`), user nav (`nav-user.tsx`)

### Data Architecture

- **Sample Events**: Hardcoded in `big-calendar.tsx` with date calculations relative to current date
- **Event Types**: Defined in `components/event-calendar/types.ts` with support for colors, locations, all-day events
- **Etiquettes**: Color-coded categories for filtering events (My Events, Marketing Team, Interviews, etc.)

### Registry Configuration

This project uses a custom shadcn registry configuration:
- Registry defined in `registry.json` with custom color schemes for light/dark themes
- Components registered for external use via shadcn CLI
- Custom CSS variables for consistent theming

### Styling

- Uses Tailwind CSS v4 with custom oklch color system
- Dark mode support with automatic theme switching
- Custom sidebar styling with specialized color variables
- Responsive design patterns throughout components

## Salon SAAS MVP Features (In Development)

### Core Business Entities
- **Salon Management**: Business profiles, settings, multi-location support
- **Staff Management**: Employee profiles, roles, permissions, scheduling
- **Customer Database**: Client profiles, preferences, appointment history
- **Service Catalog**: Service offerings, pricing, duration, staff assignments
- **Appointment System**: Enhanced calendar with salon-specific features
- **Shift Management**: Staff schedules, availability, time blocking

### Backend Architecture
- **Database Schema**: PostgreSQL with 8 core entities + relationships
- **API Design**: RESTful endpoints following OpenAPI 3.0 specification
- **Authentication**: Role-based access (OWNER, STAFF, CUSTOMER)
- **Real-time Updates**: WebSocket integration for live appointment sync
- **Data Validation**: Zod schemas for request/response validation

### Enhanced Calendar Features
- **Staff-Centric Views**: Multi-staff scheduling with color coding
- **Conflict Detection**: Prevent double-booking and scheduling conflicts
- **Drag-and-Drop Rescheduling**: Enhanced with business logic validation
- **Real-time Synchronization**: Live updates across multiple user sessions
- **Availability Management**: Integration with staff shifts and time blocks

### Implementation Status
- **Phase 0**: Research and technology decisions ✅ Complete
- **Phase 1**: Data model and API contracts ✅ Complete  
- **Phase 2**: Task generation - Next step
- **Phase 3-5**: Implementation and validation - Planned

### Specification Documents
- Feature Spec: `/specs/001-salon-saas-mvp/spec.md`
- Implementation Plan: `/specs/001-salon-saas-mvp/plan.md`
- Data Model: `/specs/001-salon-saas-mvp/data-model.md`
- API Contracts: `/specs/001-salon-saas-mvp/contracts/api-schema.yaml`
- Integration Tests: `/specs/001-salon-saas-mvp/quickstart.md`

### Development Notes
- Maintain existing calendar component architecture
- Extend CalendarEvent interface for salon-specific data
- Use progressive enhancement to preserve existing functionality
- Follow constitution principles for modular, TypeScript-first development