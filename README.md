[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/JwSLLxUh)

## Assignment #1. Preparation of the DB Migration

## Database Migration and Rollback Instructions
This document represents SQL scripts for migration and rolling back changes to a PostgreSQL Database Schema meet the specified requirements. The migration process covers renaming columns in two tables, increasing column lengths, and converting a column type to an array type. The rollback process reverts all modifications to their initial states.

## Prerequisites
* PostgreSQL database schema with version 9.1 or higher [Download PostgreSQL](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
* Connect the PostgreSQL database with appropriate privileges to execute schema changes
* Access to a PostgreSQL command-line tool (SQL Shell) or PgAdmin4

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




