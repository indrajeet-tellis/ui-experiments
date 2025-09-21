# Experiment-06 Calendar Constitution
<!-- UI Experiments Calendar System -->

## Core Principles

### I. Component Modularity
<!-- Component-First Architecture -->
Every calendar feature is built as a modular, reusable component with clear boundaries. Components must be self-contained within their directories, have defined interfaces (types.ts), and maintain separation of concerns. The event-calendar system exemplifies this with distinct view components (month, week, day, agenda) that share common interfaces.

### II. Context-Driven State Management
<!-- Centralized State with Context -->
All shared state flows through React Context providers. Calendar state (date, view) and theme state are managed centrally via CalendarProvider and ThemeProvider. Local component state is only used for UI-specific interactions that don't need to be shared across the application.

### III. TypeScript-First Development (NON-NEGOTIABLE)
<!-- Strong Typing Required -->
All components, hooks, and utilities must be fully typed with TypeScript. Interface definitions in types.ts files are mandatory. Props interfaces, event handlers, and data structures require explicit typing. No 'any' types except for well-documented edge cases.

### IV. Accessibility-First UI
<!-- Universal Design Principles -->
All interactive elements must support keyboard navigation, screen readers, and ARIA attributes. Calendar navigation includes keyboard shortcuts (M/W/D/A for views). Focus management and semantic HTML structure are required. Dark/light theme support is mandatory for all UI components.

### V. Performance & User Experience
<!-- Optimized Interactions -->
Event operations must provide immediate visual feedback via toast notifications. Drag-and-drop interactions should snap to logical time boundaries (15-minute intervals). Loading states and transitions must be smooth. Date calculations are optimized to prevent unnecessary re-renders using useMemo and proper dependency arrays.

## Technology Standards
<!-- Required Stack & Patterns -->

- **Next.js 15** with App Router for routing and server components
- **shadcn/ui** components following the established registry configuration
- **Tailwind CSS v4** with custom oklch color system for consistent theming
- **@dnd-kit** for all drag-and-drop functionality
- **date-fns** for date manipulation (no other date libraries)
- **Sonner** for toast notifications
- **Lucide React** for iconography

## Development Patterns
<!-- Required Implementation Patterns -->

- **Event Handling**: All calendar events must flow through the CalendarProvider context
- **Component Structure**: Follow the event-calendar pattern with index.ts exports and organized subdirectories
- **Responsive Design**: Mobile-first approach with max-sm, max-md breakpoints
- **Error Boundaries**: Event operations must handle errors gracefully with user feedback
- **State Updates**: Use functional state updates for arrays and objects to prevent mutation bugs

## Governance
<!-- Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

This constitution governs all development within the experiment-06 calendar system. New features must align with the modular component architecture. Breaking changes to shared interfaces (CalendarEvent, CalendarView) require updating all dependent components. All calendar functionality must maintain backwards compatibility with the existing registry configuration.

**Version**: 1.0.0 | **Ratified**: 2025-01-21 | **Last Amended**: 2025-01-21
<!-- Initial constitution based on existing codebase analysis -->