 Oracle Pluggable Database Assignment II

Student Name: Kevin  
Student ID: 28746  
Course: Database Development with PL/SQL (INSY 8311)  
Instructor: Eric Maniraguha  
Submission Date:16/feb/2025



Overview

This repository documents my completion of Individual Assignment II focusing on Oracle Pluggable Database (PDB) management. The assignment demonstrates practical skills in Oracle Multitenant Architecture, PDB creation/deletion, user management, and Oracle Enterprise Manager usage.
 Oracle Environment

Oracle Version: ORACLE 21C
Operating System:  Windows 11 
Tools Used: SQL*Plus, Oracle Enterprise Manager, SQL Developer
CDB Name: XE

 Tasks Completed

 Task 1: Create a New Pluggable Database

PDB Name: kv_pdb_28746  
Username: kevin_plsqlauca_28746

Steps Executed
1. Connected to CDB as SYSDBA
2. Created pluggable database with admin user
3. Opened PDB in READ WRITE mode
4. Created user kevin_plsqlauca_28746 inside the PDB
5. Granted necessary privileges (CONNECT, RESOURCE, DBA, UNLIMITED TABLESPACE)
6. Verified user creation and tested connection



Key Commands Used:
sql
CREATE PLUGGABLE DATABASE kv_pdb_28746
ADMIN USER pdbadmin IDENTIFIED BY auca123
FILE_NAME_CONVERT = ('/pdbseed/', '/kv_pdb_28746/');

ALTER PLUGGABLE DATABASE kv_pdb_28746 OPEN;

ALTER SESSION SET CONTAINER = kv_pdb_28746;

CREATE USER kevin_plsqlauca_28746 IDENTIFIED BY [password];
GRANT CONNECT, RESOURCE, DBA TO kevin_plsqlauca_28746;


<img width="693" height="235" alt="create pluggable database step1" src="https://github.com/user-attachments/assets/8988f849-3efe-4ac1-b0cb-0787edc34f09" />

<img width="1056" height="176" alt="alter session step1" src="https://github.com/user-attachments/assets/22e0a09c-edf7-478a-808b-5a2f1d8f41fe" />

<img width="1021" height="161" alt="alter pluggable database step1" src="https://github.com/user-attachments/assets/353d8d77-4aee-4106-a2d3-8946aa0deccb" />

<img width="1501" height="204" alt="create user" src="https://github.com/user-attachments/assets/751486b8-2f11-4990-b5a7-fc340e4ab2af" />

<img width="1401" height="201" alt="grant session to user" src="https://github.com/user-attachments/assets/7e386723-5860-4959-b20e-6b1d73255c11" />




Steps Executed:
1. Created temporary PDB with admin user
2. Opened and verified PDB existence
3. Closed PDB before deletion
4. Dropped PDB including datafiles
5. Verified successful deletion (no rows returned)


Key Commands Used:
sql
CREATE PLUGGABLE DATABASE kv_to_delete_pdb_28746
ADMIN USER tempadmin IDENTIFIED BY 12345
FILE_NAME_CONVERT = ('/pdbseed/', '/kv_to_delete_pdb_28746/');

ALTER PLUGGABLE DATABASE kv_to_delete_pdb_28746 OPEN;

ALTER PLUGGABLE DATABASE kv_to_delete_pdb_28746 CLOSE IMMEDIATE;

DROP PLUGGABLE DATABASE kv_to_delete_pdb_28746 INCLUDING DATAFILES;

<img width="1525" height="261" alt="create pluggable delete step2" src="https://github.com/user-attachments/assets/82b2f3e1-993b-4212-898b-d672394d7548" />

<img width="1809" height="113" alt="pluggable database open step2" src="https://github.com/user-attachments/assets/1a8de3d5-f307-42cd-8bda-60a63ef33e9d" />

<img width="1903" height="204" alt="pluggable database close step2" src="https://github.com/user-attachments/assets/25d7963b-d0d0-425e-87a4-da766f39dedc" />

<img width="1855" height="202" alt="drop pluggable database step2" src="https://github.com/user-attachments/assets/9641bd8b-e666-4ab5-9034-ef2f1111aa10" />


 Task 3: Oracle Enterprise Manager (OEM)

Steps Executed:
1. Started OEM Database Console using emctl command
2. Accessed OEM web interface at https://localhost:5500/em
3. Navigated to Pluggable Databases section
4. Verified kv_pdb_28746 appears in dashboard
5. Confirmed username is visible in the interface



<img width="1908" height="885" alt="OEM dashboard" src="https://github.com/user-attachments/assets/bb6df5d5-4062-4fb2-89bd-3ad12cf878be" />


 Challenge 1: 
Issue: Initial PDB creation failed with insufficient space error  
Solution: Checked available space using df -h, cleaned up old archive logs, and successfully created PDB

Challenge 2: 
Issue: OEM web console was not accessible initially  
Solution: Restarted Enterprise Manager services using 'emctl start dbconsole' and waited for full initialization


 Key Learnings

1. Oracle Multitenant Architecture: Understanding the relationship between Container Database (CDB) and Pluggable Databases (PDB)
2. PDB Lifecycle Management: Mastered creating, opening, closing, and dropping PDBs
3. User Management: Learned to create users within specific PDB context using ALTER SESSION SET CONTAINER
4. Privilege Management: Understanding of CONNECT, RESOURCE, DBA, and UNLIMITED TABLESPACE privileges
5. Oracle Enterprise Manager: Gained experience monitoring and managing database components through OEM
6. Naming Conventions: Importance of following exact naming standards in enterprise database environments
7. Professional Documentation: Developed skills in documenting technical work using GitHub and Markdown

 Academic Integrity Statement

I, Kevin (Student ID: 28746), hereby declare that:

This assignment represents my own original work
All commands were executed by me individually on my Oracle environment
All screenshots are from my own database system
 No AI tools (ChatGPT, etc.) were used to generate solutions or commands
No collaboration, copying, or sharing occurred with other students
No screenshots or repositories were reused from classmates
This work was completed in full accordance with AUCA's academic integrity policies
I understand that violation of these principles results in ZERO marks


















