
# Google Gruyere Vulnerabilities and Exploits

Google Gruyere is a deliberately insecure web application designed for educational purposes. Below are the key vulnerabilities present in the application, along with step-by-step guides to exploit them.

---

## 1. Cross-Site Scripting (XSS)

### Reflected XSS

**Steps to Exploit:**
1. Navigate to the search feature.
2. Enter a payload like `<script>alert('XSS')</script>` in the input.
3. If the page reflects the script without sanitization, an alert box pops up.

### Stored XSS

**Steps to Exploit:**
1. Login to the application.
2. Navigate to `New Snippet` or `Add Comment` section.
3. Enter `<script>alert('Stored XSS')</script>` in the snippet/comment.
4. When another user views this content, the script is executed.

---

## 2. Cross-Site Request Forgery (CSRF)

**Steps to Exploit:**
1. Create an HTML file on your system with the following content:

```html
<form action="http://google-gruyere.appspot.com/1234567890/password" method="POST">
  <input type="hidden" name="pw" value="hacked" />
  <input type="submit" value="Submit request" />
</form>
```

2. While logged in as a user in Gruyere, open the HTML file in your browser.
3. If CSRF protections are missing, the password is changed without your knowledge.

---

## 3. Cross-Site Script Inclusion (XSSI)

**Steps to Exploit:**
1. Look for `.js` files with sensitive data (like `snippets.gtl?uid=<user>`).
2. Write a malicious JS script on another site to steal data.
3. Due to improper headers (`Content-Type` as JSON not enforced), the data can be stolen via `<script src>` tag.

---

## 4. Directory Traversal

**Steps to Exploit:**
1. Attempt accessing internal files via URL manipulation:
   ```
   http://google-gruyere.appspot.com/1234567890/snippets.gtl?uid=../../../../etc/passwd
   ```
2. If not properly sanitized, this may return sensitive server data.

---

## 5. Information Disclosure

**Steps to Exploit:**
1. Observe error messages or use parameters like `debug=true`.
2. URL like `http://google-gruyere.appspot.com/1234567890/snippets.gtl?debug=true` may leak debug info.

---

## 6. Remote Code Execution (via insecure file uploads)

**Steps to Exploit:**
1. Go to the upload feature.
2. Try uploading `.jsp`, `.php`, or `.py` files.
3. If allowed and executed server-side, this can lead to RCE.

---

## 7. Open Redirect

**Steps to Exploit:**
1. Try URLs like:

```
http://google-gruyere.appspot.com/1234567890/login?redirect=http://evil.com
```

2. If redirected to `evil.com` without validation, it's an open redirect.

---

## 8. Insecure Direct Object References (IDOR)

**Steps to Exploit:**
1. After login, observe URLs with user IDs:
   ```
   http://google-gruyere.appspot.com/1234567890/snippets.gtl?uid=USER_ID
   ```
2. Change `USER_ID` to another valid user's ID.
3. If data is shown without permission checks, it's vulnerable.

---

## 9. Session Fixation

**Steps to Exploit:**
1. Log in to the app and observe session cookies.
2. Try setting cookies manually before login.
3. If session isn't regenerated on login, attacker can hijack session.

---

## 10. Cookie Manipulation

**Steps to Exploit:**
1. Modify the `GRUYERE_ID` cookie using browser dev tools.
2. Try escalating privileges or impersonating another user.

---

## Note

This document is for **educational purposes only** and should not be used on live applications without permission.

---
