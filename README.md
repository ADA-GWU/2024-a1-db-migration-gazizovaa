[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/JwSLLxUh)

## Assignment #1. Preparation of the DB Migration

## Database Migration and Rollback Instructions
This document will describe SQL scripts necessarily for migration and rolling back changes made to a PostgreSQL database schema in accordance with specified requirements. The migration process involves renaming columns in two tables, extending column lengths, and converting a column type to an array type. The rollback process reverts all modifications to their original states.

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

**Step 2:** Click the green **CODE** button, click on **Download ZIP** under HTTPS, and unzip the repository in your computer.

**Step 3:** Open pgAdmin app on your computer and enter your password to establish a connection to your PostgreSQL Server. 

**Step 4:** To initiate the creation of a database, press on **Servers** and you will see PostgreSQL 16. 

**Step 5:** Right click on PostgreSQL 16, select **Create** and go to **Database**. In "General" section, you will **Database** field and you will need to give a name to your database, for example, it's "postgres" in my case. Then, press **Save** button.

**Step 5:** Right click on the database you created, for example "postgres" and select **Query Tool** from drop-down list. Now, you're ready to run the scripts by adding the script files.


## Run Scripts using pgAdmin
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

| st_id| st_name |  st_last
|------|---------|------------
|    1 | Konul   | Gurbanova
|    2 | Shahnur | Isgandarli
|    3 | Natavan | Mammadova
(3 rows)

`<!--Create INTERESTS TABLE-->`
```sql
CREATE TABLE INTERESTS(
	STUDENT_ID INT NOT NULL,
	INTEREST VARCHAR(20) NOT NULL
);
```

`<!--Insert sample data into the INSERTS table-->`
```sql
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

| student_id |  interest  |
|-----------+|----------  |
|          1 | Tennis     |
|          1 | Literature |
|          2 | Math       |
|          2 | Tennis     |
|          3 | Math       |
|          3 | Music      |
|          2 | Football   |
|          1 | Chemistry  |
|          3 | Chess      |
(9 rows)

### Run Migration Script
**Step 1:** To import the next SQL script, just press **New query tool for current connection**, it will automatically create new Query Tool. Then, select "migration_script.sql" SQL file. 

**Step 2:** Start running the scripts sequentially, beginning from top to down.

`<!--Rename the STUDENTS.ST_ID to STUDENTS.STUDENT_ID-->`
```sql
ALTER TABLE STUDENTS
RENAME COLUMN ST_ID TO STUDENT_ID;
```

```sql
SELECT * FROM STUDENTS;
```

`<!--Change the length of STUDENTS.ST_NAME from 20 to 30-->`
```sql
ALTER TABLE STUDENTS
ALTER COLUMN ST_NAME TYPE VARCHAR(30);
```

`<!--Change the length of STUDENTS.ST_LAST from 20 to 30-->`
```sql
ALTER TABLE STUDENTS
ALTER COLUMN ST_LAST TYPE VARCHAR(30);
```

```sql
SELECT * FROM STUDENTS;
```

`<!--Change the name of the INTERESTS.INTEREST to INTERESTS-->`
```sql
ALTER TABLE INTERESTS
RENAME COLUMN INTEREST TO INTERESTS;
```

```sql
SELECT * FROM INTERESTS;
```

`<!--Create new table with an array type that stores the INTERESTS table-->`
```sql
CREATE TABLE TEMP_INTERESTS AS
SELECT STUDENT_ID,
       CONCAT('{', STRING_AGG(CONCAT('"', INTERESTS, '"'), ','), '}') AS INTERESTS
FROM INTERESTS
GROUP BY STUDENT_ID
ORDER BY STUDENT_ID;
```

```sql
SELECT * FROM TEMP_INTERESTS;
```

| student_id |              interests              |
|-----------+|------------------------------------ |
|          1 | {"Tennis","Literature","Chemistry"} |
|          2 | {"Math","Tennis","Football"}        |
|          3 | {"Math","Music","Chess"}            |
(3 rows)

```sql
SELECT * FROM TEMP_INTERESTS; 
```

`<!--Drop the old table-->`
```sql
DROP TABLE INTERESTS;
```

`<!--Replace the TEMP_INTERESTS table with the INTERESTS table-->`
```sql
ALTER TABLE TEMP_INTERESTS
RENAME TO INTERESTS;
```

```sql
SELECT * FROM INTERESTS;
```

### Run Rollback Script
**Step 1:** To import the last SQL script, just press **New query tool for current connection**, it will automatically create new Query Tool. Then, select "rollback_script.sql" SQL file. 

**Step 2:** Start running the scripts sequentially, beginning from top to down.

`<!-- Rename back the STUDENTS.STUDENT_ID to STUDENTS.ST_ID-->`
```sql
ALTER TABLE STUDENTS
RENAME COLUMN STUDENT_ID TO ST_ID;
```

```sql
SELECT * FROM STUDENTS;
```

`<!--Revert the change in the lenght of STUDENTS.ST_NAME from 30 to 20-->`
```sql
ALTER TABLE STUDENTS
ALTER COLUMN ST_NAME TYPE VARCHAR(20);
```

`<!--Revert the change in the lenght of STUDENTS.ST_LAST from 30 to 20-->`
```sql
ALTER TABLE STUDENTS
ALTER COLUMN ST_LAST TYPE VARCHAR(20);
```

```sql
SELECT * FROM STUDENTS;
```

`<!--Revert the name of the INTERESTS.INTERESTS to INTEREST-->`
```sql
ALTER TABLE INTERESTS
RENAME COLUMN INTERESTS TO INTEREST;
```

```sql
SELECT * FROM INTERESTS;
```

`<!--Drop the previous version of INTERESTS table-->`
```sql
DROP TABLE INTERESTS;
```

`<!--Recreate the dropped INTERESTS table-->`
```sql
CREATE TABLE INTERESTS( 
	STUDENT_ID INT NOT NULL,
	INTEREST VARCHAR(20) NOT NULL
);
```

`<!--Re-insert the sample data into INTERESTS table-->`
```sql
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

## (Another Option)/Run Scripts using SQL Shell (psql)
**Step 1:** When entering SQL Shell, type the following details:

Server [localhost]: localhost

Database [postgres]: # enter your database name (in my case, it's "postgres")

Port [5432]: 5432

Username [postgres]: # enter your username (in my case, it's "postgres")

Password for user postgres: # enter your password


**Step 2:** Once you have connected to a PostgreSQL, you just need to start copying the SQL scripts sequentially(first created_tables.sql, second migration_script.sql, last rollback_script.sql) to the terminal and press **Enter**.


       

       





