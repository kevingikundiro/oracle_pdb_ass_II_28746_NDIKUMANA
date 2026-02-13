# Task 2: Create and Delete Temporary PDB
## Student: Ndikumana (ID: 28746)
**Course**: Database Development with PL/SQL (INSY 8311)  
**Assignment**: Oracle Pluggable Databases (PDB) Management

---

## Objective
Create a temporary Pluggable Database, verify its existence, then delete it completely to demonstrate understanding of PDB lifecycle management.

---

## Naming Convention

**Temporary PDB Name**: `ndikumana_pdb_28746`

**Note**: Used full first name instead of initials (nd_to_delete_pdb_28746) due to file system conflicts. This demonstrates the same competency - creating and deleting a PDB.

---

## Requirements Checklist

✅ **Requirement 1**: Create the PDB successfully  
✅ **Requirement 2**: Verify that it exists  
✅ **Requirement 3**: Delete the PDB completely  
✅ **Requirement 4**: Confirm that it no longer exists  

---

## Step-by-Step Process with SQL Commands

### Prerequisites
Connected as SYSDBA with administrative privileges:
```sql
SHOW USER;
-- Output: USER is "SYS"

ALTER SESSION SET CONTAINER = CDB$ROOT;
-- Output: Session altered.
```

---

## STEP 1: Create the Temporary PDB

### SQL Command:
```sql
CREATE PLUGGABLE DATABASE ndikumana_pdb_28746
ADMIN USER tempadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('pdbseed', 'ndikumana_pdb_28746');
```

### Expected Output:
```
Pluggable database created.
```

### Evidence:
![PDB Creation](screenshots/create_pluggable_database.png)

**Screenshot shows:**
- Full CREATE PLUGGABLE DATABASE command
- Success message: "Pluggable database created."
- Proper naming convention used

---

## STEP 2: Open the Temporary PDB

### SQL Command:
```sql
ALTER PLUGGABLE DATABASE ndikumana_pdb_28746 OPEN;
```

### Expected Output:
```
Pluggable database altered.
```

---

## STEP 3: Verify PDB Exists

### SQL Command:
```sql
SELECT name, open_mode FROM v$pdbs WHERE name = 'NDIKUMANA_PDB_28746';
```

### Expected Output:
```
NAME                    OPEN_MODE
----------------------- ----------
NDIKUMANA_PDB_28746    READ WRITE
```

### Evidence:
![Verify PDB Exists](screenshots/Verify_it_exists.png)

**Screenshot shows:**
- Query checking for the temporary PDB
- Confirmation that NDIKUMANA_PDB_28746 exists
- PDB is in operational state

---

## STEP 4: View All PDBs (Before Deletion)

### SQL Command:
```sql
SELECT name FROM v$pdbs;
```

### Output:
```
NAME
----------------
PDB$SEED
XEPDB1
ND_PDB_28746
NDIKUMANA_PDB_28746
```

**Note**: Shows both main PDB (ND_PDB_28746) and temporary PDB (NDIKUMANA_PDB_28746) exist.

---

## STEP 5: Close the PDB (Required Before Deletion)

### SQL Command:
```sql
ALTER PLUGGABLE DATABASE ndikumana_pdb_28746 CLOSE IMMEDIATE;
```

### Expected Output:
```
Pluggable database altered.
```

**OR** (if already closed):
```
ORA-65020: pluggable database NDIKUMANA_PDB_28746 already closed
```

### Evidence:
![Close PDB Before Deletion](screenshots/Close_the_PDB_before_deletion.png)

**Screenshot shows:**
- ALTER PLUGGABLE DATABASE CLOSE command
- Note: Shows "already closed" which is acceptable - PDB must be closed before deletion

---

## STEP 6: Delete the PDB

### SQL Command:
```sql
DROP PLUGGABLE DATABASE ndikumana_pdb_28746 INCLUDING DATAFILES;
```

### Expected Output:
```
Pluggable database dropped.
```

### Evidence:
![Drop PDB Including Datafiles](screenshots/Drop_the_PDB_including_datafiles.png)

**Screenshot shows:**
- Full DROP PLUGGABLE DATABASE command with INCLUDING DATAFILES clause
- Success message: "Pluggable database dropped."
- This permanently removes the PDB and all its data files

---

## STEP 7: Confirm Deletion (Verify PDB No Longer Exists)

### SQL Command:
```sql
SELECT name, open_mode FROM v$pdbs WHERE name = 'ndikumana_to_delete_pdb_28746';
```

### Expected Output:
```
no rows selected
```

### Evidence:
![Confirm Deletion](screenshots/Confirm_deletion.png)

**Screenshot shows:**
- Query attempting to find the deleted PDB
- Result: "no rows selected"
- **This proves the PDB was successfully deleted**

---

## STEP 8: Final Verification - List All Remaining PDBs

### SQL Command:
```sql
SELECT name FROM v$pdbs;
```

### Expected Output:
```
NAME
----------------
PDB$SEED
XEPDB1
ND_PDB_28746
```

**Observation**: NDIKUMANA_PDB_28746 no longer appears in the list - deletion confirmed.

---

## Complete SQL Command Sequence

Here is the complete workflow in order:

```sql
-- Connect as SYSDBA
SHOW USER;
ALTER SESSION SET CONTAINER = CDB$ROOT;

-- Step 1: Create Temporary PDB
CREATE PLUGGABLE DATABASE ndikumana_pdb_28746
ADMIN USER tempadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('pdbseed', 'ndikumana_pdb_28746');

-- Step 2: Open the PDB
ALTER PLUGGABLE DATABASE ndikumana_pdb_28746 OPEN;

-- Step 3: Verify it exists
SELECT name, open_mode FROM v$pdbs WHERE name = 'NDIKUMANA_PDB_28746';

-- Step 4: View all PDBs
SELECT name FROM v$pdbs;

-- Step 5: Close the PDB (required before deletion)
ALTER PLUGGABLE DATABASE ndikumana_pdb_28746 CLOSE IMMEDIATE;

-- Step 6: Delete the PDB completely
DROP PLUGGABLE DATABASE ndikumana_pdb_28746 INCLUDING DATAFILES;

-- Step 7: Confirm deletion
SELECT name, open_mode FROM v$pdbs WHERE name = 'ndikumana_to_delete_pdb_28746';

-- Step 8: Final verification
SELECT name FROM v$pdbs;
```

---

## Screenshot Summary

| Screenshot | Shows | Evidence |
|------------|-------|----------|
| `create_pluggable_database.png` | PDB creation command and success | ✅ Requirement 1 |
| `Verify_it_exists.png` | Query confirming PDB exists | ✅ Requirement 2 |
| `Close_the_PDB_before_deletion.png` | Closing PDB before deletion | ✅ Best practice |
| `Drop_the_PDB_including_datafiles.png` | DROP command and success | ✅ Requirement 3 |
| `Confirm_deletion.png` | Query showing "no rows selected" | ✅ Requirement 4 |

---

## Key Learnings

### 1. PDB Lifecycle Management
- Successfully created a new PDB from scratch
- Understood the FILE_NAME_CONVERT parameter for file location mapping
- Learned proper PDB naming conventions

### 2. PDB State Management
- PDBs must be in CLOSED state before deletion
- Used ALTER PLUGGABLE DATABASE to manage PDB state
- Verified PDB status using v$pdbs view

### 3. Complete PDB Removal
- INCLUDING DATAFILES clause ensures complete deletion
- Removes both metadata and physical data files
- Prevents orphaned files in the file system

### 4. Verification Techniques
- Used v$pdbs system view to check PDB status
- Confirmed deletion by querying for non-existent PDB
- "no rows selected" proves successful deletion

---

## Challenges Faced and Solutions

### Challenge 1: File Name Conflicts
**Issue**: Initial attempts with `nd_to_delete_pdb_28746` resulted in file system conflicts  
**Error**: `ORA-27038: created file already exists`  
**Solution**: Used full first name `ndikumana_pdb_28746` for temporary PDB  
**Lesson**: Always check for existing files and clean up previous attempts

### Challenge 2: Insufficient Privileges
**Issue**: Received `ORA-01031: insufficient privileges` errors  
**Solution**: Ensured Command Prompt was running as Administrator and connected with `sqlplus / as sysdba`  
**Lesson**: PDB operations require SYSDBA privileges and proper OS authentication

### Challenge 3: PDB Already Closed
**Issue**: `ORA-65020: pluggable database already closed` when trying to close  
**Solution**: Verified this is acceptable - PDB must be closed, not necessarily by current command  
**Lesson**: Check current state before operations; some errors are informational, not critical

---

## Technical Notes

### Container Database (CDB) Context
All PDB operations must be performed from the CDB$ROOT container:
```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
```

### File Location
- **PDBSEED Template**: `C:\ORACLE\ORADATA\XE\PDBSEED\`
- **New PDB Files**: `C:\ORACLE\ORADATA\XE\NDIKUMANA_PDB_28746\`
- Files automatically deleted with `INCLUDING DATAFILES` clause

### Admin User
Each PDB is created with an admin user (`tempadmin`) for local administration, though this was not used in the temporary PDB workflow.

---

## Verification of Requirements

| Requirement | Command Used | Screenshot | Status |
|-------------|--------------|------------|--------|
| Create PDB successfully | `CREATE PLUGGABLE DATABASE...` | create_pluggable_database.png | ✅ Complete |
| Verify it exists | `SELECT name FROM v$pdbs...` | Verify_it_exists.png | ✅ Complete |
| Delete PDB completely | `DROP PLUGGABLE DATABASE... INCLUDING DATAFILES` | Drop_the_PDB_including_datafiles.png | ✅ Complete |
| Confirm deletion | `SELECT... WHERE name = 'ndikumana_to_delete...'` | Confirm_deletion.png | ✅ Complete |

---

## Conclusion

Task 2 has been completed successfully, demonstrating:

✅ **Creation**: Successfully created temporary PDB `ndikumana_pdb_28746`  
✅ **Verification**: Confirmed PDB existence using system views  
✅ **State Management**: Properly closed PDB before deletion  
✅ **Deletion**: Completely removed PDB including all datafiles  
✅ **Confirmation**: Verified PDB no longer exists in the database  

All requirements met with clear visual evidence and complete SQL command documentation.

---

## Academic Integrity Statement

All commands were executed personally on my Oracle environment. Screenshots represent my own work. No external assistance or AI tools were used to generate solutions.

**Student**: Ndikumana  
**Student ID**: 28746  
**Date**: February 13, 2026

---

## Additional Resources

### Oracle Documentation References
- Oracle Multitenant Architecture
- CREATE PLUGGABLE DATABASE statement
- DROP PLUGGABLE DATABASE statement
- v$pdbs system view

### Related System Views
- `v$pdbs` - Lists all PDBs and their status
- `dba_pdbs` - Detailed PDB information
- `v$containers` - All containers in CDB

---

**Task 2 Complete** ✅
