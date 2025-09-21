Calendar View Task
User Story: Calendar View for Staff Scheduling
 
As a staff member or business owner,I want to view and manage my daily schedule in a calendar format,
So that I can easily see available time slots, booked appointments, and organize my workday.
 

 
The screen is divided into a header with primary controls and a main content area for the calendar grid.
 
  1. Header Controls (From Left to Right):
 
   * Location Dropdown:
       * Appearance: A dropdown menu labeled "Location." For a new owner, it will be
         pre-selected with the single location they just created.
       * Functionality: If they add more locations later, they can switch between them here
         to view location-specific calendars. Initially, it's set and doesn't require
         interaction.
 
   * Staff Dropdown:
       * Appearance: A dropdown menu labeled "Staff" with a multi-select option. It shows a
         list of all staff members with checkboxes next to their names. An "All Staff"
         option is selected by default.
       * Functionality: The owner can filter the calendar to see the schedule for one or
         more specific staff members. Initially, it will only contain the owner's name and
         any staff they added during setup. Checking/unchecking names instantly updates the
         calendar view below.
 
   * Date Navigation Controls:
       * Appearance: A group of controls in the center of the header.
           * A "Today" button.
           * Left (<) and right (>) arrow icons for moving to the previous/next day or week.
           * A Date Counter displaying the currently viewed date range (e.g., "September 20,
             2025" for Day View, or "Sep 15-21, 2025" for Week View).
       * Functionality:
           * Clicking "Today" immediately snaps the calendar view back to the current date.
           * The arrow icons navigate the calendar backward or forward.
           * For Day View → arrows (</>) move by 1 day (for selected staff & location).
              3-Day View → arrows move by 3 days(for selected staff & location).
              Week View → arrows move by 7 days(for selected staff & location). 
              List View → arrows scroll through list entries (for selected staff & location).
           * Clicking the Date Counter opens a mini-calendar (a date picker), allowing the
             owner to jump directly to any specific month or day.
 
   * View Selector:
       * Appearance: A group of buttons on the right side of the header. The currently
         active view is highlighted.
           * Day: Shows a single day's schedule.
           * 3-Day: Shows a rolling 3-day view.
           * Week: Shows the full week.
           * List: Shows upcoming appointments in a chronological list format.
 
  2. Main Content Area (The Calendar Grid):
 
   * Appearance (First-Time View):
       * The grid is clean and mostly empty, as there are no appointments yet.
       * The vertical axis shows time slots (e.g., 9:00 AM, 9:30 AM) based on the
         Appointment Slot and Visible Hours settings configured during setup.
       * The calendar shows time slots in one hour increments from morning to evening separataed by 10 minutes intervals. Therefore 1 hour block is divided into 6 sub blocks of 10 mins each.
       * The horizontal axis shows the day(s) and the assigned staff members.
       * The business's non-working hours (e.g., before 9 AM, after 7 PM) are grayed out,
         indicating they are unavailable for booking.
       * Booked appointments are highlighted only for their exact time interval.
       * Appointment Management -I can click on the “+” button to create a new appointment in the selected time slot.
   * Functionality:
       * Creating an Appointment: The owner can click directly on any available (white) time
         slot. This action opens a pop-up with two choices: "New Appointment" or "Block
         Time."
       * Hovering: Hovering over a time slot shows the exact time and staff member.
Navigation Panel
I can quickly access Quick Sale, Appointment, Clients, Services, Staff, Product, Settings, and Logout from the left-side menu.
 
 
Acceptance Criteria-
-Default View
When the owner logs in for the first time, the Day View is displayed by default.
The grid shows columns for each staff member with vertical time slots.
As there are no appointments existing, the grid appears empty.
 
-Staff Filtering
When the owner opens the Staff Dropdown, they can uncheck "All Staff" and select a specific staff.
The calendar refreshes to show only the selected staff’s schedule.
Staff dropdown shows all active staff with visual indicators
 
-Location Filtering
Location dropdown appears only for multi-location businesses
New staff/locations appear immediately without page refresh
 
 
-View Navigation
Clicking Day View shows a detailed, single-column schedule for the selected staff.
Clicking 3-Day View shows today, tomorrow, and the next day side by side.
Clicking List View shows a simple message: "You have no upcoming appointments" if no bookings exist.
- Important -- only Day View will have the option to show all the selected staff appointments. But on 3 day view and week view, and list view only the selected staff appointments will be shown(one staf at a time).
-List View Updates
When switching to List View, all scheduled items are shown in chronological order.


-Date Navigation Controls
Today's date is always prominently highlighted
View switching is instantaneous without data loss
Navigation works smoothly in either direction
View preferences persist across sessions
 
 
Validations
 
  – State Persistence: Remember preferred view for future sessions.
  – Responsive Behavior: Automatic view adjustment for different screen sizes
  - Staff Filter: Verify staff members exist and are active
  - Performance Limits: Restrict data requests to manageable timeframes
  – All transitions are smooth 
 
 
Error Handling
  - Network Failure: Display retry option 
  – API Timeout: Show cached basic dashboard with offline indicator
  - Invalid Response: Display error notification
  - Date Navigation Error: Return to today's date 
  - Performance Issues: Show loading states and offer simplified views
 

