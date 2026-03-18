# 📱 MobileOps Pro: Native iOS CI/CD Orchestrator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![CI: GitHub Actions](https://img.shields.io/badge/CI-GitHub%20Actions-blue?logo=githubactions)](https://github.com/features/actions)
[![Environment: macOS 15](https://img.shields.io/badge/OS-macOS%2015-black?logo=apple)](https://developer.apple.com/xcode/)
![Capacitor](https://img.shields.io/badge/Capacitor-119EFF?style=for-the-badge&logo=capacitor&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)

---

## 🚀 Overview

**MobileOps Pro** is a production-grade **Native iOS CI/CD Orchestrator** designed for hybrid applications (Ionic/Capacitor).

It eliminates the traditional macOS dependency for **build and deployment** by shifting the entire iOS pipeline to **cloud-based macOS runners**, while retaining a one-time secure credential provisioning step on a local Mac environment.

> 💡 This system was successfully used to deploy the **Afronet Dating App** to TestFlight without relying on local build machines.


---

## 🏗️ Architecture Diagram
![Architecture](https://raw.githubusercontent.com/olubusade/mobile-ops-pro/main/assets/images/mobileops/mobileops-pro-architecture.png)

## 🏗️ Successful Github Action Build
![Github Action Build ](https://raw.githubusercontent.com/olubusade/mobile-ops-pro/main/assets/images/mobileops/githubaction-build-success.png)


---

## 🧠 Core Concept

Traditional iOS pipelines:

- Require physical Mac hardware ❌  
- Depend on manual signing ❌  
- Are difficult to scale ❌  

### ✅ MobileOps Pro Approach:

- Cloud-native iOS builds via GitHub Actions  
- Headless authentication using `.p8` API keys  
- Fully automated CI/CD pipeline  
- Deterministic and reproducible builds  

---

## 🛠️ Core Technologies

| Technology | Purpose |
|----------|--------|
| **GitHub Actions** | Cloud orchestration (macOS runners) |
| **Xcode CLI (`xcodebuild`)** | Native archive & IPA export |
| **Node.js** | Hybrid build environment |
| **Capacitor** | Web-to-native synchronization |
| **PlistBuddy** | Runtime metadata injection |
| **altool** | App Store Connect upload |

---

## ⚠️ Real-World Case Study: Afronet Dating App

### 🧩 Challenge

During the deployment of the **Afronet Dating App**:

- The available MacBook reached **OS End-of-Life**  
- Required Xcode version was unavailable  
- Local build pipeline became unusable  

---

### 💡 Solution

Instead of upgrading hardware, I redesigned the deployment strategy:

- Performed **one-time Apple credential provisioning** on Mac  
- Migrated all build and deployment processes to GitHub Actions  

This enabled:

- Compilation using latest iOS SDKs (cloud Xcode)  
- Headless signing via App Store Connect API  
- Fully automated TestFlight deployment  

---

## 🍎 Apple Credential Provisioning Note

While this pipeline removes macOS dependency for CI/CD execution, a Mac environment is still required **once** for secure setup.

### 🔐 MacBook was used for:

- Generating App Store Connect API Key (`.p8`)  
- Creating Bundle Identifier  
- Apple Developer account configuration  

### 🚀 MacBook was NOT used for:

- ❌ App build process  
- ❌ Archive generation  
- ❌ IPA export  
- ❌ TestFlight deployment  

👉 All CI/CD execution happens in **GitHub Actions macOS runners**

---

## ⚙️ Pipeline Flow

### 1. Trigger

- Push to `main`  
- Manual workflow dispatch  

---

### 2. Environment Setup

```bash
npm install
```
```bash
npm run build
```
```bash
npx cap sync ios
```

### 3. Native Build Phase

Compile using xcodebuild

Generate .xcarchive

### 4. Metadata Injection

Uses PlistBuddy to:

Auto-increment build number

Inject runtime metadata

### 5. Code Signing (Headless)

Uses .p8 API Key authentication

No certificates exposed

No interactive login required

### 6. Export IPA

Uses exportOptions.plist

Generates signed .ipa

### 7. Distribution

Upload via altool

Deploy to TestFlight

### 🔐 Environment Configuration
Required GitHub Secrets
Secret	Description

```bash
APP_STORE_CONNECT_API_KEY	.p8 key content
APP_STORE_CONNECT_API_KEY_ID	Apple Key ID
APP_STORE_CONNECT_API_ISSUER	Issuer ID
TEAM_ID	Apple Team ID
```
```bash
### 📦 Project Structure
mobile-ops-pro/
 ┣ 📂 ci-architecture/
 ┃ ┗ 📜 ios-deploy.yml
 ┣ 📂 ios/
 ┃ ┗ 📂 App/
 ┃    ┗ 📜 exportOptions.plist
 ┣ 📂 assets/
 ┃ ┗ 📂 images/
 ┃    ┗ 📜 mobileops-architecture.png
 ┣ 📜 README.md
 ┗ 📜 LICENSE
```
### 🧠 Key Engineering Decisions
Avoided Fastlane → used native Apple CLI tools

Designed for deterministic CI builds

Separated credential provisioning from execution

Ensured reproducible signing

### ⚡ Challenges Solved

Xcode version incompatibility

Hardware dependency for builds

Secure CI/CD signing

Hybrid app deployment complexity

### 🚀 Outcome

100% cloud-based iOS deployment pipeline

Zero reliance on local hardware for CI/CD

Reduced deployment time from hours → minutes

Enabled scalable DevOps workflow

---
### 📸 Screenshot
![Github Setup Credentials ](https://raw.githubusercontent.com/olubusade/mobile-ops-pro/main/assets/images/mobileops/ios-credentials-archive.png)

---

CI Pipeline Execution

Credentials Archive

Build Output

### 🎥 Demo

🚧 Demo video coming soon (TestFlight deployment pipeline)

### 🛠️ How to Use
git clone https://github.com/olubusade/mobile-ops-pro.git
Setup Steps

Configure exportOptions.plist

Add GitHub Secrets

Rename:

ci-architecture → .github/workflows

Trigger pipeline

### 📜 License

MIT License

### 🤝 Contact

Busade Adedayo
🌐 https://busade.dev

🐙 https://github.com/olubusade