🐘 Zoo Management System 🦒
This is a console-based Java application designed to manage various aspects of a zoo's operations. It provides functionalities for managing animal assets, daily activities like attraction entries and concession sales, and generating various management and financial reports.

The project was originally developed using an Oracle database, but for ease of setup and portability on GitHub, it is highly recommended to configure it with a lightweight, embedded database like H2 Database. An Oracle XE setup is also possible if preferred and disk space allows.

✨ Features
The application is structured into three main menus, accessible from a central ZooMenu:

1. 🐾 Asset Management
Insert/View/Update Animals: Manage details about animals, including species, building, and enclosure information.

Insert/View/Update Buildings: Handle information about the physical buildings within the zoo.

Insert/View/Update Animal Shows: Manage details of animal shows, including pricing and attendance.

Insert/View/Update Employees: Maintain employee records, including job types and hourly rates.

2. 🗓️ Daily Zoo Activity
Attraction Entry: Record entries into attractions and calculate revenue.

Insert Concession: Log concession sales and associated revenue.

Insert Attendee: Record attendee information and ticket sales.

3. 📊 Management and Reporting
Revenue Report by Source: Generate reports on revenue based on different sources (attractions, concessions, admissions).

Animal Population Report: View reports related to animal populations and associated costs.

Top 3 Attractions: Identify the top-performing attractions by revenue.

Best Days in Month: Determine the days with the highest revenue within a specified period.

Average Attraction Attendance and Concession: Analyze average attendance and concession revenue.

🛠️ Technologies Used
Core Language: Java

Database Connectivity: JDBC (Java Database Connectivity)

Build Tool: Maven

Database (Recommended for Local Setup): H2 Database (embedded mode)

Database (Original / Alternative): Oracle Database

📂 Project Structure
The project is organized into several Java classes, each handling specific functionalities:

ZooMenu.java: The main entry point of the application, providing the top-level menu.

Asset_Management_Menu.java: Handles all operations related to zoo assets (animals, buildings, shows, employees).

Daily_Zoo_Activity_Menu.java: Manages daily activities like attraction entries, concessions, and attendee admissions.

Management_Reporting_Menu.java: Contains methods for generating various reports.

Asset_Management.java: Core logic for interacting with the database for asset-related operations (insert, update, view).

Daily_Zoo_Activity.java: Core logic for daily activity database interactions.

Management_And_Reporting.java: Core logic for generating reports from the database.

Print.java: Utility class for formatting and printing ResultSet data to the console.

IDAlreadyExistsException.java: Custom exception for handling cases where an ID already exists.

IDDoesntExistException.java: Custom exception for handling cases where an ID does not exist.

🚀 Setup Instructions
Prerequisites
Java Development Kit (JDK) 8 or higher

Maven (if you don't have it, you can download it or use your IDE's built-in Maven support)

Git (for cloning the repository)

Database Setup (Choose ONE of the following options)
Option 1: H2 Database (Recommended for Ease of Use and Portability)
This option requires minimal setup and disk space, as H2 runs as an embedded database within your application.

Add H2 Dependency:
Ensure your pom.xml file (see example below) includes the H2 dependency within the <dependencies> section:

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>2.2.224</version> </dependency>

Update JDBC Connection URL:
Modify the DriverManager.getConnection() calls in Asset_Management.java, Daily_Zoo_Activity.java, and Management_And_Reporting.java.
Change the connection URL from jdbc:oracle:thin:@prophet.njit.edu:1521:course to:

String DB_URL = "jdbc:h2:./data/zoo_db"; // Database file will be created in a 'data' folder
String USER = "sa"; // Default H2 username
String PASS = "";   // Default H2 password (empty)
Connection con = DriverManager.getConnection(DB_URL, USER, PASS);

Important: You will need to remove or comment out Class.forName("oracle.jdbc.OracleDriver"); as it's no longer needed for H2.

Create Database Schema and Data Scripts:
You will need to create SQL scripts for your database schema (table creation, sequences, etc.) and optionally for inserting sample data.

Create a sql directory (e.g., src/main/resources/sql/) in your project.

Inside this directory, create:

schema.sql: Contains CREATE TABLE statements for all your tables (e.g., ANIMAL, BUILDING, ANIMALSHOW, EMPLOYEE, SPECIES, ENCLOSURE, HOURLYRATE, REVENUETYPES, REVENUE_EVENTS, CONCESSION, ZOOADMISSIONS, CARES_FOR). You might need to adjust Oracle-specific syntax (e.g., VARCHAR2 to VARCHAR, date functions, TO_DATE).

data.sql (Optional): Contains INSERT statements for any initial data you want to populate your tables with.

Implement Schema Initialization: In your Java code (e.g., in ZooMenu or a dedicated DatabaseInitializer class), add logic to run these schema.sql and data.sql scripts when the application starts for the first time (e.g., check if a table exists, if not, run the schema script). This makes the project truly plug-and-play.

Option 2: Oracle Database Express Edition (XE) - Local Installation
This option requires installing Oracle XE on your machine. Ensure you have sufficient disk space.

Download and Install Oracle Database XE:

Go to the official Oracle website (e.g., search for "Oracle Database XE download").

Follow the detailed installation instructions provided by Oracle for your operating system. Remember the SYSTEM user password you set during installation.

Create Database User and Schema:

Open Oracle SQL Developer (or SQL*Plus).

Connect as a privileged user (e.g., SYSTEM or SYS AS SYSDBA) using the password you set during XE installation.

Create a user for your project (e.g., abs83) and grant necessary permissions:

CREATE USER abs83 IDENTIFIED BY YourChosenPassword;
GRANT CONNECT, RESOURCE, DBA TO abs83; -- Adjust grants for production!
ALTER USER abs83 ACCOUNT UNLOCK; -- Ensure it's unlocked

Crucial: Export your database schema (table definitions, sequences, etc.) and data from your original NJIT Oracle database (if you have temporary access) or recreate them manually. Save these as .sql files (e.g., schema.sql, data.sql).

Connect to your local XE instance as your newly created user (abs83).

Run your schema.sql and data.sql scripts to create tables and populate data.

Update JDBC Connection URL:

In your Java code, the connection URL should point to your local XE instance:

String DB_URL = "jdbc:oracle:thin:@localhost:1521/xe"; // Default for XE
String USER = "abs83"; // Your chosen username
String PASS = "YourChosenPassword"; // Your chosen password
Connection con = DriverManager.getConnection(DB_URL, USER, PASS);

Ensure Class.forName("oracle.jdbc.OracleDriver"); is present.

🔒 Secure Your Credentials (Important for GitHub!)
NEVER hardcode your database username and password directly in your Java files or commit them to GitHub.

Create a .gitignore file: In your project's root directory, create a file named .gitignore (if you don't have one) and add the following lines to it:

# Maven
/target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
.mvn/timing.properties
.mvn/wrapper/maven-wrapper.jar

# Eclipse
.classpath
.project
.settings/
.idea/
*.iml

# IntelliJ IDEA
.idea/
*.iml
*.ipr
*.iws

# VS Code
.vscode/

# OS generated files
.DS_Store
.Trashes
Thumbs.db

# Database files (H2 embedded)
/data/*.mv.db
/data/*.trace.db

# Configuration files (containing sensitive info)
config.properties

Create a config.properties file: In your project's root directory, create a file named config.properties. This file will NOT be committed to GitHub due to the .gitignore rule.

db.url=jdbc:h2:./data/zoo_db
db.username=sa
db.password=

(Or for Oracle XE: db.url=jdbc:oracle:thin:@localhost:1521/xe, db.username=abs83, db.password=YourChosenPassword)

Modify Java Code to Read Properties: Update your Asset_Management, Daily_Zoo_Activity, and Management_And_Reporting classes to read connection details from this config.properties file.

import java.io.InputStream;
import java.util.Properties;
import java.io.IOException; // Add this import

public class Asset_Management {
    private Properties props = new Properties();

    public Asset_Management() {
        try (InputStream input = getClass().getClassLoader().getResourceAsStream("config.properties")) {
            // If running from IDE, config.properties might be in project root, not classpath
            if (input == null) {
                try (InputStream fileInput = new java.io.FileInputStream("config.properties")) {
                    props.load(fileInput);
                }
            } else {
                props.load(input);
            }
        } catch (IOException ex) {
            System.err.println("Error loading config.properties: " + ex.getMessage());
            // Handle error, e.g., exit or use default values
        }
    }

    public Connection getConnection() throws SQLException { // Added a helper method for connection
        return DriverManager.getConnection(
            props.getProperty("db.url"),
            props.getProperty("db.username"),
            props.getProperty("db.password")
        );
    }

    public ResultSet getAnimalView() throws SQLException {
        Connection con = getConnection(); // Use the helper method
        // ... rest of your method
    }
    // Apply this pattern to all methods that establish a connection
}

Note: The getClass().getClassLoader().getResourceAsStream("config.properties") path assumes config.properties is in your src/main/resources folder (which it should be for Maven projects). The added FileInputStream fallback is for when running directly from the project root in an IDE.

🏃 How to Run the Application
Clone the Repository:

git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name

Build the Project:

mvn clean install

Run the Main Menu:

java -cp target/your-project-name-1.0-SNAPSHOT.jar ZooMenu

(Replace your-project-name-1.0-SNAPSHOT.jar with the actual name of your generated JAR file, found in the target/ directory after building. You might need to adjust the command if your project structure is different, e.g., java -jar target/your-app.jar if it's an executable JAR).

The application will launch in your console, presenting the main menu.

🚨 Custom Exceptions
The project includes custom exception classes for better error handling:

IDAlreadyExistsException: Thrown when an attempt is made to insert a record with an ID that already exists.

IDDoesntExistException: Thrown when an operation is attempted on a record with an ID that does not exist.

💡 Future Enhancements
Graphical User Interface (GUI): Implement a GUI (e.g., with Swing, JavaFX, or a web framework) for a more user-friendly experience.

Improved Input Validation: Add more robust input validation to prevent NumberFormatException and other issues.

Modular Database Access: Refactor database connection and query logic into a dedicated DAO (Data Access Object) layer for cleaner code and easier database switching.

Logging: Implement a logging framework (e.g., Log4j, SLF4j) for better error tracking and application monitoring.

Unit/Integration Tests: Add tests to ensure the correctness of business logic and database interactions.

More Comprehensive Reports: Expand the reporting capabilities with more advanced queries and data visualizations.

User Authentication/Roles: Implement a proper user authentication and authorization system.

Pagination for Views: For large datasets, implement pagination when viewing records to improve performance and user experience.
