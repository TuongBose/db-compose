# Docker Database Suite

This repository contains Docker Compose configurations for running multiple database systems and their management tools in Docker containers.

## Databases Included

- **PostgreSQL** with pgAdmin 4
- **MySQL** with phpMyAdmin
- **Microsoft SQL Server**
- **MongoDB** with Mongo Express
- **Cassandra**
- **Oracle**
- **Neo4j** with Neo4j Browser & Bloom

## How to Use

### Running All Databases

To start all database services:

```bash
docker-compose up -d
```

To stop all services:

```bash
docker-compose down
```

### Running Individual Databases

You can also run each database system separately by navigating to their respective folders:

```bash
# For PostgreSQL
cd postgre-compose
docker-compose up -d

# For MySQL
cd mysql-compose
docker-compose up -d

# For SQL Server
cd mssql-compose
docker-compose up -d

# For MongoDB
cd mongo-compose
docker-compose up -d

# For Cassandra
cd cassandra-compose
docker-compose up -d

# For Oracle
cd oracle-compose
docker-compose up -d

# For Neo4j
cd neo4j-compose
docker-compose up -d
```

## Connection Information

### PostgreSQL
- **Host**: localhost
- **Port**: 5432
- **Username**: postgres
- **Password**: postgres
- **Admin Tool**: pgAdmin at http://localhost:5050

### MySQL
- **Host**: localhost
- **Port**: 3306
- **Root Username**: root
- **Root Password**: abcd@1234
- **Default Database**: mydb
- **Admin Tool**: phpMyAdmin at http://localhost:8100

### Microsoft SQL Server
- **Host**: localhost
- **Port**: 1433
- **Username**: sa
- **Password**: abcd@1234

### MongoDB
- **Host**: localhost
- **Port**: 27017
- **Admin Username**: root
- **Admin Password**: PassWord
- **Admin Tool**: Mongo Express at http://localhost:8081

### Apache Cassandra
- **Host**: localhost  
- **Port**: 9042  
- **Cluster Name**: Test Cluster (default)  
- **Admin Tool**: No built-in web UI (you can use DataStax DevCenter, TablePlus, or other third-party tools if needed).  
- **Default CLI Tool**: `cqlsh`

### Oracle
- **Host**: localhost  
- **Port**: 1521   
- **SID**: XEPDB1
- **Username**: sys
- **Password**: MyPassword123
- **Admin Tool**: No built-in web UI (you can use DataStax DevCenter, TablePlus, or other third-party tools if needed).  
- **Default CLI Tool**: `sqlplus`

### Neo4j
* **Host**: localhost
* **HTTP Port**: 7474
* **Bolt Port**: 7687
* **Username**: neo4j
* **Password**: 12345678
* **Plugins Enabled**: APOC, Bloom *(Bloom requires license — optional)*
* **Admin Tool**: Neo4j Browser at [http://localhost:7474](http://localhost:7474)
* **Default Connection URI**: `neo4j://localhost:7687`
* **Default CLI Tool**: `cypher-shell -u neo4j -p 12345678`

## Accessing Containers and Using Databases via Command Line

You can access the containers and interact with each database directly using command-line tools. Below are the instructions for each database system.

### PostgreSQL
To access the PostgreSQL container and use the `psql` command-line tool:

1. **Access the container**:
   ```bash
   docker exec -it <postgresql_container_name> bash
   ```
   Replace `<postgresql_container_name>` with the name of your PostgreSQL container (e.g., `postgres` or check using `docker ps`).

2. **Connect to PostgreSQL**:
   ```bash
   psql -U postgres
   ```
   - Use the username `postgres` and password `postgres` when prompted.
   - You can now run SQL commands like `\l` to list databases or `\c mydb` to connect to a specific database.

### MySQL
To access the MySQL container and use the `mysql` command-line tool:

1. **Access the container**:
   ```bash
   docker exec -it <mysql_container_name> bash
   ```
   Replace `<mysql_container_name>` with the name of your MySQL container (e.g., `mysql` or check using `docker ps`).

2. **Connect to MySQL**:
   ```bash
   mysql -u root -p
   ```
   - Enter the root password `abcd@1234` when prompted.
   - You can now run SQL commands like `SHOW DATABASES;` or `USE mydb;` to work with the database.

### Microsoft SQL Server
To access the SQL Server container and use the `sqlcmd` command-line tool:

1. **Access the container**:
   ```bash
   docker exec -it <mssql_container_name> bash
   ```
   Replace `<mssql_container_name>` with the name of your SQL Server container (e.g., `mssql` or check using `docker ps`).

2. **Connect to SQL Server**:
   ```bash
   /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P 'abcd@1234' -C
   ```
   - This connects to SQL Server with the username `sa` and password `abcd@1234`.
   - You can now run SQL commands like `SELECT @@VERSION; GO` to check the server version or `USE mydb; GO` to switch to a database.

### MongoDB
To access the MongoDB container and use the `mongo` command-line tool:

1. **Access the container**:
   ```bash
   docker exec -it <mongo_container_name> bash
   ```
   Replace `<mongo_container_name>` with the name of your MongoDB container (e.g., `mongo` or check using `docker ps`).

2. **Connect to MongoDB**:
   ```bash
   mongo -u root -p PassWord
   ```
   Or
   ```bash
   mongo -u root -p
   ```
   - Enter password: PassWord
   - Use the admin username `root` and password `PassWord` when prompted.
   - You can now run MongoDB commands like `show dbs` to list databases or `use mydb` to switch to a database.

### Apache Cassandra
To access the Cassandra container and use the `cqlsh` command-line tool:

1. **Access the container**:
   ```bash
   docker exec -it <cassandra_container_name> bash
   ```
   Replace `<cassandra_container_name>` with the name of your Cassandra container (e.g., `cassandra-container` or check using `docker ps`).

2. **Connect to Cassandra**:
   ```bash
   cqlsh
   ```

   * Default port: `9042`
   * Cluster: `Test Cluster` (default)
   * No password is required for the default setup unless you enable authentication.

### Oracle
To access the Oracle container and use the `sqlplus` command-line tool:

1. **Access the container**:
   ```bash
   docker exec -it <oracle_container_name> bash
   ```
   Replace `<oracle_container_name>` with the name of your Oracle container (e.g., `oracle-container` or check using `docker ps`).

2. **Connect to Oracle**:
   ```bash
   sqlplus sys/MyPassword123@XEPDB1 as sysdba
   ```

   * Default port: `1521`
   * You can now run SQL commands like `SELECT * FROM v$version;` to check the server version or `SELECT username FROM all_users;` to list schemas.

### Neo4j
To access the Neo4j container and use the Neo4j command-line tools or connect via the Neo4j Browser:

1. **Access the container**:

   ```bash
   docker exec -it <neo4j_container_name> bash
   ```

   Replace `<neo4j_container_name>` with the name of your Neo4j container (e.g., `neo4j-container` or check using `docker ps`).

2. **Connect to Neo4j using the command-line tool (`cypher-shell`)**:

   ```bash
   cypher-shell -u neo4j -p 12345678
   ```

   * This logs into the Neo4j database using the default username and password (`neo4j` / `12345678`).
   * Once connected, you can run Cypher queries, for example:

     ```cypher
     SHOW DATABASES;
     CREATE (n:Person {name: 'Alice'}) RETURN n;
     MATCH (n) RETURN n;
     ```

### Notes
- To find the container names, run `docker ps` to list running containers.
- If you prefer not to enter the container, you can run the database client commands directly from the host using `docker exec`. For example:
  - PostgreSQL: `docker exec -it <postgresql_container_name> psql -U postgres`
  - MySQL: `docker exec -it <mysql_container_name> mysql -u root -p`
  - SQL Server: `docker exec -it <mssql_container_name> /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P abcd@1234`
  - MongoDB: `docker exec -it <mongo_container_name> mongo -u root -p PassWord`
  - Cassandra: `docker exec -it <cassandra_container_name> cqlsh`
  - Oracle: `docker exec -it <oracle_container_name> sqlplus sys/MyPassword123@XEPDB1 as sysdba`
  - Neo4j: `docker exec -it <neo4j_container_name> cypher-shell -u neo4j -p 12345678`