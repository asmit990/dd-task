

**Live URL:** http://3.109.157.238

---

## 🐳 Dockerization

### Backend — `backend/Dockerfile`

- **Base Image:** Node (Alpine)
- **Optimization:** Dependencies installed via `npm ci` for clean, reproducible installs
- **Database Connection:** Uses environment variable `MONGO_URL=mongodb://mongo:27017/db`
- **Internal Port:** `8080`

### Frontend — `frontend/Dockerfile`

Multi-stage build for an optimized production image:

- **Stage 1 (Build):** Node image compiles the Angular production build
- **Stage 2 (Serve):** Nginx image serves the compiled static files from `/usr/share/nginx/html`
- **Exposed Port:** `80`

### Database

- Uses the official `mongo:7` image
- Configured in `docker-compose.yml`
- Runs inside an isolated Docker network and is **not publicly exposed**

---

## 🌐 Nginx Reverse Proxy

Nginx acts as the primary entry point, routing requests based on path:

| Path | Destination |
|---|---|
| `/` | Angular Frontend |
| `/api` | Node.js Backend |

---

## 🔁 CI/CD Pipeline

Every push to `main` triggers the automated workflow defined in `.github/workflows/deploy.yml`:

1. **Build** — Builds multi-architecture Docker images (`linux/amd64`, `linux/arm64`)
2. **Push** — Uploads images to Docker Hub:
   - `asmit990/backend-app`
   - `asmit990/angular-15-crud`
3. **Deploy** — SSHs into the EC2 instance, pulls the latest images, and restarts containers via Docker Compose

---

## 📁 Repository Structure

```
.
├── nginx.conf
├── docker-compose.yml
├── README.md
├── appworking.png
├── dockerhub-image.png
├── ec2-instance.png
├── github-actions-success.png
└── nginx-config.png
```

---

## 📸 Deployment Evidence

| Screenshot | Description |
|---|---|
| `github-actions-success.png` | Successful CI/CD pipeline execution |
| `dockerhub-image.png` | Images built and pushed to Docker Hub |
| `ec2-instance.png` | AWS EC2 instance running status |
| `appworking.png` | Application live via public IP |
| `nginx-config.png` | Nginx reverse proxy routing configuration |

---



