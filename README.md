# SQL Injection

The goal of this exercise is to hack web application on <https://kbe.felk.cvut.cz> that is vulnerable to [SQL injection](https://en.wikipedia.org/wiki/SQL_injection). The whole attack is divided into a series of tasks that build on each other. Therefore it is recommended to solve the tasks one by one in the given order.

## Task 1: Identify the first username.

On the main page, there is a login form that you need to pass through, without knowledge of credentials of any valid user. Hence try to login as the first user that is stored in the database.

Hints:
- In the backend code, there is an SQL query of the following form: `SELECT username FROM users WHERE username = '$_POST[username]' AND password = SHA1('$_POST[password]' . '$salt')`.
- Fill the username input with a value such that the `WHERE` clause is always true.
- Use hash symbol `#` to comment out an unwanted rest of the query.

## Task 2: Determine PIN of the first user.

As you can see, the accounts are not only password-protected, but also PIN-protected. Try to determine PIN of the first user using the vulnerability from the previous task.

Hints:
- Table `users` contains `pin` column.
- The `WHERE` clause can provide you a binary signal.
- Use [EXISTS operator](https://www.w3schools.com/sql/sql_exists.asp) in combination with [SQL LIKE operator](https://www.w3schools.com/sql/sql_like.asp) to check presence of individual digits in the PIN string associated with the first username.

## Task 3: Exfiltrate a list of all usernames, passwords, salts and pins.

Once you get in, there is hidden another SQL injection vulnerability, which will allow you to retrieve all the data from the database. Use it for printing usernames, passwords, salts and pins of all user accounts.

Hints:
- The pagination mechanism is based on the following SQL query: `SELECT date_time, base64_message_xor_key AS message FROM messages WHERE username = '$_SESSION[username]' LIMIT 1 OFFSET $_GET[offset]`.
- Use [UNION operator](https://www.w3schools.com/sql/sql_union.asp) to add more rows to the above `SELECT`. Note that the number of columns for both SELECTs must be the same. A dummy column can be added e.g. this way: `SELECT password, 1 FROM ...`.
- [SQL CONCAT function](https://www.w3schools.com/sql/func_sqlserver_concat.asp) can be also handy. 

## Task 4: Crack password hash of the first user.

Warning: Do not use brute-force as the password is quite long. Online tools should be sufficient.

## Task 5: Explain why this password is insecure despite it's length.

## Task 6: Crack your password hash.

Indication: Password hashes are [salted](https://en.wikipedia.org/wiki/Salt_(cryptography)) in the following way: `sha1($password . $salt)`, where students passwords are five characters long strings consisting of lowercase letters and numbers.

## Task 7: Derive xor key used for encrypting your messages.

As you might have noticed, the messages are stored in an encrypted form in the database, but before printing they are decrypted on the backend. Since you have access to both forms of messages, try to derive the xor key used for encrypting/decrypting your messages.

## Task 8: Decrypt the fourth message from the teacher's account.

## [BONUS] Task 9: Print a list of all table names and their columns.

Hint: [INFORMATION_SCHEMA](https://dev.mysql.com/doc/refman/5.5/en/tables-table.html)

## [BONUS] Task 10: Hack the teacher's account and stole the secure code.

Warning: Do not try to crack teacher's password as it is 12 characters long!

## [BONUS] Task 11: What is the key used for encrypting the secure codes?

## Task 12: Fix SQL queries from Task 1 and 3 to not be vulnerable to SQL injection.
