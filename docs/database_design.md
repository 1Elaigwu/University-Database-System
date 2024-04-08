# Database Schema

  The University Database System manages students, professors, courses, enrollments, grades, course feedback, teaching assignments, prerequisites, and course materials.

### Key Features:

- Students can enroll in courses and provide feedback.
- Professors teach courses and have associated office locations.
- Courses have prerequisites, credits, and associated teaching details.

## Entity-Relationship Diagram (ERD)
The database design on the University Database System includes several entities and their relationships. The ERD visually represents these entities and their associations.
![UDS](https://github.com/1Elaigwu/University-Database-System/assets/85877218/f2296aad-28c9-41fc-afaa-1a9bdddd81fc)

## Tables

Entities:

**Student**:

Attributes:
- StudentID (Primary Key)
- Name
- Email
- PhoneNumber
- Address
- DateOfBirth

**Professor**:

- Attributes:
- ProfessorID (Primary Key)
- Name
- Email
- PhoneNumber
- OfficeLocation

**Course**:

Attributes:
- CourseID (Primary Key)
- CourseName
- CourseCode
- Credits

**Enrollment**:

Attributes:
- EnrollmentID (Primary Key)
- StudentID (Foreign Key referencing Student.StudentID)
- CourseID (Foreign Key referencing Course.CourseID)
- EnrollmentDate

**Teaching**:

Attributes:
- TeachingID (Primary Key)
- ProfessorID (Foreign Key referencing Professor.ProfessorID)
- CourseID (Foreign Key referencing Course.CourseID)
- Semester
- Year

**Prerequisites**:

Attributes:
- CourseID (Foreign Key referencing Course.CourseID)
- PrerequisiteCourseID (Foreign Key referencing Course.CourseID)

**Grades**:

Attributes:
- EnrollmentID (Primary Key, Foreign Key referencing Enrollment.EnrollmentID)
- StudentID (Foreign Key referencing Student.StudentID)
- Grade

**CourseFeedback**:

Attributes:
- FeedbackID (Primary Key)
- StudentID (Foreign Key referencing Student.StudentID)
- CourseID (Foreign Key referencing Course.CourseID)
- Feedback

**CourseMaterials**:

Attributes:
- MaterialID (Primary Key)
- CourseID (Foreign Key referencing Course.CourseID)
- MaterialType
- Title
- Description
- MaterialLink

## Relationships:

One-to-Many:

- Student to Enrollment (One student can enroll in many courses)
- Professor to Teaching (One professor can teach multiple courses)
- Course to Enrollment (One course can have multiple enrollments)
- Course to Prerequisites (One course can have multiple prerequisites)

Many-to-Many:
- Student to Course (Through Enrollment)
- Course to Professor (Through Teaching)

## Conclusion:
The database design supports the university's academic operations by facilitating course enrollment, teaching assignments, grading, and student feedback. The schema ensures data 
integrity and efficient retrieval for administrative and academic purposes.
