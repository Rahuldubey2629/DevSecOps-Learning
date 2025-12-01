# 3-Tier DevSecOps Project

This repository contains a complete DevSecOps CI/CD pipeline for a 3-tier application with Node.js API backend, React frontend, and MySQL database. The pipeline includes comprehensive security scanning, code quality analysis, and automated Docker deployment.

## 🚀 Project Overview

This project demonstrates a production-grade DevSecOps pipeline implementing:

- **Security Scanning:** GitLeaks (secret detection) + Trivy (vulnerability scanning)
- **Code Quality:** SonarQube analysis with quality gates
- **Containerization:** Docker multi-stage builds
- **CI/CD:** Jenkins declarative pipeline with 10 stages
- **Deployment:** Docker Compose orchestration

## 📋 Architecture

```
┌─────────────────┐
│   React Client  │  (Port 80)
│    (Nginx)      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Node.js API   │  (Port 5000)
│   (Express)     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  MySQL Database │  (Port 3306)
└─────────────────┘
```

## 📚 Documentation

### Quick Links

| Documentation | Description |
|---------------|-------------|
| **[SETUP.md](./SETUP.md)** | Complete guide for Jenkins and SonarQube installation |
| **[PIPELINE_GUIDE.md](./PIPELINE_GUIDE.md)** | Detailed pipeline configuration and execution guide |
| **[README.md](./README.md)** | This file - project overview and quick start |

### What's Included

- ✅ Jenkins installation and configuration
- ✅ SonarQube setup with Docker
- ✅ All required plugins and tools setup
- ✅ Step-by-step pipeline configuration
- ✅ Detailed explanation of each pipeline stage
- ✅ Troubleshooting guides and best practices
- ✅ Security considerations and optimization tips

## 🎯 Quick Start

### Prerequisites

- Ubuntu/Debian Linux system
- Java 21 (OpenJDK)
- Docker & Docker Compose
- Git

### Option 1: Local Development Setup

1. **Install Node.js** (version 18 or later)

2. **Install dependencies:**
   ```bash
   cd api && npm install
   cd ../client && npm install
   ```

3. **Start the API server:**
   ```bash
   cd api
   npm start
   ```

4. **Start the React client:**
   ```bash
   cd client
   npm start
   ```

5. **Access the application:**
   - Frontend: `http://localhost:3000`
   - Backend API: `http://localhost:5000`

### Option 2: Docker Deployment

1. **Build and start all services:**
   ```bash
   docker-compose up -d
   ```

2. **Access the application:**
   - Frontend: `http://localhost`
   - Backend API: `http://localhost:5000`
   - MySQL: `localhost:3306`

3. **Stop services:**
   ```bash
   docker-compose down
   ```

### Option 3: DevSecOps Pipeline

For complete CI/CD pipeline setup with security scanning and automated deployment:

1. **Install Jenkins & SonarQube** - Follow **[SETUP.md](./SETUP.md)**
2. **Configure Pipeline** - Follow **[PIPELINE_GUIDE.md](./PIPELINE_GUIDE.md)**
3. **Run Pipeline** - Trigger from Jenkins dashboard

## 🔐 DevSecOps Pipeline Stages

![Jenkins Pipeline Stage View](docs/images/image.png)

| Stage | Tool | Purpose | Duration |
|-------|------|---------|----------|
| 1️⃣ Git Checkout | Git | Clone repository | ~5s |
| 2️⃣ Frontend Compilation | Node.js | Validate React code | ~15s |
| 3️⃣ Backend Compilation | Node.js | Validate Node.js code | ~10s |
| 4️⃣ GitLeaks Scan | GitLeaks | Detect secrets | ~20s |
| 5️⃣ SonarQube Analysis | SonarQube | Code quality & security | ~60s |
| 6️⃣ Quality Gate Check | SonarQube | Enforce standards | ~30s |
| 7️⃣ Trivy FS Scan | Trivy | Scan dependencies | ~45s |
| 8️⃣ Backend Docker Build | Docker + Trivy | Build & scan API image | ~3m |
| 9️⃣ Frontend Docker Build | Docker + Trivy | Build & scan Client image | ~5m |
| 🔟 Docker Deploy | Docker Compose | Deploy all services | ~60s |

**Total Pipeline Time:** ~10-12 minutes

## 🛠️ Technology Stack

### Frontend
- **React** - UI framework
- **Axios** - HTTP client
- **React Router** - Navigation
- **Nginx** - Production web server

### Backend
- **Node.js** - Runtime
- **Express** - Web framework
- **MySQL2** - Database driver
- **JWT** - Authentication
- **bcryptjs** - Password hashing

### Database
- **MySQL 8.0** - Relational database

### DevSecOps Tools
- **Jenkins** - CI/CD automation
- **SonarQube** - Code quality
- **GitLeaks** - Secret detection
- **Trivy** - Vulnerability scanning
- **Docker** - Containerization
- **Docker Compose** - Orchestration

## 🔧 Project Structure

```
3-Tier-DevSecOps-Mega-Project/
├── api/                          # Backend API
│   ├── controllers/              # Business logic
│   ├── middleware/               # Auth & role middleware
│   ├── models/                   # Database models
│   ├── routes/                   # API routes
│   ├── app.js                    # Entry point
│   ├── Dockerfile                # Backend container config
│   └── package.json              # Dependencies
│
├── client/                       # Frontend React app
│   ├── public/                   # Static files
│   ├── src/
│   │   ├── components/           # React components
│   │   ├── context/              # Auth context
│   │   ├── pages/                # Page components
│   │   ├── App.js                # Main component
│   │   └── index.js              # Entry point
│   ├── nginx/                    # Nginx config
│   ├── Dockerfile                # Frontend container config
│   └── package.json              # Dependencies
│
├── mysql-init/                   # Database initialization
│   └── init.sql                  # Schema & seed data
│
├── JenkinsFile                   # CI/CD pipeline definition
├── docker-compose.yml            # Multi-container orchestration
├── SETUP.md                      # Tools installation guide
├── PIPELINE_GUIDE.md             # Pipeline configuration guide
└── README.md                     # This file
```

## 🖼️ Screenshots

### Jenkins Pipeline View
![Jenkins Pipeline](docs/images/image.png)

### SonarQube Dashboard
![SonarQube Running](docs/images/image-1.png)

### Docker Services
![Docker Services Running](docs/images/image-2.png)

## 🔐 Security Features

### Implemented Security Measures
- ✅ **Secret Detection** - GitLeaks scans for hardcoded credentials
- ✅ **Dependency Scanning** - Trivy identifies vulnerable packages
- ✅ **Code Quality** - SonarQube detects security hotspots
- ✅ **Container Scanning** - Trivy scans Docker images for CVEs
- ✅ **JWT Authentication** - Secure API endpoints
- ✅ **Password Hashing** - bcryptjs for secure storage
- ✅ **Environment Variables** - Secrets stored securely
- ✅ **CORS Protection** - Controlled cross-origin requests
- ✅ **Input Validation** - Prevent injection attacks

## 📊 Reports & Monitoring

### Available Reports
- **SonarQube Dashboard** - `http://localhost:9000`
  - Code quality metrics
  - Security vulnerabilities
  - Technical debt
  - Code coverage

- **Trivy Reports** - Jenkins workspace
  - `fs-report.html` - Filesystem scan
  - `backend-scan-report.html` - API image scan
  - `frontend-scan-report.html` - Client image scan

- **Jenkins Build History**
  - Stage view
  - Console output
  - Build artifacts

## 🚨 Troubleshooting

### Common Issues

**Pipeline fails at GitLeaks stage:**
- Remove hardcoded secrets from code
- Use environment variables
- Check `.env` is in `.gitignore`

**SonarQube connection fails:**
```bash
docker restart sonarqube
curl http://localhost:9000
```

**Docker build fails:**
```bash
# Test builds locally
cd api && docker build -t test-api .
cd ../client && docker build -t test-client .
```

**Port conflicts:**
```bash
# Check what's using the port
sudo netstat -tulpn | grep :80
sudo netstat -tulpn | grep :5000
sudo netstat -tulpn | grep :3306
```

For detailed troubleshooting, see **[PIPELINE_GUIDE.md](./PIPELINE_GUIDE.md)**.

## 📝 Configuration

### Environment Variables

Create `.env` file in project root:

```bash
# Database
DB_HOST=mysql
DB_USER=root
DB_PASSWORD=Admin@123
DB_NAME=userdb

# API
API_PORT=5000
JWT_SECRET=your-secret-key-here

# Client
REACT_APP_API_URL=http://localhost:5000
```

### Docker Compose Services

```yaml
services:
  mysql:    # Database on port 3306
  api:      # Backend on port 5000
  client:   # Frontend on port 80
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📖 Additional Resources

- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [SonarQube Documentation](https://docs.sonarqube.org/)
- [Docker Documentation](https://docs.docker.com/)
- [Trivy Documentation](https://aquasecurity.github.io/trivy/)
- [GitLeaks Documentation](https://github.com/gitleaks/gitleaks)

## 📄 License

This project is licensed under the MIT License.

## 👤 Author

**Rahul Dubey**
- GitHub: [@Rahuldubey2629](https://github.com/Rahuldubey2629)

## 🙏 Acknowledgments

- Jenkins community for excellent CI/CD tools
- SonarSource for code quality platform
- Aqua Security for Trivy scanner
- GitLeaks team for secret detection

---

**⭐ Star this repository if you find it helpful!**

For detailed setup and configuration instructions, please refer to:
- **[SETUP.md](./SETUP.md)** - Tools installation
- **[PIPELINE_GUIDE.md](./PIPELINE_GUIDE.md)** - Pipeline configuration
