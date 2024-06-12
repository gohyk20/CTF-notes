1. View the HTML page source
2. View the network request sent while loading the web page to analyze any clues hidden in Cookies or Headers (eg https://c12yptonic.github.io/ctf-writeups/ctfs/deconstructf21#please-)
3. Look through any linked resources like scripts, images, hidden HTML content, stylesheets, favicons etc.

## SQL injection

*links*
https://ctf101.org/web-exploitation/sql-injection/what-is-sql-injection/
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#cheatsheets

example query: `SELECT * FROM users WHERE username='$username'` 

*payloads*
`hello" OR 1=1 ;--` -->  returns everything cuz condition is true (1=1) and -- comments out last quote

## Command injection

can be used when `system()` command or equivalent is used (basically it runs things on cmd)
e.g. `os.system('ping ' + domain)` --> domain is user input

*payloads*
`;ls`
`$(ls)`