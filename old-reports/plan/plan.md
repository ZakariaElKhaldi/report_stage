# LearnExpert Connect - Front-end Development Plan

## Project Overview

**Project Title:** LearnExpert Connect (Placeholder)

**Project Description:**
LearnExpert Connect is envisioned as a premier, integrated online platform designed to bridge the gap between structured e-learning and personalized expert guidance. It serves as a comprehensive ecosystem where both individual professionals and entire organizations can foster skill development through high-quality online courses, while also having seamless access to on-demand support, consulting, and specialized services ("prestations") delivered by vetted experts via a dedicated, built-in real-time communication platform.

The platform offers a robust course catalog featuring modules, video/image-based lessons, and interactive quizzes. For enterprises, it provides tools for managing employee learning, tracking progress, and ensuring team upskilling aligns with organizational goals. For individuals, it offers a flexible subscription model to access the full range of learning content.

Beyond self-paced learning, LearnExpert Connect's key differentiator is its integrated consultation layer. Users can browse expert profiles, check availability, and book one-on-one sessions for mentorship, problem-solving, or specialized services directly within the platform's ecosystem. These sessions occur on a custom-built, secure meeting platform designed to provide a professional and branded experience, distinct from generic meeting tools.

Built on a modern microservices architecture, LearnExpert Connect prioritizes scalability, resilience, and maintainability, allowing for continuous feature enhancement and adaptation to user needs.

## Project Objectives:

- [ ] Empower Skill Development: Provide a rich library of high-quality, accessible e-learning courses for both individual and corporate learners to acquire new skills and enhance existing ones.
- [ ] Facilitate Personalized Guidance: Offer seamless access to expert consultants and prestation providers for tailored support, problem-solving, and specialized services, moving beyond generic Q&A forums.
- [ ] Serve Enterprise Needs: Deliver a robust solution for companies to manage, assign, and track employee training effectively, demonstrating clear ROI on learning investments.
- [ ] Create a Unified Ecosystem: Integrate self-paced learning and real-time expert interaction within a single, intuitive platform experience, minimizing friction for the user.
- [ ] Build a Scalable & Reliable Platform: Utilize a microservices architecture and modern technologies to ensure the platform can grow with its user base and feature set while maintaining high availability and performance.
- [ ] Establish a Sustainable Business Model: Implement clear subscription models for solo learners and enterprises, potentially augmented by pay-per-session consultation fees (depending on final model).
- [ ] Deliver an Exceptional User Experience: Ensure the platform is intuitive, engaging, professional, and easy to navigate for all user types.

## Target Audience:

- Individual Professionals / Solo Learners
- Enterprises / Companies (SMBs, Larger Organizations)
- Internal User Groups (Course Creators, Consultants/Experts, Platform Admin)

## Key Features & Functionalities (Summary):

- E-Learning: Course catalog, module/lesson structure, video/image content, quizzes, progress tracking, certificates, multi-language.
- Consultation: Service browsing, consultant profiles/availability, booking system, integration with custom meeting platform API, billing mechanism for sessions.
- User Management: Individual & company accounts, role-based access, profile/settings management.
- Enterprise Tools: Employee management, course assignment, team analytics, company settings/branding.
- Monetization: Solo & enterprise subscription management, payment gateway integration, invoicing.
- Notifications: In-app and email alerts for key activities.
- Admin Panel: Platform oversight, user/content management, global settings.

## Unique Selling Propositions (USPs):

- Integrated Learning & Consultation
- Custom Meeting Platform (Note: This will be a separate platform developed later)
- Enterprise Focus
- Scalable Architecture

## Important Considerations:

- Meeting Platform Development (Separate)
- Expert Vetting
- Content Quality
- Inter-Service Communication (Kafka)
- Data Privacy & Security

## Overall Design Philosophy:

- Modern & Clean
- Intuitive & User-Friendly
- Professional & Trustworthy
- Responsive (Desktop First, Tablet Friendly)
- Accessible (WCAG guidelines)
- Consistent Branding
- Personalized Where Applicable

## Front-end Development Plan (Next.js, Tailwind CSS, Shadcn UI)

### Proposed Folder Structure:

```
/
├── app/
│   ├── page.tsx (Landing Page)
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   ├── register/
│   │   │   └── page.tsx
│   │   ├── forgot-password/
│   │   │   └── page.tsx
│   │   └── reset-password/
│   │       └── page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx
│   │   ├── dashboard/
│   │   │   └── page.tsx (Learner Dashboard)
│   │   ├── courses/
│   │   │   ├── page.tsx (Course Catalog)
│   │   │   ├── [courseId]/
│   │   │   │   ├── page.tsx (Course Detail)
│   │   │   │   ├── learn/
│   │   │   │   │   ├── [lessonId]/
│   │   │   │   │   │   └── page.tsx (Lesson Viewer)
│   │   │   │   │   └── quiz/
│   │   │   │   │       └── [quizId]/
│   │   │   │   │           └── page.tsx (Quiz Page)
│   │   │   │   └── quiz-results/
│   │   │   │       └── [quizId]/
│   │   │   │           └── page.tsx (Quiz Results Page)
│   │   ├── consultations/
│   │   │   ├── page.tsx (My Consultations)
│   │   │   └── request/
│   │   │       └── page.tsx (Consultation Request Page)
│   │   ├── certificates/
│   │   │   └── page.tsx (My Certificates)
│   │   ├── subscriptions/
│   │   │   └── page.tsx (My Subscriptions - Solo)
│   │   ├── billing-history/
│   │   │   └── page.tsx (Billing History)
│   │   ├── profile/
│   │   │   └── page.tsx (User Profile Page)
│   │   └── settings/
│   │       └── page.tsx (User Settings Page)
│   ├── (enterprise)/
│   │   ├── layout.tsx
│   │   ├── dashboard/
│   │   │   └── page.tsx (Enterprise Admin Dashboard)
│   │   ├── employees/
│   │   │   └── page.tsx (Manage Employees Page)
│   │   ├── assign-courses/
│   │   │   └── page.tsx (Assign Courses Page)
│   │   ├── reports/
│   │   │   └── page.tsx (Company Reports/Analytics Page)
│   │   ├── subscription/
│   │   │   └── page.tsx (Company Subscription & Billing Page)
│   │   └── settings/
│   │       └── page.tsx (Company Settings Page)
│   ├── (creator)/
│   │   ├── layout.tsx
│   │   ├── dashboard/
│   │   │   └── page.tsx (Course Creator Dashboard)
│   │   ├── courses/
│   │   │   ├── create/
│   │   │   │   └── page.tsx
│   │   │   └── [courseId]/
│   │   │       └── edit/
│   │   │           └── page.tsx (Course Editor Interface)
│   │   └── media-library/
│   │       └── page.tsx (My Media Library - Optional)
│   ├── (consultant)/
│   │   ├── layout.tsx
│   │   ├── dashboard/
│   │   │   └── page.tsx (Consultant Dashboard / My Schedule)
│   │   ├── availability/
│   │   │   └── page.tsx (Manage Availability Page)
│   │   ├── requests/
│   │   │   └── page.tsx (Consultation Requests Page)
│   │   └── sessions/
│   │       └── [sessionId]/
│   │           └── page.tsx (Session Details Page)
│   ├── (admin)/
│   │   ├── layout.tsx
│   │   ├── dashboard/
│   │   │   └── page.tsx (Admin Dashboard)
│   │   ├── users/
│   │   │   └── page.tsx (User Management Page)
│   │   ├── companies/
│   │   │   └── page.tsx (Company Management Page)
│   │   ├── courses/
│   │   │   └── page.tsx (Course Management Page)
│   │   ├── subscriptions/
│   │   │   └── page.tsx (Subscription & Billing Management Page)
│   │   ├── consultation-services/
│   │   │   └── page.tsx (Consultation Service Management Page)
│   │   ├── notifications/
│   │   │   └── page.tsx (Notification Management Page)
│   │   └── moderation/
│   │       └── page.tsx (Content Moderation Page - Optional)
│   ├── layout.tsx (Root Layout)
│   └── globals.css
├── components/
│   ├── ui/ (Shadcn UI components)
│   ├── shared/ (Components used across multiple sections)
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   ├── NotificationPanel.tsx
│   │   ├── SearchBar.tsx
│   │   ├── UserProfileDropdown.tsx
│   │   ├── DevNavigation.tsx (Development Navigation Tool)
│   │   └── ...
│   ├── auth/ (Components specific to authentication)
│   │   ├── LoginForm.tsx
│   │   ├── RegistrationForm.tsx
│   │   └── ...
│   ├── courses/ (Components specific to courses)
│   │   ├── CourseCard.tsx
│   │   ├── CourseDetail.tsx
│   │   ├── LessonViewer.tsx
│   │   ├── Quiz.tsx
│   │   └── ...
│   ├── enterprise/ (Components specific to enterprise features)
│   │   ├── EmployeeTable.tsx
│   │   ├── AssignCoursesForm.tsx
│   │   └── ...
│   ├── creator/ (Components specific to course creation)
│   │   ├── CourseEditor.tsx
│   │   ├── QuizEditor.tsx
│   │   └── ...
│   ├── consultant/ (Components specific to consultants)
│   │   ├── AvailabilityCalendar.tsx
│   │   ├── ConsultationRequestList.tsx
│   │   └── ...
│   └── admin/ (Components specific to admin panel)
│       ├── UserTable.tsx
│       ├── CompanyTable.tsx
│       └── ...
├── lib/
│   ├── utils.ts (for cn function and other utilities)
│   ├── api.ts (API client)
│   └── ...
├── hooks/ (Custom React hooks)
│   └── useAuth.ts
├── types/ (TypeScript types and interfaces)
│   └── index.d.ts
├── public/ (Static assets)
│   └── ...
└── tailwind.config.ts
```

### Proposed Component Architecture:

- Follow a component-based approach, composing pages from smaller, reusable components.
- Utilize Shadcn UI for foundational components.
- Create `shared` components for elements used across sections (Header, Footer, DevNavigation).
- Develop section-specific components (e.g., `courses/CourseCard`, `enterprise/EmployeeTable`).
- Use Next.js layouts for shared UI across groups of pages.

### Implementation Plan (Phased Approach with Detailed Checkboxes):

- [ ] **Phase 1: Core Authentication, Basic Navigation & Dev Tool**
    - [ ] Set up the Next.js project with Tailwind CSS and Shadcn UI.
    - [ ] Implement the `(auth)` pages:
        - [ ] `login/page.tsx`:
            - [ ] Design and implement the login form UI (email, password fields, remember me checkbox, forgot password link, login button).
            - [ ] Add basic client-side form validation (e.g., required fields).
            - [ ] Include platform logo and link to registration.
        - [ ] `register/page.tsx`:
            - [ ] Design and implement the registration form UI (full name, email, password, confirm password fields).
            - [ ] Add basic client-side form validation.
            - [ ] Include checkbox for terms of service and privacy policy with links.
            - [ ] Include platform logo and link to login.
        - [ ] `forgot-password/page.tsx`:
            - [ ] Design and implement the forgot password form UI (email field, send reset link button).
            - [ ] Add basic client-side form validation.
            - [ ] Include instructions and link back to login.
        - [ ] `reset-password/page.tsx`:
            - [ ] Design and implement the reset password form UI (new password, confirm new password fields, reset password button).
            - [ ] Add basic client-side form validation and password strength indicator (optional initially).
            - [ ] Include success message and redirect logic (placeholder).
    - [ ] Create the basic `Header.tsx` component in `components/shared`:
        - [ ] Include platform logo (links to landing page `/`).
        - [ ] Placeholder for primary navigation links (will be dynamic later).
        - [ ] Placeholder for search bar, notification icon, user profile dropdown.
    - [ ] Create the basic `Footer.tsx` component in `components/shared`:
        - [ ] Include placeholder links for About Us, Contact, FAQ, Terms, Privacy.
        - [ ] Include copyright information.
    - [ ] Implement the root `layout.tsx` in `app/`:
        - [ ] Include the `Header` and `Footer` components.
        - [ ] Set up basic page structure and styling with Tailwind CSS.
    - [ ] Implement the `DevNavigation.tsx` component in `components/shared/`:
        - [ ] Create a simple UI (e.g., a fixed sidebar or modal).
        - [ ] Manually list the initial routes for quick navigation.
        - [ ] Add logic to show/hide the component based on a development environment check (`process.env.NODE_ENV === 'development'`).
        - [ ] Implement the `Ctrl + X` keyboard shortcut to toggle visibility.
        - [ ] Update routes to include the Landing Page (`/`) and the new Dashboard path (`/dashboard`).
    - [ ] Implement the `app/page.tsx` (Landing Page):
        - [ ] Design and implement the basic landing page UI with a title, description, and links to Login and Sign Up.

- [ ] **Phase 2: Learner Dashboard & Course Browsing**
    - [ ] Implement the `(dashboard)/layout.tsx`:
        - [ ] Include the `Header`.
        - [ ] Design the main content area layout.
    - [ ] Implement the `(dashboard)/dashboard/page.tsx` (Learner Dashboard - Updated Path):
        - [ ] Display a personalized welcome message (placeholder).
        - [ ] Create a "Continue Learning" section with placeholder course cards and progress bars.
        - [ ] Create "My Courses" / "Assigned Courses" section (list of courses).
        - [ ] Create "Recommended Courses" section (placeholder).
        - [ ] Create "Upcoming Consultations" section (placeholder list).
        - [ ] Create "Recent Achievements" section (placeholder list).
        - [ ] Include quick links to "Browse Courses" and "Request Consultation".
    - [ ] Implement the `courses/page.tsx` (Course Catalog):
        - [ ] Design and implement the search bar UI.
        - [ ] Design and implement the filters UI (categories, tags, skill level, etc. - placeholder functionality).
        - [ ] Implement the layout for displaying course listings (grid or list).
        - [ ] Create the `CourseCard.tsx` component:
            - [ ] Display course thumbnail, title, brief description, author, rating, enrollment count (placeholders).
            - [ ] Add "View Details" or "Enroll" button (placeholder links).
        - [ ] Implement sorting options UI (placeholder functionality).
        - [ ] Implement pagination UI (placeholder functionality).
    - [ ] Implement the basic structure of the `[courseId]/page.tsx` (Course Detail):
        - [ ] Display course title, author, rating, enrollment count (placeholders).
        - [ ] Design the layout for detailed course information (description, objectives, curriculum).
        - [ ] Design the curriculum section with expandable modules and lessons (placeholder structure).
        - [ ] Design the sidebar/action area with "Enroll Now" / "Start Learning" button (placeholder).
        - [ ] Design the Reviews & Ratings section (placeholder).
        - [ ] Fix the `params` access error by awaiting `params`.

- [ ] **Phase 3: Course Consumption**
    - [ ] Implement the `learn/[lessonId]/page.tsx` (Lesson Viewer):
        - [ ] Design the main content area for displaying video, text, or image lessons.
        - [ ] Implement a placeholder video player UI.
        - [ ] Implement basic text and image display.
        - [ ] Design the navigation sidebar/panel showing course structure and lesson list.
        - [ ] Display current lesson title.
        - [ ] Add "Mark as Complete" / "Next Lesson" button (placeholder functionality).
        - [ ] Design a placeholder for the Discussion/Q&A section (optional).
        - [ ] Design a placeholder for the Resources/Downloads section (if applicable).
    - [ ] Implement the `quiz/[quizId]/page.tsx` (Quiz Page):
        - [ ] Display quiz title, instructions, and time limit (placeholder).
        - [ ] Design the question navigation UI.
        - [ ] Design the question display area with different answer types (radio, checkbox, text input - placeholder).
        - [ ] Add "Next Question" / "Previous Question" buttons (placeholder functionality).
        - [ ] Add "Submit Quiz" button (placeholder functionality).
        - [ ] Display a progress bar (placeholder).
    - [ ] Implement the `quiz-results/[quizId]/page.tsx` (Quiz Results Page):
        - [ ] Display Pass/Fail status and score (placeholder).
        - [ ] Design the review of answers section (placeholder).
        - [ ] Add "Retake Quiz" button (placeholder functionality).
        - [ ] Add link to "Continue Course".

- [ ] **Phase 4: User Management & Settings (Learner/Employee)**
    - [ ] Implement `profile/page.tsx` (User Profile):
        - [ ] Design the UI to display and edit user information (name, email, password change link, avatar upload - placeholder).
        - [ ] Add "Save Changes" button (placeholder).
    - [ ] Implement `settings/page.tsx` (User Settings):
        - [ ] Design the layout with tabs or sections for different settings (Account, Preferences, Notifications).
        - [ ] Design UI for language preference, theme toggle (optional initially).
        - [ ] Design UI for granular notification controls (toggle switches - placeholder).
        - [ ] Add "Save Changes" buttons (placeholder).
    - [ ] Implement `certificates/page.tsx` (My Certificates):
        - [ ] Design the UI to list earned certificates.
        - [ ] Display course title, date issued, certificate UID (placeholders).
        - [ ] Add "View Certificate" and "Download PDF" buttons (placeholder functionality).
    - [ ] Implement `subscriptions/page.tsx` (My Subscriptions - Solo):
        - [ ] Design the UI to display current plan details (name, price, billing cycle, next billing date - placeholders).
        - [ ] Add "Change Plan", "Cancel Subscription", "Update Payment Method" links/buttons (placeholder functionality).
        - [ ] Add link to "Billing History".
    - [ ] Implement `billing-history/page.tsx` (Billing History):
        - [ ] Design the UI to list past transactions/invoices.
        - [ ] Display date, description, amount, status (placeholders).
        - [ ] Add "Download Invoice (PDF)" button (placeholder functionality).
    - [ ] Implement `consultations/page.tsx` (My Consultations):
        - [ ] Design the layout with tabs/sections for Upcoming, Pending Requests, Past Sessions.
        - [ ] Design the UI for listing sessions/requests with relevant details (placeholders).
        - [ ] Add "Join Meeting" link (placeholder).
        - [ ] Add options to cancel/reschedule/provide feedback (placeholder functionality).
    - [ ] Implement `consultations/request/page.tsx` (Consultation Request Page):
        - [ ] Design the form UI to request a consultation (service type dropdown, problem description textarea, preferred availability input - placeholder).
        - [ ] Add "Submit Request" button (placeholder).
        - [ ] Display confirmation message (placeholder).

- [ ] **Phase 5: Enterprise Admin Pages**
    - [ ] Implement the `(enterprise)` layout.
    - [ ] Implement the `dashboard/page.tsx` (Enterprise Admin Dashboard):
        - [ ] Display key metrics (active employees, completion rate, learning hours - placeholders).
        - [ ] Display subscription overview (plan, seats, billing date - placeholders).
        - [ ] Display recent employee activity (placeholder list).
        - [ ] Display pending approvals (placeholder list).
        - [ ] Include quick links to management pages.
    - [ ] Implement the `employees/page.tsx` (Manage Employees Page):
        - [ ] Design and implement the UI for listing employees (table).
        - [ ] Create the `EmployeeTable.tsx` component (using Shadcn UI table).
        - [ ] Display employee details (name, email, status, etc. - placeholders).
        - [ ] Add actions per employee (View Progress, Assign Courses, Edit Role, Remove - placeholder functionality).
        - [ ] Design and implement the "Invite Employee" section (individual invite, shareable link - placeholder UI).
        - [ ] Add search and filter UI (placeholder functionality).
    - [ ] Implement the `assign-courses/page.tsx` (Assign Courses Page):
        - [ ] Design the UI to select employees (checkbox list, search).
        - [ ] Design the UI to select courses (similar to course catalog browsing).
        - [ ] Add "Assign Selected Courses to Selected Employees" button (placeholder functionality).
        - [ ] Add option to set deadlines UI (optional, placeholder).
    - [ ] Implement the `reports/page.tsx` (Company Reports/Analytics Page):
        - [ ] Design the UI to display overall stats and reports (placeholders for data visualizations).
        - [ ] Design the UI for course-specific and employee-specific reports (placeholder tables/lists).
        - [ ] Add filters (date range, department, course - placeholder functionality).
        - [ ] Add "Export Data" button (placeholder functionality).
    - [ ] Implement the `subscription/page.tsx` (Company Subscription & Billing Page):
        - [ ] Design the UI similar to the solo learner subscription page but for the company (plan details, seat count, pricing - placeholders).
        - [ ] Add options to upgrade/downgrade, adjust seat count (placeholder functionality).
        - [ ] Design UI to manage company payment methods (placeholder).
        - [ ] Link to billing history (reusing the billing history component).
    - [ ] Implement the `settings/page.tsx` (Company Settings Page):
        - [ ] Design the layout with tabs/sections for company profile, branding, feature toggles, certificate customization (placeholders).
        - [ ] Design UI for uploading company logo and setting brand color (placeholder).
        - [ ] Add "Save Changes" buttons (placeholder).

- [ ] **Phase 6: Course Creator Pages**
    - [ ] Implement the `(creator)` layout.
    - [ ] Implement the `dashboard/page.tsx` (Course Creator Dashboard):
        - [ ] List courses created/being created (title, status, last modified, enrolled students - placeholders).
        - [ ] Add actions (Edit, View, Submit for Review, Manage Versions, View Analytics - placeholder functionality).
        - [ ] Add "Create New Course" button.
    - [ ] Implement the `courses/[courseId]/edit/page.tsx` (Course Editor Interface):
        - [ ] Design the multi-step or tabbed interface layout.
        - [ ] Design sections for Course Information, Curriculum Builder, Quiz Manager, Versioning, Preview.
        - [ ] Design UI elements within each section based on the detailed description (input fields, text areas, upload areas, list builders - placeholders).
        - [ ] Design the Curriculum Builder UI with drag-and-drop reordering (optional initially).
        - [ ] Design the Quiz Manager/Editor UI (linking to quizzes, adding/editing questions and answers - placeholder).
        - [ ] Add "Save Draft" and "Submit for Review" buttons (placeholder functionality).
    - [ ] Implement the `media-library/page.tsx` (My Media Library - Optional):
        - [ ] Design the UI for uploading and viewing media assets (placeholders).
        - [ ] Add options to organize and search media (placeholder functionality).

- [ ] **Phase 7: Consultant Pages**
    - [ ] Implement the `(consultant)` layout.
    - [ ] Implement the `dashboard/page.tsx` (Consultant Dashboard / My Schedule):
        - [ ] Design a calendar view for scheduled consultations (placeholder).
        - [ ] Design a list of upcoming sessions (placeholders).
        - [ ] Design a list of pending requests (placeholders).
        - [ ] Add quick link to "Manage Availability".
        - [ ] Display statistics (optional, placeholders).
    - [ ] Implement the `availability/page.tsx` (Manage Availability Page):
        - [ ] Design a calendar interface for managing availability (placeholder).
        - [ ] Design UI to select dates/times and mark availability (placeholder functionality).
        - [ ] Add "Save Availability" button (placeholder).
    - [ ] Implement the `requests/page.tsx` (Consultation Requests Page):
        - [ ] Design the UI to list pending requests (placeholders).
        - [ ] Display requester name, service type, request details, preferred times (placeholders).
        - [ ] Add "Accept & Schedule" and "Decline" actions (placeholder functionality).
    - [ ] Implement the `sessions/[sessionId]/page.tsx` (Session Details Page - Pre-Session):
        - [ ] Design the UI to display session details (requester name, service type, date/time, topic - placeholders).
        - [ ] Add "Join Meeting" link (placeholder).
        - [ ] Add option to cancel/reschedule (placeholder functionality).

- [ ] **Phase 8: Platform Administrator Pages**
    - [ ] Implement the `(admin)` layout.
    - [ ] Implement the `dashboard/page.tsx` (Admin Dashboard):
        - [ ] Display platform statistics (placeholders).
        - [ ] Display system health indicators (placeholder).
        - [ ] Display moderation queues (placeholder lists).
        - [ ] Include quick links to major admin sections.
    - [ ] Implement the `users/page.tsx` (User Management Page):
        - [ ] Design and implement a searchable, filterable table for all users (using Shadcn UI table).
        - [ ] Display user details (ID, name, email, role, company, status - placeholders).
        - [ ] Add actions per user (View Details, Edit Profile, Change Roles, Suspend/Unsuspend, Delete, Impersonate - placeholder functionality).
    - [ ] Implement the `companies/page.tsx` (Company Management Page):
        - [ ] Design and implement a searchable, filterable table for all companies.
        - [ ] Display company details (ID, name, contact, plan, employees - placeholders).
        - [ ] Add actions (View Details, Edit, Manage Subscription, View Employees - placeholder functionality).
    - [ ] Implement the `courses/page.tsx` (Course Management Page):
        - [ ] Design and implement a searchable, filterable list/table for all courses and versions.
        - [ ] Display course details (title, creator, status, date - placeholders).
        - [ ] Add actions (View, Edit, Approve/Reject, Publish/Unpublish, Feature - placeholder functionality).
        - [ ] Design UI for managing global Categories and Tags.
    - [ ] Implement the `subscriptions/page.tsx` (Subscription & Billing Management Page):
        - [ ] Design UI for managing subscription plans (create, edit, deactivate - placeholder).
        - [ ] Design UI for viewing and filtering all subscriptions (placeholder table).
        - [ ] Design UI for viewing payment transaction logs (placeholder).
        - [ ] Design UI for processing refunds/manual adjustments (placeholder).
        - [ ] Design UI for managing Promo Codes (placeholder).
    - [ ] Implement the `consultation-services/page.tsx` (Consultation Service Management Page):
        - [ ] Design UI for managing service types (create, edit, pricing - placeholder).
        - [ ] Design UI for viewing active consultants (placeholder list/table).
        - [ ] Design UI for overseeing pending/scheduled sessions globally (placeholder).
        - [ ] Design UI for manual assignment tools (placeholder).
    - [ ] Implement the `notifications/page.tsx` (Notification Management Page):
        - [ ] Design UI for managing notification types and templates (create, edit, activate/deactivate - placeholder).
        - [ ] Design UI for viewing sending logs (placeholder).
        - [ ] Design UI for triggering test notifications (placeholder).
    - [ ] Implement the `settings/page.tsx` (Platform Settings Page):
        - [ ] Design UI for editing global settings (default language, maintenance mode, API keys - placeholder).
        - [ ] Design UI for managing Terms of Service / Privacy Policy content (placeholder).
        - [ ] Design UI for managing Feature Flags (placeholder).
    - [ ] Implement the `moderation/page.tsx` (Content Moderation Page - Optional):
        - [ ] Design UI for moderation queues (pending reviews, flagged items - placeholder).
        - [ ] Design UI for tools to approve, reject, edit, or delete content (placeholder functionality).

- [ ] **Phase 9: Integration & Refinement**
    - [ ] Connect the front-end components and pages with the backend APIs as they become available.
    - [ ] Implement state management solutions (e.g., React Context, Zustand, or Redux Toolkit) for managing application state.
    - [ ] Add comprehensive client-side form validation using a library like Zod and React Hook Form.
    - [ ] Implement robust error handling and display appropriate loading states for data fetching.
    - [ ] Refine UI based on the design philosophy, ensuring responsiveness, accessibility (WCAG guidelines), and consistent branding.
    - [ ] Implement the notification system to display in-app alerts.
    - [ ] Address any remaining features, edge cases, and perform thorough testing.

## UI/UX Improvement Phase

After the basic structure and functionality of the pages are implemented in each phase, a dedicated effort will be made to refine the user interface and user experience. This phase will focus on:

- Enhancing the visual design based on the "Overall Design Philosophy" (Modern & Clean, Professional, etc.).
- Improving the intuitiveness and user-friendliness of the interface.
- Implementing detailed styling using Tailwind CSS to match the desired look and feel.
- Ensuring responsiveness across different screen sizes (Desktop First, Tablet Friendly).
- Addressing accessibility guidelines (WCAG) for a more inclusive experience.
- Implementing consistent branding elements throughout the application.
- Iterating on the design based on user feedback or further design specifications.

This phase will likely overlap with the later stages of the implementation phases and continue throughout the development process as needed.

---

**8. Notification Service**
   *Manages notification templates, sends notifications (in-app, email) based on events from other services, and tracks delivery status.*

```dbml
// Notification Service DBML
Project notification_service {
  database_type: 'PostgreSQL'
}

Table notification_types { // Defines the types of notifications the system can send
  id uuid [pk, default: `uuid_generate_v4()`]
  event_name varchar [unique, not null, note: 'e.g., USER_REGISTERED, COURSE_ENROLLED, PAYMENT_SUCCEEDED, CONSULTATION_REMINDER']
  description text
  default_channels text[] [not null, default: '{"in_app", "email"}', note: 'Default channels for this type, e.g., in_app, email, sms']
  // is_user_configurable boolean [default: true, note: 'Can users disable this notification type?']
  // priority varchar [default: 'medium', note: 'e.g., low, medium, high']
  created_at timestamp [default: `now()`]
}

Table notification_templates { // Stores templates for different languages and channels
  id uuid [pk, default: `uuid_generate_v4()`]
  notification_type_id uuid [ref: > notification_types.id, not null]
  language_code varchar(10) [not null, note: 'e.g., en, fr-CA']
  channel varchar [not null, note: 'e.g., email, in_app, sms'] // Template per channel
  subject_template text [null, note: 'For email subject, uses template variables like {{user_name}}']
  body_template text [not null, note: 'For email/sms body or in-app message, uses template variables']
  // template_variables_schema jsonb [null, note: 'Schema of expected variables for validation (e.g., {"user_name": "string", "course_title": "string"})']
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (notification_type_id, language_code, channel) [unique]
  }
}

Table notifications_sent { // Log of individual notifications that have been processed/sent
  id uuid [pk, default: `uuid_generate_v4()`]
  recipient_user_id uuid [not null, note: 'User ID from IAM service']
  notification_type_id uuid [ref: > notification_types.id, not null]
  // triggering_event_id varchar [null, note: 'Original event ID that triggered this notification, for idempotency/tracing']
  // entity_type varchar [null, note: 'e.g., course, subscription, consultation_session']
  // entity_id uuid [null, note: 'ID of the related entity']
  // data_payload jsonb [null, note: 'The actual data used to render the template variables']
  created_at timestamp [default: `now()`] // When the notification was generated by this service
  // read_at timestamp [null, note: 'For in-app notifications, when the user read it']
  // is_archived boolean [default: false, note: 'For in-app notifications, if user archived it']
}

Table notification_deliveries { // Tracks delivery status for each channel of a sent notification
  id uuid [pk, default: `uuid_generate_v4()`]
  notification_sent_id uuid [ref: > notifications_sent.id, not null]
  channel varchar [not null, note: 'e.g., email, in_app, sms']
  status varchar [not null, default: 'pending', note: 'e.g., pending, queued, sent, failed, delivered, opened, clicked']
  // external_message_id varchar [null, note: 'ID from the email/SMS provider']
  // failure_reason text [null]
  // attempts int [default: 0]
  last_attempted_at timestamp
  // delivered_at timestamp [null, note: 'Actual delivery time if provider confirms']
  created_at timestamp [default: `now()`] // When this specific delivery attempt was initiated
  updated_at timestamp

  indexes {
    (notification_sent_id, channel) [unique]
    (status, last_attempted_at) // For retry queues or monitoring
  }
}

// User preferences for notifications are in IAM Service (user_settings table)
// This Notification Service would query IAM Service or listen to preference update events
// to respect user choices before sending.
```

---

**9. Platform Configuration & Admin Service**
   *Manages global platform settings, company-specific settings (branding, features), and potentially feature flags for A/B testing.*

```dbml
// Platform Configuration & Admin Service DBML
Project platform_config_service {
  database_type: 'PostgreSQL'
}

Table platform_settings { // Global settings for the entire platform
  // id int [pk, default: 1] // Often a single row table
  // CHECK (id = 1)
  setting_key varchar [pk, note: 'e.g., DEFAULT_LANGUAGE, SUPPORTED_LANGUAGES, MAINTENANCE_MODE, TOS_URL']
  setting_value text [not null]
  value_type varchar [not null, default: 'string', note: 'e.g., string, json, boolean, integer, array_string']
  description text
  // is_editable_by_admin boolean [default: true]
  last_updated_by_user_id uuid [null, note: 'Admin User ID from IAM service']
  updated_at timestamp [default: `now()`]

  // Example rows:
  // ('DEFAULT_LANGUAGE', 'en', 'string', 'Default language for new users')
  // ('SUPPORTED_LANGUAGES', '["en", "fr"]', 'array_string', 'List of BCP 47 language codes supported')
  // ('MAINTENANCE_MODE', 'false', 'boolean', 'Is the platform in maintenance mode?')
  // ('PAYMENT_GATEWAY_CONFIG', '{"publicKey": "pk_...", "secretKeyName": "stripe_secret"}', 'json', 'Configuration for payment gateway integration')
}


Table company_settings { // Company-specific overrides or settings
  company_id uuid [pk, note: 'Company ID from IAM service']
  // branding_logo_url text [null] // URL to company logo (might be stored in Media Service)
  // branding_primary_color varchar(7) [null, note: 'e.g., #RRGGBB']
  // custom_css_url text [null]
  // feature_flags_override jsonb [null, note: 'Overrides for global feature flags, e.g., {"enable_beta_dashboard": true}']
  // certificate_custom_template text [null, note: 'Custom template ID or content for certificates']
  // allowed_course_creator_user_ids uuid[] [null, note: 'Specific users within this company allowed to create courses, if applicable']
  last_updated_by_user_id uuid [null, note: 'Admin User ID from IAM service (platform admin or company admin)']
  updated_at timestamp [default: `now()`]

  // This table can be a key-value store per company as well, similar to platform_settings
  // Or have fixed columns for common settings.
  // Example using fixed columns:
  brand_logo_media_id uuid [null, note: 'Media ID from Media Service']
  primary_color_hex varchar(7) [null, note: '#RRGGBB']
  // default_employee_roles uuid[] [null, note: 'Role IDs from IAM to assign by default to new company users']
}

Table feature_flags { // For A/B testing or phased rollouts
  id uuid [pk, default: `uuid_generate_v4()`]
  flag_key varchar [unique, not null, note: 'e.g., NEW_COURSE_PLAYER_UX, AI_RECOMMENDATIONS_ENABLED']
  description text
  is_globally_enabled boolean [default: false]
  // rollout_percentage int [default: 0, note: '0-100, for percentage-based rollout']
  // target_user_ids uuid[] [null, note: 'Specific user_ids this flag applies to']
  // target_company_ids uuid[] [null, note: 'Specific company_ids this flag applies to']
  // target_attributes jsonb [null, note: 'e.g., {"country": "US", "user_segment": "power_users"}']
  created_at timestamp [default: `now()`]
  updated_at timestamp
  // created_by_user_id uuid [null, note: 'Admin User ID from IAM service']
}

// Table user_experiments { // Tracks which users are in which experiment variants
//   // id uuid [pk, default: `uuid_generate_v4()`]
//   user_id uuid [not null, note: 'User ID from IAM service']
//   feature_flag_id uuid [ref: > feature_flags.id, not null]
//   variant_name varchar [not null, note: 'e.g., control, treatment_A, treatment_B']
//   assigned_at timestamp [default: `now()`]

//   indexes {
//     (user_id, feature_flag_id) [pk] // User is in one variant per experiment
//   }
// }
// Note: user_experiments might be better managed by a dedicated A/B testing library/service
// or directly evaluated in application code based on feature_flag rules. Storing assignments
// can be useful for consistent experience.
```

---

**10. Analytics & Reporting Service**
    *This service typically consumes events from other services and builds aggregated data or data marts for reporting. It might not *own* many primary source-of-truth tables but rather derived ones. The schema here is highly dependent on the specific reports needed.*

```dbml
// Analytics & Reporting Service DBML (Example - highly customizable)
Project analytics_service {
  database_type: 'PostgreSQL (or specialized analytics DB like ClickHouse, BigQuery)'
}

// Example: Aggregated daily active users
Table daily_active_users_summary {
  summary_date date [pk]
  active_user_count int [not null]
  // solo_learner_active_count int
  // enterprise_employee_active_count int
  // new_registrations_on_day int
  created_at timestamp [default: `now()`]
}

// Example: Aggregated course engagement
Table course_engagement_summary {
  // id uuid [pk] // or composite key
  course_id uuid [not null, note: 'Course ID from Course service']
  summary_period_start date [not null]
  summary_period_end date [not null]
  total_enrollments_in_period int [default: 0]
  total_completions_in_period int [default: 0]
  average_time_spent_seconds int [null]
  average_quiz_score_percentage float [null]
  // last_calculated_at timestamp [default: `now()`]

  indexes {
    (course_id, summary_period_start, summary_period_end) [pk, name: 'pk_course_engagement']
  }
}

// Example: Company-level learning summary
Table company_learning_summary {
  company_id uuid [not null, note: 'Company ID from IAM service']
  summary_date date [not null]
  active_learners_count int [default: 0]
  total_courses_completed_by_employees int [default: 0]
  total_learning_time_seconds_by_employees bigint [default: 0]
  // last_calculated_at timestamp [default: `now()`]

  indexes {
    (company_id, summary_date) [pk, name: 'pk_company_learning']
  }
}

// Example: Storing raw events for later processing (could also be in a data lake / event store like Kafka)
Table raw_events_log {
  id uuid [pk, default: `uuid_generate_v4()`]
  event_timestamp timestamp [not null, default: `now()`]
  event_source_service varchar [not null] // e.g., IAM, CourseService, BillingService
  event_type varchar [not null] // e.g., USER_LOGIN, LESSON_COMPLETED, PAYMENT_SUCCEEDED
  user_id uuid [null]
  company_id uuid [null]
  entity_id uuid [null] // ID of the primary entity related to the event
  payload jsonb // The full event payload
  // processed_at timestamp [null] // If this table is used as a staging area

  indexes {
    (event_timestamp)
    (event_type)
    (user_id)
  }
}

// Note: For complex analytics, this service might read from a replica of other services' databases
// (read-only access), or more commonly, consume event streams and build its own optimized
// data structures (data marts, OLAP cubes). The tables above are just illustrative of
// potential aggregated views it might maintain.
```

---

**11. Search Service**
    *If you build a dedicated search service, its primary "database" is often the search engine index itself (e.g., Elasticsearch, OpenSearch, MeiliSearch). It might have minimal relational storage, perhaps for managing indexing tasks or synonyms.*

```dbml
// Search Service DBML (Minimal relational store, main data in Search Engine)
Project search_service {
  database_type: 'PostgreSQL (for operational data, not the search index itself)'
}

Table indexed_document_sources { // Tracks what kind of documents are being indexed
  source_key varchar [pk, note: 'e.g., courses, users, consultants']
  description text
  last_indexed_at timestamp [null, note: 'When this source was last fully re-indexed']
  // indexing_status varchar [default: 'idle', note: 'e.g., idle, indexing, error']
  // error_message text [null]
}

Table search_synonyms {
  id uuid [pk, default: `uuid_generate_v4()`]
  term varchar [not null]
  synonyms text[] [not null, note: 'Array of synonyms for the term']
  language_code varchar(10) [not null, default: 'en']
  is_two_way boolean [default: true, note: 'If true, synonyms also map back to term']
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (term, language_code) [unique]
  }
}

// The actual search index (e.g., in Elasticsearch) would have documents like:
// Document for a Course:
// {
//   "id": "course_uuid_from_course_service",
//   "type": "course",
//   "title_en": "English Title",
//   "title_fr": "French Title",
//   "description_en": "...",
//   "description_fr": "...",
//   "tags_en": ["tag1", "tag2"],
//   "tags_fr": ["tag_un", "tag_deux"],
//   "category_names_en": ["catA"],
//   "author_name": "...", // Denormalized from IAM/Course service
//   "average_rating": 4.5, // Denormalized from Feedback service
//   "enrollment_count": 1200 // Denormalized/aggregated
// }
// This structure is managed within the search engine, not typically in PostgreSQL for this service.
```

---

This completes the set of DBMLs for the microservices we've discussed. Remember that these are starting points. As you delve into the implementation details of each service, you'll likely refine these schemas further, add more specific constraints, optimize indexes, and potentially introduce more denormalization where it makes sense for performance while managing the consistency trade-offs.

</environment_details>
