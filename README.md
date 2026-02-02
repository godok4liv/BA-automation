# BA-automation
Pharmacy BAS Automation Suite
This repository contains the end-to-end (E2E) automation framework for the Business Automation System (BAS). The project focuses on streamlining pharmacy operations, ensuring data integrity, and preventing theft through automated security audits.

🚀 Key Objectives
Workflow Integrity: Automating Procurement → Stocking → Selling.

Security: Monitoring staff activity through automated Audit Log verification.

Scalability: Testing multi-branch operations with isolated data.

Customer Engagement: Validating prescription reminders and notifications.

🛠 Tech Stack
Framework: Testing Frameworks for Javascript | Write, Run, Debug | Cypress

Language: JavaScript / TypeScript

Reporting: Allure Reports/Mochawesome

Design Pattern: Page Object Model (POM)

IDE: VS Code

📁 Folder Structure
Plaintext
├── .vscode/                 # Editor settings & extensions
├── cypress/
│   ├── e2e/                 # Test scenarios (Onboarding, Sales, Security)
│   ├── fixtures/            # Test data (logos, user credentials)
│   ├── support/             # Custom commands & Page Objects
├── allure-results/          # Raw data for Allure reports
└── cypress.config.js        # Main configuration

💻 Getting Started

Prerequisites
Ensure you have Node.js installed.

Installation
Bash

Install dependencies
npm install
3. Running Tests
Open Cypress UI: npx cypress open

Run all tests (Headless): npm run cy:run

Generate & View Allure Report: npm run test:allure

🔒 Security & Audit Features
The suite includes specialized tests to ensure the system's "Transparency and Security" goals:

RBAC Checks: Ensures staffs cannot access sensitive financial or branch-level settings.

Audit Logs: Every price override or stock adjustment is verified against the backend log.

Data Consistency: Cross-checks the UI stock levels against the database using cy.task

Running Test in CI/CD Pipeline

name: BAS Automation Pipeline

on:
push:
branches: [ main, develop ]
pull_request:
branches: [ main ]

jobs:
cypress-run:
runs-on: ubuntu-latest
steps:
- name: Checkout Code
uses: actions/checkout@v4



  - name: Setup Node.js
    uses: actions/checkout@v4
    with:
      node-version: 18
  - name: Install Dependencies
    run: npm ci
  - name: Run Cypress Tests
    run: npm run cy:run # Runs our headless script with Allure enabled
    continue-on-error: true # Ensures we still generate reports if tests fail
  - name: Generate Allure Report
    run: npm run allure:report
  - name: Deploy Report to GitHub Pages
    uses: peaceiris/actions-gh-pages@v3
    with:
      github_token: ${{ secrets.GITHUB_TOKEN }}
      publish_dir: ./allure-report
      Pipeline Strategy for Multi-Branch Testing
To achieve the goal of "smooth multi-branch operations,"  use of Matrix Strategy. This runs the tests against different branch environments simultaneously to save time.
YAML
strategy:
  matrix:
    branch: [branch-a, branch-b, branch-c]
steps:
  - name: Run Tests per Branch
    run: npx cypress run --env branch_url=${{ matrix.branch }}.pharmacy.com

