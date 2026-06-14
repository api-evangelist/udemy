# Udemy GraphQL Schema

## Overview

This conceptual GraphQL schema models the Udemy online learning platform, covering the full lifecycle of courses, instructors, students, enrollment, learning progress, purchases, and enterprise learning management as exposed through the Udemy Affiliate API, Instructor API, and Udemy Business API.

Udemy does not currently publish a public GraphQL endpoint; this schema is a conceptual representation derived from the Udemy REST API surface (Affiliate, Instructor, and Business APIs) and the published data models at https://www.udemy.com/developers/.

## Schema Source

- REST API Reference: https://www.udemy.com/developers/
- Affiliate API Docs: https://www.udemy.com/developers/affiliate/
- Instructor API Docs: https://www.udemy.com/developers/instructor/
- Business API Reference: https://s3.amazonaws.com/udemy-images/support/Udemy_for_Business_API_Reference_v2.0.pdf
- GitHub Organization: https://github.com/udemy

## Types

### Course Domain

- **Course** — Top-level course entity with title, description, URL, price, rating, and category references.
- **CourseDetails** — Extended course metadata including headline, objectives, requirements, targets, and curriculum summary.
- **CourseImage** — Image asset variants for a course (75x75, 240x135, 480x270).
- **CourseRating** — Aggregate rating data including average score, count of reviews, and distribution breakdown.
- **CourseObjective** — A single learning objective statement for a course.
- **CourseRequirement** — A prerequisite or requirement listed for a course.
- **CourseTarget** — A target audience description for a course.
- **CourseBadge** — A badge applied to a course (e.g., Bestseller, Hot & New).
- **CourseLabel** — Internal or editorial label associated with a course.
- **CourseCategory** — Top-level category grouping courses (e.g., Development, Business).
- **CourseSubcategory** — Subcategory nested within a category (e.g., Web Development under Development).

### Instructor Domain

- **Instructor** — Instructor account with name, title, URL, and ratings.
- **InstructorBio** — Extended biography content for an instructor.
- **InstructorRating** — Aggregate instructor rating including average score and total number of students.

### Curriculum Domain

- **Curriculum** — Full curriculum for a course listing sections in order.
- **CurriculumSection** — A titled section grouping related lectures within a curriculum.
- **CurriculumLecture** — A lecture item within a curriculum section.
- **Lecture** — A single lecture with title, type, duration, and preview availability.
- **LectureType** — Enum of lecture content types: VIDEO, ARTICLE, PRACTICE_TEST, ASSIGNMENT, SUPPLEMENTARY.
- **VideoLecture** — Lecture subtype with video asset references and captions.
- **ArticleLecture** — Lecture subtype with rich-text article body content.
- **PracticeTest** — Quiz or practice exam attached to a course section.
- **AssignmentLecture** — Graded assignment lecture with submission instructions.
- **SupplementaryMaterial** — Downloadable file or external link attached to a lecture.

### Student Engagement Domain

- **StudentQuestion** — A Q&A question posted by a student in a course discussion.
- **StudentAnswer** — An answer to a student question, from another student or the instructor.
- **StudentFeedback** — A written feedback item on a course section or lecture.
- **CourseReview** — A student's numeric rating and written review of a course.

### Enrollment and Progress Domain

- **EnrolledCourse** — A course a student is currently enrolled in.
- **EnrollmentRecord** — Record of when a student enrolled in a course and the enrollment source.
- **LearningProgress** — Aggregate progress for a student across an enrolled course.
- **LectureProgress** — Progress record for a single lecture (watched, completed, position).
- **QuizProgress** — Progress and score record for a practice test or quiz.

### Certification Domain

- **Certificate** — A course completion certificate definition.
- **CertificateRecord** — A student's earned certificate for a completed course.

### Pricing and Commerce Domain

- **Coupon** — A discount coupon with code, percentage, and expiry.
- **PricingPlan** — A pricing plan (free, paid, subscription) available for a course.
- **CoursePrice** — The current retail and sale price for a course in a given currency.
- **Discount** — An active promotional discount applied to a course or order.
- **Sale** — A time-bounded sale event applying discounts across multiple courses.
- **Wishlist** — A student's saved course wishlist.
- **WishlistItem** — A single course entry on a student's wishlist.
- **ShoppingCart** — A student's active shopping cart.
- **Purchase** — A completed purchase transaction.
- **Order** — An order record grouping one or more purchased courses.
- **Refund** — A refund request or completed refund for a purchase.

### Student and Account Domain

- **Student** — A student account with profile references and enrollment summary.
- **StudentProfile** — Detailed student profile including headline, bio, and learning goals.

### API and Platform Domain

- **APIKey** — An API client credential (client ID and secret) for REST API access.
- **Token** — A bearer token issued for authenticated API sessions.
- **Webhook** — A registered webhook endpoint for receiving Udemy event notifications.

### Business / Enterprise Domain

- **BusinessAccount** — An enterprise Udemy Business account.
- **BusinessUser** — A user within a Udemy Business account with role and seat assignment.
- **BusinessGroup** — A named group of users within a Udemy Business account.
- **CourseAssignment** — An assignment of a course to a user or group in Udemy Business.
- **LearningActivity** — A log entry of learning activity for a business user.
- **CompletionReport** — An aggregate report of course completions for a business account.

## Relationships

- Course has one CourseDetails, one CourseRating, many CourseObjectives, CourseRequirements, CourseTargets, CourseBadges, and CourseLabels.
- Course belongs to one CourseCategory and one CourseSubcategory.
- Course has one Curriculum composed of CurriculumSections, each containing CurriculumLectures.
- Each CurriculumLecture resolves to one of: VideoLecture, ArticleLecture, PracticeTest, or AssignmentLecture.
- Instructor has one InstructorBio and one InstructorRating; teaches many Courses.
- Student has one StudentProfile, many EnrolledCourses, one Wishlist, one ShoppingCart, and many Orders.
- EnrolledCourse links a Student to a Course and has one LearningProgress, many LectureProgress and QuizProgress records.
- Completed EnrolledCourse may produce a CertificateRecord.
- Order contains many Purchases; each Purchase may produce a Refund.
- CoursePrice references PricingPlan and may carry an active Discount or Coupon.
- BusinessAccount has many BusinessUsers and BusinessGroups; groups receive CourseAssignments.
