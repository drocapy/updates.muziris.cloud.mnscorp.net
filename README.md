# Understanding SQL Injection (SQLi) 🛠️🔓

**SQL Injection** is one of the most critical web application vulnerabilities. It occurs when an attacker is able to manipulate SQL queries made to a database by injecting malicious input into the application's data fields.

---

## 🚀 What is SQL Injection?

SQL Injection allows attackers to:
1. Access sensitive data (e.g., user credentials, payment details).
2. Modify or delete data within the database.
3. Execute administrative operations on the database.
4. Sometimes, take control of the underlying server.

---

## 🎯 Example: Exploiting an SQL Injection Vulnerability

### Vulnerable Code:

```php
<?php
  // Vulnerable query
  $username = $_GET['username'];
  $password = $_GET['password'];

  $query = "SELECT * FROM users WHERE username = '$username' AND password = '$password';";
  $result = mysqli_query($conn, $query);
?>


 If an attacker inputs:
username: ' OR 1=1 --
password: anything
The query becomes:
SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = 'anything';
Here, 1=1 always evaluates as true, and -- comments out the rest of the query. This grants unauthorized access.


 🌟 Visual Demo
<div align="center"> <img src="https://media.giphy.com/media/JT1cvR5Q8geF1xBYAM/giphy.gif" width="400" alt="SQL Injection Animation"/> </div>


🛠️ How to Fix It?
1. Use Parameterized Queries:

Avoid direct concatenation of user input in SQL queries. Instead, use prepared statements:
<?php
  $stmt = $conn->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
  $stmt->bind_param("ss", $username, $password);
  $stmt->execute();
?>


2. Validate User Input:

    Only allow expected characters (e.g., alphanumeric).
    Reject special characters that could alter SQL syntax.

3. Limit Database Privileges:

Ensure that the database account used by the application has only the necessary permissions.

4. Use Web Application Firewalls (WAFs):

A WAF can help detect and block SQL Injection attempts.
📖 Learn More

   - OWASP SQL Injection Guide
   - SQL Injection Explanation (MDN)
