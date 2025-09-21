# Research Phase: Salon SAAS MVP

**Date**: 2025-01-21  
**Feature**: 001-salon-saas-mvp

## Technology Research & Decisions

### State Management Architecture

#### Decision: Zustand for Client State Management
**Rationale**: 
- Lightweight (2KB) vs Redux (>10KB)
- TypeScript-first design aligns with constitution
- Simple API reduces learning curve for salon business logic
- Excellent devtools and middleware support
- Works seamlessly with existing React Context patterns

**Alternatives Considered**:
- Redux Toolkit: Too heavy for salon MVP scope
- Valtio: Less mature ecosystem
- Jotai: Atomic approach unnecessary for salon domain

#### Decision: TanStack Query v5 for Server State
**Rationale**:
- Industry standard for server state management (40M+ downloads/month)
- Built-in caching, background updates, optimistic updates
- Perfect for real-time appointment synchronization
- Excellent TypeScript support and documentation
- Handles offline scenarios crucial for salon operations

**Alternatives Considered**:
- SWR: Less feature-complete for complex mutations
- Apollo Client: Overkill without GraphQL
- React Query v4: Outdated, v5 has better TypeScript

### Backend Architecture

#### Decision: Express.js + TypeScript
**Rationale**:
- Team familiarity with JavaScript ecosystem
- Extensive middleware ecosystem for auth, validation, CORS
- Easy integration with Prisma and PostgreSQL
- Fast development cycle for MVP requirements
- Strong typing with TypeScript maintains constitution compliance

**Alternatives Considered**:
- Fastify: Higher performance but less mature ecosystem
- Nest.js: Too heavy for MVP scope, adds complexity
- Next.js API routes: Insufficient for complex business logic

#### Decision: Prisma ORM with PostgreSQL
**Rationale**:
- Type-safe database client with excellent TypeScript integration
- Automatic migration generation and management
- Built-in connection pooling and query optimization
- Studio GUI for database management
- Strong relational data modeling for salon entities

**Alternatives Considered**:
- Drizzle ORM: Less mature, smaller ecosystem
- Raw PostgreSQL: Too much boilerplate for MVP timeline
- MongoDB: Poor fit for relational salon data (staff ↔ services)

### Real-time Communication

#### Decision: Socket.io for Real-time Updates
**Rationale**:
- Proven solution for multi-user calendar applications
- Automatic fallback to polling if WebSockets unavailable
- Room-based subscriptions perfect for salon organization
- Built-in conflict resolution and error handling
- Excellent TypeScript support with typed events

**Alternatives Considered**:
- Native WebSockets: Too much boilerplate for reliability
- Server-Sent Events: One-way only, insufficient for appointments
- Supabase Realtime: Vendor lock-in, adds external dependency

### Authentication & Authorization

#### Decision: NextAuth.js v5 (Auth.js)
**Rationale**:
- Seamless Next.js integration with existing architecture
- Multiple provider support (email, Google, etc.)
- Built-in JWT and session management
- Role-based access control for salon hierarchy
- GDPR compliant by design

**Alternatives Considered**:
- Clerk: Commercial service, potential vendor lock-in
- Auth0: Overkill for MVP, expensive for small salons
- Custom JWT: Security risks, maintenance overhead

### Database Schema Patterns

#### Decision: Multi-tenant Single Database
**Rationale**:
- Simpler architecture for MVP scope
- Cost-effective for small salon customer base
- Easy data backup and maintenance
- Fast queries with proper indexing
- Future-proof for multi-salon expansion

**Alternatives Considered**:
- Database per tenant: Over-engineering for MVP
- Shared everything: Security and data isolation issues
- Multi-database: Complex deployment and maintenance

### API Design Patterns

#### Decision: RESTful API with Resource-Based URLs
**Rationale**:
- Industry standard, easy to understand and consume
- Aligns with CRUD operations for salon entities
- Simple caching strategies with HTTP headers
- Clear separation of concerns for each resource
- OpenAPI documentation generation support

**Alternatives Considered**:
- GraphQL: Over-engineering for MVP, adds complexity
- RPC-style: Less standardized, harder to cache
- Event-driven: Too complex for synchronous salon operations

### Frontend Architecture Extensions

#### Decision: Feature-Based Directory Structure
**Rationale**:
- Extends existing event-calendar modular pattern
- Clear separation between calendar and salon features
- Easy to maintain and scale individual features
- Aligns with constitution's component modularity principle

**Structure**:
```
components/
├── event-calendar/        # Existing calendar system
├── salon-management/      # New salon features
│   ├── staff/
│   ├── customers/
│   ├── services/
│   └── reports/
└── ui/                   # Shared shadcn components
```

#### Decision: Progressive Enhancement of Calendar
**Rationale**:
- Maintains existing calendar functionality
- Gradual addition of salon-specific features
- Backwards compatibility with current event system
- Reduced risk of breaking existing functionality

**Implementation Strategy**:
1. Extend CalendarEvent interface for salon data
2. Add salon-specific context providers
3. Create salon-aware calendar views
4. Maintain existing view switching logic

### Performance Optimization

#### Decision: Optimistic Updates for Appointments
**Rationale**:
- Immediate user feedback for appointment operations
- Reduces perceived latency in busy salon environment
- Built-in rollback for failed operations
- TanStack Query handles complexity automatically

#### Decision: Intelligent Calendar Data Loading
**Rationale**:
- Load only visible date range data
- Background prefetch for adjacent weeks
- Aggressive caching with smart invalidation
- Lazy loading for detailed appointment data

### Development & Testing Strategy

#### Decision: Test-Driven Development Approach
**Rationale**:
- Ensures reliability for business-critical operations
- Documents expected behavior for salon workflows
- Supports confident refactoring during rapid development
- Aligns with constitution's performance requirements

**Testing Stack**:
- **Unit**: Jest + React Testing Library
- **API**: Supertest for endpoint testing
- **E2E**: Playwright for critical salon workflows
- **Integration**: Test database with Docker

## Integration Patterns

### Calendar System Integration
**Pattern**: Event Interface Extension
- Extend existing CalendarEvent with salon-specific fields
- Maintain compatibility with existing calendar views
- Add salon context to calendar providers
- Progressive enhancement of existing components

### State Management Integration
**Pattern**: Provider Composition
- SalonProvider wraps existing providers
- Shared state for cross-cutting concerns (theme, user)
- Feature-specific state in component trees
- Clear data flow between calendar and salon features

### Component Library Integration
**Pattern**: shadcn/ui Extension
- Maintain existing component registry structure
- Add salon-specific component variants
- Extend existing forms for salon data entry
- Consistent styling with existing color scheme

## Security Considerations

### Data Protection
- Row-level security for multi-tenant data
- Input validation with Zod schemas
- SQL injection prevention with Prisma
- XSS protection with proper sanitization

### Authentication Security
- Secure session management with httpOnly cookies
- CSRF protection for state-changing operations
- Rate limiting for authentication attempts
- Proper logout and session cleanup

### Business Logic Security
- Role-based access control (Owner, Staff, Customer)
- Appointment ownership validation
- Data scope restriction by salon context
- Audit logging for sensitive operations

## Deployment & Infrastructure

### Development Environment
- Docker Compose for local development
- Hot reload for both frontend and backend
- Database seeding with sample salon data
- Environment variable management

### Production Considerations
- Horizontal scaling capability with stateless backend
- Database connection pooling and optimization
- CDN for static assets
- Health checks and monitoring

---
**Status**: Complete ✅  
**All Technology Choices Finalized**: Yes  
**Ready for Phase 1**: Yes