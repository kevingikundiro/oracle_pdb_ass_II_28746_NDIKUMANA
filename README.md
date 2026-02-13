NAMES:NDIKUMANA GIKUNDIRO KEVIN
STUEDENT ID:28746
PL/SQL ASSIGNMENT II(PLUGGABLE DATABASE)

TASK1: Create a New Pluggable Database
# Screenshot Guide for Oracle PDB Assignment
## Student: Ndikumana (Initials: ND, ID: 28746)

---

## TASK 1: Main PDB Screenshots Required

### SCREENSHOT #1: PDB Creation Command
**What to show:** The CREATE PLUGGABLE DATABASE command and its success message

**Run this command:**
```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;

CREATE PLUGGABLE DATABASE nd_pdb_28746
ADMIN USER pdb_admin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('C:\ORACLE\ORADATA\XE\PDBSEED\', 'C:\ORACLE\ORADATA\XE\ND_PDB_28746\');
```

**Expected Output:**
```
Pluggable database created.
```

**📸 TAKE SCREENSHOT showing:**
- The full CREATE PLUGGABLE DATABASE command
- "Pluggable database created." message
- The SQL> prompt

**If PDB already exists:**
You can document this by showing:
```sql
SELECT pdb_name, status, creation_time 
FROM dba_pdbs 
WHERE pdb_name = 'ND_PDB_28746';
```

---

### SCREENSHOT #2: PDB Open State
**What to show:** That nd_pdb_28746 is in READ WRITE mode

**Run this command:**
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

**📸 TAKE SCREENSHOT showing:**
- The SELECT command
- All PDBs listed
- ND_PDB_28746 with OPEN_MODE = READ WRITE
- Make sure the full query and results are visible

---

### SCREENSHOT #3: User Created Inside the PDB
**What to show:** That user ndikumana_plsqlauca_28746 was created in nd_pdb_28746

**Step 1: Switch to the PDB**
```sql
ALTER SESSION SET CONTAINER = nd_pdb_28746;
SHOW CON_NAME;
```

**Step 2: Check if user exists (or create if needed)**

**Option A - If user already exists, just verify:**
```sql
SELECT username, account_status, created 
FROM dba_users 
WHERE username = 'NDIKUMANA_PLSQLAUCA_28746';
```

**Option B - If user doesn't exist, create it:**
```sql
CREATE USER ndikumana_plsqlauca_28746 IDENTIFIED BY Oracle123;
GRANT CONNECT, RESOURCE, DBA TO ndikumana_plsqlauca_28746;
GRANT UNLIMITED TABLESPACE TO ndikumana_plsqlauca_28746;

SELECT username, account_status, created 
FROM dba_users 
WHERE username = 'NDIKUMANA_PLSQLAUCA_28746';
```

**Expected Output:**
```
USERNAME                      ACCOUNT_STATUS       CREATED
----------------------------- -------------------- ---------
NDIKUMANA_PLSQLAUCA_28746    OPEN                 13-FEB-26
```

**📸 TAKE SCREENSHOT showing:**
- SHOW CON_NAME output showing you're in ND_PDB_28746
- SELECT query
- Username: NDIKUMANA_PLSQLAUCA_28746
- Account status
- Make sure username is clearly visible

---

### SCREENSHOT #4 (BONUS): Test User Connection
**What to show:** Successfully connecting as your user

**Step 1: Exit SQL*Plus**
```sql
EXIT;
```

**Step 2: Connect as your user**
```cmd
sqlplus ndikumana_plsqlauca_28746/Oracle123@localhost:1521/nd_pdb_28746
```

**Expected Output:**
```
Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production

SQL>
```

**Step 3: Verify who you are**
```sql
SHOW USER;
SELECT SYS_CONTEXT('USERENV', 'CURRENT_USER') FROM DUAL;
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') FROM DUAL;
```

**Expected Output:**
```
USER is "NDIKUMANA_PLSQLAUCA_28746"

SYS_CONTEXT('USERENV','CURRENT_USER')
--------------------------------------
NDIKUMANA_PLSQLAUCA_28746

SYS_CONTEXT('USERENV','CON_NAME')
----------------------------------
ND_PDB_28746
```

**📸 TAKE SCREENSHOT showing:**
- The sqlplus connection command
- "Connected to:" message
- SHOW USER output showing NDIKUMANA_PLSQLAUCA_28746
- The PDB name (ND_PDB_28746)

---

## COMPLETE WORKFLOW FOR ALL TASK 1 SCREENSHOTS

**Copy and paste these commands in order:**

```sql
-- Make sure you're running Command Prompt as Administrator
-- Connect as SYSDBA

ALTER SESSION SET CONTAINER = CDB$ROOT;

-- SCREENSHOT #1: PDB Creation (if not already created)
CREATE PLUGGABLE DATABASE nd_pdb_28746
ADMIN USER pdb_admin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('C:\ORACLE\ORADATA\XE\PDBSEED\', 'C:\ORACLE\ORADATA\XE\ND_PDB_28746\');
-- 📸 SCREENSHOT after you see "Pluggable database created."

-- Open the PDB
ALTER PLUGGABLE DATABASE nd_pdb_28746 OPEN;

-- SCREENSHOT #2: PDB Open State
SELECT name, open_mode FROM v$pdbs;
-- 📸 SCREENSHOT showing ND_PDB_28746 is READ WRITE

-- Switch to the PDB
ALTER SESSION SET CONTAINER = nd_pdb_28746;
SHOW CON_NAME;

-- Create user (if not exists)
CREATE USER ndikumana_plsqlauca_28746 IDENTIFIED BY Oracle123;
GRANT CONNECT, RESOURCE, DBA TO ndikumana_plsqlauca_28746;
GRANT UNLIMITED TABLESPACE TO ndikumana_plsqlauca_28746;

-- SCREENSHOT #3: User Created
SELECT username, account_status, created 
FROM dba_users 
WHERE username = 'NDIKUMANA_PLSQLAUCA_28746';
-- 📸 SCREENSHOT showing the username clearly

EXIT;
```

```cmd
-- SCREENSHOT #4: User Connection Test
sqlplus ndikumana_plsqlauca_28746/Oracle123@localhost:1521/nd_pdb_28746
-- 📸 SCREENSHOT after successful connection
```

```sql
SHOW USER;
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') FROM DUAL;
-- 📸 SCREENSHOT showing you're connected as NDIKUMANA_PLSQLAUCA_28746

EXIT;
```

---

## IF YOUR PDB ALREADY EXISTS

If you already created nd_pdb_28746, you can still get proper screenshots:

```sql
-- Reconnect as SYSDBA
ALTER SESSION SET CONTAINER = CDB$ROOT;

-- For Screenshot #1 (proof it was created)
SELECT pdb_name, status, creation_time, open_mode 
FROM dba_pdbs 
WHERE pdb_name = 'ND_PDB_28746';
-- 📸 SCREENSHOT showing creation details

-- For Screenshot #2 (PDB is open)
SELECT name, open_mode FROM v$pdbs;
-- 📸 SCREENSHOT showing ND_PDB_28746 READ WRITE

-- For Screenshot #3 (user exists)
ALTER SESSION SET CONTAINER = nd_pdb_28746;
SHOW CON_NAME;

SELECT username, account_status, created 
FROM dba_users 
WHERE username = 'NDIKUMANA_PLSQLAUCA_28746';
-- 📸 SCREENSHOT showing user clearly

-- For Screenshot #4 (connect as user)
EXIT;
sqlplus ndikumana_plsqlauca_28746/Oracle123@localhost:1521/nd_pdb_28746
SHOW USER;
-- 📸 SCREENSHOT showing connection
```

---

## SCREENSHOT QUALITY CHECKLIST

For each screenshot, make sure:

✅ **Full SQL Command Visible** - Don't crop the command
✅ **Complete Output Visible** - Show the full results
✅ **SQL> Prompt Visible** - Shows you're in SQL*Plus
✅ **Usernames/PDB Names Clear** - Text must be readable
✅ **No Overlapping Windows** - Full screen or maximize SQL*Plus
✅ **Timestamp/Context** - If possible, show it's your session

---

## WHAT EACH SCREENSHOT MUST PROVE

| Screenshot | Must Show | Why |
|------------|-----------|-----|
| #1 | CREATE PLUGGABLE DATABASE command or PDB details | Proves you created nd_pdb_28746 |
| #2 | SELECT name, open_mode showing READ WRITE | Proves PDB is operational |
| #3 | Username NDIKUMANA_PLSQLAUCA_28746 in DBA_USERS | Proves user created in correct PDB |
| #4 | Connected as ndikumana_plsqlauca_28746 | Proves user account works |

---

## YOUR SPECIFIC NAMING (DON'T CHANGE THESE!)

- **PDB Name**: `nd_pdb_28746`
- **Username**: `ndikumana_plsqlauca_28746`
- **Container**: Must show `ND_PDB_28746` when doing user operations

---

**Execute these commands now and take the screenshots! 📸**
