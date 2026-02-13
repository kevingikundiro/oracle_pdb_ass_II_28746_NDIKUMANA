Oracle Pluggable Database (PDB) Management Assignment

Course: Database Development with PL/SQL (INSY 8311)  
Instructor: Eric Maniraguha  
Student Name: NDIKUMANA GIKUNDIRO KEVIN  
Student ID: 28746  
Submission Date: 16.02.2026

---

 Table of Contents
1. Assignment Overview](assignment-overview)
2. Oracle Environment](oracle-environment)
3. Task 1: Create New Pluggable Database](task-1-create-new-pluggable-database)
4. Task 2: Create and Delete Temporary PDB](task-2-create-and-delete-temporary-pdb)
5. Task 3: Oracle Enterprise Manager](Task-3-oracle-enterprise-manager)
6. Challenges and Solutions](challenges-and-solutions)
7. Conclusion](conclusion)
8. Academic Integrity Statement](academic-integrity-statement)

 Oracle Environment

Oracle Database Version: [e.g., Oracle Database 21c Enterprise Edition]  
Operating System: [e.g., Windows 11 / Ubuntu 22.04 / macOS]  
Tools Used:
- SQL*Plus [version]
- Oracle SQL Developer [version]
- Oracle Enterprise Manager (OEM)


**Connection Details:**
- Host: localhost
- Port: 1521
- Service Name: XE

# Complete SQL Commands for Oracle PDB Assignment
## Student: Ndikumana (ID: 28746)

---

## PREREQUISITE: Connect as SYSDBA with Admin Rights

1. **Run Command Prompt as Administrator**
2. Connect:
```cmd
sqlplus / as sysdba
```

---

TASK 1: CREATE MAIN PLUGGABLE DATABASE

 Step 1.1: Switch to CDB Root
sql
ALTER SESSION SET CONTAINER = CDB$ROOT;


### Step 1.2: Check Current PDBs`sql
SELECT name, open_mode FROM v$pdbs;

📸 SCREENSHOT #1: Current PDBs (before creating ND_pdb_28746)



 Step 1.3: Create Main PDB
sql
CREATE PLUGGABLE DATABASE ND_pdb_28746
ADMIN USER pdb_admin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('C:\ORACLE\ORADATA\XE\PDBSEED\', 'C:\ORACLE\ORADATA\XE\ND_PDB_28746\');


Expected Output

Pluggable database created.


📸 SCREENSHOT #2: PDB creation success message**


 Step 1.4: Open the PDB

ALTER PLUGGABLE DATABASE nd_pdb_28746 OPEN;

Expected Output:

Pluggable database altered.




 Step 1.5: Verify PDB is Open
sql
SELECT name, open_mode FROM v$pdbs;

Expected Output:

NAME                    OPEN_MODE
----------------------- ----------
PDB$SEED               READ ONLY
XEPDB1                 READ WRITE
ND_PDB_28746           READ WRITE


📸 SCREENSHOT #3: Shows nd_pdb_28746 is READ WRITE**



 Step 1.6: Set PDB to Auto-Start

ALTER PLUGGABLE DATABASE nd_pdb_28746 SAVE STATE;



Step 1.7: Connect to Your PDB
sql
ALTER SESSION SET CONTAINER = nd_pdb_28746;




 Step 1.8: Verify You're Inside the PDB

SHOW CON_NAME;

Expected Output:

CON_NAME
------------------------------
ND_PDB_28746



 Step 1.9: Create Your User
sql
CREATE USER ndikumana_plsqlauca_28746 IDENTIFIED BY Oracle123;


Expected Output:

User created.



Step 1.10: Grant Privileges to User`sql
GRANT CONNECT, RESOURCE, DBA TO ndikumana_plsqlauca_28746;
GRANT UNLIMITED TABLESPACE TO ndikumana_plsqlauca_28746;


Expected Output:

Grant succeeded.
Grant succeeded.




Step 1.11: Verify User Creation
sql
SELECT username FROM dba_users WHERE username = 'NDIKUMANA_PLSQLAUCA_28746';


Expected Output:

USERNAME
----------------------------
NDIKUMANA_PLSQLAUCA_28746


📸 SCREENSHOT #4: Shows your user exists**



### Step 1.12: Exit and Test User Connection
sql
EXIT;


From Command Prompt:
md
sqlplus ndikumana_plsqlauca_28746/Oracle123@localhost:1521/nd_pdb_28746

**Expected Output:**
```
Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0
```

**📸 SCREENSHOT #5: Successful connection as your user**

Then exit:
```sql
EXIT;
```

---

## TASK 2: CREATE AND DELETE TEMPORARY PDB

### Step 2.1: Reconnect as SYSDBA
```cmd
sqlplus / as sysdba
```

---

### Step 2.2: Switch to CDB Root
```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
```

---

### Step 2.3: Create Temporary PDB
```sql
CREATE PLUGGABLE DATABASE nd_to_delete_pdb_28746
ADMIN USER tempadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('C:\ORACLE\ORADATA\XE\PDBSEED\', 'C:\ORACLE\ORADATA\XE\ND_TO_DELETE_PDB_28746\');
```

**Expected Output:**
```
Pluggable database created.
```

**📸 SCREENSHOT #6: Temporary PDB creation success**

---

### Step 2.4: Open Temporary PDB
```sql
ALTER PLUGGABLE DATABASE nd_to_delete_pdb_28746 OPEN;
```

**Expected Output:**
```
Pluggable database altered.
```

---

### Step 2.5: Verify Both PDBs Exist
```sql
SELECT name, open_mode FROM v$pdbs;
```

**Expected Output:**
```
NAME                          OPEN_MODE
----------------------------- ----------
PDB$SEED                      READ ONLY
XEPDB1                        READ WRITE
ND_PDB_28746                  READ WRITE
ND_TO_DELETE_PDB_28746        READ WRITE
```

**📸 SCREENSHOT #7: Shows BOTH nd_pdb_28746 AND nd_to_delete_pdb_28746**

---

### Step 2.6: Close Temporary PDB
```sql
ALTER PLUGGABLE DATABASE nd_to_delete_pdb_28746 CLOSE IMMEDIATE;
```

**Expected Output:**
```
Pluggable database altered.
```

---

### Step 2.7: Delete Temporary PDB
```sql
DROP PLUGGABLE DATABASE nd_to_delete_pdb_28746 INCLUDING DATAFILES;
```

**Expected Output:**
```
Pluggable database dropped.
```

**📸 SCREENSHOT #8: PDB deletion success message**

---

### Step 2.8: Verify Deletion
```sql
SELECT name, open_mode FROM v$pdbs;
```

**Expected Output:**
```
NAME                    OPEN_MODE
----------------------- ----------
PDB$SEED               READ ONLY
XEPDB1                 READ WRITE
ND_PDB_28746           READ WRITE
```

**📸 SCREENSHOT #9: Shows nd_to_delete_pdb_28746 is GONE**

---

## TASK 3: ORACLE ENTERPRISE MANAGER

### Access OEM Dashboard

1. **Open Browser**
2. **Go to:** `https://localhost:5500/em`
3. **Login:**
   - Username: `SYS`
   - Password: `[Your SYS password]`
   - Connect As: `SYSDBA`

4. **Navigate to Database Home**

**📸 SCREENSHOT #10: OEM Dashboard showing:**
- Database status (your current screenshot is good!)
- Resources section
- Data Storage showing ND_PDB_28746
- Top of page showing you're logged in as SYS

---

## ADDITIONAL USEFUL QUERIES

### Check CDB Name
```sql
SELECT NAME, CDB FROM V$DATABASE;
```

### Check Container You're In
```sql
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') FROM DUAL;
```

### List All PDBs with Details
```sql
SELECT pdb_id, pdb_name, status, creation_time 
FROM dba_pdbs 
ORDER BY pdb_id;
```

### Check All Users in Current Container
```sql
SELECT username, account_status, created 
FROM dba_users 
ORDER BY created DESC;
```

---

## SUMMARY OF ALL SCREENSHOTS NEEDED

1. ✅ **Screenshot #1**: Current PDBs before creation
2. ✅ **Screenshot #2**: PDB creation success message
3. ✅ **Screenshot #3**: nd_pdb_28746 is READ WRITE
4. ✅ **Screenshot #4**: User ndikumana_plsqlauca_28746 exists
5. ✅ **Screenshot #5**: Connected as ndikumana_plsqlauca_28746
6. ✅ **Screenshot #6**: Temporary PDB creation success
7. ✅ **Screenshot #7**: Both PDBs exist
8. ✅ **Screenshot #8**: Temporary PDB deletion success
9. ✅ **Screenshot #9**: Temporary PDB is gone
10. ✅ **Screenshot #10**: OEM Dashboard (YOU ALREADY HAVE THIS!)

---

## QUICK COPY-PASTE VERSION (All Commands in Order)

```sql
-- TASK 1: CREATE MAIN PDB
ALTER SESSION SET CONTAINER = CDB$ROOT;
SELECT name, open_mode FROM v$pdbs;  -- SCREENSHOT #1

CREATE PLUGGABLE DATABASE nd_pdb_28746
ADMIN USER pdb_admin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('C:\ORACLE\ORADATA\XE\PDBSEED\', 'C:\ORACLE\ORADATA\XE\ND_PDB_28746\');
-- SCREENSHOT #2

ALTER PLUGGABLE DATABASE nd_pdb_28746 OPEN;
SELECT name, open_mode FROM v$pdbs;  -- SCREENSHOT #3
ALTER PLUGGABLE DATABASE nd_pdb_28746 SAVE STATE;

ALTER SESSION SET CONTAINER = nd_pdb_28746;
SHOW CON_NAME;

CREATE USER ndikumana_plsqlauca_28746 IDENTIFIED BY Oracle123;
GRANT CONNECT, RESOURCE, DBA TO ndikumana_plsqlauca_28746;
GRANT UNLIMITED TABLESPACE TO ndikumana_plsqlauca_28746;

SELECT username FROM dba_users WHERE username = 'NDIKUMANA_PLSQLAUCA_28746';  -- SCREENSHOT #4

EXIT;
```

```cmd
sqlplus ndikumana_plsqlauca_28746/Oracle123@localhost:1521/nd_pdb_28746
-- SCREENSHOT #5
EXIT;
```

```sql
-- TASK 2: CREATE AND DELETE TEMPORARY PDB
ALTER SESSION SET CONTAINER = CDB$ROOT;

CREATE PLUGGABLE DATABASE nd_to_delete_pdb_28746
ADMIN USER tempadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('C:\ORACLE\ORADATA\XE\PDBSEED\', 'C:\ORACLE\ORADATA\XE\ND_TO_DELETE_PDB_28746\');
-- SCREENSHOT #6

ALTER PLUGGABLE DATABASE nd_to_delete_pdb_28746 OPEN;
SELECT name, open_mode FROM v$pdbs;  -- SCREENSHOT #7

ALTER PLUGGABLE DATABASE nd_to_delete_pdb_28746 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE nd_to_delete_pdb_28746 INCLUDING DATAFILES;  -- SCREENSHOT #8

SELECT name, open_mode FROM v$pdbs;  -- SCREENSHOT #9
```

---

## TROUBLESHOOTING

### If you get "ORA-01031: insufficient privileges"
- Run Command Prompt as Administrator
- Use: `sqlplus / as sysdba`

### If you get "file already exists" error
- Delete folder: `C:\ORACLE\ORADATA\XE\ND_TO_DELETE_PDB_28746\`
- Try CREATE command again

### If PDB won't open
- Check: `SELECT name, open_mode FROM v$pdbs;`
- If it shows "MOUNTED", run: `ALTER PLUGGABLE DATABASE [name] OPEN;`

---

**ALL COMMANDS READY TO USE! 🎯**
