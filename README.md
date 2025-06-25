# 🐳 Nginx Reverse Proxy with Go & Python Microservices

This project sets up an Nginx reverse proxy using Docker Compose to route requests to two backend services:

- `service1`: A Go (Golang) application
- `service2`: A Python Flask application

---

## ⚙️ Setup Instructions

1. **Clone the repository** (or download ZIP and unzip):
   ```bash
   git clone https://github.com/Malan-1/Devops_internship_assignment.git
   cd Devops_internship_assignment
   ```

2. **Start the system with one command**:
   ```bash
   docker compose up -d --build
   ```

3. **Stop the system**:
   ```bash
   docker compose down
   ```

Make sure Docker & Docker Compose are installed.

---

## How Routing Works

Nginx listens on `localhost:8080` and forwards requests based on the URL prefix:

| URL                         | Target Service | Container Port |
|----------------------------|----------------|----------------|
| `/service1/`               | Go (service1)  | `5000`         |
| `/service2/`               | Python (service2) | `6000`       |

📍 Example URLs to test:
- [http://localhost:8080/service1/ping](http://localhost:8080/service1/ping)
- [http://localhost:8080/service1/hello](http://localhost:8080/service1/hello)
- [http://localhost:8080/service2/](http://localhost:8080/service2/)

---

## Bonus Implementations

### 1. **Rate Limiting**
Nginx rate limits requests to avoid abuse:
```nginx
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=5r/s;

location /service1/ {
    limit_req zone=api_limit burst=10 nodelay;
    proxy_pass http://service1:5000/;
}
```

- Max 5 requests/second/IP
- Bursts up to 10
- Exceeding limit returns HTTP `503`

### 2. **Structured Logging**
Nginx logs all requests with timestamps in `access.log` for traceability.

---

## Requirements

- Docker
- Docker Compose

