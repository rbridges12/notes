# Web Attacks

## CSRF - Cross Site Request Forgery

CSRF exploits trust in the browser, and specifically attacks state changing requests not data theft.

### Setup
- If a user is logged into a certain site (such as a bank), then their browser will store a valid login cookie
- if a request (GET, POST, etc) is sent by the user to that site, the browser will automatically send the login cookie with the request

### Basic CSRF Attack
- An attacker can make a malicious site that will send a request to the other site (the bank), asking to do something that only a logged in user would have permission to do (such as transfer money)
- If the attacker can get the user to go to the malicious site, then when the request is sent, the user's browser will automatically send the login cookie to the site and will successfully execute the attacker's order (and the user's money will be transferred)

### Login CSRF Attack
- An attacker can make a link that will send a POST request to the the targeted site, logging in as the attacker
- If the attacker can get the user to click on the link, the user will be unknowingly logged into the attackers account (this is bad if they go to enter their credit card number or something)

### Defenses
- **Referer Validation**: make sure the domain of an incoming request is the same as the domain of the site. if a request is sent to `bank.com` but the request is coming from `attacker.com` then don't accept the request. unfortunately the referer domain isn't always sent
- **Secret Token Validation**: on the bank's form, generate and submit a hidden secret value along with the request. If the secret value recieved by the server is different from the one that was generated, then the request is invalid
- **SameSite Cookies**: you can set the `SameSite` attribute to `Strict`, which prevents cookies being sent from any cross site request, or you can set it to `Lax` which allows GET requests but restricts others.


## SQLi - SQL Injection
If the server passes unsanitized inputs into SQL statements and then sends them to the database, it can be attacked by inserting an SQL command into the input, allowing you to access data from the database.

for example, if you enter `'` into an unsanitized input, it may be parsed into

`SELECT id FROM users WHERE username=''';`

which would lead to a syntax error since the semicolon is now inside of a quote. This is a good way to test if an SQL injection is possible.

To exploit a login, you could enter `' OR 1=1;` into an input, which would be parsed into

`SELECT id FROM users WHERE username='' OR 1=1;' AND password='';`

This will return all values in the table, meaning the server would consider you this a valid login despite you not having entered a valid username or password.

This attack can be mitigated easily using Parametrized SQL. Parametrized SQL only inserts the input at execution time after a query plan has been made, so the input cannot change the query plan and can only be used as a data value.

For example, with the previous SQL command, parametrized SQL would send the template query to the database, then it would "decide" to search for the id of a user with a certain username and password. Then, it would take the user inputs and use them as the username and password to look up. This is different because the `OR 1=1` cannot possibly be executed, since it wasn't part of the query plan that was made before the inputs were added.


## XSS - Cross Site Scripting

### Reflected XSS
- If an attacker convinces a user to follow a malicious link, that link can cause malicious code to be run on the user's browser
- The malicious code can be directly encoded into the URL as one of the input parameters using a script tag, for example `%3Cscript%3Ealert%28%22Hi%22%29%3B%3C%2Fscript%3E` is decoded into `<script>alert("Hi");</script>`
- if this string is used as an input parameter to a website that doesn't properly escape inputs, this code will be _reflected_ back to the user and executed on their browser

### Stored XSS
- an attacker uploads content to a site that shares their content with other people
- their content can be a script, and if the site doesn't escape it properly, then when another user goes to the site to see the attacker's content, the code will be loaded and executed in that user's browser
- example: an attacker sets their social media bio to `<script>alert("Hi");</script>`, when another user goes to their profile, this text is fetched from the site's server and loaded into the user's browser, where it is then recognized as a script and run by the browser

### XSS Defenses
- You can prevent users from entering certain elements in input fields, such as html tags, etc
- Content Security Policy (CSP) is a newer better approach that only allows your browser to execute scripts loaded in source files received from certain domains, and prevents inline scripts