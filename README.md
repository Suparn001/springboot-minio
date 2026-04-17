# 📦 MinIO + Spring Boot File Upload Project

This project demonstrates how to integrate **MinIO (S3-compatible object storage)** with a Spring Boot application to upload, store, and retrieve files (images). It also includes **Swagger (OpenAPI)** for API documentation and testing.

---

# 🚀 What You Will Learn

* How object storage works (MinIO / S3)
* Upload files using Spring Boot
* Store file metadata in database
* Generate and use file URLs
* Clean architecture (Controller → Service → Repository)
* API documentation using Swagger UI

---

# 🧠 MinIO Concepts (Important)

## 1. Bucket

* Top-level container that holds objects (files)
* Similar to a folder

---

## 2. Object (Actual File)

An object consists of:

* File data (binary data)
* Metadata
* Object key (path)

📌 Example:

```
/user/101/profile.jpg
```

---

## 3. Object Key (Very Important)

* Unique identifier of a file inside a bucket
* Works like a file path

📌 Example:

```
products/123/image.png
```

---

## 4. Metadata

Extra information about files:

* content-type (image/png, image/jpeg)
* size
* uploaded-by

👉 Used for:

* Filtering
* Security
* Processing

---

## 5. Access Key & Secret Key

* Used for authentication (like username & password)
* Required to access MinIO APIs

---

## 6. Endpoint

* URL where MinIO server runs

📌 Example:

```
http://localhost:9000   → API
http://localhost:9001   → Console UI
```

---

## 7. MinIO Server

Main engine that:

* Handles API requests
* Manages buckets and objects
* Stores files
* Handles replication

---

# 🏗️ Project Architecture

```
Controller  →  Service  →  Repository  →  Database
                    ↓
                 MinIO
```

---

# 📂 Project Flow

## 🟢 Upload Flow

1. User sends request with file
2. Controller receives request
3. Service uploads file to MinIO
4. MinIO stores file and returns objectKey
5. Save objectKey + metadata in database
6. Return response to client

---

## 🔵 Fetch Flow

1. Fetch product from database
2. Get stored objectKey / URL
3. Return URL to frontend
4. Frontend displays image

---

## 🔴 Delete Flow

1. Delete product from database
2. (Optional) Delete file from MinIO bucket

---

# 📊 Example Stored Data

## Database

| Field     | Value                                          |
| --------- | ---------------------------------------------- |
| productId | 1                                              |
| title     | Shoes                                          |
| objectKey | products/uuid.png                              |
| url       | http://localhost:9000/bucket/products/uuid.png |

---

# ⚙️ Tech Stack

* Spring Boot
* MinIO
* MySQL (or any relational DB)
* Maven
* Swagger (OpenAPI)

---

# 📘 Swagger API Documentation

This project uses Swagger via:

👉 `springdoc-openapi`

## ▶️ Access Swagger UI

After running the application, open:

```
http://localhost:8080/swagger-ui/index.html
```

---

## 🔥 Features

* Interactive API documentation
* Test APIs directly from browser
* File upload support (multipart/form-data)
* Request & response examples

---

## 🧪 Available APIs in Swagger

### ➕ Upload Product

```
POST /api/products
```

* Upload product with image using form-data

---

### 📥 Get Product

```
GET /api/products/{id}
```

---

### ❌ Delete Product

```
DELETE /api/products/{id}
```

---

## ⚠️ Important Notes for File Upload

* Endpoint must use:

```
consumes = multipart/form-data
```

* DTO must include:

```
MultipartFile file
```

* Swagger uses:

```
type: string
format: binary
```

---

# ▶️ Running MinIO (Docker)

```bash
docker run -p 9000:9000 -p 9001:9001 \
--name minio \
-e MINIO_ROOT_USER=admin \
-e MINIO_ROOT_PASSWORD=password123 \
-v C:\Users\your-user\minio\data:/data \
quay.io/minio/minio server /data --console-address ":9001"
```

---

# ⚙️ Application Configuration

```yaml
minio:
  url: http://localhost:9000
  bucket-name: my-bucket
```

---

# 🌐 API Endpoints

## ➕ Upload Product

```
POST /api/products
```

Form-data:

* title
* description
* price
* file

---

## 📥 Get Product

```
GET /api/products/{id}
```

---

## ❌ Delete Product

```
DELETE /api/products/{id}
```

---

# 🔥 Key Takeaways

* MinIO works like AWS S3
* Files are NOT stored in DB, only metadata
* Object key is the most important concept
* Swagger helps visualize and test APIs easily
* Clean separation of concerns is important

---

# 🚀 Future Improvements

* Pre-signed URLs (secure access)
* Multiple image support
* File validation & size limits
* Delete file from MinIO when deleting product
* Use UUID naming to avoid conflicts
* Add authentication (JWT) in Swagger

---


⭐ If you found this useful, keep building and scaling it further!
