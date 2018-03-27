## Cassandra Security Setup
The following changes are needed on each Cassandra node to enable Authentication.
1. In __cassandra.yaml__ change ```authenticator: AllowAllAuthenticator``` to ```authenticator: PasswordAuthenticator```
1. In __cassandra.yaml__ change ```authorizer: AllowAllAuthorizer``` to ```authorizer: CassandraAuthorizer``` [We want authorization as well along with authentication]
1. Restart Cassandra process.
1. Increase the replication factor of system_auth keyspace to N where N is the number of nodes in the cluster.
Example: cqlsh> ALTER KEYSPACE system_auth WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3};
1. Connect to cql - ```./cqlsh `hostname` -u cassandra -p cassandra``` [*Note:* default superuser credentials are cassandra/cassandra]
1. Create another super user within the same cqlsh session
Example: CREATE USER IF NOT EXISTS adminuser WITH PASSWORD 'adminuserpassword' SUPERUSER; [ Ensure you use SUPERUSER otherwise it will default to NONSUPERUSER]
1. Login as adminuser (created in step 6)
1. Drop the default user Cassandra
1. Create a user for application access with SELECT, MODIFY privileges.



## Common Operations:
__Create User:__

For DBA User/Superuser:

```CREATE USER IF NOT EXISTS cassdba WITH PASSWORD 'cassdba' SUPERUSER;```

For Application User/Non-Superuser:

```CREATE USER IF NOT EXISTS <user_name> WITH PASSWORD '<password>' NOSUPERUSER;```

https://docs.datastax.com/en/cql/3.1/cql/cql_reference/create_user_r.html



__List All Users:__

```LIST USERS;```

https://docs.datastax.com/en/cql/3.1/cql/cql_reference/list_users_r.html



__Alter User:__

```ALTER USER <user_name> WITH PASSWORD '<password>' NOSUPERUSER;```

https://docs.datastax.com/en/cql/3.1/cql/cql_reference/alter_user_r.html



__Drop User:__

```DROP USER IF EXISTS <user_name>;```

https://docs.datastax.com/en/cql/3.1/cql/cql_reference/drop_user_r.html



__Grant Permission:__

```GRANT SELECT PERMISSION ON KEYSPACE <keyspace> TO <user_name>;```

```GRANT MODIFY PERMISSION ON KEYSPACE <keyspace> TO <user_name>;```

https://docs.datastax.com/en/cql/3.1/cql/cql_reference/grant_r.html



__Revoke Permission:__

```REVOKE MODIFY ON KEYSPACE <keyspace> FROM <user_name>;```
https://docs.datastax.com/en/cql/3.1/cql/cql_reference/revoke_r.html



__List Permissions:__

To List All Permissions of a use to a keyspace:
```LIST ALL PERMISSIONS ON  KEYSPACE <keyspace> ;```

To List ALL permissions of a user across all the keyspaces:
```LIST ALL PERMISSIONS OF <user-name>;```

https://docs.datastax.com/en/cql/3.1/cql/cql_reference/list_permissions_r.html
