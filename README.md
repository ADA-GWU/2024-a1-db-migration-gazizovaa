[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/JwSLLxUh)

## Assignment #1. Preparation of the DB Migration

## Database Migration and Rollback Instructions
This document delineates SQL scripts for migration and rolling back changes to a PostgreSQL database schema due to the specified requirements. The migration process covers renaming columns in two tables, increasing column lengths, and converting a column type to an array type. The rollback process reverts all modifications to their initial states.
I used the latest version of PostgreSQL, 16.2 to create tables, implement migration and rollback processes for this assignment.

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
Step 1: Go to my GitHub repository [https://github.com/ADA-GWU/2024-a1-db-migration-gazizovaa](https://github.com/ADA-GWU/2024-a1-db-migration-gazizovaa)
Step 2: Click the green **CODE"" button, click on **Download ZIP** under HTTPS, and unzip the repository in yur computer.
Step 3: Open pgAdmin 4 on your computer and create your database. To create a database, first press on **Servers** and you will see PostgreSQL 16. 
Step 4: Right click on PostgreSQL 16, select **Create**, then **Database**. You need to give a name to your database, for example, it's "postgres" in my case and press **Save** button.
Step 5: Right click on the database you created, for example "postgres" and select **Query Tool** button. After completing this step, you're ready to run the scripts.

## Run Scripts from pgAdmin
### Perform Tables
Step 1: In the "Query Tool", you will see **Open File** which locates above "Query" and click on it. It will take you into your directory (unziped GitHub repository). Select created_tables.sql script and run the following SQL scripts step by step as shown below:

Create STUDENTS Table
CREATE TABLE STUDENTS(
	ST_ID INT PRIMARY KEY,
	ST_NAME VARCHAR(20) NOT NULL,
	ST_LAST VARCHAR(20) NOT NULL
);

Then, insert sample data provided in our assignment document into this table
INSERT INTO STUDENTS(ST_ID, ST_NAME, ST_LAST) VALUES(1, 'Konul', 'Gurbanova');
INSERT INTO STUDENTS(ST_ID, ST_NAME, ST_LAST) VALUES(2, 'Shahnur', 'Isgandarli');
INSERT INTO STUDENTS(ST_ID, ST_NAME, ST_LAST) VALUES(3, 'Natavan', 'Mammadova');

View the table using the following command
SELECT * FROM STUDENTS;

Create INTERESTS table
CREATE TABLE INTERESTS(
	STUDENT_ID INT NOT NULL,
	INTEREST VARCHAR(20) NOT NULL
);

Then, insert sample data into the INSERTS table
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(1, 'Tennis');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(1, 'Literature');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(2, 'Math');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(2, 'Tennis');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(3, 'Math');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(3, 'Music');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(2, 'Football');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(1, 'Chemistry');
INSERT INTO INTERESTS(STUDENT_ID, INTEREST) VALUES(3, 'Chess');

View the table
SELECT * FROM INTERESTS;

