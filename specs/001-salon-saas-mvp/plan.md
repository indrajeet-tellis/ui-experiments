
# Implementation Plan: Salon SAAS Appointment Management MVP

**Branch**: `001-salon-saas-mvp` | **Date**: 2025-01-21 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-salon-saas-mvp/spec.md`

## Execution Flow (/plan command scope)
```
1. Load feature spec from Input path
   → If not found: ERROR "No feature spec at {path}"
2. Fill Technical Context (scan for NEEDS CLARIFICATION)
   → Detect Project Type from context (web=frontend+backend, mobile=app+api)
   → Set Structure Decision based on project type
3. Fill the Constitution Check section based on the content of the constitution document.
4. Evaluate Constitution Check section below
   → If violations exist: Document in Complexity Tracking
   → If no justification possible: ERROR "Simplify approach first"
   → Update Progress Tracking: Initial Constitution Check
5. Execute Phase 0 → research.md
   → If NEEDS CLARIFICATION remain: ERROR "Resolve unknowns"
6. Execute Phase 1 → contracts, data-model.md, quickstart.md, agent-specific template file (e.g., `CLAUDE.md` for Claude Code, `.github/copilot-instructions.md` for GitHub Copilot, `GEMINI.md` for Gemini CLI, `QWEN.md` for Qwen Code or `AGENTS.md` for opencode).
7. Re-evaluate Constitution Check section
   → If new violations: Refactor design, return to Phase 1
   → Update Progress Tracking: Post-Design Constitution Check
8. Plan Phase 2 → Describe task generation approach (DO NOT create tasks.md)
9. STOP - Ready for /tasks command
```

**IMPORTANT**: The /plan command STOPS at step 7. Phases 2-4 are executed by other commands:
- Phase 2: /tasks command creates tasks.md
- Phase 3-4: Implementation execution (manual or via tools)

## Summary
Transform existing UI calendar experiment into comprehensive Salon SAAS appointment management system. MVP includes staff management, customer database, service catalog, enhanced calendar with real-time synchronization, shift scheduling, and basic reporting. Built on existing Next.js 15 + shadcn/ui foundation with new Node.js backend, PostgreSQL database, Zustand state management, and TanStack Query for server state.

## Technical Context
**Language/Version**: TypeScript/JavaScript (Node.js 18+, Next.js 15, React 19)  
**Primary Dependencies**: Next.js 15, shadcn/ui, Tailwind CSS v4, Zustand, TanStack Query, Prisma, Express.js  
**Storage**: PostgreSQL with Prisma ORM for relational data integrity  
**Testing**: Jest + React Testing Library (frontend), Supertest (API), Playwright (E2E)  
**Target Platform**: Web application (desktop + mobile responsive), Linux/Docker deployment
**Project Type**: web (frontend + backend)  
**Performance Goals**: Calendar loads <1s with 1000+ appointments, real-time updates <200ms, 50+ concurrent users  
**Constraints**: Must preserve existing calendar architecture, maintain responsive design, GDPR compliance  
**Scale/Scope**: MVP for single salon (50-100 appointments/day), 5-15 staff members, 500+ customers

**User-Provided Context**: Using existing UI as starting base for Salon SAAS appointment management. Need backend APIs, state management with Zustand, TanStack Query and other libraries for full stack application. MVP requires: add staff, customers, services, shifts, minimum settings for registrations, reports and other important salon management features.

## Constitution Check
*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

**✅ Component Modularity**: Salon features will extend existing event-calendar modular architecture  
**✅ Context-Driven State Management**: Will add SalonProvider alongside existing CalendarProvider/ThemeProvider  
**✅ TypeScript-First Development**: All new code fully typed with proper interfaces  
**✅ Accessibility-First UI**: Maintain existing keyboard shortcuts and ARIA support  
**✅ Performance & User Experience**: Real-time updates with optimistic UI and proper loading states  

**Technology Standards Compliance**:
- ✅ Next.js 15 with App Router (existing)
- ✅ shadcn/ui components (extending existing registry)
- ✅ Tailwind CSS v4 with oklch colors (existing)
- ✅ @dnd-kit for appointment management (existing)
- ✅ date-fns for date operations (existing)
- ✅ Sonner for notifications (existing)
- ✅ Lucide React for icons (existing)

**New Technology Justification**:
- **Zustand**: Required for complex salon state (staff, customers, services) beyond calendar scope
- **TanStack Query**: Essential for server state management and real-time synchronization
- **Prisma + PostgreSQL**: Necessary for relational data integrity (appointments, staff, customers)
- **Express.js Backend**: Required for business logic and data persistence

**Potential Concerns**: None - all additions align with modular architecture and extend existing patterns.

## Project Structure

### Documentation (this feature)
```
specs/[###-feature]/
├── plan.md              # This file (/plan command output)
├── research.md          # Phase 0 output (/plan command)
├── data-model.md        # Phase 1 output (/plan command)
├── quickstart.md        # Phase 1 output (/plan command)
├── contracts/           # Phase 1 output (/plan command)
└── tasks.md             # Phase 2 output (/tasks command - NOT created by /plan)
```

### Source Code (repository root)
```
# Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# Option 2: Web application (when "frontend" + "backend" detected)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# Option 3: Mobile + API (when "iOS/Android" detected)
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure]
```

**Structure Decision**: Option 2 (Web application) - Frontend and Backend separation required for salon business logic

## Phase 0: Outline & Research
1. **Extract unknowns from Technical Context** above:
   - For each NEEDS CLARIFICATION → research task
   - For each dependency → best practices task
   - For each integration → patterns task

2. **Generate and dispatch research agents**:
   ```
   For each unknown in Technical Context:
     Task: "Research {unknown} for {feature context}"
   For each technology choice:
     Task: "Find best practices for {tech} in {domain}"
   ```

3. **Consolidate findings** in `research.md` using format:
   - Decision: [what was chosen]
   - Rationale: [why chosen]
   - Alternatives considered: [what else evaluated]

**Output**: research.md with all NEEDS CLARIFICATION resolved

## Phase 1: Design & Contracts
*Prerequisites: research.md complete*

1. **Extract entities from feature spec** → `data-model.md`:
   - Entity name, fields, relationships
   - Validation rules from requirements
   - State transitions if applicable

2. **Generate API contracts** from functional requirements:
   - For each user action → endpoint
   - Use standard REST/GraphQL patterns
   - Output OpenAPI/GraphQL schema to `/contracts/`

3. **Generate contract tests** from contracts:
   - One test file per endpoint
   - Assert request/response schemas
   - Tests must fail (no implementation yet)

4. **Extract test scenarios** from user stories:
   - Each story → integration test scenario
   - Quickstart test = story validation steps

5. **Update agent file incrementally** (O(1) operation):
   - Run `.specify/scripts/bash/update-agent-context.sh claude` for your AI assistant
   - If exists: Add only NEW tech from current plan
   - Preserve manual additions between markers
   - Update recent changes (keep last 3)
   - Keep under 150 lines for token efficiency
   - Output to repository root

**Output**: data-model.md, /contracts/*, failing tests, quickstart.md, agent-specific file

## Phase 2: Task Planning Approach
*This section describes what the /tasks command will do - DO NOT execute during /plan*

**Task Generation Strategy**:
- Load `.specify/templates/tasks-template.md` as base structure
- Generate from Phase 1 artifacts: data-model.md, contracts/, quickstart.md
- Database setup tasks: Schema creation, migrations, seeding
- Backend API tasks: Express server, auth, endpoints, real-time features
- Frontend enhancement tasks: State management, enhanced calendar, salon UI
- Integration tasks: Connect frontend to backend, test scenarios

**Ordering Strategy**:
- **Phase A**: Foundation (Database schema, auth, basic API structure)
- **Phase B**: Core APIs (Staff, Customer, Service, Appointment CRUD)
- **Phase C**: Enhanced Features (Real-time sync, availability, calendar integration)
- **Phase D**: UI Integration (Zustand stores, TanStack Query, salon components)
- **Phase E**: Testing & Validation (Contract tests, integration tests, E2E scenarios)

**Parallel Execution Opportunities [P]**:
- Independent entity CRUD operations
- Frontend component development alongside API development
- Test file creation parallel to implementation
- Documentation updates

**Estimated Task Breakdown**:
- **Database & Setup**: 5-7 tasks
- **Backend API Development**: 15-18 tasks  
- **Frontend Integration**: 10-12 tasks
- **Real-time Features**: 3-4 tasks
- **Testing & Validation**: 8-10 tasks
- **Total**: 40-50 numbered, sequenced tasks

**Critical Dependencies**:
1. Database schema → API models → Frontend interfaces
2. Authentication → Protected endpoints → Role-based UI
3. Basic CRUD → Business logic → Real-time features
4. Component interfaces → Calendar integration → User workflows

**IMPORTANT**: This phase is executed by the /tasks command, NOT by /plan

## Phase 3+: Future Implementation
*These phases are beyond the scope of the /plan command*

**Phase 3**: Task execution (/tasks command creates tasks.md)  
**Phase 4**: Implementation (execute tasks.md following constitutional principles)  
**Phase 5**: Validation (run tests, execute quickstart.md, performance validation)

## Complexity Tracking
*Fill ONLY if Constitution Check has violations that must be justified*

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |


## Progress Tracking
*This checklist is updated during execution flow*

**Phase Status**:
- [x] Phase 0: Research complete (/plan command) ✅ 
- [x] Phase 1: Design complete (/plan command) ✅
- [x] Phase 2: Task planning complete (/plan command - describe approach only) ✅
- [ ] Phase 3: Tasks generated (/tasks command) - READY
- [ ] Phase 4: Implementation complete
- [ ] Phase 5: Validation passed

**Gate Status**:
- [x] Initial Constitution Check: PASS ✅
- [x] Post-Design Constitution Check: PASS ✅ 
- [x] All NEEDS CLARIFICATION resolved ✅
- [x] Complexity deviations documented: N/A (no violations) ✅

**Artifacts Generated**:
- [x] Feature specification: `spec.md` ✅
- [x] Research findings: `research.md` ✅ 
- [x] Data model: `data-model.md` ✅
- [x] API contracts: `contracts/api-schema.yaml` ✅
- [x] Integration tests: `quickstart.md` ✅
- [x] Agent context: `CLAUDE.md` updated ✅
- [ ] Task breakdown: `tasks.md` - Awaiting /tasks command

**Ready for Next Phase**: /tasks command can now generate implementation tasks

---
*Based on Constitution v2.1.1 - See `/memory/constitution.md`*
