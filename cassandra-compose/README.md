# Apache Cassandra Docker Compose
This is a simple docker-compose configuration to run an Apache Cassandra database node.

## Requirements
- Docker
- Docker Compose
- Internet connection (to download the images)
- A terminal to run the commands
- Basic knowledge of Docker and Docker Compose
- Basic knowledge of Apache Cassandra
- Basic knowledge of CQL (Cassandra Query Language)
- Basic knowledge of Linux commands

## Installation
1. Clone this repository
2. Edit the environment variables in the `.env` file
3. Run the `docker-compose up -d` command
4. Connect to Cassandra using:
   - Command line tool (cqlsh)
   - Third-party tools like:
     - DBeaver
     - TablePlus
     - DataStax DevCenter
5. Run CQL queries using your preferred client
6. Stop the containers with the `docker-compose down` command
7. Start the containers with the `docker-compose up -d` command
8. Remove the containers with the `docker-compose down -v` command
9. Remove the volumes with the `docker volume prune` command

## Environment Variables
- `CASSANDRA_CLUSTER_NAME`: Name of the Cassandra cluster (default: Test Cluster)
- `CASSANDRA_DC`: Datacenter name (default: datacenter1)
- `CASSANDRA_RACK`: Rack name (default: rack1)
- `CASSANDRA_USER`: Username for authentication (default: cassandra)
- `CASSANDRA_PASSWORD`: Password for authentication (default: cassandra)
- `CASSANDRA_PORT`: Port for Cassandra (default: 9042)

## Access Information
- Cassandra node: `localhost:9042`
- Username: `cassandra` (default)
- Password: `cassandra` (default)

**Note**: Unlike other databases in this collection, Cassandra does not come with a built-in web UI. However, you can use third-party tools like DBeaver, TablePlus, or DataStax DevCenter for GUI-based management.

## Working with Cassandra

### Accessing the CQL Shell
1. Enter the container:
```bash
docker exec -it cassandra-container bash
```

2. Start CQL shell with authentication:
```bash
cqlsh -u cassandra -p cassandra
```

### Basic CQL Commands
```sql
-- List all keyspaces
DESCRIBE KEYSPACES;

-- Create a new keyspace
CREATE KEYSPACE IF NOT EXISTS my_keyspace 
WITH REPLICATION = {
    'class' : 'SimpleStrategy',
    'replication_factor' : 1
};

-- Use the keyspace
USE my_keyspace;

-- Create a table
CREATE TABLE users (
    id uuid PRIMARY KEY,
    username text,
    email text,
    created_at timestamp
);

-- Insert data
INSERT INTO users (id, username, email, created_at)
VALUES (uuid(), 'john_doe', 'john@example.com', toTimestamp(now()));

-- Query data
SELECT * FROM users;
```

## Connection String Examples
### Java (with DataStax driver)
```java
CqlSession session = CqlSession.builder()
   .addContactPoint(new InetSocketAddress("localhost", 9042))
   .withLocalDatacenter("datacenter1")
   .withAuthCredentials("cassandra", "cassandra")
   .withKeyspace("my_keyspace")
   .build();
```

### Python (with cassandra-driver)
```python
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider

auth_provider = PlainTextAuthProvider(
    username='cassandra', 
    password='cassandra'
)
cluster = Cluster(
    ['localhost'], 
    port=9042,
    auth_provider=auth_provider
)
session = cluster.connect('my_keyspace')
```

### Node.js (with cassandra-driver)
```javascript
const cassandra = require('cassandra-driver');
const client = new cassandra.Client({
  contactPoints: ['localhost'],
  localDataCenter: 'datacenter1',
  keyspace: 'my_keyspace',
  credentials: { username: 'cassandra', password: 'cassandra' }
});
```

## Notes
- The default image uses Cassandra 4.1
- Data is persisted in a Docker volume named `cassandra_data`
- The Cassandra container exposes port 9042 for CQL native protocol
- The cluster is configured with GossipingPropertyFileSnitch for simple deployment
- Default authentication is enabled with username/password