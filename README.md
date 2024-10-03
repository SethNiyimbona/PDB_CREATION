README: Oracle Pluggable Database (PDB) Creation and Management
Introduction
A Pluggable Database (PDB) is a self-contained, fully functional Oracle database that resides within a Container Database (CDB). PDBs allow easy provisioning, management, and consolidation of multiple databases. In this guide, we'll create a PDB, perform alterations, and manage users. This README will walk you through the key SQL operations and their outputs.

Enter into SQLPlus
Before starting, enter into SQLPlus as the SYSDBA user with the following command:
sql
copy code
sqlplus sys/seth123@localhost:1521/xe AS SYSDBA
This logs you into Oracle as a DBA, allowing you to manage the databases.

Check Existing PDBs
To check the list of Pluggable Databases (PDBs) within the Container Database (CDB), use:

sql
Copy code
show pdbs;
Output:
sql
Copy code
CON_ID CON_NAME     OPEN MODE  RESTRICTED
------- ------------ ---------- ----------
2       PDB$SEED     READ ONLY  NO
3       XEPDB1       READ WRITE NO
Create a New Pluggable Database (PDB)
We will now create a new PDB called plsql_class2024db. The FILE_NAME_CONVERT clause is used to specify the location of the data files for the new PDB.

sql
Copy code
CREATE PLUGGABLE DATABASE plsql_class2024db
  FROM XEPDB1
  FILE_NAME_CONVERT =
  ('C:\app\PC\product\21c\oradata\XE\XEPDB1',
   'C:\app\PC\product\21c\oradata\XE\plsql_class2024db');
Output:
Copy code
Pluggable database created.
Open the New PDB
To use the PDB, you need to open it. The following command will open the plsql_class2024db PDB:

sql
Copy code
ALTER PLUGGABLE DATABASE plsql_class2024db OPEN;
Output:
Copy code
Pluggable database altered.
Save the State of the PDB
To ensure the PDB automatically opens when the CDB is restarted, you must save its state:

sql
Copy code
ALTER PLUGGABLE DATABASE plsql_class2024db SAVE STATE;
Output:
Copy code
Pluggable database altered.
Create a New User in the PDB
Now that the PDB is open, let's create a new user se_plsqlauca for database operations:

sql
Copy code
CREATE USER se_plsqlauca IDENTIFIED BY admin;
Output:
sql
Copy code
User created.
Create and Delete a PDB
Create a PDB to be Deleted Let's create another PDB named se_to_delete_pdb, which we will later delete:

sql
Copy code
CREATE PLUGGABLE DATABASE se_to_delete_pdb
  FROM XEPDB1
  FILE_NAME_CONVERT =
  ('C:\app\PC\product\21c\oradata\XE\XEPDB1',
   'C:\app\PC\product\21c\oradata\XE\se_to_delete_pdb');
Output:
Copy code
Pluggable database created.
Delete the PDB Before dropping a PDB, it must be closed. First, we close the se_to_delete_pdb PDB:

sql
Copy code
ALTER PLUGGABLE DATABASE se_to_delete_pdb CLOSE IMMEDIATE;
Output:
Copy code
Pluggable database altered.
Finally, drop the se_to_delete_pdb along with its associated data files:

sql
Copy code
DROP PLUGGABLE DATABASE se_to_delete_pdb INCLUDING DATAFILES;
Output:
Copy code
Pluggable database dropped.
Conclusion
This guide demonstrates how to create, open, manage, and delete pluggable databases in Oracle SQLPlus. You now have a functional PDB named plsql_class2024db and a new user se_plsqlauca ready for operations.



