We’ll have:

1. **Web application** – Node.js Express example (can also swap for Django if you prefer)
2. **Database** – MySQL container (optional)
3. **Docker Compose** – To run app + db locally for CI/CD testing

---

## 1️⃣ `web/app.js` – Node.js Example Web Server

```javascript
// app.js
const express = require('express');
const mysql = require('mysql');
const app = express();
const port = 8080;

// MySQL connection (optional)
const db = mysql.createConnection({
  host: 'db',            // matches docker-compose service name
  user: 'appuser',
  password: 'app123',
  database: 'appdb'
});

db.connect((err) => {
  if (err) {
    console.error('Database connection failed:', err);
  } else {
    console.log('Connected to MySQL database!');
  }
});

// Home route
app.get('/', (req, res) => {
  db.query('SELECT * FROM users', (err, results) => {
    if (err) res.send('Error fetching data');
    else res.json(results);
  });
});

app.listen(port, () => {
  console.log(`App running at http://localhost:${port}`);
});
```

---

## 2️⃣ `web/package.json`

```json
{
  "name": "ci-cd-web",
  "version": "1.0.0",
  "description": "Node.js app for CI/CD deployment",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No tests configured\""
  },
  "dependencies": {
    "express": "^4.18.2",
    "mysql": "^2.18.1"
  }
}
```

---

## 3️⃣ `db/init.sql` – MySQL Database Script

```sql
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(50)
);

INSERT INTO users (name, email) VALUES
('Alice', 'alice@example.com'),
('Bob', 'bob@example.com'),
('Charlie', 'charlie@example.com');
```

---

## 4️⃣ `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "8080:8080"
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: mysql:8
    container_name: db_container
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: appdb
      MYSQL_USER: appuser
      MYSQL_PASSWORD: app123
    volumes:
      - db_data:/var/lib/mysql
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  db_data:
```

---

## 5️⃣ `.gitignore`

```gitignore
node_modules/
.env
*.log
backup/
```

---

## 6️⃣ How to Test Locally (Before CI/CD)

```bash
# Build and start containers
docker-compose up -d --build

# Access web app
http://localhost:8080

# Stop containers
docker-compose down
```

---

## ✅ Folder Structure

```
project-3-ci-cd-aws/
│-- README.md
│-- .gitlab-ci.yml
│-- docker-compose.yml
│-- web/
│   ├── app.js
│   └── package.json
│-- db/
│   └── init.sql
│-- .gitignore
```

---

### ⚡ Summary

* Web app communicates with MySQL database in Docker containers.
* Docker setup is ready for **GitLab CI/CD pipeline**.
* CI/CD pipeline will build, test, and deploy app to **AWS EC2** automatically.

---

