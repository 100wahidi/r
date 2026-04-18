# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project.

---

# Deployment Process for GitHub Pages

This document outlines the steps taken to deploy the project to GitHub Pages, the issues encountered during the process, and how they were resolved. It also includes explanations of key concepts such as relative paths, the `main` branch, and Git configuration.

---

## **Steps Taken**

### **1. Initial Setup**
- Installed the `gh-pages` package to handle deployment:
  ```bash
  npm install gh-pages --save-dev
  ```
- Added the following scripts to `package.json`:
  ```json
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist -b main -r https://github.com/100wahidi/application_data.git"
  }
  ```
  - `predeploy`: Builds the project before deployment.
  - `deploy`: Deploys the `dist` folder to the `main` branch.

### **2. Configured `vite.config.js`**
- Updated the `vite.config.js` file to set the correct `base` path for GitHub Pages:
  ```javascript
  import { defineConfig } from 'vite';
  import react from '@vitejs/plugin-react';

  export default defineConfig({
    plugins: [react()],
    base: '/application_data/', // Matches the repository name
  });
  ```
  - The `base` property ensures that all assets are loaded relative to the repository name.

### **3. Built the Project**
- Ran the build command to generate the production-ready files:
  ```bash
  npm run build
  ```
  - This created the `dist` folder containing the optimized files.

### **4. Deployed the Project**
- Ran the deploy command to push the `dist` folder to the `main` branch:
  ```bash
  npm run deploy
  ```

---

## **Issues Encountered and Solutions**

### **1. Relative Paths Issue**
- **Problem:** The application was not loading assets correctly because the `src` attribute in `index.html` used an absolute path (`/main.tsx`).
- **Solution:** Updated the `src` attribute to use a relative path:
  ```html
  <script type="module" src="./main.tsx"></script>
  ```
  - Relative paths ensure that assets are loaded correctly regardless of the deployment environment.

### **2. GitHub Pages Branch Configuration**
- **Problem:** GitHub Pages was configured to build from the `main` branch, but the `gh-pages` package was initially set to deploy to the `gh-pages` branch.
- **Solution:** Updated the `deploy` script to deploy directly to the `main` branch:
  ```json
  "deploy": "gh-pages -d dist -b main -r https://github.com/100wahidi/application_data.git"
  ```
  - The `-b main` flag specifies the `main` branch for deployment.

### **3. Git User Identity Not Configured**
- **Problem:** Git threw an error because the user name and email were not configured:
  ```
  Author identity unknown
  ```
- **Solution:** Configured the Git user identity globally:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```

### **4. Manual Upload Alternative**
- **Problem:** Considered manually uploading the `dist` folder to GitHub Pages.
- **Solution:** Automated the process using the `gh-pages` package to avoid manual uploads.

---

## **Key Concepts**

### **1. Relative Paths**
- Relative paths (e.g., `./main.tsx`) ensure that assets are loaded relative to the current directory. This is critical for GitHub Pages, where the base URL includes the repository name.

### **2. GitHub Pages and the `main` Branch**
- GitHub Pages can serve files from a specific branch. In this case, the `main` branch was used to host the `dist` folder.

### **3. Git Configuration**
- Git requires a user name and email to create commits. These were configured globally to enable the deployment process.

---

## **Final Verification**
- Verified the deployment by visiting the GitHub Pages URL:
  ```
  https://100wahidi.github.io/application_data/
  ```
- Ensured that all assets loaded correctly and the application functioned as expected.

---

## **Conclusion**
This document summarizes the deployment process, issues encountered, and solutions implemented. By following these steps, the project was successfully deployed to GitHub Pages.
