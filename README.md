# 📦 MinIO + Spring Boot File Upload Project

This project demonstrates how to integrate **MinIO (S3-compatible object storage)** with a Spring Boot application to upload, store, and retrieve files (images). It also includes **Swagger (OpenAPI)** for API documentation and **Docker support** for easy deployment.

---

# 🚀 Features

* Upload images to MinIO
* Store file metadata in MySQL
* Retrieve images via URL
* Delete product (and optionally file)
* Swagger UI for API testing
* Dockerized application
* Docker Compose (multi-container setup)

---

# 🧠 MinIO Concepts (Important)

## 1. Bucket

* Top-level container that holds objects (files)

## 2. Object (Actual File)

An object consists of:

* File data (binary)
* Metadata
* Object key (path)

Example:

```
/user/101/profile.jpg
```

## 3. Object Key

* Unique identifier of file inside bucket

Example:

```
products/123/image.png
```

## 4. Metadata

* content-type
* size
* uploaded-by

## 5. Access Key & Secret Key

* Used for authentication

## 6. Endpoint

```
http://localhost:9000 → API
http://localhost:9001 → Console
```

---

# 🏗️ Architecture

```
Controller → Service → Repository → Database
                  ↓
                MinIO
```

---

# 📂 Project Flow

## Upload Flow

1. Upload file
2. Store in MinIO
3. Save metadata in DB
4. Return URL

## Fetch Flow

1. Fetch from DB
2. Return URL
3. Frontend displays image

---

# ⚙️ Tech Stack

* Spring Boot
* MinIO
* MySQL
* Maven
* Docker
* Swagger (OpenAPI)

---

# 📘 Swagger API

Open:

```
http://localhost:8080/swagger-ui/index.html
```

---

# 🔐 Environment Setup

Create `.env`:

```
DB_URL=jdbc:mysql://localhost:3306/minio_db
DB_USERNAME=root
DB_PASSWORD=yourpassword

MINIO_ENDPOINT=http://localhost:9000
MINIO_ACCESS_KEY=admin
MINIO_SECRET_KEY=password123
MINIO_BUCKET=my-basket-product-images
```

---

# ▶️ Run Without Docker

## 1. Start MinIO

```
docker run -p 9000:9000 -p 9001:9001 \
--name minio \
-e MINIO_ROOT_USER=admin \
-e MINIO_ROOT_PASSWORD=password123 \
-v C:\Users\your-user\minio\data:/data \
quay.io/minio/minio server /data --console-address ":9001"
```

## 2. Start MySQL

```
CREATE DATABASE minio_db;
```

## 3. Run App

```
mvn spring-boot:run
```

---

# 🐳 Run With Docker (Single Container)

## Build JAR

```
mvn clean package -DskipTests
```

## Build Image

```
docker build -t minio-spring-app .
```

## Run Container

```
docker run -p 8080:8080 \
-e DB_URL=jdbc:mysql://host.docker.internal:3306/minio_db \
-e DB_USERNAME=root \
-e DB_PASSWORD=yourpassword \
-e MINIO_ENDPOINT=http://host.docker.internal:9000 \
-e MINIO_ACCESS_KEY=admin \
-e MINIO_SECRET_KEY=password123 \
-e MINIO_BUCKET=my-basket-product-images \
minio-spring-app
```

---

# 🐳 Run Full System (Docker Compose) 🚀

## docker-compose.yml

```
version: "3.9"

services:
  mysql:
    image: mysql:8
    container_name: mysql-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: minio_db
    ports:
      - "3307:3306"

  minio:
    image: quay.io/minio/minio
    container_name: minio-server
    command: server /data --console-address ":9001"
    environment:
      MINIO_ROOT_USER: admin
      MINIO_ROOT_PASSWORD: password123
    ports:
      - "9000:9000"
      - "9001:9001"

  app:
    build: .
    container_name: spring-app
    depends_on:
      - mysql
      - minio
    ports:
      - "8080:8080"
    environment:
      DB_URL: jdbc:mysql://mysql:3306/minio_db
      DB_USERNAME: root
      DB_PASSWORD: root
      MINIO_ENDPOINT: http://minio:9000
      MINIO_ACCESS_KEY: admin
      MINIO_SECRET_KEY: password123
      MINIO_BUCKET: my-basket-product-images
```

---

## Run Everything

```
docker-compose up --build
```

---

## Access

* App → http://localhost:8080
* Swagger → http://localhost:8080/swagger-ui/index.html
* MinIO → http://localhost:9001

---

# 📡 API Endpoints

### Upload

```
POST /api/products
```

### Get

```
GET /api/products/{id}
```

### Delete

```
DELETE /api/products/{id}
```

---

# ⚠️ Important Notes

* Files are NOT stored in DB
* Object key is critical
* Use `host.docker.internal` only in single-container mode
* Use service names (`mysql`, `minio`) in docker-compose

---

# 🚀 Future Improvements

* Pre-signed URLs
* Multiple images
* JWT authentication
* Bucket auto-creation
* CI/CD pipeline

---

# 💡 Author Notes

This project demonstrates:

* Object storage (MinIO)
* Backend file handling
* Docker-based deployment
* Clean architecture

---

⭐ If you found this useful, feel free to fork and build on it!
