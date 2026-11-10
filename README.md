# 📚 Library API Automation Testing Suite

An end-to-end Automated Testing framework built for the Library REST API using Postman, executed via Newman CLI, and powered by Data-Driven Testing (DDT) with custom interactive HTML reporting.

---

## 🌟 Project Overview & Key Highlights
This project demonstrates complete API test coverage for managing a library system (Adding, Retrieving, and Deleting books). It incorporates advanced Postman features such as environment dynamic scoping, automated request chaining, custom schema assertions, and headless CLI execution.

- 100% Test Pass Rate: 54 out of 54 assertions passed seamlessly across all iterations.
- Data-Driven Execution: External CSV integration (book.csv) for dynamic payload population and multi-iteration testing.
- Automated Request Chaining: Dynamic variable passing across dependent API endpoints.
- Automated Teardown & Clean-up: Smart fallback cleanup logic using pm.execution.setNextRequest to delete created records automatically after test runs.
- CI/CD & Headless Ready: Integrated with Newman CLI and htmlextra for stakeholder-ready HTML reporting.

---

## 🛠️ Tech Stack & Key Concepts Covered
- API Client: Postman Desktop & Postman Runner
- Command Line Execution: Newman CLI (newman)
- Reporting Framework: htmlextra Reporter (newman-reporter-htmlextra)
- Data Source: External CSV Dataset (book.csv)
- Scripting & Logic: JavaScript (Postman Sandbox)
- Key Concepts:
  - RESTful API Testing & Status Code Validations
  - JSON Schema Structural Verification
  - Dynamic Environment & Collection Scope Variable Management
  - Pre-request Scripts & Test Assertions
  - Response Time Performance Metrics (<1000ms)

---

## 🏗️ Detailed Endpoint Workflow & Test Logic

### 1. POST /Library/Addbook.php (Add Book)
- Pre-request Script: Generates dynamic attributes (randomized ISBN/aisle numbers) and extracts payload values directly from book.csv.
- Assertions:
  - Status Code 200 OK check.
  - Validates response properties (Msg and ID).
  - Asserts response message equals "successfully added".
  - Verifies Header Content-Type contains application/json.
  - Asserts response time is below 1000ms.
  - Stores created bookID globally for subsequent endpoint validations.

### 2. GET /Library/GetBook.php?ID={{bookID}} (Retrieve Book)
- Logic: Queries book details using the dynamically stored bookID.
- Assertions:
  - Status Code validation (200 OK).
  - Verifies response array elements against stored collection variables (bookName, isbn, aisle, authorName).
  - JSON Schema Validation: Structural check ensuring types (string, object) match specifications.

### 3. POST /Library/DeleteBook.php (Delete Book)
- Logic: Deletes the created book record to ensure database cleanliness.
- Assertions:
  - Status Code 200 OK check.
  - Verifies deletion success message ("book is successfully deleted").
  - JSON Schema Validation: Ensures payload structural integrity.
  - Dynamic Cleanup Logic: Conditional checks evaluating execution flags to automatically redirect request flows where necessary.

---

## 📂 Repository Structure

```text
.
├── library.postman_collection.json    # Exported Postman Collection
├── Testing.postman_environment.json   # Exported Environment Variables File
├── book.csv                           # Test Dataset for Data-Driven Testing
├── newman/                            # Directory containing generated HTML Reports
└── README.md                          # Documentation
 
```

## 💡 Real-world Challenges & Engineering Solutions

### Challenge 1: Environment Variables Export Issue (Shared vs. Current Value)
- Issue: When exporting the testing environment JSON file, the base_url and url values were not reflecting or persisting inside the exported file, causing Newman CLI to fail to reach the server endpoints.
- Root Cause: The values were stored locally as Current Value in Postman rather than being synchronized to the Shared Value (Initial Value) column which gets exported.
- Solution: Navigated to the environment settings, enabled the Shared Value column view, populated the environment URLs explicitly into the shared fields, and re-exported the environment file. This ensured complete data persistence for headless execution.

### Challenge 2: CSV Data Scope & Null Values in Individual Request Executions
- Issue: Executing individual endpoints (e.g., GET /GetBook) directly in Postman returned null / undefined values for data variables extracted from book.csv (such as bookName and authorName).
- Root Cause: Data-driven CSV variables are bound to the IterationData scope, which is only injected when executing requests through the Collection Runner or Newman CLI, not during standalone single-request triggers.
- Solution: Executed the test suite through Postman Collection Runner and Newman CLI, passing book.csv as the data source across 3 iterations. This bound the CSV values correctly, resulting in 3 successful iterations with 100% assertions passed (54/54).

---

## 📊 Test Execution Summary & Report

| Metric | Details |
| :--- | :--- |
| Total Iterations | 3 |
| Total Requests Executed | 9 |
| Total Assertions | 54 |
| Passed Assertions | 54 (100%) |
| Failed Assertions | 0 |
| Average Response Time | ~257ms |

> Full HTML execution reports complete with interactive graphs and request/response logs are stored in the /newman directory.
> > 🔗 Live Interactive Report: [Click Here to View HTML Execution Report](https://htmlpreview.github.io/?https://github.com/hanin-magdy/Library-API-Newman-Automation/blob/main/newman/Library-2026-11-10-19-55-18-895-0.html)

---

## 🚀 How to Run the Tests Locally

### 1. Prerequisites
Ensure you have Node.js installed on your system. Then install Newman and the HTML extra reporter globally:

`bash
npm install -g newman
npm install -g newman-reporter-htmlextra

2. Execution Command Clone this repository, open your terminal inside the project root folder, and execute:

newman run library.postman_collection.json -e env -g globals -d "Book 2.csv" -r htmlextra


 

 
