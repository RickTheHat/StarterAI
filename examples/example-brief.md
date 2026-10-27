# Project brief: Harbor Lane Library Room Booking

**Human owner:** Example Owner  
**Last updated:** 2026-10-26  
**Status:** Approved

This is a completed sample brief for a fictional project. It shows what a
kickoff-ready `3-brief.md` looks like. Copy the structure, not the
content. Every name, address, and policy here is invented.

Operating rules live in `1-director.md` and defaults in `2-standards.md`.
Approved exceptions to a standard are listed below under "Approved
exceptions" and recorded again in `docs/decisions.md`.

## Goal

- **What are we building?** A small public website where residents of a
  fictional town can reserve one of three study rooms at the Harbor Lane
  branch library, and where library staff can see and manage the day's
  bookings.
- **Problem and desired outcome:** Bookings are currently taken by phone
  and written on a paper calendar, which causes double bookings and ties
  up the front desk. The site should let residents book a free slot in
  under two minutes and give staff one accurate schedule.
- **Intended users:** Adult library card holders who want a quiet room,
  and two staff members who manage the schedule.
- **Launch context:** Public website for one branch. Target launch is the
  start of the next school term. Traffic is low: a few dozen bookings per
  week.

## MVP

- A resident can view room availability for the next 14 days and see
  which hour-long slots are free. Done when the calendar matches the
  database for every room and day and loads on a phone.
- A signed-in resident can book one free slot and receives a confirmation
  email. Done when a booked slot is no longer available to anyone else,
  even under concurrent attempts.
- A signed-in resident can cancel their own upcoming booking. Done when
  the slot immediately becomes available again.
- A staff member can view today's bookings per room and cancel any
  booking with a reason that is emailed to the resident. Done when the
  staff view is restricted to staff accounts.

## Exclusions

- Payments, fines, or deposits.
- Recurring bookings and waitlists.
- Integration with the library's catalog or card system; identity is by
  email only.
- Other branches or more than three rooms.
- A native mobile app or offline use.

## Key workflows and content

- **Primary workflows:** browse availability, sign in by email code, book
  a slot, cancel a slot, staff daily view.
- **Public and private areas:** availability calendar and about page are
  public. Booking, cancelling, and the account page require sign-in. The
  staff view requires the staff role.
- **Content and branding:** no existing brand. Use the default theme. The
  library name and address are invented and may be changed later. Tone is
  plain and friendly. Domain is not yet chosen.
- **Integrations:** a transactional email provider for confirmations, to
  be decided before public launch. None otherwise.

## Project decisions and constraints

- **User accounts:** Yes. Any verified email may register as a resident.
  Two staff accounts are promoted by the owner through the admin
  bootstrap. Roles: resident, staff.
- **Data:** email address, booking times, and cancellation reasons. Low
  sensitivity, but email addresses are personal data. Keep bookings for
  12 months, then delete. A resident may delete their account, which
  removes future bookings and anonymizes past ones. No export needed.
  Expected volume: under 5,000 bookings per year.
- **Payments or money:** None.
- **Languages, locale, and time zones:** English only. All times are
  shown in the library's local time zone, set in configuration. Dates in
  ISO format.
- **Hosting and availability:** Public, on a single small virtual server
  with durable disk. Short outages are acceptable; losing bookings is
  not.
- **Budget and paid services:** Zero spend during development. Before
  launch, the owner will approve a domain, a server, and an email
  provider; expected total under 20 USD per month.
- **Technical constraints:** Follow `2-standards.md` as written. Staff
  must be able to use the site on a shared library desktop running a
  current browser.
- **Approved exceptions:** None.
- **Open decisions:** Whether residents may hold more than one future
  booking at a time. Recommended default: a maximum of two. Blocks the
  first delivery: no; implement the default and revisit.

## Project-specific rules

- A slot is one hour on the hour, between 09:00 and 20:00, closed
  Sundays. Enforce in the database and in validation, not only in the UI.
- A resident may not book a slot that starts in the past or more than 14
  days ahead.
- Cancelling a booking less than one hour before it starts is allowed but
  recorded as a late cancellation on the booking.
- Staff can see a resident's email and bookings but nothing else. There
  are no other admin powers.
- Confirmation and cancellation emails are plain text with no tracking.

## User experience and acceptance

- **Visual direction:** default theme. No light theme or theme switching
  in the first release.
- **Responsive behavior:** most residents will book from a phone. The
  calendar must be usable at 320px wide without horizontal scrolling.
- **Accessibility:** standard baseline. The calendar must be navigable by
  keyboard and readable by a screen reader as a list of slots per room.
- **Performance:** standard baseline.
- **Critical acceptance tests:**
  - Two simultaneous attempts to book the same slot result in exactly one
    booking and one clear failure message.
  - A cancelled slot appears as available on the next page load.
  - A resident cannot view, change, or cancel another resident's booking
    by altering a URL or form value.
  - A resident account cannot reach the staff view.
  - The sign-in code is never shown in the production login page or logs.
  - Bookings survive an application restart and a backup-and-restore
    drill.

## Initial delivery

- First vertical slice: public availability calendar for one room backed
  by the real database, with sign-in and a working booking flow, deployed
  locally only.
- Evidence required: passing build and tests, an end-to-end booking test
  including the concurrency case, an accessibility scan of the calendar
  and booking pages, and screenshots at 320px and desktop widths.
- Known constraints: nothing is deployed publicly and no paid service is
  provisioned until the owner approves the launch checklist.

Do not deploy publicly, access production data, or provision paid
resources without explicit human approval.
