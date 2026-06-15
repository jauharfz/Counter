# Counter Project - CI/CD Pipeline Implementation

## 📋 Project Overview
Java Maven project dengan implementasi **CI/CD Pipeline** menggunakan **GitHub Actions** yang mencakup 4 komponen utama.

## 🔄 CI/CD Pipeline Components

### 1. **Continuous Integration (CI) - Build Stage**
- **Trigger:** Push ke branch `master` atau `main`
- **Actions:**
  - Checkout source code
  - Setup JDK 11 dengan caching Maven dependencies
  - Clean project (`mvn clean`)
  - Compile source code (`mvn compile`)
- **Output:** Build artifact atau notification

### 2. **Continuous Testing - Test Stage**
- **Depends on:** Build stage selesai dengan sukses
- **Actions:**
  - Compile test code (`mvn test-compile`)
  - Run unit tests (`mvn test`)
  - Upload test results sebagai artifact
- **Output:** Test reports di `target/surefire-reports/`

### 3. **Continuous Inspection - SonarQube Analysis**
- **Depends on:** Test stage selesai dengan sukses
- **Actions:**
  - Analyze code quality dengan SonarQube
  - Verify build and analyze (`mvn verify sonar:sonar`)
  - Push results ke SonarCloud
- **Requirements:**
  - `SONAR_TOKEN` secret di GitHub (SonarCloud token)
  - SonarCloud organization: `jauharfz`
  - Project key: `jauharfz_Counter`

### 4. **Continuous Deployment/Delivery - Release Stage**
- **Depends on:** Inspection stage selesai dengan sukses
- **Trigger:** Push ke `master` branch (tidak untuk PR)
- **Actions:**
  - Package aplikasi (`mvn package -DskipTests`)
  - Upload JAR ke GitHub Releases (jika ada tag)
  - Upload artifact sebagai build artifact
- **Output:** 
  - JAR file di GitHub Releases
  - Build artifact tersedia di GitHub Actions

## 📁 Project Structure
```
.
├── .github/workflows/
│   └── maven.yml              # CI/CD Pipeline definition
├── src/
│   ├── main/java/             # Source code
│   └── test/java/             # Test code
├── pom.xml                    # Maven configuration
└── README.md                  # This file
```

## ⚙️ Setup Instructions

### Prerequisites
- Java JDK 8+
- Maven 3.6+
- GitHub account dengan admin access ke repository

### Local Setup
```bash
# Clone repository
git clone https://github.com/jauharfz/Counter.git
cd Counter

# Build locally
mvn clean compile

# Run tests
mvn test

# Package
mvn package
```

### GitHub Actions Setup

#### 1. Environment Variables & Secrets
Tambahkan ke GitHub repository settings → Secrets and variables:

- **SONAR_TOKEN** (Required untuk SonarQube analysis)
  - Dapatkan dari: https://sonarcloud.io/account/security
  - Buka akun SonarCloud dan generate token
  - Add secret dengan nama `SONAR_TOKEN`

- **GITHUB_TOKEN** (Automatic, tersedia di GitHub Actions)
  - Tidak perlu setup manual

#### 2. Workflow Trigger Configuration
Pipeline otomatis berjalan saat:
- ✅ Push ke branch `master` atau `main`
- ✅ Pull request ke branch `master` atau `main`

#### 3. View Pipeline Status
- Buka tab "Actions" di GitHub repository
- Lihat status setiap stage
- Download artifacts jika diperlukan

## 🚀 Running Pipeline

### Manual Trigger (Jika diperlukan)
```bash
git add .github/workflows/maven.yml
git commit -m "Update: CI/CD Pipeline with 4 stages"
git push origin master
```

### Monitoring Pipeline
1. Buka GitHub repository
2. Tab "Actions"
3. Pilih workflow "Maven CI/CD Pipeline"
4. Lihat progress setiap stage dengan detail logs

## 📊 Pipeline Outputs

### Build Artifacts
```
target/
├── classes/                   # Compiled source
├── test-classes/             # Compiled tests
├── surefire-reports/         # Test results (XML/HTML)
└── *.jar                      # Packaged application
```

### Test Results
- Location: `target/surefire-reports/`
- Format: XML untuk CI integration
- Accessible di GitHub Actions → Artifacts

### SonarQube Analysis
- Dashboard: https://sonarcloud.io/dashboard?id=jauharfz_Counter
- Metrics: Code coverage, bugs, vulnerabilities, code smells
- Update: Automatic setelah inspect stage

### Deployment Artifacts
- JAR files uploaded ke:
  - GitHub Releases (ketika ada tag)
  - GitHub Actions artifacts (setiap deployment)

## 🔍 Troubleshooting

### Build Failure
- Check workflow logs di GitHub Actions tab
- Verify pom.xml dependencies
- Ensure JDK 11 compatibility

### Test Failures
- Check test-results artifact
- Review surefire-reports HTML files
- Debug locally dengan `mvn test`

### SonarQube Analysis Failed
- Verify SONAR_TOKEN is set correctly
- Check organization name dan project key
- Ensure SonarCloud account active

### Deployment Issues
- Verify JAR generation: `mvn package`
- Check artifacts upload permissions
- Review GitHub Actions logs

## 📝 Notes
- Build cache helps reduce execution time
- All stages have success/failure notifications
- Artifacts auto-deleted sesuai GitHub retention policy
- SonarQube analysis requires valid SONAR_TOKEN

## 👥 Contributors
- Jauhar Fz (jauharfz)

## 📅 Last Updated
2026-06-15
