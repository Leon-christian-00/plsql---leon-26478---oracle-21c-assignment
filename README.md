# Oracle Database 21c — Assignment II Report  
**Student Name**:ABUMUKIZA Leon Christian  
**Student ID**: 26478  
**Course**: PL/SQL & Database Systems  
**Date**: October 6, 2025  

---

## 📌 Overview

This report documents the successful completion of Assignment II, which required:
1. Creating a new Pluggable Database (PDB) with a custom name and admin user.
2. Creating and then deleting a second PDB for demonstration.
3. Configuring Oracle Enterprise Manager (EM) Express and logging in with the student account.

All tasks were performed on **Oracle Database 21c Enterprise Edition** installed on **Windows 10/11**.

Per instructor guidance, the surname "Abumukiza" was excluded. The first name used is **Leon**, resulting in:
- **PDB Name**: `le_pdb_26478`
- **Admin User**: `leon_plsqlauca_26478`

---

## ✅ Task 1: Create a New Pluggable Database (PDB)

### Steps Performed

1. Connected as `SYSDBA` to the CDB root.
2. Created PDB `le_pdb_26478` with admin user `leon_plsqlauca_26478`.
3. Opened the PDB and saved its state for auto-start on reboot.

### Key Command Used

```sql
CREATE PLUGGABLE DATABASE le_pdb_26478
ADMIN USER leon_plsqlauca_26478 IDENTIFIED BY password123
ROLES = (CONNECT, RESOURCE)
DEFAULT TABLESPACE USERS
DATAFILE 'C:\oracle\oradata\ORCL\le_pdb_26478\le_pdb_26478.dbf'
SIZE 100M AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED
FILE_NAME_CONVERT = (
    'C:\oracle\oradata\ORCL\pdbseed\', 
    'C:\oracle\oradata\ORCL\le_pdb_26478\'
);

ALTER PLUGGABLE DATABASE le_pdb_26478 OPEN;
ALTER PLUGGABLE DATABASE le_pdb_26478 SAVE STATE;
```

### Screenshot

<!-- successful PDB creation and OPEN/SAVE STATE commands -->
[Task 1: PDB le_pdb_26478 created and opened](![alt text](<screenshoots/1 connecting to database.png>)
![alt text](<screenshoots/2 creating pluggable database 26478.png>)
![alt text](<screenshoots/3 - Show successful creation + open state..png>)
![alt text](<screenshoots/10 - ADDING FILES AND TABLE IN PDB.png>)
---

## ✅ Task 2: Create and Delete a PDB

### Steps Performed

1. Created a temporary PDB named `le_to_delete_pdb_26478`.
2. Verified it was open and registered.
3. Deleted the PDB permanently, including all datafiles.

### Key Commands Used

```sql
-- Create
CREATE PLUGGABLE DATABASE le_to_delete_pdb_26478
ADMIN USER leon_to_delete_plsqlauca_26478 IDENTIFIED BY password123
ROLES = (CONNECT, RESOURCE)
DEFAULT TABLESPACE USERS
DATAFILE 'C:\oracle\oradata\ORCL\le_to_delete_pdb_26478\le_to_delete_pdb_26478.dbf'
SIZE 100M AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED
FILE_NAME_CONVERT = (
    'C:\oracle\oradata\ORCL\pdbseed\', 
    'C:\oracle\oradata\ORCL\le_to_delete_pdb_26478\'
);

ALTER PLUGGABLE DATABASE le_to_delete_pdb_26478 OPEN;

-- Delete
ALTER PLUGGABLE DATABASE le_to_delete_pdb_26478 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE le_to_delete_pdb_26478 INCLUDING DATAFILES;
```

### Screenshots

<!-- INSERT SCREENSHOT 1: Show successful creation and opening of le_to_delete_pdb_26478 -->
[Task 2: PDB le_to_delete_pdb_26478 created]
![alt text](<screenshoots/4 - creation of “to-delete” PDB.png>)

<!-- INSERT SCREENSHOT 2: Show successful DROP command -->
[Task 2: PDB le_to_delete_pdb_26478 deleted]
![alt text](<screenshoots/5 - to open to delete pluggable database.png>)
![alt text](<screenshoots/6 - altering and Show the DROP command executed successfully.png>)

---

## ✅ Task 3: Configure Oracle Enterprise Manager (EM) Express

### Steps Performed

1. Enabled EM Express on port `5501` (port `5500` was in use by `tnslsnr.exe`).
2. Granted `DBA` role to `leon_plsqlauca_26478` to allow EM login.
3. Accessed EM Express via browser and logged in with student credentials.

### Key Commands Used

```sql
-- Enable EM Express on port 5501
ALTER SESSION SET CONTAINER = le_pdb_26478;
EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5501);

-- Grant DBA for EM access (class project only)
GRANT DBA TO leon_plsqlauca_26478 CONTAINER=CURRENT;
```

Access URL: `https://localhost:5501/em`

### Screenshot

<!-- EM Express dashboard showing username "leon_plsqlauca_26478" clearly -->
[Task 3: EM Express logged in as leon_plsqlauca_26478]
![alt text](<screenshoots/9 - Configure OEM  EM Express AND login as user.png>)

---

## 🛠️ Issues Encountered & Solutions

| Issue | Error Code | Solution |
|------|-----------|--------|
| Invalid common user name | ORA-65096 | Switched to PDB before creating user; used local user (no `C##`) |
| No privileges on tablespace | ORA-01950 | Ran `ALTER USER ... QUOTA UNLIMITED ON USERS` |
| TNS: no listener | ORA-12541 | Started `OracleOraDB21Home1TNSListener` service; used `lsnrctl start` |
| FILE_NAME_CONVERT missing | ORA-65016 | Added `FILE_NAME_CONVERT` clause to PDB creation |
| Access denied for EM config | ORA-31050 | Ran `SETHTTPSPORT` as `SYSDBA` inside PDB |
| Invalid EM credentials | — | Granted `DBA` role to student user |
| Port conflict | ORA-44718 | Changed EM port from `5500` → `5501` |

---
![alt text](<screenshoots/11 - GRANT EMS OMS.png>)

---

> **"Every expert was once a beginner who didn’t quit."**  
> — Leon Christian, Student ID 26478  
> Oracle Database 21c Assignment II — Completed ✅


---