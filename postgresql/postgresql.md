PostgreSQL queries and operations
=================================

-   Get table names from schema **myschema**:

        # select table_name from information_schema.tables where table_schema = 'myschema';

-   Get schema sizes (1 way):

        SELECT schema_name, 
               pg_size_pretty(sum(table_size)::bigint),
               (sum(table_size) / pg_database_size(current_database())) * 100
        FROM (
          SELECT pg_catalog.pg_namespace.nspname as schema_name,
                 pg_relation_size(pg_catalog.pg_class.oid) as table_size
          FROM   pg_catalog.pg_class
             JOIN pg_catalog.pg_namespace ON relnamespace = pg_catalog.pg_namespace.oid
        ) t
        GROUP BY schema_name
        ORDER BY schema_name;
        
-   Get schema sizes (2 way):

        WITH                                                               
        schemas AS (                                                         
        SELECT schemaname as name, sum(pg_relation_size(quote_ident(schemaname) || '.' || quote_ident(tablename)))::bigint as size FROM pg_tables                                              
        GROUP BY schemaname                                             
        ),                                                                                                         
        db AS (
        SELECT pg_database_size(current_database()) AS size
        )                    
        SELECT schemas.name, 
               pg_size_pretty(schemas.size) as absolute_size,
               schemas.size::float / (SELECT size FROM db)  * 100 as relative_size
        FROM schemas;

