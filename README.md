# Chetana Hazrimal Somani College Placement Management System

A Java web application for college placement workflows. Students can create a profile, set first-time job preferences, browse approved opportunities, apply, and follow application status. Recruiters can maintain a company profile, post jobs, and manage their applicants. Placement administrators approve recruiter accounts and job listings and download placement reports.

## Technology

- Java 11
- Jakarta EE 10 Servlets and JSP
- MySQL with JDBC
- HTML, CSS, and JavaScript
- Apache Maven (WAR packaging)
- Apache Tomcat 10.1+

## Main source layout

```text
src/main/java/com/placement/controller/
  LoginServlet.java       Sign-in and session creation
  RegisterServlet.java    Student registration
  LogoutServlet.java      Session logout
  PortalServlet.java      Student, recruiter, and admin workflows
src/main/java/com/placement/dao/
  DatabaseConnection.java MySQL connection setup
src/main/java/com/placement/security/
  PasswordHasher.java     PBKDF2 password hashing and verification
src/main/webapp/
  *.jsp                   Login, registration, onboarding, and dashboards
  css/enterprise.css      Shared styles
  images/                 College and campus assets
database/
  placement_schema.sql    MySQL database and table definitions
pom.xml                   Maven dependencies and WAR build configuration
```

## Requirements

1. Java 11 or a compatible JDK.
2. Apache Maven (NetBeans bundles Maven).
3. MySQL Server.
4. Apache Tomcat 10.1 or a Jakarta EE 10 compatible server.

## Database setup

1. Open `database/placement_schema.sql` in MySQL Workbench and run it. It creates the `chs_placement_portal` database and its tables.
2. Configure the connection locally using environment variables, or create `%USERPROFILE%\Documents\Codex\.placement-db.properties` outside this source folder:

   ```properties
   PLACEMENT_DB_URL=jdbc:mysql://localhost:3306/chs_placement_portal?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
   PLACEMENT_DB_USER=root
   PLACEMENT_DB_PASSWORD=your_local_mysql_password
   ```

   Do not commit your actual database password. The repository intentionally contains no local database credentials.

## Run in NetBeans

1. Open this folder as a Maven project in Apache NetBeans.
2. Start MySQL and Apache Tomcat.
3. Build and run the project from NetBeans.
4. Open `http://localhost:8080/studentplacement_System/` (the context path can differ if you rename the project).
5. Register a student account to explore student onboarding and the student dashboard. Recruiter accounts require placement-office approval before they can sign in and post jobs.

## Main workflow and safeguards

- Student preferences are requested during first-time onboarding; a resume upload is optional.
- Recruiter accounts and recruiter job posts have separate admin approval steps.
- Each recruiter is limited to ten job posts.
- Students see approved and open jobs; eligibility is checked before applying.
- Recruiters can view or download resumes only for applicants to their own jobs.
- Passwords are stored as salted PBKDF2-HMAC-SHA256 hashes, not plain text.
- Resume and company-logo files are stored outside the web root; database rows store generated filenames.

## First administrator

After registering a trusted account, promote it in MySQL Workbench by replacing the email below with that account’s email:

```sql
UPDATE users SET role='admin', status='active' WHERE email='trusted-admin@example.edu';
```

## Database tables

`users`, `students`, `student_preferences`, `recruiters`, `jobs`, `applications`, `skills`, and `student_skills`.

## Notes

Maven creates the deployable WAR and generated files under `target/`. The `target/` directory is ignored by Git because it can be regenerated from the source. NetBeans private settings and local database configuration should also remain uncommitted.
