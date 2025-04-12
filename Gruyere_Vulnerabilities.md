
# Gruyere Vulnerability Write-Up

**Target App:** Gruyere (by Google)  
**Purpose:** Educational, intentionally vulnerable web app for learning security.  
**Author:** Google Security Team

---

## High Severity Vulnerabilities

### 1. Cross-Site Scripting (XSS)
- **Type:** Stored & Reflected XSS
- **Location:** Comment fields, search bars, user input
- **Impact:** Arbitrary JavaScript execution, cookie theft, UI manipulation
- **Payload Example:**
  ```html
  <script>alert('XSS')</script>
  ```
- **Fix:** Input validation, output encoding, use CSP headers

---

### 2. Command Injection
- **Type:** OS Command Injection
- **Location:** File upload functionality, system commands
- **Impact:** Full system compromise, remote command execution
- **Payload Example:**
  ```bash
  ; cat /etc/passwd
  ```
- **Fix:** Use secure APIs like subprocess.run() with arguments array

---

### 3. Access Control Bypass (IDOR)
- **Type:** Insecure Direct Object Reference
- **Location:** User profile & snippets accessed via user ID
- **Impact:** Unauthorized access to other users' data
- **Payload:**
  ```http
  GET /snippets.gtl?uid=admin
  ```
- **Fix:** Server-side access checks and RBAC implementation

---

## Medium Severity Vulnerabilities

### 4. CSRF (Cross-Site Request Forgery)
- **Type:** No token in state-changing requests
- **Location:** Admin actions, password change
- **Impact:** Forced user action without consent
- **Payload Example:**
  ```html
  <img src="http://gruyere/change_password?newpw=hacked">
  ```
- **Fix:** Implement CSRF tokens

---

### 5. Authentication Bypass (Weak Session Management)
- **Type:** Predictable tokens
- **Location:** Cookie-based session
- **Impact:** Session hijacking
- **Fix:** Use strong, unpredictable tokens with expiration

---

### 6. Information Disclosure
- **Type:** Directory listing, stack traces, file source view
- **Location:** /source endpoint or URL leakage
- **Impact:** Reveal internal code or system structure
- **Fix:** Disable directory listing, generic error messages

---

## Low Severity Vulnerabilities

### 7. Clickjacking
- **Type:** UI Redress
- **Impact:** Tricking users into performing unintended clicks
- **Fix:** Use header `X-Frame-Options: DENY`

---

### 8. Open Redirect
- **Type:** Unsafe redirection based on user input
- **Location:** Login or redirect page
- **Impact:** Redirect to malicious external domains (phishing)
- **Payload:**
  ```http
  /login?next=http://evil.com
  ```
- **Fix:** Validate and restrict redirection domains

---

### 9. Lack of Rate Limiting
- **Type:** Brute-force opportunity
- **Location:** Login endpoint
- **Impact:** Login brute-force attacks possible
- **Fix:** Implement request throttling and CAPTCHA

---
