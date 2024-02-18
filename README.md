[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/JwSLLxUh)

## Assignment #1. Preparation of the DB Migration

## Database Migration and Rollback Instructions
This document delineates SQL scripts for migration and rolling back changes to a PostgreSQL database schema due to the specified requirements. The migration process covers renaming columns in two tables, increasing column lengths, and converting a column type to an array type. The rollback process reverts all modifications to their initial states.

## Prerequisites
* PostgreSQL database schema with version 9.1 or higher [Download PostgreSQL](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads);
* Connect the PostgreSQL database with appropriate privileges to execute schema changes;
* Access to a PostgreSQL command-line tool (SQL Shell) or PgAdmin4.

## Migration Process
The migration_script.sql file performs the essential steps that are in the following way:
1. Rename the STUDENTS.ST_ID to STUDENTS.STUDENT_ID;
2. Change the lengths of STUDENTS.ST_NAME and STUDENTS.ST_LAST from 20 to 30;
3. Change the name of the INTERESTS.INTEREST to INTERESTS;
4. Create new table with an array type that stores the INTERESTS table.

## Rollback Process
The rollback_script.sql file performs the essential steps that are in the following way:
1. Rename back the STUDENTS.STUDENT_ID to STUDENTS.ST_ID;
2. Revert the changes in the lenghts of STUDENTS.ST_NAME and STUDENTS.ST_LAST from 30 to 20;
3. Revert the name of the INTERESTS.INTERESTS to INTEREST;
4. Drop the previous version of INTERESTS table;
5. Recreate the dropped INTERESTS table;
6. Re-insert the sample data into INTERESTS table.

## Set up Environment
I used the latest version of PostgreSQL, 16.2 to create tables, implement migration and rollback processes for this assignment.

**Step 1:** Go to my GitHub repository ([https://github.com/ADA-GWU/2024-a1-db-migration-gazizovaa](https://github.com/ADA-GWU/2024-a1-db-migration-gazizovaa))

**Step 2:** Click the green **CODE** button, click on **Download ZIP** under HTTPS, and unzip the repository in yur computer.

**Step 3:** Open pgAdmin 4 app on your computer and enter your password to establish a connection to your PostgreSQL Server. 

**Step 4:** To initiate the creation of a database, press on **Servers** and you will see PostgreSQL 16. 

**Step 5:** Right click on PostgreSQL 16, select **Create**, go to **Database**. In "General" section, you will **Database** field and you will need to give a name to your database, for example, it's "postgres" in my case. Then, press **Save** button.

**Step 5:** Right click on the database you created, for example "postgres" and select **Query Tool** from drop-down list. Now, you're ready to run the scripts by adding the script files.


## Run Scripts from pgAdmin
### Perform Tables
**Step 1:** To import any SQL script, you will see **Open File** icon positioned above "Query" section and click on it. Navigate to your directory containing the unzipped GitHub repository. Then, select "created_tables.sql" SQL file. 

**Step 2:** Start running the scripts sequentially, beginning from top to down.  

`<!--Create STUDENTS table-->`
```sql
CREATE TABLE STUDENTS(
	ST_ID INT PRIMARY KEY,
	ST_NAME VARCHAR(20) NOT NULL,
	ST_LAST VARCHAR(20) NOT NULL
);
```

`<!--Insert sample data into the STUDENTS table-->`
```sql
INSERT INTO STUDENTS(ST_ID, ST_NAME, ST_LAST) VALUES(1, 'Konul', 'Gurbanova');
INSERT INTO STUDENTS(ST_ID, ST_NAME, ST_LAST) VALUES(2, 'Shahnur', 'Isgandarli');
INSERT INTO STUDENTS(ST_ID, ST_NAME, ST_LAST) VALUES(3, 'Natavan', 'Mammadova');
```
```sql
SELECT * FROM STUDENTS;
```

```sql
`<!--Create INTERESTS TABLE-->`
CREATE TABLE INTERESTS(
	STUDENT_ID INT NOT NULL,
	INTEREST VARCHAR(20) NOT NULL
);
```

```sql
`<!--Insert sample data into the INSERTS table-->`
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(1, 'Tennis');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(1, 'Literature');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(2, 'Math');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(2, 'Tennis');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(3, 'Math');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(3, 'Music');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(2, 'Football');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(1, 'Chemistry');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(3, 'Chess');
```
```sql
SELECT * FROM INTERESTS;
``` 

