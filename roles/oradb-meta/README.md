oradb-meta
=========

This role colects basic information about each database (CDB and non-CDB, but not PDBs). especially, it finds out whether a database is primary or not primary when dataguard: true parameter is set at the oracle_databases block.

Requirements
------------

This role may be called directly, but can also be included via dependencies from other roles like oradb-manage-db, where for a non-primary DB the dbca is not being started.

Role Variables
--------------

This role defines followin variables:

* via defaults:
  * db_block - either dbh, or item[0] or item.0 depending on the loop variables db_block is called from (can be usefull in other roles as well, just load this role via dependency and you can use db_block also elsewhere.
  * oracle_user_home - copied from other roles, may be placed here centrally at 1 place
  * oracle_profile_name - as above
  * is_dataguard - boolean, is valid only within a loop over oracle_databases (outside always false)
  * is_primary - boolean, always true if not dataguard or run outside loop, otherwise true only on host being primary for a given database the loop is running for.
* via set_facts: (valid on host level from outside loops as well)
  * db_role: array of dict for each database present on the host in format: "- { DB_NAME: Role }". DB_NAME corresponds to oracle_db_name and Role can be one of: "primary", "standby", "unknown"
  * db_status: array of dict for each database present on the host in format: "- { DB_NAME: Status }". DB_NAME corresponds to oracle_db_name and Status can be one of: "OPEN", "MOUNT", "absent" and "broken"
  * primary_for_db: array of dict for each database present on the host in format: "- { DB_NAME: primary_inventory_hostname }". It contains the inventory_hostname of a host, where the DB_NAME is running as primary. Only 

Dependencies
------------

This role does not depend on any other role nor takes any input variables. Ir might be used however as a dependency from other roles (like oradb-manage-db or oradb-manage-dataguard)

The role can be either included explicit via roles: or implicit via dependency from other roles.

License
-------

BSD

Author Information
------------------

Bartlomiej Sowa <bartowl@gmail.com>
