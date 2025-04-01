# OWASP Juice Shop Writeup Workshop (UPR Style)

## Overview
This workshop introduces students to web application security using OWASP Juice Shop. The goal is to explore common vulnerabilities, understand their impact, and document findings in GitHub writeup format, following the style used in [bennyspr/workshops](https://github.com/bennyspr/workshops/tree/main/upr).

## Objectives
- Learn to identify and exploit common web application vulnerabilities.
- Create clear, structured writeups using Markdown and GitHub.
- Develop skills in documentation, collaboration, and presentation.

## Setup Instructions
### Juice Shop Instance
- Use this for demos and testing: [http://167.71.160.25:3000/#/login](http://167.71.160.25:3000/#/login)

### Juice Shop (Local Setup)
```bash
git clone https://github.com/juice-shop/juice-shop.git
cd juice-shop
npm install
npm start
```

### GitHub Repository
```bash
gh repo create juice-shop-writeups --public --clone
```

---

## Core Challenge List for Class

### ✅ Basic Challenges
- Score Board
- Login Admin / Login Jim / Login Bender

### 🧠 XSS & Injection Challenges
- DOM XSS
- Reflected XSS

Each challenge in this list includes:
- **Category & Difficulty**
- **Walkthrough Steps**
- **Expected Result**
- **Explanation & Mitigation**

---

## Challenge: Score Board

**Category:** Information Disclosure  
**Difficulty:** 🟢 Easy

### 🧩 Step-by-Step Walkthrough
1. Navigate to the login page: [http://167.71.160.25:3000/#/login](http://167.71.160.25:3000/#/login)
2. Without logging in, inspect the site’s source or open DevTools.
3. Press `Ctrl+F` and search for the word `score-board`.
4. Discover the hidden route `/#/score-board`.
5. Manually enter the URL:
   ```
   http://167.71.160.25:3000/#/score-board
   ```
6. View the challenge list and confirm it marks “Score Board” as solved.

### ✅ Expected Result
- You successfully access the hidden scoreboard.

### 📘 Explanation
- This route is not linked anywhere in the UI and must be manually discovered or guessed.
- This mimics real-world scenarios where admin or dev panels are obscured but not protected.

### 💡 Mitigation
- Use proper authentication and role-based access controls.
- Do not rely on obscurity as a security mechanism.

---

## Challenge: Login Admin / Jim / Bender

**Category:** Injection  
**Difficulty:** 🟠 Medium

### 🧩 Step-by-Step Walkthrough
1. Navigate to the Juice Shop login page: [http://167.71.160.25:3000/#/login](http://167.71.160.25:3000/#/login)

2. In the **Email** field, enter a single quote character (`'`) to test the application's behavior.
   - If you receive a SQL-related error, the input is vulnerable to injection.

3. Try the following SQL injection payload:
   ```
   ' OR 1=1--
   ```
   - Enter it in the **Email** field.
   - Type anything in the **Password** field.

4. Click **Log in** — you should now be authenticated as the first user in the database (usually the admin).

5. You can also target specific known users:
   - Email field: `jim@juice-sh.op' --`
   - Or: `bender@juice-sh.op' --`
   - Or: `admin@juice-sh.op' --`
   - Use any password or leave it blank.

### ✅ Expected Result
- You are logged in as one of the target users.

### 📘 Explanation
- The injected `' OR 1=1--` payload modifies the SQL query logic:
   - `'` ends the string literal.
   - `OR 1=1` makes the `WHERE` clause always true.
   - `--` comments out the rest of the query, skipping password checks.
- Entering `jim@juice-sh.op' --` applies the same bypass logic using a valid username.

### 💡 Mitigation
- **Use Parameterized Queries:** Prevent SQL injection by separating SQL logic from user input.
- **Input Validation:** Ensure input matches expected formats (e.g., valid emails).
- **Authentication Security:** Use strong passwords, rate-limiting, and MFA.
- **Error Handling:** Hide detailed error messages to prevent attacker insight.

---

## Challenge: DOM XSS

**Category:** XSS / Client-Side  
**Difficulty:** 🟡 Medium

### 🧩 Step-by-Step Walkthrough
1. On the homepage, use the **Search** bar and type any term (e.g., `apple`).
2. Observe the URL format:
   ```
   http://167.71.160.25:3000/#/search?q=apple
   ```
3. Replace `apple` in the URL with:
   ```
   <script>alert('DOM XSS')</script>
   ```
   Full URL:
   ```
   http://167.71.160.25:3000/#/search?q=<script>alert('DOM XSS')</script>
   ```
4. Load the modified URL.

### ✅ Expected Result
- The browser executes the alert script, showing a popup.

### 📘 Explanation
- DOM XSS occurs when user input is inserted into the DOM using unsafe methods like `innerHTML` without sanitization.
- Juice Shop's search input reflects the `q` parameter directly into the page content.

### 💡 Mitigation
- Avoid using `innerHTML` with user content.
- Use DOM sanitizers like **DOMPurify**.
- Implement a strict **Content Security Policy (CSP)**.

---

## Challenge: Reflected XSS

**Category:** XSS / Reflected  
**Difficulty:** 🟠 Medium

### 🧩 Step-by-Step Walkthrough
1. Log in at [http://167.71.160.25:3000/#/login](http://167.71.160.25:3000/#/login).
2. Go to **Account > Orders & Payment > Track Orders**.
3. In the **Order ID** field, enter:
   ```
   test
   ```
   and click **Track**.
4. Observe the URL:
   ```
   http://167.71.160.25:3000/#/track-result?id=test
   ```
5. Replace `test` with a URL-encoded payload:
   ```
   %3Ciframe%20src%3D%22javascript:alert('XSS')%22%3E
   ```
6. Final URL:
   ```
   http://167.71.160.25:3000/#/track-result?id=%3Ciframe%20src%3D%22javascript:alert('XSS')%22%3E
   ```
7. Press Enter — an alert box should pop up.

### ✅ Expected Result
- An alert box runs in the browser, indicating the payload executed.

### 📘 Explanation
- Reflected XSS occurs when input from the URL is reflected directly into the page output.
- Juice Shop does not sanitize the `id` parameter in this route, allowing scripts to run.

### 💡 Mitigation
- Encode all reflected input.
- Use secure frameworks that escape HTML.
- Set a strong CSP and avoid inline scripts.

---

