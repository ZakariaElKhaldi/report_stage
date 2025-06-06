# Platform Pages Documentation

## I. Shared Pages (Accessible to most logged-in users, content may vary by role)

### Login Page
**Usecase**: Allow registered users to access their accounts.

**Description**:
- Clean layout with platform logo prominently displayed.
- Input fields: Email, Password.
- "Remember Me" checkbox.
- "Forgot Password?" link.
- Clear "Login" button.
- Link to "Sign Up" / "Create an Account" for new users.
- Optional: Social login buttons (Google, LinkedIn - if implemented).

### Registration Page
**Usecase**: Allow new users to create an account (defaults to "Solo Learner").

**Description**:
- Platform logo and a brief value proposition (e.g., "Unlock Your Potential").
- Input fields: Full Name, Email, Password, Confirm Password.
- Checkbox for agreeing to Terms of Service & Privacy Policy (with links).
- Clear "Sign Up" or "Create Account" button.
- Link to "Already have an account? Login."
- Success message upon completion, prompting for email verification if applicable.

### Password Reset Request Page
**Usecase**: Allow users who forgot their password to initiate the reset process.

**Description**:
- Input field: Email.
- "Send Reset Link" button.
- Instructions on what to expect (e.g., "Check your email for a password reset link.").
- Link back to Login.

### Password Reset Form Page
**Usecase**: Allow users to set a new password after clicking the reset link.

**Description**:
- Input fields: New Password, Confirm New Password.
- Password strength indicator.
- "Reset Password" button.
- Success message and redirect to Login.

### Main Navigation / Header (Persistent across most authenticated pages)
**Usecase**: Provide consistent access to key platform areas and user account options.

**Description**:
- Platform Logo (links to user's dashboard or homepage).
- Primary navigation links (dynamically shown based on user role):
  - Common: Dashboard, Courses, Consultations (if applicable).
  - Enterprise Admin: Manage Company, Reports.
  - Course Creator: My Creations.
  - Consultant: My Schedule, Requests.
  - Platform Admin: Admin Panel.
- Search Bar (global search for courses, etc.).
- Notification Icon (with a badge for unread notifications).
- User Profile Dropdown/Icon:
  - Links to: My Profile, Settings, Subscriptions (for solo/enterprise admin), Billing History, Logout.
  - Displays user's name/avatar.

### Footer (Persistent across most pages)
**Usecase**: Provide access to informational links and legal documents.

**Description**:
- Links: About Us, Contact Support, FAQ, Terms of Service, Privacy Policy.
- Copyright information.
- Social media links (optional).
- Language switcher (if multiple languages supported globally).

### Notifications Panel/Page
**Usecase**: Allow users to view and manage their in-app notifications.

**Description**:
- Accessible via the notification icon in the header.
- List of notifications (chronological, with unread indicators).
- Each notification: brief message, timestamp, link to relevant content (e.g., a new course, a scheduled session).
- Actions: Mark as Read, Mark All as Read, Archive (optional).
- Link to Notification Settings (if preferences are detailed).

### User Profile Page
**Usecase**: Allow users to view and edit their personal information.

**Description**:
- Display user's full name, email (read-only or with verification for change).
- Option to change password.
- Avatar/profile picture upload.
- Other profile fields (e.g., bio, professional title - especially for Consultants).
- "Save Changes" button.

### User Settings Page
**Usecase**: Allow users to customize their platform experience.

**Description**:
- Tabs or sections for:
  - Account: (Links back to profile for some items) Delete account option (with confirmation).
  - Preferences: Preferred language, theme (light/dark).
  - Notifications: Granular control over which email and in-app notifications to receive (toggle switches).
- "Save Changes" button for each section.

## II. Solo Learner & Enterprise Employee Pages

(Enterprise Employees will see similar pages but within the context of their company's subscription and assignments).

### Learner Dashboard
**Usecase**: Provide a personalized overview of learning activities, progress, and recommendations.

**Description**:
- Welcome Message: Personalized greeting.
- "Continue Learning" Section: Prominently displays courses in progress, with direct links to resume the last viewed lesson. Shows progress bars.
- "My Courses" / "Assigned Courses" Section: List of all enrolled/assigned courses.
- "Recommended Courses" Section: Suggestions based on interests or company recommendations.
- "Upcoming Consultations" Section: (If applicable) List of scheduled sessions.
- "Recent Achievements" Section: Display recently earned certificates or badges.
- Quick links to "Browse Courses" or "Request Consultation."

### Course Catalog Page
**Usecase**: Allow users to browse, search, and filter available courses.

**Description**:
- Search Bar: Prominent search functionality.
- Filters:
  - Categories (e.g., Software Development, Marketing, Leadership).
  - Tags/Keywords.
  - Skill Level (Beginner, Intermediate, Advanced - if applicable).
  - Language.
  - Duration (optional).
- Course Listings: Displayed as cards or a list. Each entry shows:
  - Course Thumbnail/Image.
  - Course Title.
  - Brief Description.
  - Author (if applicable).
  - Average Rating (stars).
  - Number of enrollments (optional social proof).
  - "View Details" or "Enroll" (if direct enrollment is possible for solo users not yet subscribed) button.
- Sorting Options: By relevance, popularity, newness, rating.
- Pagination for many courses.

### Course Detail Page
**Usecase**: Provide comprehensive information about a specific course and allow enrollment/access.

**Description**:
- Header: Course Title, Author, Average Rating, Number of Enrolled Students.
- Main Content Area:
  - Course Thumbnail/Preview Video.
  - Detailed Description.
  - Learning Objectives / What You'll Learn.
  - Target Audience / Prerequisites (if any).
  - Course Curriculum/Syllabus:
    - Expandable list of Modules.
    - Within each module, list of Lessons (title, type - video/text/quiz, duration).
    - Indication of completed lessons for enrolled users.
  - Instructor Bio (if applicable).
- Sidebar/Action Area:
  - "Enroll Now" / "Start Learning" / "Go to Course" button (context-dependent).
  - Price (if individually purchasable, less relevant for your subscription model).
  - Course completion time estimate.
  - Certificate availability.
  - Share course (optional).
- Reviews & Ratings Section: Display user reviews and overall rating distribution. Option for enrolled users to add/edit their review.

### Lesson Viewer Page
**Usecase**: Allow enrolled users to consume lesson content.

**Description**:
- Main Content Area (focus):
  - Video Player: For video lessons (with controls: play/pause, volume, fullscreen, speed, subtitles).
  - Text & Image Display: For text/image lessons, well-formatted and readable.
- Navigation Sidebar/Panel (Course Context):
  - Course Title.
  - List of all Modules and Lessons in the course.
  - Current lesson highlighted.
  - Checkmarks for completed lessons.
  - Easy navigation to previous/next lesson.
- Lesson Title Display.
- "Mark as Complete" / "Next Lesson" button. (Auto-advance option in settings).
- Discussion/Q&A section for the lesson (optional).
- Resources/Downloads section for the lesson (if any, though you mentioned no downloads).

### Quiz Page
**Usecase**: Allow users to take quizzes associated with a course/lesson.

**Description**:
- Quiz Title and Instructions.
- Time Limit display (if applicable).
- Question Navigation (e.g., list of question numbers, jump to question).
- Question Display Area:
  - Question number and text.
  - Answer options (radio buttons for single choice, checkboxes for multiple choice, text field for short answer).
- "Next Question" / "Previous Question" buttons.
- "Submit Quiz" button (with confirmation).
- Progress bar showing questions answered.

### Quiz Results Page
**Usecase**: Display the outcome of a quiz attempt.

**Description**:
- Clear indication of Pass/Fail.
- Score (e.g., 85% or 17/20).
- Review of answers:
  - List of questions.
  - User's answer vs. correct answer highlighted.
  - Feedback per question/answer (if provided by course creator).
- Option to "Retake Quiz" (if allowed).
- Link to "Continue Course" or back to course details.

### My Certificates Page
**Usecase**: Allow users to view and download their earned certificates.

**Description**:
- List of earned certificates. Each entry shows:
  - Course Title.
  - Date Issued.
  - Certificate UID.
  - "View Certificate" button (opens a preview).
  - "Download PDF" button.
- Visual representation of the certificate (thumbnail or actual design).

### My Subscriptions Page (Solo Learner)
**Usecase**: Allow solo learners to manage their subscription plan.

**Description**:
- Current Plan Details: Plan name, price, billing cycle (monthly/yearly), next billing date.
- Option to "Change Plan" (upgrade/downgrade, if applicable).
- Option to "Cancel Subscription" (with confirmation and info about access until end of period).
- Link to "Update Payment Method."
- Link to "Billing History."

### Billing History Page (Solo Learner & Enterprise Admin)
**Usecase**: Allow users/admins to view past payments and download invoices.

**Description**:
- List of past transactions/invoices (chronological).
- Each entry: Date, Description (e.g., "Monthly Subscription Fee"), Amount, Status (Paid, Failed).
- "Download Invoice (PDF)" button for each paid transaction.

### Consultation Request Page
**Usecase**: Allow users to request a consultation session.

**Description**:
- Form to:
  - Select Consultation Service Type (dropdown).
  - Describe their problem/topic for discussion (textarea).
  - Optionally select a preferred consultant (if feature exists).
  - Indicate preferred availability (e.g., date/time slots - could integrate with a calendar view of general availability).
- "Submit Request" button.
- Confirmation message and information on next steps.

### My Consultations Page
**Usecase**: Allow users to view their requested, scheduled, and past consultation sessions.

**Description**:
- Tabs/Sections: Upcoming, Pending Requests, Past Sessions.
- Upcoming: List of scheduled sessions with consultant name, date/time, topic, "Join Meeting" link (to Meeting Platform). Option to cancel (if allowed).
- Pending Requests: List of submitted requests with status (e.g., Pending Assignment, Awaiting Confirmation).
- Past Sessions: History of completed sessions, with date, consultant, topic. Option to provide feedback/rating.

## III. Enterprise Admin / Company Manager Pages

(Includes access to Learner pages for their own learning)

### Enterprise Admin Dashboard
**Usecase**: Provide an overview of company learning activity, subscription status, and quick actions.

**Description**:
- Key Metrics: Number of active employees, overall course completion rate, total learning hours.
- Subscription Overview: Current plan, number of used/available seats, next billing date.
- Recent Employee Activity: List of employees who recently completed courses or achieved milestones.
- Pending Approvals: (e.g., consultation requests from employees, if moderation is enabled).
- Quick links: Manage Employees, Assign Courses, View Reports, Manage Billing.

### Manage Employees Page
**Usecase**: Allow admins to add, view, and manage employee accounts within their company.

**Description**:
- List of all employees linked to the company.
- Each entry: Name, Email, Date Joined, Status (Active, Invited), Last Active (optional).
- Actions per employee: View Progress, Assign Courses, Edit Role (if company-internal roles exist), Remove from Company.
- "Invite Employee" button/section:
  - Option for individual email invites.
  - Option to generate/manage a shareable invitation link (with seat limits).
- Search and filter employees.

### Assign Courses Page
**Usecase**: Allow admins to assign specific courses to individual employees or groups.

**Description**:
- Interface to select employees (checkbox list, search).
- Interface to select courses (similar to course catalog browsing).
- "Assign Selected Courses to Selected Employees" button.
- Option to set deadlines (optional).
- Confirmation of assignment.

### Company Reports/Analytics Page
**Usecase**: Provide detailed analytics on employee learning.

**Description**:
- Overall Stats: Total learning hours, average completion rate, most popular courses within the company.
- Course-Specific Reports: For each course, see which employees are enrolled, their progress, completion status, quiz scores.
- Employee-Specific Reports: For each employee, see all enrolled courses, progress, certificates.
- Filters: By date range, department (if applicable), course.
- Data visualizations (charts, graphs).
- Option to export data (CSV, PDF).

### Company Subscription & Billing Page
**Usecase**: Allow admins to manage the company's subscription and billing details.

**Description**: (Similar to Solo Learner's "My Subscriptions" and "Billing History" but for the company)
- Current Plan Details, seat count, pricing.
- Options to "Upgrade/Downgrade Plan," "Adjust Seat Count."
- Manage company payment methods.
- View and download company invoices.

### Company Settings Page
**Usecase**: Allow admins to customize platform settings for their company.

**Description**:
- Tabs/Sections for:
  - Company Profile: Edit company name, contact info.
  - Branding: Upload company logo, set primary brand color (if feature exists).
  - Feature Toggles: Enable/disable specific platform features for their company (if applicable).
  - Certificate Customization: (If applicable) Upload custom certificate templates or add company logo.
- "Save Changes" buttons.

## IV. Course Creator Pages

### Course Creator Dashboard ("My Creations")
**Usecase**: Provide an overview of courses created by the user, their status, and quick actions.

**Description**:
- List of all courses they have created/are creating.
- Each entry: Course Title, Version, Status (Draft, Pending Review, Published, Rejected), Last Modified Date, Number of Enrolled Students (for published courses).
- Actions: Edit, View, Submit for Review, Manage Versions, View Analytics (for published courses).
- "Create New Course" button.

### Course Editor Interface (Multi-step or Tabbed)
**Usecase**: Allow course creators to build and edit all aspects of a course.

**Description**: A comprehensive interface, likely with sections/tabs:
- Course Information:
  - Title (per language).
  - Description (per language).
  - Category, Tags.
  - Target Audience, Learning Objectives.
  - Course Thumbnail upload (to Media Service).
  - Language management for translations.
  - Certificate Rules (e.g., min score).
- Curriculum Builder:
  - Interface to add, reorder, delete Modules.
  - Within each module, interface to add, reorder, delete Lessons.
  - For each lesson: define type (video, text, quiz), add title (per language).
  - Video Lesson Editor: Field to select/upload video (integrates with Media Service).
  - Text/Image Lesson Editor: Rich text editor, image upload/selection (integrates with Media Service).
  - Quiz Linker: Select/create a quiz for a quiz-type lesson.
- Quiz Manager (can be a sub-section or separate interface linked from curriculum):
  - List of quizzes associated with the course.
  - "Create New Quiz" / "Edit Quiz" button.
  - Quiz Editor:
    - Quiz Title, Description, Instructions (per language).
    - Pass Threshold, Time Limit, Max Attempts.
    - Interface to add, reorder, delete Questions.
    - Question Editor: Question Text (per language), Question Type, Points. Interface to add, edit, mark correct Answers (text per language, feedback).
- Versioning: Manage different versions of the course, view changelog, create new version from existing.
- Preview: Ability to preview the course as a learner would see it.
- "Save Draft" / "Submit for Review" buttons.

### My Media Library (Optional, if creators manage a personal media pool)
**Usecase**: Allow course creators to upload, organize, and reuse their media assets.

**Description**:
- Interface to upload videos and images (to Media Service).
- View of uploaded assets (thumbnails, file names, types).
- Options to organize into folders.
- Search/filter media.

## V. Consultant / Prestation Provider Pages

### Consultant Dashboard / My Schedule
**Usecase**: Provide an overview of upcoming sessions, pending requests, and quick access to availability.

**Description**:
- Calendar View: Displaying scheduled consultations.
- "Upcoming Sessions" List: Detailed list for today/this week.
- "Pending Requests" List: Requests assigned to them requiring acceptance/scheduling.
- Quick link to "Manage Availability."
- Statistics (e.g., completed sessions this month, average rating - optional).

### Manage Availability Page
**Usecase**: Allow consultants to define when they are available for consultations.

**Description**:
- Calendar interface.
- Ability to select dates/times and mark them as "Available" or "Unavailable."
- Option for recurring availability (e.g., "Available every Monday 9-5").
- "Save Availability" button.

### Consultation Requests Page
**Usecase**: Allow consultants to view and manage consultation requests assigned to them.

**Description**:
- List of pending requests.
- Each request: Requester Name, Service Type, Request Details, Preferred Times (if any).
- Actions: "Accept & Schedule" (leads to scheduling interface, possibly suggesting times based on mutual availability), "Decline" (with reason).

### Session Details Page (Pre-Session)
**Usecase**: View details of a specific upcoming scheduled session.

**Description**:
- Requester Name & Contact (if appropriate).
- Service Type.
- Scheduled Date & Time.
- Request Details/Topic.
- "Join Meeting" link (to Meeting Platform).
- Option to cancel/reschedule (if permitted by rules).

## VI. Platform Administrator Pages (Admin Panel)

(This is a comprehensive backend interface, often less stylized and more functional).

### Admin Dashboard
**Usecase**: Overview of platform health, key metrics, and quick admin actions.

**Description**:
- Statistics: Total Users, Active Users, New Registrations, Total Courses, Revenue, Active Subscriptions.
- System Health Indicators (e.g., service status, error rates - if monitored).
- Moderation Queues (e.g., pending course reviews, flagged content).
- Quick links to major admin sections.

### User Management Page
**Usecase**: Manage all user accounts on the platform.

**Description**:
- Searchable, filterable list of all users.
- Columns: User ID, Name, Email, Role(s), Company (if applicable), Registration Date, Status (Active, Suspended).
- Actions per user: View Details, Edit Profile, Change Roles, Suspend/Unsuspend, Delete, Impersonate (for support, with audit trail).

### Company Management Page
**Usecase**: Manage all enterprise accounts.

**Description**:
- Searchable, filterable list of all companies.
- Columns: Company ID, Name, Primary Contact, Subscription Plan, Number of Employees, Creation Date.
- Actions: View Details, Edit Company Info, Manage Subscription, View Employees.

### Course Management Page
**Usecase**: Oversee all courses, manage publication status, categories, etc.

**Description**:
- Searchable, filterable list of all courses (and their versions).
- Columns: Course Title, Creator, Status (Draft, Pending, Published, Archived), Creation Date.
- Actions: View Course, Edit (as admin), Approve/Reject Submission, Publish/Unpublish, Feature Course.
- Management of global Categories and Tags.

### Subscription & Billing Management Page
**Usecase**: Manage subscription plans, view all subscriptions, handle billing issues.

**Description**:
- Manage Subscription Plans (create, edit, deactivate).
- View all active user and company subscriptions.
- Search/filter subscriptions.
- View payment transaction logs.
- Process refunds or manual billing adjustments (with strong audit trail).
- Manage Promo Codes.

### Consultation Service Management Page
**Usecase**: Manage types of consultation services offered and oversee consultant assignments.

**Description**:
- Manage Consultation Service Types (create, edit, pricing if centralized here).
- View all active consultants.
- Oversee pending and scheduled consultation sessions globally.
- Manual assignment tools if needed.

### Notification Management Page
**Usecase**: Manage notification templates and view sending logs.

**Description**:
- Manage Notification Types and Templates (create, edit, activate/deactivate).
- View logs of sent notifications and delivery statuses.
- Trigger test notifications.

### Platform Settings Page
**Usecase**: Configure global platform parameters.

**Description**:
- Interface to edit global settings (default language, maintenance mode, API keys for external services like payment gateway, email provider).
- Manage Terms of Service / Privacy Policy content or links.
- Manage Feature Flags.

### Content Moderation Page (if applicable)
**Usecase**: Review and moderate user-generated content (e.g., course reviews, discussion forums).

**Description**:
- Queues for pending reviews, flagged items.
- Tools to approve, reject, edit, or delete content.