# Neo4j Database Docker Compose
This is a simple Docker Compose configuration to run a **Neo4j 5.23.0 Enterprise** instance with **APOC** and **Bloom** plugins enabled.

## Requirements
* Docker
* Docker Compose
* Internet connection (to download the image)
* A terminal to run the commands
* Basic knowledge of Docker and Docker Compose
* Basic knowledge of Cypher and Neo4j
* Basic knowledge of Linux commands

## Installation
1. Clone this repository
2. Edit the environment variables in the `.env` file if needed
3. Run the container in detached mode:

   ```bash
   docker compose up -d
   ```

4. Access the Neo4j Browser or connect via Cypher shell.
5. Stop the container:

   ```bash
   docker compose down
   ```

6. Restart it:

   ```bash
   docker compose up -d
   ```

7. Remove containers and volumes:

   ```bash
   docker compose down -v
   ```

8. Prune all unused Docker volumes:

   ```bash
   docker volume prune
   ```

## Environment Variables

| Variable          | Description               | Default  |
| ----------------- | ------------------------- | -------- |
| `NEO4J_USER`      | Neo4j database username   | neo4j    |
| `NEO4J_PASSWORD`  | Neo4j database password   | 12345678 |
| `NEO4J_APOC`      | Enable APOC plugin        | true     |
| `NEO4J_BLOOM`     | Enable Bloom plugin       | true     |
| `NEO4J_PORT`      | Web interface port (HTTP) | 7474     |
| `NEO4J_BOLT_PORT` | Bolt protocol port        | 7687     |

---

## Access Information

* **Neo4j Browser (Web UI):** [http://localhost:7474](http://localhost:7474)
* **Bolt Protocol URL:** `neo4j://localhost:7687`
* **Username:** `neo4j`
* **Password:** `12345678`

> **Note:**
>
> * APOC and Bloom are pre-enabled for convenience.
> * Bloom requires a **valid Enterprise license** to work.
> * Data is persisted in a Docker volume named `neo4j`.

---

## Working with Neo4j Database

### Accessing the Neo4j CLI (Cypher Shell)

1. Enter the container:

   ```bash
   docker exec -it neo4j-container bash
   ```

2. Connect using Cypher shell:

   ```bash
   cypher-shell -u neo4j -p 12345678
   ```

3. Run basic Cypher commands:

   ```cypher
   SHOW DATABASES;
   CREATE (n:Person {name: 'Alice', age: 28});
   MATCH (n:Person) RETURN n;
   ```

---

### Using Neo4j Browser (Web Interface)

1. Open your browser and go to:

   ```
   http://localhost:7474
   ```

2. In the connection dialog, use:

   * **Protocol:** `neo4j://`
   * **Connection URL:** `localhost:7687`
   * **Authentication type:** `Username / Password`
   * **Username:** `neo4j`
   * **Password:** `12345678`

3. Click **Connect** and start running Cypher queries interactively.

---

### Using Bloom (Optional)

If you have a **valid Neo4j Bloom license**, you can access it through:

```
http://localhost:7474/browser/bloom
```

If you see an error like:

> “We couldn’t find a license for the Neo4j Bloom Server.”

It means your Neo4j instance does not have an active Bloom license.
You can still use **Neo4j Browser** or **Cypher Shell** normally.

---

## Example Queries

```cypher
// Create nodes and relationships
CREATE (a:Person {name: 'Alice'})-[:KNOWS]->(b:Person {name: 'Bob'});

// Query all nodes
MATCH (n) RETURN n;

// Query specific relationship
MATCH (a:Person)-[:KNOWS]->(b:Person)
RETURN a.name AS From, b.name AS To;
```

---

## Connection String Examples

### Java (Spring Boot or JDBC)

```java
spring.neo4j.uri=bolt://localhost:7687
spring.neo4j.authentication.username=neo4j
spring.neo4j.authentication.password=12345678
```

### Python (with neo4j-driver)

```python
from neo4j import GraphDatabase

uri = "bolt://localhost:7687"
driver = GraphDatabase.driver(uri, auth=("neo4j", "12345678"))

with driver.session() as session:
    result = session.run("MATCH (n) RETURN n LIMIT 5")
    for record in result:
        print(record)
driver.close()
```

### Node.js (with neo4j-driver)

```javascript
const neo4j = require('neo4j-driver');

const driver = neo4j.driver(
  'bolt://localhost:7687',
  neo4j.auth.basic('neo4j', '12345678')
);

async function run() {
  const session = driver.session();
  const result = await session.run('MATCH (n) RETURN n LIMIT 5');
  console.log(result.records);
  await session.close();
  await driver.close();
}

run();
```

---

## Notes

* Image used: **neo4j:5.23.0-enterprise**
* Data persisted in volume: **neo4j**
* Default ports:

  * HTTP: **7474**
  * Bolt: **7687**
* Plugins enabled:

  * **APOC** (Advanced Procedures and Functions)
  * **Bloom** (requires Enterprise license)
* To list running containers:

  ```bash
  docker ps
  ```
* To access Cypher shell without entering the container:

  ```bash
  docker exec -it neo4j-container cypher-shell -u neo4j -p 12345678
  ```

