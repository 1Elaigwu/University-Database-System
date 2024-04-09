# University Database System Usage Guide
This guide provides a comprehensive overview of utilizing the University Database System, including schema setup, data insertion, usage of views, functions, and stored procedures to 
achieve project goals and objectives.

## Schema Setup
**Database Creation**: Create a MySQL database named Universitydatabasesystem.
```sql
CREATE DATABASE Universitydatabasesystem;
```
**Table Creation**: Set up tables to store student, professor, course, enrollment, teaching, prerequisites, grades, course feedback, and other relevant information.

```sql
-- Table `Universitydatabasesystem`.`Student'
CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Student` (
  `StudentID` INT NOT NULL,
  `Name` VARCHAR(100) NULL,
  `Email` VARCHAR(100) NULL,
  `PhoneNumber` VARCHAR(20) NULL,
  `Address` VARCHAR(255) NULL,
  `DateOfBirth` DATE NULL,
  PRIMARY KEY (`StudentID`)
) ENGINE = InnoDB;

-- Table `Universitydatabasesystem`.`Professor`
CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Professor` (
  `ProfessorID` INT NOT NULL,
  `Name` VARCHAR(100) NULL,
  `Email` VARCHAR(100) NULL,
  `PhoneNumber` VARCHAR(20) NULL,
  `OfficeLocation` VARCHAR(255) NULL,
  PRIMARY KEY (`ProfessorID`)
) ENGINE = InnoDB;

-- Table `Universitydatabasesystem`.`Course`
CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Course` (
  `CourseID` INT NOT NULL,
  `CourseName` VARCHAR(100) NULL,
  `CourseCode` VARCHAR(20) NULL,
  `Credits` INT NULL,
  PRIMARY KEY (`CourseID`)
) ENGINE = InnoDB;

-- Table `Universitydatabasesystem`.`Enrollment`
CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Enrollment` (
  `EnrollmentID` INT NOT NULL,
  `StudentID` INT NOT NULL,
  `CourseID` INT NOT NULL,
  `EnrollmentDate` DATE NULL,
  PRIMARY KEY (`EnrollmentID`),
  CONSTRAINT `FK_Enrollment_Student` FOREIGN KEY (`StudentID`) REFERENCES `Universitydatabasesystem`.`Student` (`StudentID`) ON DELETE NO ACTION ON UPDATE NO ACTION,
  CONSTRAINT `FK_Enrollment_Course` FOREIGN KEY (`CourseID`) REFERENCES `Universitydatabasesystem`.`Course` (`CourseID`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE = InnoDB;

-- Table `Universitydatabasesystem`.`Teaching`
CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Teaching` (
  `TeachingID` INT NOT NULL,
  `ProfessorID` INT NOT NULL,
  `CourseID` INT NOT NULL,
  `Semester` VARCHAR(20) NULL,
  `Year` INT NULL,
  PRIMARY KEY (`TeachingID`),
  CONSTRAINT `FK_Teaching_Professor` FOREIGN KEY (`ProfessorID`) REFERENCES `Universitydatabasesystem`.`Professor` (`ProfessorID`) ON DELETE NO ACTION ON UPDATE NO ACTION,
  CONSTRAINT `FK_Teaching_Course` FOREIGN KEY (`CourseID`) REFERENCES `Universitydatabasesystem`.`Course` (`CourseID`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Prerequisites` (
  `CourseID` INT NOT NULL,
  `PrerequisiteCourseID` INT NOT NULL,
  PRIMARY KEY (`CourseID`, `PrerequisiteCourseID`),
  CONSTRAINT `FK_Prerequisites_Course` FOREIGN KEY (`CourseID`) REFERENCES `Universitydatabasesystem`.`Course` (`CourseID`) ON DELETE CASCADE ON UPDATE NO ACTION,
  CONSTRAINT `FK_Prerequisites_PrerequisiteCourse` FOREIGN KEY (`PrerequisiteCourseID`) REFERENCES `Universitydatabasesystem`.`Course` (`CourseID`) ON DELETE CASCADE ON UPDATE NO ACTION
) ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`Grades` (
  `EnrollmentID` INT NOT NULL,
  `StudentID` INT NOT NULL,
  `Grade` VARCHAR(2) NULL,
  PRIMARY KEY (`EnrollmentID`),
  CONSTRAINT `FK_Grades_Enrollment` FOREIGN KEY (`EnrollmentID`) REFERENCES `Universitydatabasesystem`.`Enrollment` (`EnrollmentID`) ON DELETE CASCADE ON UPDATE NO ACTION,
  CONSTRAINT `FK_Grades_Student` FOREIGN KEY (`StudentID`) REFERENCES `Universitydatabasesystem`.`Student` (`StudentID`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE = InnoDB;

-- Table `Universitydatabasesystem`.`CourseFeedback`
CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`CourseFeedback` (
  `FeedbackID` INT AUTO_INCREMENT PRIMARY KEY,
  `StudentID` INT NOT NULL,
  `CourseID` INT NOT NULL,
  `Feedback` TEXT,
  CONSTRAINT `FK_CourseFeedback_Student` FOREIGN KEY (`StudentID`) REFERENCES `Universitydatabasesystem`.`Student` (`StudentID`) ON DELETE NO ACTION ON UPDATE NO ACTION,
  CONSTRAINT `FK_CourseFeedback_Course` FOREIGN KEY (`CourseID`) REFERENCES `Universitydatabasesystem`.`Course` (`CourseID`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `Universitydatabasesystem`.`CourseMaterials` (
  `MaterialID` INT AUTO_INCREMENT PRIMARY KEY,
  `CourseID` INT,
  `MaterialType` VARCHAR(50),
  `Title` VARCHAR(255),
  `Description` TEXT,
  `MaterialLink` VARCHAR(255),
  CONSTRAINT `FK_CourseMaterials_Course` FOREIGN KEY (`CourseID`) REFERENCES `Universitydatabasesystem`.`Course` (`CourseID`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE = InnoDB;
```
## Data Insertion
Insert data into tables to populate the database with initial records.

```sql
-- Inserting sample student data (26 rows)
INSERT INTO Student (StudentID, Name, Email, PhoneNumber, Address, DateOfBirth)
VALUES 
    (1, 'John Doe', 'johndoe@example.com', '123-456-7890', '123 Main St, Anytown, USA', '1995-08-15'),
    (2, 'Jane Smith', 'janesmith@example.com', '987-654-3210', '456 Elm St, Anytown, USA', '1998-03-22'),
    (3, 'Michael Johnson', 'michaelj@example.com', '555-123-4567', '789 Oak St, Anytown, USA', '1997-11-10'),
    (4, 'Emily Brown', 'emilyb@example.com', '333-555-7777', '567 Pine St, Anytown, USA', '1996-05-27'),
    (5, 'David Lee', 'davidlee@example.com', '222-888-9999', '890 Maple Ave, Anytown, USA', '1999-02-14'),
    (6, 'Sarah Taylor', 'sarahtaylor@example.com', '444-777-2222', '321 Birch Rd, Anytown, USA', '1998-09-30'),
    (7, 'Alex Johnson', 'alexj@example.com', '555-777-8888', '456 Cedar St, Anytown, USA', '1997-07-12'),
    (8, 'Jessica White', 'jessicaw@example.com', '111-222-3333', '789 Pine St, Anytown, USA', '1999-01-05'),
    (9, 'Matthew Clark', 'mattc@example.com', '333-999-4444', '890 Oak Ave, Anytown, USA', '1996-10-20'),
    (10, 'Sophia Martinez', 'sophiam@example.com', '777-888-5555', '234 Elm St, Anytown, USA', '1997-04-18'),
    (11, 'Daniel Garcia', 'danielg@example.com', '555-444-3333', '678 Maple Rd, Anytown, USA', '1998-12-03'),
    (12, 'Olivia Rodriguez', 'oliviar@example.com', '999-888-7777', '345 Cedar Ave, Anytown, USA', '1995-06-28'),
    (13, 'Ethan Wilson', 'ethanw@example.com', '666-333-1111', '432 Birch St, Anytown, USA', '1996-09-15'),
    (14, 'Ava Moore', 'avam@example.com', '222-444-6666', '567 Pine Ave, Anytown, USA', '1999-03-25'),
    (15, 'Noah Lopez', 'noahl@example.com', '888-777-9999', '789 Oak Rd, Anytown, USA', '1997-11-30'),
    (16, 'Mia Hill', 'miah@example.com', '333-555-7777', '890 Elm Ave, Anytown, USA', '1998-08-08'),
    (17, 'William Scott', 'williams@example.com', '111-777-8888', '123 Maple St, Anytown, USA', '1996-02-12'),
    (18, 'Isabella Green', 'isabellag@example.com', '777-333-1111', '456 Cedar Rd, Anytown, USA', '1999-07-04'),
    (19, 'James Baker', 'jamesb@example.com', '444-888-2222', '678 Pine Ave, Anytown, USA', '1995-10-10'),
    (20, 'Sophie King', 'sophiek@example.com', '666-444-2222', '345 Elm St, Anytown, USA', '1996-04-22'),
    (21, 'Logan Young', 'logany@example.com', '222-999-8888', '432 Oak Rd, Anytown, USA', '1998-01-15'),
    (22, 'Chloe Adams', 'chloea@example.com', '999-111-7777', '567 Birch Ave, Anytown, USA', '1997-12-20'),
    (23, 'Ryan Wright', 'ryanw@example.com', '888-222-1111', '789 Cedar St, Anytown, USA', '1999-05-07'),
    (24, 'Ella Collins', 'ellac@example.com', '111-888-4444', '890 Pine Rd, Anytown, USA', '1995-08-29'),
    (25, 'Luke Mitchell', 'lukem@example.com', '555-666-2222', '234 Maple Ave, Anytown, USA', '1996-03-17'),
    (26, 'Grace Walker', 'gracew@example.com', '777-222-9999', '678 Elm Rd, Anytown, USA', '1998-07-01');

-- Inserting sample professor data (26 rows)
INSERT INTO Professor (ProfessorID, Name, Email, PhoneNumber, OfficeLocation)
VALUES 
    (101, 'Dr. Rachel Adams', 'radams@example.com', '555-111-2222', 'Science Building, Room 301'),
    (102, 'Prof. Mark Roberts', 'mroberts@example.com', '555-333-4444', 'Arts Building, Room 201'),
    (103, 'Dr. Lisa Anderson', 'landerson@example.com', '555-555-6666', 'Engineering Building, Room 101'),
    (104, 'Prof. David Wilson', 'dwilson@example.com', '555-777-8888', 'Science Building, Room 201'),
    (105, 'Dr. Emily Moore', 'emoore@example.com', '555-999-1111', 'Arts Building, Room 301'),
    (106, 'Prof. Michael Taylor', 'mtaylor@example.com', '555-222-3333', 'Engineering Building, Room 201'),
    (107, 'Dr. Jessica Baker', 'jbaker@example.com', '555-444-5555', 'Science Building, Room 101'),
    (108, 'Prof. Daniel Johnson', 'djohnson@example.com', '555-666-7777', 'Arts Building, Room 202'),
    (109, 'Dr. Sophia Lee', 'slee@example.com', '555-888-9999', 'Engineering Building, Room 301'),
  (110, 'Prof. William Clark', 'wclark@example.com', '555-123-4567', 'Science Building, Room 202'),
	(111, 'Dr. Olivia Harris', 'oharris@example.com', '555-234-5678', 'Arts Building, Room 101'),
	(112, 'Prof. Ethan Allen', 'eallen@example.com', '555-345-6789', 'Engineering Building, Room 202'),
	(113, 'Dr. Grace Turner', 'gturner@example.com', '555-456-7890', 'Science Building, Room 301'),
	(114, 'Prof. Lucas Parker', 'lparker@example.com', '555-567-8901', 'Arts Building, Room 201'),
	(115, 'Dr. Lily Young', 'lyoung@example.com', '555-678-9012', 'Engineering Building, Room 101'),
	(116, 'Prof. Samuel Martin', 'smartin@example.com', '555-789-0123', 'Science Building, Room 202'),
	(117, 'Dr. Bella Evans', 'bevans@example.com', '555-890-1234', 'Arts Building, Room 301'),
	(118, 'Prof. Jacob White', 'jwhite@example.com', '555-901-2345', 'Engineering Building, Room 101'),
	(119, 'Dr. Hailey Cooper', 'hcooper@example.com', '555-012-3456', 'Science Building, Room 201'),
	(120, 'Prof. Noah King', 'nking@example.com', '555-123-4567', 'Arts Building, Room 302'),
	(121, 'Dr. Zoe Morgan', 'zmorgan@example.com', '555-234-5678', 'Engineering Building, Room 102'),
	(122, 'Prof. Benjamin Hill', 'bhill@example.com', '555-345-6789', 'Science Building, Room 301'),
	(123, 'Dr. Emma Wright', 'ewright@example.com', '555-456-7890', 'Arts Building, Room 202'),
	(124, 'Prof. Nathan Collins', 'ncollins@example.com', '555-567-8901', 'Engineering Building, Room 201'),
	(125, 'Dr. Sarah Adams', 'sadams@example.com', '555-678-9012', 'Science Building, Room 101'),
	(126, 'Prof. Isaac Parker', 'iparker@example.com', '555-789-0123', 'Arts Building, Room 301');

-- Inserting sample course data (26 rows)
INSERT INTO Course (CourseID, CourseName, CourseCode, Credits)
VALUES 
    (1001, 'Introduction to Computer Science', 'CS101', 3),
    (1002, 'Art History', 'AH201', 4),
    (1003, 'Statistics', 'STAT301', 3),
    (1004, 'Chemistry', 'CHEM101', 4),
    (1005, 'English Composition', 'ENG102', 3),
    (1006, 'Algebra', 'MATH201', 3),
    (1007, 'Biology', 'BIO101', 4),
    (1008, 'Psychology', 'PSYCH202', 3),
    (1009, 'Economics', 'ECON301', 3),
    (1010, 'Physics', 'PHY101', 4),
    (1011, 'Sociology', 'SOC201', 3),
    (1012, 'History', 'HIST101', 3),
    (1013, 'Anthropology', 'ANTH201', 4),
    (1014, 'Political Science', 'POL101', 3),
    (1015, 'Music Theory', 'MUS101', 3),
    (1016, 'Engineering Mechanics', 'ENGM201', 4),
    (1017, 'Philosophy', 'PHIL101', 3),
    (1018, 'Marketing', 'MKTG301', 3),
    (1019, 'Operations Management', 'OPSMGT301', 3),
    (1020, 'Biochemistry', 'BIOCHEM101', 4),
    (1021, 'Literature', 'LIT101', 3),
    (1022, 'Geology', 'GEOL101', 4),
    (1023, 'Drama', 'DRAM101', 3),
    (1024, 'Computer Networks', 'NET101', 4),
    (1025, 'Public Speaking', 'SPEECH101', 3),
    (1026, 'Graphic Design', 'GD101', 3);

-- Enroll students in courses (26 rows)
INSERT INTO Enrollment (EnrollmentID, StudentID, CourseID, EnrollmentDate)
VALUES 
    (1, 1, 1001, '2024-01-15'),
    (2, 1, 1002, '2024-01-15'),
    (3, 2, 1001, '2024-01-15'),
    (4, 2, 1003, '2024-01-15'),
    (5, 3, 1004, '2024-01-15'),
    (6, 3, 1005, '2024-01-15'),
    (7, 4, 1006, '2024-01-15'),
    (8, 4, 1007, '2024-01-15'),
    (9, 5, 1008, '2024-01-15'),
    (10, 5, 1009, '2024-01-15'),
    (11, 6, 1001, '2024-01-15'),
    (12, 6, 1002, '2024-01-15'),
    (13, 7, 1003, '2024-01-15'),
    (14, 7, 1004, '2024-01-15'),
    (15, 8, 1005, '2024-01-15'),
    (16, 8, 1006, '2024-01-15'),
    (17, 9, 1007, '2024-01-15'),
    (18, 9, 1008, '2024-01-15'),
    (19, 10, 1009, '2024-01-15'),
    (20, 10, 1010, '2024-01-15'),
    (21, 11, 1011, '2024-01-15'),
    (22, 11, 1012, '2024-01-15'),
    (23, 12, 1013, '2024-01-15'),
    (24, 12, 1014, '2024-01-15'),
    (25, 13, 1015, '2024-01-15'),
    (26, 13, 1016, '2024-01-15');

-- Assign professors to teach courses (26 rows)
INSERT INTO Teaching (TeachingID, ProfessorID, CourseID, Semester, Year)
VALUES 
    (1, 101, 1001, 'Spring', 2024),
    (2, 102, 1002, 'Spring', 2024),
    (3, 103, 1003, 'Spring', 2024),
    (4, 104, 1004, 'Spring', 2024),
    (5, 105, 1005, 'Spring', 2024),
    (6, 106, 1006, 'Spring', 2024),
    (7, 107, 1007, 'Spring', 2024),
    (8, 108, 1008, 'Spring', 2024),
    (9, 109, 1009, 'Spring', 2024),
    (10, 110, 1010, 'Spring', 2024),
    (11, 111, 1011, 'Spring', 2024),
    (12, 112, 1012, 'Spring', 2024),
    (13, 113, 1013, 'Spring', 2024),
    (14, 114, 1014, 'Spring', 2024),
    (15, 115, 1015, 'Spring', 2024),
    (16, 116, 1016, 'Spring', 2024),
    (17, 117, 1017, 'Spring', 2024),
    (18, 118, 1018, 'Spring', 2024),
    (19, 119, 1019, 'Spring', 2024),
    (20, 120, 1020, 'Spring', 2024),
    (21, 121, 1021, 'Spring', 2024),
    (22, 122, 1022, 'Spring', 2024),
    (23, 123, 1023, 'Spring', 2024),
    (24, 124, 1024, 'Spring', 2024),
    (25, 125, 1025, 'Spring', 2024),
    (26, 126, 1026, 'Spring', 2024);

    -- Inserting sample prerequisites data (26 rows)
INSERT INTO Prerequisites (CourseID, PrerequisiteCourseID)
VALUES
    (1003, 1001),  -- Statistics requires Introduction to Computer Science
    (1004, 1002),  -- Chemistry requires Art History
    (1005, 1001),  -- English Composition requires Introduction to Computer Science
    (1006, 1001),  -- Algebra requires Introduction to Computer Science
    (1007, 1004),  -- Biology requires Chemistry
    (1008, 1005),  -- Psychology requires English Composition
    (1009, 1008),  -- Economics requires Psychology
    (1010, 1009),  -- Physics requires Economics
    (1011, 1005),  -- Sociology requires English Composition
    (1012, 1006),  -- History requires Algebra
    (1013, 1007),  -- Anthropology requires Biology
    (1014, 1011),  -- Political Science requires Sociology
    (1015, 1002),  -- Music Theory requires Art History
    (1016, 1006),  -- Engineering Mechanics requires Algebra
    (1017, 1014),  -- Philosophy requires Political Science
    (1018, 1008),  -- Marketing requires Psychology
    (1019, 1010),  -- Operations Management requires Physics
    (1020, 1007),  -- Biochemistry requires Biology
    (1021, 1014),  -- Literature requires Political Science
    (1022, 1004),  -- Geology requires Chemistry
    (1023, 1015),  -- Drama requires Music Theory
    (1024, 1001),  -- Computer Networks requires Introduction to Computer Science
    (1025, 1005),  -- Public Speaking requires English Composition
    (1026, 1018),  -- Graphic Design requires Marketing
    (1026, 1023);  -- Graphic Design requires Drama
    
-- Inserting sample grades data (26 rows)
INSERT INTO Grades (EnrollmentID, StudentID, Grade)
VALUES
    (1, 1, 'A'),     -- John Doe's grade for Introduction to Computer Science
    (2, 1, 'B+'),    -- John Doe's grade for Art History
    (3, 2, 'A-'),    -- Jane Smith's grade for Introduction to Computer Science
    (4, 2, 'B'),     -- Jane Smith's grade for Statistics
    (5, 3, 'B+'),    -- Michael Johnson's grade for Chemistry
    (6, 3, 'A-'),    -- Michael Johnson's grade for English Composition
    (7, 4, 'A'),     -- Emily Brown's grade for Algebra
    (8, 4, 'B'),     -- Emily Brown's grade for Biology
    (9, 5, 'A-'),    -- David Lee's grade for Psychology
    (10, 5, 'A'),    -- David Lee's grade for Economics
    (11, 6, 'B'),    -- Sarah Taylor's grade for Introduction to Computer Science
    (12, 6, 'B+'),   -- Sarah Taylor's grade for Art History
    (13, 7, 'A-'),   -- Alex Johnson's grade for Statistics
    (14, 7, 'A'),    -- Alex Johnson's grade for Chemistry
    (15, 8, 'A'),    -- Jessica White's grade for English Composition
    (16, 8, 'B+'),   -- Jessica White's grade for Algebra
    (17, 9, 'B+'),   -- Matthew Clark's grade for Biology
    (18, 9, 'A-'),   -- Matthew Clark's grade for Psychology
    (19, 10, 'A'),   -- Sophia Martinez's grade for Economics
    (20, 10, 'A-'),  -- Sophia Martinez's grade for Physics
    (21, 11, 'B'),   -- Daniel Garcia's grade for Sociology
    (22, 11, 'B+'),  -- Daniel Garcia's grade for History
    (23, 12, 'A-'),  -- Olivia Rodriguez's grade for Anthropology
    (24, 12, 'A'),   -- Olivia Rodriguez's grade for Political Science
    (25, 13, 'B+'),  -- Ethan Wilson's grade for Music Theory
    (26, 13, 'A-');  -- Ethan Wilson's grade for Engineering Mechanics

    -- Inserting sample feedback for each course and student
-- Feedback for CourseID 1001 (Introduction to Computer Science)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (6, 1001, 'Great course content and practical exercises.'),
    (11, 1001, 'Introduction to programming was very informative.'),
    (14, 1001, 'Computer Science opened new career opportunities.'),
    (19, 1001, 'Helped me understand the basics of computer science.'),
    (25, 1001, 'Lectures were engaging and informative.');

-- Feedback for CourseID 1002 (Art History)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (1, 1002, 'Art History lectures were fascinating.'),
    (2, 1002, 'Enjoyed exploring different art movements.'),
    (11, 1002, 'Art History broadened my perspective.'),
    (14, 1002, 'Loved learning about famous artists and their works.'),
    (19, 1002, 'Helped me appreciate art more.'),
    (25, 1002, 'Art History assignments were enjoyable.');

-- Feedback for CourseID 1003 (Statistics)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (2, 1003, 'Statistics was a challenging but insightful course.'),
    (7, 1003, 'Enjoyed learning about data analysis and probability.'),
    (13, 1003, 'Statistics improved my analytical skills.'),
    (18, 1003, 'Helped me understand statistical concepts.'),
    (21, 1003, 'Statistics lectures were clear and well-explained.'),
    (24, 1003, 'The course enhanced my problem-solving abilities.');

-- Feedback for CourseID 1004 (Chemistry)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (3, 1004, 'Chemistry labs were engaging and informative.'),
    (7, 1004, 'Enjoyed conducting experiments in Chemistry.'),
    (12, 1004, 'Chemistry lectures were well-organized.'),
    (16, 1004, 'The course improved my understanding of chemical reactions.'),
    (22, 1004, 'Chemistry assignments were challenging yet rewarding.'),
    (26, 1004, 'Chemistry practicals were my favorite part of the course.');

-- Feedback for CourseID 1005 (English Composition)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (3, 1005, 'English Composition improved my writing skills.'),
    (8, 1005, 'Enjoyed analyzing literature in English Composition.'),
    (13, 1005, 'The course helped me express my ideas more clearly.'),
    (17, 1005, 'English Composition was insightful and thought-provoking.'),
    (20, 1005, 'Learned new vocabulary and writing techniques in this course.'),
    (25, 1005, 'English Composition assignments were engaging and diverse.');

-- Feedback for CourseID 1010 (Physics)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (10, 1010, 'Physics course was challenging but fascinating.'),
    (15, 1010, 'Enjoyed learning about laws of motion and thermodynamics.'),
    (19, 1010, 'Physics lectures were engaging and informative.'),
    (21, 1010, 'The course enhanced my problem-solving skills.'),
    (24, 1010, 'Physics assignments were thought-provoking.'),
    (26, 1010, 'Developed a deeper interest in Physics after taking this course.');

-- Feedback for CourseID 1011 (Sociology)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (11, 1011, 'Sociology course provided a new perspective on society.'),
    (14, 1011, 'Enjoyed analyzing societal structures and cultural phenomena.'),
    (18, 1011, 'Sociology lectures were thought-provoking and insightful.'),
    (22, 1011, 'The course broadened my understanding of human behavior.'),
    (25, 1011, 'Sociology assignments were engaging and relevant.'),
    (26, 1011, 'Developed critical thinking skills through Sociology.');

-- Feedback for CourseID 1020 (Biochemistry)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (3, 1020, 'Biochemistry labs were interesting and hands-on.'),
    (7, 1020, 'Enjoyed learning about biochemical processes and reactions.'),
    (12, 1020, 'Biochemistry lectures were detailed and well-explained.'),
    (16, 1020, 'The course deepened my understanding of molecular biology.'),
    (21, 1020, 'Biochemistry assignments were challenging yet rewarding.'),
    (24, 1020, 'Developed a keen interest in Biochemistry.');

-- Feedback for CourseID 1021 (Literature)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (4, 1021, 'Literature course enriched my appreciation for classic works.'),
    (8, 1021, 'Enjoyed analyzing literary themes and characters.'),
    (13, 1021, 'Literature lectures were insightful and thought-provoking.'),
    (17, 1021, 'The course enhanced my writing and critical reading skills.'),
    (20, 1021, 'Explored diverse literary genres and styles.'),
    (25, 1021, 'Literature assignments were engaging and intellectually stimulating.');

-- Feedback for CourseID 1026 (Graphic Design)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (4, 1026, 'Graphic Design course was practical and creative.'),
    (8, 1026, 'Learned essential design skills.'),
    (12, 1026, 'Great introduction to graphic design tools.'),
    (16, 1026, 'Enjoyed creating visual designs.'),
    (20, 1026, 'Graphic Design helped me develop my portfolio.'),
    (23, 1026, 'Instructor provided valuable feedback on projects.'),
    (26, 1026, 'Graphic Design assignments were fun and challenging.');
    
    -- Feedback for CourseID 1022 (Geology)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (5, 1022, 'Geology course provided a fascinating look into Earth sciences.'),
    (9, 1022, 'Enjoyed learning about rocks, minerals, and geological processes.'),
    (14, 1022, 'Geology lectures were informative and engaging.'),
    (18, 1022, 'The course deepened my understanding of Earth\'s history.'),
    (22, 1022, 'Geology assignments were hands-on and educational.'),
    (25, 1022, 'Developed a new appreciation for geology.');

-- Feedback for CourseID 1023 (Drama)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (6, 1023, 'Drama course was an exciting exploration of theatrical arts.'),
    (10, 1023, 'Enjoyed studying different drama techniques and performances.'),
    (13, 1023, 'Drama lectures were dynamic and inspiring.'),
    (17, 1023, 'The course enhanced my acting and presentation skills.'),
    (21, 1023, 'Drama assignments allowed for creative expression.'),
    (26, 1023, 'Developed confidence in public speaking through Drama.');

-- Feedback for CourseID 1024 (Computer Networks)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (7, 1024, 'Computer Networks course was crucial for understanding network infrastructure.'),
    (11, 1024, 'Enjoyed configuring and troubleshooting network setups.'),
    (15, 1024, 'Computer Networks lectures were comprehensive and practical.'),
    (19, 1024, 'The course deepened my knowledge of network protocols.'),
    (23, 1024, 'Computer Networks assignments were hands-on and challenging.'),
    (25, 1024, 'Developed networking skills through practical exercises.');

-- Feedback for CourseID 1025 (Public Speaking)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (8, 1025, 'Public Speaking course improved my communication skills.'),
    (12, 1025, 'Enjoyed delivering speeches and presentations.'),
    (16, 1025, 'Public Speaking lectures were engaging and practical.'),
    (20, 1025, 'The course helped overcome fear of public speaking.'),
    (24, 1025, 'Public Speaking assignments were empowering.'),
    (26, 1025, 'Developed confidence in public speaking.');

-- Feedback for CourseID 1013 (Anthropology)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (2, 1013, 'Anthropology course provided fascinating insights into human cultures.'),
    (4, 1013, 'Enjoyed studying evolution and cultural diversity.'),
    (6, 1013, 'Anthropology lectures were thought-provoking and informative.'),
    (10, 1013, 'The course deepened my understanding of human societies.'),
    (14, 1013, 'Anthropology assignments were enlightening and educational.'),
    (18, 1013, 'Developed critical thinking skills through Anthropology.');

-- Feedback for CourseID 1014 (Political Science)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (1, 1014, 'Political Science course was eye-opening and relevant.'),
    (5, 1014, 'Enjoyed analyzing political systems and ideologies.'),
    (9, 1014, 'Political Science lectures were engaging and insightful.'),
    (15, 1014, 'The course deepened my understanding of government and politics.'),
    (19, 1014, 'Political Science assignments were thought-provoking.'),
    (23, 1014, 'Developed critical analysis skills through Political Science.');
    
-- Feedback for CourseID 1015 (History)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (1, 1015, 'History course was enlightening and well-taught.'),
    (3, 1015, 'Enjoyed learning about historical events and timelines.'),
    (7, 1015, 'History lectures were engaging and informative.'),
    (11, 1015, 'The course deepened my understanding of past civilizations.'),
    (15, 1015, 'History assignments were thought-provoking and educational.'),
    (20, 1015, 'Developed a keen interest in history through this course.');

-- Feedback for CourseID 1016 (Physics)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (2, 1016, 'Physics course provided a comprehensive understanding of physical laws.'),
    (4, 1016, 'Enjoyed conducting experiments in Physics.'),
    (8, 1016, 'Physics lectures were clear and well-explained.'),
    (12, 1016, 'The course improved my problem-solving skills.'),
    (16, 1016, 'Physics assignments were challenging yet rewarding.'),
    (21, 1016, 'Developed critical thinking through Physics.');

-- Feedback for CourseID 1017 (Music Theory)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (3, 1017, 'Music Theory course enhanced my understanding of musical concepts.'),
    (5, 1017, 'Enjoyed learning about harmony and composition.'),
    (9, 1017, 'Music Theory lectures were enjoyable and insightful.'),
    (13, 1017, 'The course deepened my appreciation for music.'),
    (17, 1017, 'Music Theory assignments were creative and educational.'),
    (22, 1017, 'Developed musical skills through this course.');

-- Feedback for CourseID 1018 (Philosophy)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (6, 1018, 'Philosophy course provided profound insights into human existence.'),
    (10, 1018, 'Enjoyed exploring philosophical ideas and theories.'),
    (14, 1018, 'Philosophy lectures were thought-provoking and engaging.'),
    (18, 1018, 'The course expanded my worldview and critical thinking.'),
    (23, 1018, 'Philosophy assignments were intellectually stimulating.'),
    (25, 1018, 'Developed philosophical reasoning through this course.');

-- Feedback for CourseID 1019 (Chemical Engineering)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (7, 1019, 'Chemical Engineering course provided practical knowledge of chemical processes.'),
    (11, 1019, 'Enjoyed designing and optimizing chemical systems.'),
    (15, 1019, 'Chemical Engineering lectures were comprehensive and insightful.'),
    (19, 1019, 'The course deepened my understanding of chemical principles.'),
    (24, 1019, 'Chemical Engineering assignments were hands-on and challenging.'),
    (26, 1019, 'Developed problem-solving skills in chemical engineering.');

-- Feedback for CourseID 1020 (Sociology)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (8, 1020, 'Sociology course provided valuable insights into social structures.'),
    (12, 1020, 'Enjoyed studying human behavior and societal norms.'),
    (16, 1020, 'Sociology lectures were engaging and thought-provoking.'),
    (20, 1020, 'The course deepened my understanding of cultural diversity.'),
    (21, 1020, 'Sociology assignments were enlightening and relevant.'),
    (26, 1020, 'Developed sociological perspective through this course.');

-- Feedback for CourseID 1021 (Foreign Language)
INSERT INTO CourseFeedback (StudentID, CourseID, Feedback) VALUES
    (1, 1021, 'Foreign Language course improved my language proficiency.'),
    (9, 1021, 'Enjoyed learning new vocabulary and grammar.'),
    (13, 1021, 'Language classes were interactive and engaging.'),
    (17, 1021, 'The course deepened my cultural understanding.'),
    (22, 1021, 'Language assignments were practical and useful.'),
    (25, 1021, 'Developed communication skills through language learning.');
    
-- Inserting sample course materials data (for example purposes)
INSERT INTO CourseMaterials (CourseID, MaterialType, Title, Description, MaterialLink)
VALUES 
    (1001, 'Lecture Notes', 'Introduction to Computer Science Lecture 1', 'Lecture slides for the first session of Introduction to Computer Science.', 'https://example.com/cs101/lecture1'),
    (1001, 'Textbook', 'Computer Science: An Overview', 'Required textbook for Introduction to Computer Science course.', 'https://example.com/cs101/textbook'),
    (1002, 'Lecture Slides', 'Art History Week 1 Slides', 'Slides covering the first week of Art History lectures.', 'https://example.com/arthistory/slides1'),
    (1002, 'Reading Material', 'Art Movements Guide', 'Guide to different art movements discussed in the course.', 'https://example.com/arthistory/artmovements'),
    (1003, 'Lab Manual', 'Statistics Lab Exercises', 'Exercises and instructions for Statistics lab sessions.', 'https://example.com/stats/labmanual'),
    (1003, 'Textbook', 'Statistics Essentials', 'Recommended textbook for Statistics course.', 'https://example.com/stats/textbook'),
    (1004, 'Chemistry Notes', 'Chemistry Basics', 'Key notes on basic chemistry concepts covered in the course.', 'https://example.com/chemistry/basics'),
    (1004, 'Textbook', 'Chemical Reactions: Fundamentals', 'Textbook for understanding chemical reactions.', 'https://example.com/chemistry/textbook'),
    (1005, 'Lecture Notes', 'Physics Basics', 'Introductory lecture notes for Physics course.', 'https://example.com/physics/basics'),
    (1005, 'Textbook', 'Fundamentals of Physics', 'Recommended textbook for Physics course.', 'https://example.com/physics/textbook'),
    (1006, 'Slides', 'Introduction to Psychology', 'Slides covering introductory topics in Psychology.', 'https://example.com/psychology/slides'),
    (1006, 'Reading Material', 'Psychology Research Papers', 'List of research papers for Psychology students.', 'https://example.com/psychology/research'),
    (1007, 'Lab Manual', 'Biology Lab Experiments', 'Experiments manual for Biology laboratory sessions.', 'https://example.com/biology/labmanual'),
    (1007, 'Textbook', 'Biology: Concepts and Connections', 'Key textbook for Biology concepts.', 'https://example.com/biology/textbook'),
    (1008, 'Lecture Notes', 'History: Ancient Civilizations', 'Lecture notes on ancient civilizations.', 'https://example.com/history/ancient'),
    (1008, 'Textbook', 'World History: Patterns of Interaction', 'Textbook covering world history topics.', 'https://example.com/history/textbook'),
    (1009, 'Lab Manual', 'Engineering Lab Exercises', 'Exercises and instructions for Engineering lab sessions.', 'https://example.com/engineering/labmanual'),
    (1009, 'Textbook', 'Fundamentals of Engineering', 'Recommended textbook for Engineering course.', 'https://example.com/engineering/textbook'),
    (1010, 'Lecture Slides', 'Economics Fundamentals', 'Slides covering fundamental concepts in Economics.', 'https://example.com/economics/slides'),
    (1010, 'Textbook', 'Principles of Microeconomics', 'Key textbook for Microeconomics.', 'https://example.com/economics/textbook'),
    (1011, 'Course Materials Overview', 'Introduction to Marketing', 'Overview of course materials for Marketing.', 'https://example.com/marketing/overview'),
    (1011, 'Textbook', 'Marketing Management', 'Recommended textbook for Marketing course.', 'https://example.com/marketing/textbook'),
    (1012, 'Lecture Notes', 'Philosophy: Ethics', 'Lecture notes on ethical philosophy.', 'https://example.com/philosophy/ethics'),
    (1012, 'Textbook', 'Introduction to Philosophy', 'Key textbook introducing Philosophy.', 'https://example.com/philosophy/intro'),
    (1013, 'Lecture Slides', 'Music Theory Basics', 'Slides covering basics of Music Theory.', 'https://example.com/music/theory-basics'),
    (1013, 'Reading Material', 'Music Composers Guide', 'Guide to famous music composers.', 'https://example.com/music/composers'),
    (1014, 'Lab Manual', 'Geology Lab Exercises', 'Exercises and instructions for Geology lab sessions.', 'https://example.com/geology/labmanual'),
    (1014, 'Textbook', 'Introduction to Geology', 'Recommended textbook for Geology course.', 'https://example.com/geology/textbook'),
    (1015, 'Lecture Notes', 'Introduction to Astronomy', 'Introductory lecture notes for Astronomy course.', 'https://example.com/astronomy/notes'),
    (1015, 'Textbook', 'Universe: An Introduction', 'Textbook exploring the universe and celestial bodies.', 'https://example.com/astronomy/textbook'),
    (1016, 'Lecture Slides', 'Political Science: Government Systems', 'Slides covering government systems in Political Science.', 'https://example.com/politicalscience/slides'),
    (1016, 'Reading Material', 'Political Philosophers Guide', 'Guide to famous political philosophers.', 'https://example.com/politicalscience/philosophers'),
    (1017, 'Lecture Notes', 'Nutrition Basics', 'Basic lecture notes on Nutrition.', 'https://example.com/nutrition/basics'),
    (1017, 'Textbook', 'Fundamentals of Nutrition', 'Textbook covering Nutrition fundamentals.', 'https://example.com/nutrition/textbook'),
    (1018, 'Lab Manual', 'Physics Lab Exercises', 'Exercises and instructions for Physics lab sessions.', 'https://example.com/physics/labmanual'),
    (1018, 'Textbook', 'Physics: Principles with Applications', 'Key textbook for Physics principles.', 'https://example.com/physics/textbook'),
    (1019, 'Lecture Slides', 'Business Management Basics', 'Slides covering basics of Business Management.', 'https://example.com/business/management-basics'),
    (1019, 'Textbook', 'Principles of Business Administration', 'Textbook for Business Administration principles.', 'https://example.com/business/administration'),
    (1020, 'Lecture Notes', 'Environmental Science Fundamentals', 'Lecture notes on Environmental Science basics.', 'https://example.com/environmental/notes'),
    (1020, 'Textbook', 'Environmental Science: Earth as a Living Planet', 'Recommended textbook for Environmental Science.', 'https://example.com/environmental/textbook'),
    (1021, 'Lecture Slides', 'Sociology: Social Structures', 'Slides covering social structures in Sociology.', 'https://example.com/sociology/slides'),
    (1021, 'Reading Material', 'Sociological Studies Collection', 'Collection of sociological studies.', 'https://example.com/sociology/studies'),
    (1022, 'Lecture Notes', 'Digital Marketing Essentials', 'Introductory lecture notes on Digital Marketing.', 'https://example.com/digitalmarketing/notes'),
    (1022, 'Textbook', 'Digital Marketing Strategy', 'Textbook for developing digital marketing strategies.', 'https://example.com/digitalmarketing/strategy'),
    (1023, 'Lecture Slides', 'Journalism Basics', 'Slides covering basics of Journalism.', 'https://example.com/journalism/basics'),
    (1023, 'Reading Material', 'Journalism Ethics Guide', 'Guide to ethical considerations in Journalism.', 'https://example.com/journalism/ethics'),
    (1024, 'Lab Manual', 'Computer Engineering Lab Exercises', 'Exercises and instructions for Computer Engineering lab sessions.', 'https://example.com/computerengineering/labmanual'),
    (1024, 'Textbook', 'Computer Engineering: Hardware and Software', 'Key textbook for Computer Engineering.', 'https://example.com/computerengineering/textbook'),
    (1025, 'Lecture Slides', 'English Literature: Romantic Period', 'Slides covering literature from the Romantic Period.', 'https://example.com/literature/romantic-slides'),
    (1025, 'Reading Material', 'Selected Romantic Poems', 'Collection of poems from the Romantic Period.', 'https://example.com/literature/romantic-poems'),
    (1026, 'Lecture Notes', 'Spanish Language Basics', 'Introductory notes for learning Spanish language.', 'https://example.com/spanish/basics'),
    (1026, 'Textbook', 'Spanish Grammar Guide', 'Guide to Spanish grammar rules and usage.', 'https://example.com/spanish/grammar');
```

## Stored Procedures

### 1. RegisterStudentForCourse

**Description**: This stored procedure registers a student for a course by inserting a new record into the `Enrollment` table.

**Input Parameters**:
- `student_id` (INT): ID of the student to be registered.
- `course_id` (INT): ID of the course the student is registering for.
- `enrollment_date` (DATE): Date of enrollment.

**Example**:
```sql
CALL RegisterStudentForCourse(1, 1001, '2024-04-10');
```
### 2. SendNotificationToStudent

**Description**: This stored procedure sends a notification or alert to a student by inserting a message into the Notification table.

**Input Parameters**:
- `student_id` (INT): ID of the student receiving the notification.
- `message` (VARCHAR(255)): Message content of the notification.

**Example**:
```sql
CALL SendNotificationToStudent(1, 'Your course enrollment is confirmed.');
```

### 3. AssignProfessorToCourse

**Description**: This stored procedure assigns a professor to teach a course by inserting a record into the Teaching table.

**Input Parameters**:
- `professor_id` (INT): ID of the professor being assigned.
- `course_id` (INT): ID of the course the professor is assigned to teach.
- `semester` (VARCHAR(20)): Semester in which the course will be taught.
- `year` (INT): Year of the course.

**Example**:
```sql
CALL AssignProfessorToCourse(101, 1001, 'Spring', 2024);
```

### 4. GetCourseEnrollmentReport
**Description**: This stored procedure generates an enrollment report by counting the number of students enrolled in each course.

**Input Parameters**: None.

**Example**:
```sql
CALL GetCourseEnrollmentReport();
```

### 5. GetCourseFeedback
**Description**: This stored procedure retrieves the feedback given by students for a specific course.

**Input Parameters**:
- `course_id` (INT): ID of the course for which feedback is requested.
- `studentName` (OUT VARCHAR(100)): Output parameter for student name.
- `feedback` (OUT TEXT): Output parameter for student feedback.

**Example**:
```sql
CALL GetCourseFeedback(1001, @studentName, @feedback);
SELECT @studentName AS StudentName, @feedback AS Feedback;
```

### 6. ForecastCourseDemand
**Description**: This stored procedure forecasts course demand for an upcoming semester.

**Input Parameters**:

- `semester` (VARCHAR(20)): Semester for which course demand is being forecasted.
- `year (INT): Year for which course demand is being forecasted.

**Example**:
```sql
CALL ForecastCourseDemand('Fall', 2024);
```

### 7. GetCoursePrerequisites
**Description**: This stored procedure retrieves course prerequisites for a specific course.

**Input Parameters**:

- `course_id` (INT): ID of the course for which prerequisites are requested.

**Example**:
```sql
CALL GetCoursePrerequisites(1001);
```

### 8. AssignClassroomToCourse
**Description**: This stored procedure assigns a classroom to a course by updating the Course table.

**Input Parameters**:

- `course_id` (INT): ID of the course to which a classroom is being assigned.
- `classroom_id` (INT): ID of the classroom being assigned.

**Example**:
```sql
CALL AssignClassroomToCourse(1001, 101);
```

## Views

### 1. StudentEnrollmentHistory

**Description**: View to track student enrollment history and course preferences.

**Example**:
```sql
SELECT * FROM StudentEnrollmentHistory;
```

### 2. ProfessorSchedule
**Description**: View to manage scheduling office hours and professor workload.

**Example**:
```sql
SELECT * FROM ProfessorSchedule;
```

### 3. CoursePrerequisites
**Description**: View to assist in tracking course prerequisites and co-requisites.

**Example**:
```sql
SELECT * FROM CoursePrerequisites;
```

### 4. StudentProgress
**Description**: View to provide insights into student progress and completion rates.

**Example**:
```sql
SELECT * FROM StudentProgress;
```

### 5. StudentData
**Description**: View to ensure data security and compliance with privacy regulations.

**Example**:
```sql
SELECT * FROM StudentData;
```

## Functions

### 1. GetStudentRecord
**Description**: Function to retrieve student information as a JSON string.

**Input Parameters**:

- `student_id` (INT): ID of the student for which information is requested.

**Example**:
```sql
SELECT GetStudentRecord(1);
```

### 2. GetCoursePrerequisites

**Description**:: Function to retrieve course prerequisites as text.

**Input Parameters**:

- `course_id` (INT): ID of the course for which prerequisites are requested.
**Example**:

```sql
SELECT GetCoursePrerequisites(1001);
```

### 3. GetStudentAdvisor
**Description**: Function to assist academic advisors in providing personalized guidance to students.

**Input Parameters**:
- `student_id` (INT): ID of the student for which an advisor is requested.

**Example**:

```sql
SELECT GetStudentAdvisor(1);
```

## Conclusion
The University Database System is a robust toolset designed to facilitate student registration, course management, and academic record-keeping. By following this usage guide, you can 
effectively interact with the database using SQL functions, views, and stored procedures to address real-world scenarios within an educational environment. Adapt and extend the provided 
SQL code to suit specific project requirements and optimize database operations for academic advising,
course planning, and student services. Use INSERT INTO statements to populate data into tables and explore additional use cases based on the database schema and business needs.

