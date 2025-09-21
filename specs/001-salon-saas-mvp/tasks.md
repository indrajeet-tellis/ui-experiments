# Tasks: Salon SAAS Appointment Management MVP

**Input**: Design documents from `/specs/001-salon-saas-mvp/`
**Prerequisites**: plan.md (✅), research.md (✅), data-model.md (✅), contracts/ (✅), quickstart.md (✅)

## Execution Flow (main)
```
1. Load plan.md from feature directory ✅
   → Tech stack: Next.js 15, Express.js, PostgreSQL, Prisma, Zustand, TanStack Query
   → Structure: Web app (frontend + backend)
2. Load design documents ✅:
   → data-model.md: 8 entities (User, Salon, Staff, Customer, Service, Appointment, StaffShift, TimeBlock)
   → contracts/: 14 API endpoints across 6 resource groups
   → research.md: Technology decisions and architecture patterns
   → quickstart.md: 8 integration test scenarios
3. Generate tasks by category ✅
4. Apply task rules: Different files = [P], Tests before implementation ✅
5. Number tasks sequentially (T001-T048) ✅
6. Generate dependency graph ✅
7. Create parallel execution examples ✅
8. Validate task completeness ✅
```

## Format: `[ID] [P?] Description`
- **[P]**: Can run in parallel (different files, no dependencies)
- Include exact file paths in descriptions

## Path Conventions
Based on plan.md structure decision: **Web app (frontend + backend)**
- **Backend**: `backend/src/`, `backend/tests/`
- **Frontend**: `app/`, `components/`, `lib/` (existing Next.js structure)

## Phase 3.1: Project Setup & Infrastructure

### T001: Initialize Backend Project Structure
Create backend directory with Express.js + TypeScript foundation:
- `backend/src/` with subdirectories: `models/`, `services/`, `routes/`, `middleware/`, `utils/`
- `backend/tests/` with subdirectories: `contract/`, `integration/`, `unit/`
- `backend/package.json` with Express.js, Prisma, TypeScript, Jest, Supertest
- `backend/tsconfig.json` with strict TypeScript configuration
- `backend/.env.example` with database and auth configuration

### T002: [P] Configure Backend Dependencies and Tools
Setup development tooling in `backend/`:
- ESLint + Prettier configuration for TypeScript
- Jest test configuration with TypeScript support
- Nodemon for development hot reload
- Prisma CLI and database connection setup
- CORS, helmet, and security middleware

### T003: [P] Database Schema and Prisma Setup
Initialize Prisma in `backend/prisma/`:
- `schema.prisma` with 8 core entities from data-model.md
- Database indexes for performance (appointments by date, customer search)
- Migration files for schema creation
- Seed script with sample salon data
- Connection pooling configuration

### T004: [P] Frontend State Management Setup
Install and configure client-side state management:
- Zustand stores in `lib/stores/` for salon operations
- TanStack Query configuration in `lib/query/`
- Type definitions in `types/salon.ts` extending existing calendar types
- API client setup in `lib/api/` with proper error handling

## Phase 3.2: Contract Tests First (TDD) ⚠️ MUST COMPLETE BEFORE 3.3
**CRITICAL: These tests MUST be written and MUST FAIL before ANY implementation**

### T005: [P] Authentication Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `auth.test.ts`: POST /auth/register, POST /auth/login, POST /auth/logout, GET /auth/me
- Validate request/response schemas against OpenAPI spec
- Test JWT token format and expiration
- Test role-based response differences

### T006: [P] Salon Management Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `salons.test.ts`: GET /salons, POST /salons, GET /salons/{id}, PUT /salons/{id}
- Validate business hours schema and timezone handling
- Test address validation and required fields
- Test owner-only access restrictions

### T007: [P] Staff Management Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `staff.test.ts`: GET/POST /salons/{id}/staff, GET/PUT/DELETE /salons/{id}/staff/{staffId}
- Validate staff permissions enum and color code format
- Test staff-service qualification endpoints
- Test employee ID uniqueness within salon

### T008: [P] Customer Management Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `customers.test.ts`: GET/POST /salons/{id}/customers, GET/PUT /salons/{id}/customers/{customerId}
- Validate pagination for customer listing
- Test customer search functionality
- Test GDPR compliance fields (marketing consent)

### T009: [P] Service Catalog Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `services.test.ts`: GET/POST /salons/{id}/services, GET/PUT /salons/{id}/services/{serviceId}
- Validate service category enum and duration constraints
- Test pricing and deposit requirements
- Test service-staff qualification relationships

### T010: [P] Appointment Management Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `appointments.test.ts`: GET/POST /salons/{id}/appointments, GET/PUT/DELETE /salons/{id}/appointments/{appointmentId}
- Validate appointment status transitions
- Test conflict detection for double-booking
- Test date range queries and timezone handling

### T011: [P] Availability & Time Blocks Contract Tests
Create failing contract tests in `backend/tests/contract/`:
- `availability.test.ts`: GET /salons/{id}/availability
- `time-blocks.test.ts`: GET/POST /salons/{id}/staff/{staffId}/time-blocks
- Validate availability calculation logic
- Test recurring time block patterns
- Test overlap conflict detection

### T012: [P] Integration Test Scenarios
Create failing integration tests in `backend/tests/integration/`:
- `onboarding.test.ts`: Scenario 1 - Salon owner registration and setup
- `staff-workflow.test.ts`: Scenario 2 - Complete staff management workflow
- `customer-operations.test.ts`: Scenario 3 - Customer database operations
- `service-catalog.test.ts`: Scenario 4 - Service catalog management
- `calendar-operations.test.ts`: Scenario 5 - Enhanced calendar operations
- `shift-management.test.ts`: Scenario 6 - Staff shift and availability
- `real-time-sync.test.ts`: Scenario 7 - WebSocket synchronization
- `reporting.test.ts`: Scenario 8 - Basic reporting functionality

## Phase 3.3: Database Models & Core Logic (ONLY after tests are failing)

### T013: [P] Prisma Models Implementation
Implement database models in `backend/prisma/schema.prisma`:
- User model with role enum and authentication fields
- Salon model with business hours JSON field and address
- Staff model with permissions array and default hours
- Customer model with preferences and marketing consent
- Service model with category enum and pricing rules
- Appointment model with status enum and audit fields
- StaffShift model with recurrence patterns
- TimeBlock model with overlap prevention

### T014: [P] Database Service Layer
Create service classes in `backend/src/services/`:
- `UserService.ts`: Authentication, role management, profile updates
- `SalonService.ts`: CRUD operations with owner authorization
- `StaffService.ts`: Staff management with qualification tracking
- `CustomerService.ts`: Customer CRUD with search and pagination
- `ServiceCatalogService.ts`: Service management with staff assignments
- `AppointmentService.ts`: Booking logic with conflict detection
- `AvailabilityService.ts`: Time slot calculation with business rules
- `TimeBlockService.ts`: Staff availability management

### T015: [P] Validation Schemas
Create Zod validation schemas in `backend/src/schemas/`:
- `auth.schemas.ts`: Registration, login, profile validation
- `salon.schemas.ts`: Salon creation/update with business hours
- `staff.schemas.ts`: Staff creation with permissions validation
- `customer.schemas.ts`: Customer profile with GDPR compliance
- `service.schemas.ts`: Service catalog with pricing rules
- `appointment.schemas.ts`: Booking validation with time constraints
- `shared.schemas.ts`: Common patterns (UUID, phone, email)

### T016: [P] Authentication Middleware
Implement auth system in `backend/src/middleware/`:
- `auth.middleware.ts`: JWT token validation and user context
- `rbac.middleware.ts`: Role-based access control (Owner/Staff/Customer)
- `salon-context.middleware.ts`: Salon ownership and access validation
- Password hashing utilities with bcrypt
- JWT token generation and refresh logic

## Phase 3.4: API Routes Implementation

### T017: Authentication Routes
Implement auth endpoints in `backend/src/routes/auth.routes.ts`:
- POST /auth/register with role assignment and email verification
- POST /auth/login with JWT token generation
- POST /auth/logout with token invalidation
- GET /auth/me with user profile and salon context

### T018: Salon Management Routes
Implement salon endpoints in `backend/src/routes/salons.routes.ts`:
- GET /salons with owner authorization
- POST /salons with owner role requirement
- GET /salons/{id} with access validation
- PUT /salons/{id} with owner-only updates

### T019: Staff Management Routes
Implement staff endpoints in `backend/src/routes/staff.routes.ts`:
- GET/POST /salons/{id}/staff with role-based filtering
- GET/PUT/DELETE /salons/{id}/staff/{staffId}
- POST /salons/{id}/staff/{staffId}/services (service qualifications)
- Staff permission validation and access control

### T020: Customer Management Routes
Implement customer endpoints in `backend/src/routes/customers.routes.ts`:
- GET /salons/{id}/customers with search and pagination
- POST /salons/{id}/customers with validation
- GET/PUT /salons/{id}/customers/{customerId}
- Customer data privacy and GDPR compliance

### T021: Service Catalog Routes
Implement service endpoints in `backend/src/routes/services.routes.ts`:
- GET/POST /salons/{id}/services with category filtering
- GET/PUT /salons/{id}/services/{serviceId}
- Service-staff qualification management
- Pricing and availability rule enforcement

### T022: Appointment Management Routes
Implement appointment endpoints in `backend/src/routes/appointments.routes.ts`:
- GET /salons/{id}/appointments with date range and staff filtering
- POST /salons/{id}/appointments with conflict detection
- GET/PUT/DELETE /salons/{id}/appointments/{appointmentId}
- Appointment status transitions and validation

### T023: Availability & Time Block Routes
Implement availability endpoints in `backend/src/routes/availability.routes.ts`:
- GET /salons/{id}/availability with service and staff filtering
- GET/POST /salons/{id}/staff/{staffId}/time-blocks
- Availability calculation with business hours and shifts
- Time block conflict detection and resolution

## Phase 3.5: Real-time Features & WebSocket Integration

### T024: WebSocket Server Setup
Implement real-time features in `backend/src/websocket/`:
- Socket.io server configuration with JWT authentication
- Room-based subscriptions by salon ID
- Event types for appointment changes and calendar updates
- Connection management and error handling

### T025: Real-time Event Broadcasting
Implement event broadcasting in `backend/src/services/`:
- Appointment creation/update/deletion events
- Staff availability changes
- Calendar synchronization across sessions
- Conflict resolution for simultaneous edits

### T026: Frontend WebSocket Integration
Implement real-time client in `lib/websocket/`:
- Socket.io client with automatic reconnection
- Event listeners for calendar updates
- Optimistic UI updates with rollback capability
- Connection status indicators

## Phase 3.6: Frontend Integration & Enhanced Calendar

### T027: [P] Zustand Store Implementation
Create salon state management in `lib/stores/`:
- `salon.store.ts`: Current salon context and business settings
- `staff.store.ts`: Staff members, schedules, and permissions
- `customers.store.ts`: Customer database with search state
- `services.store.ts`: Service catalog and pricing
- `appointments.store.ts`: Calendar state and appointment management

### T028: [P] TanStack Query Hooks
Create API hooks in `lib/hooks/`:
- `useAuth.ts`: Authentication state and user management
- `useSalon.ts`: Salon profile and settings queries
- `useStaff.ts`: Staff management with optimistic updates
- `useCustomers.ts`: Customer queries with pagination
- `useServices.ts`: Service catalog management
- `useAppointments.ts`: Appointment CRUD with real-time sync

### T029: Enhanced Calendar Components
Extend existing calendar in `components/salon-calendar/`:
- `SalonCalendarProvider.tsx`: Salon-specific calendar context
- `StaffCalendarView.tsx`: Multi-staff calendar display
- `AppointmentCard.tsx`: Salon appointment display component
- `AvailabilityIndicator.tsx`: Staff availability visualization
- Integration with existing drag-and-drop functionality

### T030: [P] Staff Management UI
Create staff management components in `components/staff/`:
- `StaffList.tsx`: Staff directory with permissions display
- `StaffForm.tsx`: Add/edit staff with role assignment
- `StaffSchedule.tsx`: Working hours and shift management
- `ServiceAssignment.tsx`: Staff-service qualification interface

### T031: [P] Customer Management UI
Create customer components in `components/customers/`:
- `CustomerList.tsx`: Searchable customer directory
- `CustomerForm.tsx`: Customer profile creation/editing
- `CustomerProfile.tsx`: Detailed customer view with history
- `CustomerSearch.tsx`: Advanced search and filtering

### T032: [P] Service Catalog UI
Create service management components in `components/services/`:
- `ServiceList.tsx`: Service catalog with category filtering
- `ServiceForm.tsx`: Service creation with pricing rules
- `ServiceAssignment.tsx`: Staff qualification management
- `ServiceAvailability.tsx`: Availability and booking rules

### T033: Appointment Management UI
Enhance appointment features in `components/appointments/`:
- `AppointmentDialog.tsx`: Enhanced booking dialog with service selection
- `ConflictResolution.tsx`: Handle scheduling conflicts
- `AppointmentHistory.tsx`: Customer appointment history
- Integration with existing calendar event system

### T034: [P] Dashboard & Navigation
Create salon dashboard in `components/dashboard/`:
- `SalonDashboard.tsx`: Main dashboard with key metrics
- `QuickActions.tsx`: Common salon operations shortcuts
- `RecentActivity.tsx`: Recent appointments and changes
- Update existing sidebar with salon navigation sections

## Phase 3.7: Authentication & Authorization Integration

### T035: Frontend Authentication Flow
Implement auth integration in `app/(auth)/`:
- `login/page.tsx`: Login page with role-based redirection
- `register/page.tsx`: Registration with salon setup flow
- `layout.tsx`: Auth layout with proper styling
- Integration with NextAuth.js for session management

### T036: Protected Routes & RBAC
Implement authorization in `middleware.ts`:
- Route protection based on user roles
- Salon context validation
- Redirect logic for unauthorized access
- Session validation and refresh

### T037: User Profile & Settings
Create user management in `app/(dashboard)/profile/`:
- `page.tsx`: User profile management
- `salon-settings/page.tsx`: Salon configuration
- `team/page.tsx`: Staff management interface
- Role-based UI component visibility

## Phase 3.8: Reporting & Analytics

### T038: [P] Basic Reporting Backend
Implement reporting in `backend/src/services/reports/`:
- `RevenueReports.ts`: Daily/weekly/monthly revenue calculations
- `StaffReports.ts`: Utilization and performance metrics
- `ServiceReports.ts`: Popular services and trends
- `CustomerReports.ts`: Retention and visit frequency

### T039: [P] Reporting UI Components
Create reporting interface in `components/reports/`:
- `RevenueChart.tsx`: Revenue visualization with Chart.js
- `StaffUtilization.tsx`: Staff performance metrics
- `ServiceAnalytics.tsx`: Service popularity charts
- `CustomerMetrics.tsx`: Customer retention indicators

### T040: Reports Dashboard Page
Create reports page in `app/(dashboard)/reports/`:
- `page.tsx`: Main reports dashboard
- `revenue/page.tsx`: Detailed revenue analysis
- `staff/page.tsx`: Staff performance reports
- `services/page.tsx`: Service analytics

## Phase 3.9: Testing & Quality Assurance

### T041: [P] Backend Unit Tests
Create comprehensive unit tests in `backend/tests/unit/`:
- `services/*.test.ts`: Test all service layer logic
- `middleware/*.test.ts`: Test authentication and validation
- `utils/*.test.ts`: Test utility functions
- Database mocking and isolated testing

### T042: [P] Frontend Component Tests
Create component tests in `__tests__/`:
- `components/salon-calendar/*.test.tsx`: Calendar component testing
- `components/staff/*.test.tsx`: Staff management component tests
- `components/customers/*.test.tsx`: Customer management tests
- Mock API responses and state management

### T043: [P] End-to-End Test Implementation
Implement E2E tests with Playwright in `tests/e2e/`:
- `salon-onboarding.spec.ts`: Complete salon setup workflow
- `appointment-booking.spec.ts`: Appointment creation and management
- `staff-management.spec.ts`: Staff operations and permissions
- `customer-workflow.spec.ts`: Customer management workflow
- Real browser testing with database seeding

### T044: Performance & Load Testing
Implement performance testing:
- Calendar load testing with 1000+ appointments
- Concurrent user testing (50+ users)
- Database query optimization validation
- Real-time update performance measurement
- Memory usage and resource monitoring

## Phase 3.10: Polish & Production Readiness

### T045: [P] Error Handling & Logging
Implement comprehensive error handling:
- `backend/src/middleware/error.middleware.ts`: Global error handler
- `lib/utils/error-handling.ts`: Frontend error boundaries
- Winston logging setup with structured logs
- Error tracking and monitoring setup

### T046: [P] Documentation Updates
Update project documentation:
- `README.md`: Complete setup and development guide
- `docs/api.md`: API documentation from OpenAPI spec
- `docs/deployment.md`: Production deployment guide
- `docs/development.md`: Development workflow and standards

### T047: Production Configuration
Prepare for production deployment:
- Docker configuration for backend and database
- Environment variable management
- Security headers and HTTPS configuration
- Database migration scripts for production
- Health checks and monitoring endpoints

### T048: Final Integration & Deployment Test
Complete final validation:
- Run all quickstart scenarios from quickstart.md
- Validate all integration test scenarios pass
- Performance benchmarking against requirements
- Security audit and vulnerability scanning
- Production deployment dry run

## Dependencies

### Critical Dependencies (Blocking)
- **Setup before all**: T001, T002, T003, T004
- **Tests before implementation**: T005-T012 must complete before T013-T023
- **Models before services**: T013 blocks T014, T015
- **Services before routes**: T014, T015 block T017-T023
- **Backend before frontend integration**: T017-T026 block T027-T040
- **Core features before polish**: T001-T040 block T041-T048

### Parallel Execution Opportunities
- **Phase 3.2**: All contract tests (T005-T012) can run in parallel
- **Phase 3.3**: Model creation tasks (T013-T016) can run in parallel
- **Phase 3.6**: Frontend component development (T027-T034) can run in parallel
- **Phase 3.9**: Testing tasks (T041-T043) can run in parallel
- **Phase 3.10**: Polish tasks (T045-T046) can run in parallel

## Parallel Example
```bash
# Launch contract tests together (Phase 3.2):
Task: "Authentication contract tests in backend/tests/contract/auth.test.ts"
Task: "Salon management contract tests in backend/tests/contract/salons.test.ts"
Task: "Staff management contract tests in backend/tests/contract/staff.test.ts"
Task: "Customer management contract tests in backend/tests/contract/customers.test.ts"

# Launch frontend components together (Phase 3.6):
Task: "Staff management UI in components/staff/StaffList.tsx"
Task: "Customer management UI in components/customers/CustomerList.tsx"
Task: "Service catalog UI in components/services/ServiceList.tsx"
Task: "Dashboard components in components/dashboard/SalonDashboard.tsx"
```

## Validation Checklist
*GATE: Checked before task execution*

- [x] All 14 API endpoints have corresponding contract tests (T005-T011)
- [x] All 8 entities have model creation tasks (T013)
- [x] All contract tests come before implementation (T005-T012 → T013-T023)
- [x] Parallel tasks are truly independent (different files/modules)
- [x] Each task specifies exact file path for implementation
- [x] No task modifies same file as another [P] task
- [x] Test scenarios from quickstart.md covered (T012, T043)
- [x] Real-time requirements addressed (T024-T026)
- [x] Authentication and RBAC properly implemented (T016, T035-T037)
- [x] Performance requirements testable (T044)

## Notes
- **[P] tasks** = different files, no dependencies - can run simultaneously
- **TDD approach**: Verify contract tests fail before implementing endpoints
- **Progressive enhancement**: Maintain existing calendar functionality while adding salon features
- **Constitution compliance**: TypeScript-first, modular components, accessibility maintained
- **Commit after each task**: Maintain clean git history for rollback capability

---
**Total Tasks**: 48 tasks across 10 phases  
**Estimated Timeline**: 6-8 weeks for MVP completion  
**Parallel Opportunities**: 28 tasks marked [P] for concurrent execution  
**Ready for Implementation**: ✅ All prerequisites met, tasks validated