# All-in-one SchemaSpy bundle.

On the command line just run the following command
to generate a schema for your chosen database :

$ java -jar schemaSpy_5.0.0.jar -t mysql -o library -host localhost -db yourDb -u user -p password
-dp mysql-connector-java-3.1.13-bin.jar

## PostgreSQL Driver links
https://jdbc.postgresql.org/download.html

Important : You need to specify the full class name when using the postgresql driver. You also need to download the correct driver version for your postgresql version.
eg. 

java -jar schemaSpy_5.0.0.jar -t pgsql -host 10.100.71.21[:5432] -db mydb -s public -u username -p password -dp /home/panx/postgresql-8.0-312.jdbc3.jar -o output/
