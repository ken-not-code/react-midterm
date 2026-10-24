# React Calculator - CI/CD Pipeline Midterm

**Course:** Software Quality Engineering  
**Students:** Ken & Nico  
**Repository:** https://github.com/ken-not-code/react-midterm  

---

## Project Overview
This project is a React Calculator built with Vite and tested using Vitest. It features an automated CI/CD pipeline using Jenkins and Docker to handle automated testing, build artifact packaging, and multi-environment deployment.

---

## Application Features
- Basic calculator functions: Addition and Subtraction.
- Automated unit tests covering positive numbers, negative numbers, and string conversions.
- Application version tracking displayed on the UI.

---

## Local Development & Testing

### 1. Install Dependencies
```bash
npm install
```

### 2. Run Local Development Server
```bash
npm run dev
```
Open `http://localhost:5173` in your browser.

### 3. Run Automated Unit Tests
```bash
npm test
```
Executes 5 unit tests using Vitest.

---

## Docker Support

### Multi-Stage Build
Build the full application image (runs unit tests automatically during build):
```bash
docker build -t react-midterm .
```

### Deployment Image
Build the lightweight deployment image using pre-built static assets:
```bash
docker build -f Dockerfile.deploy -t react-midterm-deploy .
```

---

## CI/CD Pipeline Architecture

1. **`React-CI` Job (Jenkins):**
   - Automatically triggered via SCM polling (`*/2 * * * *`).
   - Builds the multi-stage Dockerfile and executes unit tests.
   - Extracts static files (`dist/`) and generates `release.json` metadata.
   - Archives a release artifact named `react-app-${BUILD_NUMBER}.tar.gz`.

2. **`React-Deploy` Job (Jenkins):**
   - Manually triggered parameterized job (`TARGET_ENVIRONMENT` and `CI_BUILD_NUMBER`).
   - Copies the archived `.tar.gz` artifact from `React-CI`.
   - Deploys the application to the selected target environment container:
     - **dev:** `http://localhost:8081`
     - **test:** `http://localhost:8082`
     - **acceptance:** `http://localhost:8083`
     - **production:** `http://localhost:8084`