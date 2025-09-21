# Salon SAAS MVP - Quickstart Guide

**Feature**: 001-salon-saas-mvp  
**Date**: 2025-01-21  
**Version**: 1.0.0

## Overview

This quickstart guide validates the complete Salon SAAS MVP functionality through integration test scenarios. Each user story is tested end-to-end to ensure business requirements are met.

## Environment Setup

### Prerequisites
- Node.js 18+ and npm
- PostgreSQL 14+ running locally
- Docker (for development environment)

### Quick Start
```bash
# Clone and setup
git clone <repository>
cd salon-saas-mvp
npm install

# Setup database
docker-compose up -d postgres
npm run db:migrate
npm run db:seed

# Start development servers
npm run dev:backend  # Port 3001
npm run dev:frontend # Port 3000

# Run tests
npm run test:contracts
npm run test:integration
npm run test:e2e
```

## Integration Test Scenarios

### Scenario 1: Salon Owner Onboarding
**Story**: US006 - Basic Registration & Settings

**Test Steps**:
1. Register as salon owner
2. Create salon profile with business information
3. Configure business hours and settings
4. Verify salon dashboard loads correctly

**API Endpoints**:
- `POST /api/auth/register` (role: OWNER)
- `POST /api/salons` (salon creation)
- `PUT /api/salons/{id}` (settings update)
- `GET /api/salons/{id}` (profile retrieval)

**Expected Results**:
- Owner account created with proper role
- Salon profile stored with all required fields
- Business hours properly configured
- Dashboard shows empty state with onboarding prompts

**Validation Command**:
```bash
npm run test:scenario:onboarding
```

### Scenario 2: Staff Management Workflow
**Story**: US001 - Staff Management

**Test Steps**:
1. Login as salon owner
2. Add multiple staff members with different roles
3. Configure staff working hours and permissions
4. Assign services to qualified staff members
5. Set staff-specific pricing overrides

**API Endpoints**:
- `POST /api/salons/{id}/staff` (add staff)
- `PUT /api/salons/{id}/staff/{staffId}` (update staff)
- `POST /api/salons/{id}/staff/{staffId}/services` (assign services)
- `GET /api/salons/{id}/staff` (list staff)

**Expected Results**:
- Staff members created with unique emails
- Working hours properly configured
- Service assignments saved correctly
- Staff appears in calendar views
- Permission system restricts access appropriately

**Validation Command**:
```bash
npm run test:scenario:staff-management
```

### Scenario 3: Customer Database Operations
**Story**: US002 - Customer Database

**Test Steps**:
1. Add new customer with complete profile
2. Search customers by name and phone
3. Update customer preferences and notes
4. View customer appointment history
5. Handle customer consent and communication preferences

**API Endpoints**:
- `POST /api/salons/{id}/customers` (add customer)
- `GET /api/salons/{id}/customers?search=...` (search)
- `PUT /api/salons/{id}/customers/{customerId}` (update)
- `GET /api/salons/{id}/customers/{customerId}/appointments` (history)

**Expected Results**:
- Customer profiles saved with validation
- Search functionality works across name/phone fields
- Appointment history displays chronologically
- GDPR compliance for data handling
- Marketing consent properly tracked

**Validation Command**:
```bash
npm run test:scenario:customer-management
```

### Scenario 4: Service Catalog Management
**Story**: US003 - Service Catalog Management

**Test Steps**:
1. Create service categories (Haircut, Color, Nails, etc.)
2. Add services with pricing and duration
3. Configure service-specific booking rules
4. Assign qualified staff to services
5. Test service availability calculations

**API Endpoints**:
- `POST /api/salons/{id}/services` (create service)
- `PUT /api/salons/{id}/services/{serviceId}` (update service)
- `GET /api/salons/{id}/services` (list services)
- `POST /api/salons/{id}/staff/{staffId}/services` (staff qualification)

**Expected Results**:
- Services categorized correctly
- Pricing and duration validation works
- Staff qualifications properly enforced
- Service availability reflects staff assignments
- Booking rules properly applied

**Validation Command**:
```bash
npm run test:scenario:service-catalog
```

### Scenario 5: Enhanced Calendar Operations
**Story**: US004 - Advanced Calendar Scheduling

**Test Steps**:
1. Load calendar with multiple staff and appointments
2. Create new appointment with conflict detection
3. Drag-and-drop appointment rescheduling
4. Test different calendar views (Day, Week, Month)
5. Filter appointments by staff and service type

**API Endpoints**:
- `GET /api/salons/{id}/appointments?start_date=...&end_date=...` (calendar data)
- `POST /api/salons/{id}/appointments` (create appointment)
- `PUT /api/salons/{id}/appointments/{appointmentId}` (reschedule)
- `GET /api/salons/{id}/availability` (time slot availability)

**Expected Results**:
- Calendar loads quickly with 100+ appointments
- Conflict detection prevents double-booking
- Real-time updates across multiple sessions
- View switching maintains state and performance
- Drag-and-drop provides immediate feedback

**Validation Command**:
```bash
npm run test:scenario:calendar-operations
```

### Scenario 6: Shift Management System
**Story**: US005 - Shift Management

**Test Steps**:
1. Create staff shift schedules (recurring and one-time)
2. Block time for breaks and personal time
3. Handle shift conflicts and overlaps
4. Test availability calculation with shifts
5. Manage vacation and time-off requests

**API Endpoints**:
- `POST /api/salons/{id}/staff/{staffId}/shifts` (create shift)
- `POST /api/salons/{id}/staff/{staffId}/time-blocks` (block time)
- `GET /api/salons/{id}/staff/{staffId}/availability` (check availability)
- `PUT /api/salons/{id}/staff/{staffId}/shifts/{shiftId}` (update shift)

**Expected Results**:
- Shifts created without conflicts
- Time blocks properly restrict availability
- Recurring patterns work correctly
- Calendar shows accurate staff availability
- Appointment booking respects shift constraints

**Validation Command**:
```bash
npm run test:scenario:shift-management
```

### Scenario 7: Real-time Synchronization
**Story**: Cross-cutting requirement for real-time updates

**Test Steps**:
1. Open calendar in multiple browser sessions
2. Create appointment in session A
3. Verify immediate update in session B
4. Test appointment modification synchronization
5. Handle network interruption scenarios

**Technical Requirements**:
- WebSocket connection establishment
- Event broadcasting to relevant salon sessions
- Optimistic UI updates with rollback
- Offline state handling
- Conflict resolution for simultaneous edits

**Expected Results**:
- Updates appear within 200ms across sessions
- No data loss during network interruptions
- Conflicts resolved gracefully
- UI provides clear feedback on connection status
- Performance maintained with 10+ concurrent sessions

**Validation Command**:
```bash
npm run test:scenario:real-time-sync
```

### Scenario 8: Basic Reporting Dashboard
**Story**: US007 - Essential Reports

**Test Steps**:
1. Generate daily revenue report
2. View staff utilization metrics
3. Analyze popular services data
4. Check customer retention statistics
5. Export reports in multiple formats

**API Endpoints**:
- `GET /api/salons/{id}/reports/revenue?period=...` (revenue data)
- `GET /api/salons/{id}/reports/staff-utilization` (staff metrics)
- `GET /api/salons/{id}/reports/services` (service analytics)
- `GET /api/salons/{id}/reports/customers` (customer metrics)

**Expected Results**:
- Reports generate quickly with accurate data
- Charts and visualizations display correctly
- Date range filtering works properly
- Export functionality produces valid files
- Data privacy restrictions properly enforced

**Validation Command**:
```bash
npm run test:scenario:reporting
```

## Performance Validation

### Load Testing Scenarios

#### Calendar Performance Test
```bash
# Test calendar with 1000+ appointments
npm run test:load:calendar

# Expected: Page load < 1 second
# Expected: Scroll performance > 60fps
# Expected: View switching < 200ms
```

#### Concurrent User Test
```bash
# Test 50+ simultaneous users
npm run test:load:concurrent

# Expected: No degradation in response times
# Expected: Real-time updates maintain < 200ms latency
# Expected: Database connections managed efficiently
```

#### Data Volume Test
```bash
# Test with production-scale data
npm run test:load:data-volume

# Expected: 10,000+ customers searchable quickly
# Expected: 50+ staff members manageable
# Expected: Reports generate within 5 seconds
```

## Security Validation

### Authentication & Authorization Tests
```bash
# Test role-based access control
npm run test:security:rbac

# Expected: Staff cannot access owner functions
# Expected: Customers only see their own data
# Expected: Cross-salon data isolation maintained
```

### Data Protection Tests
```bash
# Test data validation and sanitization
npm run test:security:data-protection

# Expected: SQL injection prevented
# Expected: XSS attacks blocked
# Expected: Input validation errors handled gracefully
```

## Accessibility Validation

### Keyboard Navigation Test
```bash
npm run test:accessibility:keyboard

# Expected: All calendar functions accessible via keyboard
# Expected: Proper focus management and tab order
# Expected: Screen reader compatibility maintained
```

### Responsive Design Test
```bash
npm run test:accessibility:responsive

# Expected: Mobile layout fully functional
# Expected: Touch interactions work correctly
# Expected: Text remains readable at all screen sizes
```

## Browser Compatibility

### Cross-Browser Testing
```bash
npm run test:browsers:chrome
npm run test:browsers:firefox
npm run test:browsers:safari
npm run test:browsers:edge

# Expected: Consistent functionality across browsers
# Expected: No JavaScript errors in console
# Expected: Calendar drag-and-drop works everywhere
```

## Deployment Validation

### Production Build Test
```bash
npm run build:production
npm run test:production

# Expected: All features work in production build
# Expected: No dev-only dependencies in production
# Expected: Environment variables properly configured
```

### Database Migration Test
```bash
npm run test:migration

# Expected: Schema migrations run without errors
# Expected: Data integrity maintained during migrations
# Expected: Rollback procedures work correctly
```

## Troubleshooting Guide

### Common Issues

#### Calendar Not Loading
```bash
# Check database connection
npm run db:status

# Verify API endpoints
curl http://localhost:3001/api/health

# Check browser console for errors
```

#### Real-time Updates Not Working
```bash
# Check WebSocket connection
npm run test:websocket

# Verify port configuration
netstat -an | grep 3001

# Check firewall settings
```

#### Performance Issues
```bash
# Run performance profiler
npm run profile:performance

# Check database indexes
npm run db:analyze

# Monitor memory usage
npm run monitor:memory
```

### Support Resources
- Development documentation: `/docs/development.md`
- API reference: `/docs/api-reference.md`
- Deployment guide: `/docs/deployment.md`
- Troubleshooting: `/docs/troubleshooting.md`

---
**Status**: Ready for Implementation ✅  
**Test Coverage**: 8 core scenarios + performance/security validation  
**Validation Framework**: Complete with automated test commands  
**Next Steps**: Execute `/tasks` command to generate implementation tasks