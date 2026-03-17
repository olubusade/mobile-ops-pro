# 📱 MobileOps Pro: Native iOS CI/CD Orchestrator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: GitHub Actions](https://img.shields.io/badge/CI-GitHub%20Actions-blue?logo=githubactions)](https://github.com/features/actions)
[![Environment: macOS 15](https://img.shields.io/badge/OS-macOS%2015-black?logo=apple)](https://developer.apple.com/xcode/)

A production-grade **Native iOS CI/CD Pipeline** designed for hybrid applications (Ionic/Capacitor). This project provides a blueprint for virtualizing the Apple build ecosystem in the cloud, allowing for fully automated deployments to **App Store Connect / TestFlight** without requiring local macOS hardware.

---

## 🛠️ The Problem & Solution


**The Challenge:** iOS development traditionally creates a "hardware bottleneck," requiring physical Mac machines for archiving and signing. This limits scalability in remote-first or Linux-based engineering environments.

## 💡 The "Legacy Hardware" Case Study
**Scenario:** During the development of a production mobile application, the local development environment (Macbook) reached its End-of-Life (EOL) for OS updates, preventing the installation of the latest Xcode version required for App Store submissions.

**The Solution:** Instead of investing in new hardware, I engineered this CI/CD orchestrator. By moving the heavy lifting to GitHub Actions' `macos-15` runners, I was able to:
- Compile against the latest iOS SDKs.
- Automate the signing process via headless API keys.
- Successfully deploy to TestFlight without a single local build.
---

## 🏗️ Technical Architecture

The pipeline is architected to be "zero-dependency" regarding third-party wrappers like Fastlane. It interacts directly with the Apple toolchain:

1. **Workspace Sync**: Pulls web assets and synchronizes the Capacitor bridge.
2. **Identity Management**: Injects `.p8` API keys into the CI filesystem for headless authentication.
3. **Metadata Injection**: Uses `PlistBuddy` to manipulate the `Info.plist` at runtime for dynamic versioning.
4. **Build-to-Archive**: Compiles the project into an `.xcarchive`.
5. **Signed Distribution**: Exports a signed `.ipa` using a specific `exportOptions.plist`.



---

## 🚀 Environment Configuration

To utilize this orchestrator, the following **Encrypted GitHub Secrets** must be configured:

| Secret | Description |
| :--- | :--- |
| `APP_STORE_CONNECT_API_KEY` | The content of your `.p8` API key file. |
| `APP_STORE_CONNECT_API_KEY_ID` | The 10-character Key ID from App Store Connect. |
| `APP_STORE_CONNECT_API_ISSUER` | Your Issuer ID (UUID) from the API page. |
| `TEAM_ID` | Your Apple Developer Team ID (e.g., `10-character string`). |

---

## 📦 Project Structure

To make this a "Senior Grade" repository, we will structure the code to be modular. Instead of a single massive YAML file, we will use a **Script-Backbone** approach. This makes the logic easier to test and far more readable for a recruiter.

I have updated the structure to include a dedicated `scripts` directory that handles the heavy lifting, ensuring the GitHub Action stays clean and professional.

### 📂 Updated Project Structure

```text
mobile-ops-pro/
 ┣ 📂 .github/workflows/
 ┃ ┗ 📜 ios-deploy.yml          # The Orchestrator (Clean & High-level)
 ┣ 📂 ios/
    ┗  📂 App/
 ┃      ┗ 📜 exportOptions.plist     # Distribution metadata template
 ┣ 📜 LICENSE                   # MIT License
 ┗ 📜 README.md                 # Complete Technical Documentation

```

---

### 📜 The Full README.md

```markdown
# 📱 MobileOps Pro: Native iOS CI/CD Orchestrator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![CI: GitHub Actions](https://img.shields.io/badge/CI-GitHub%20Actions-blue?logo=githubactions)](https://github.com/features/actions)
[![Environment: macOS 15](https://img.shields.io/badge/Stack-Native%20Xcode%20CLI-black?logo=apple)](https://developer.apple.com/xcode/)

A high-performance **Native iOS CI/CD Pipeline** designed for hybrid applications (Ionic/Capacitor). This orchestrator eliminates the "Hardware Bottleneck" by virtualizing the Apple build chain on cloud-native macOS runners.

---

## 🏗️ Architectural Overview

This pipeline is triggered **post-build**. Once web assets are compiled and the Capacitor bridge is synchronized (`npx cap sync`), this engine takes over to handle the complex native compilation, signing, and distribution phases.

### Key Engineering Features:
- **Zero-Dependency Core**: Uses native Apple CLI tools (`xcodebuild`, `altool`, `PlistBuddy`) instead of high-level wrappers like Fastlane.
- **Headless Authentication**: Implements secure `.p8` API key injection for non-interactive App Store Connect communication.
- **Dynamic Metadata Injection**: Automates build-number increments via `PlistBuddy` directly into the `Info.plist`.
- **Infrastructure as Code (IaC)**: Standardizes the `exportOptions.plist` to ensure reproducible binary signing across different environments.

---

## 🔐 Credential Provisioning Guide (The .p8 Process)

To bypass 2FA and enable headless cloud builds, you must provision an App Store Connect API Key.

1. **Generate Key**: Log in to [App Store Connect](https://appstoreconnect.apple.com/) > **Users and Access** > **Integrations**.
2. **Download .p8**: Create a new API Key with **App Manager** access. Download the `.p8` file (Save it; it can only be downloaded once).
3. **Extract Identifiers**: Copy the **Key ID** and **Issuer ID** from the same dashboard.
4. **Configure Secrets**: 
   - Paste the full text of the `.p8` file into a GitHub Secret named `APP_STORE_CONNECT_API_KEY`.
   - Add `APP_STORE_CONNECT_API_KEY_ID`, `APP_STORE_CONNECT_API_ISSUER`, and `TEAM_ID` to your GitHub Secrets.

---

## 🛠️ The Pipeline Workflow

### 1. Environment Preparation
The pipeline spins up a `macos-15` runner, installs Node.js, and performs a clean install of dependencies. It then executes:
```bash
npm run build
npx cap sync ios

```

### 2. Native Metadata Sync

The orchestrator uses `PlistBuddy` to synchronize the GitHub Run Number with the iOS `CFBundleVersion`. This ensures every TestFlight build is uniquely identified.

### 3. Archive & Export

The engine executes a two-stage native build:

* **Archive**: Compiles the workspace into a `.xcarchive` using the specified Development Team ID.
* **Export**: Converts the archive into a signed `.ipa` using the `exportOptions.plist` configuration.

### 4. Distribution

The final binary is pushed to TestFlight using `altool` with API Key authentication, eliminating the need for application-specific passwords.

---

## 📦 Project Execution

To implement this in your own project:

1. **Clone the repository**:
```bash
git clone [https://github.com/olubusade/mobile-ops-pro.git](https://github.com/olubusade/mobile-ops-pro.git)

```


2. **Configure `exportOptions.plist**`: Update the `teamID` and `bundleIdentifier` in the `ios-assets` folder.
3. **Set Secrets**: Populate your GitHub repository secrets as detailed in the Provisioning Guide.
4. **Push**: Deploy to the `main` or `release/*` branch.

---

## 📜 License

Distributed under the **MIT License**. See the `LICENSE` file for details.

---

## 🤝 Contact

**Busade Adedayo** - [busade.dev](https://busade.dev) - [GitHub @olubusade](https://github.com/olubusade)

```