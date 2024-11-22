# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
no session management/authentication.
weak authorization checks.
direct role manipulation.
no validation of role values.
authentication based only on user ID from request body.

2. Briefly explain how a malicious attacker can exploit them.
attacker can send a POST request like:
fetch('/update-role', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    userId: 2,         // Any user ID
    newRole: 'admin'   // Escalate to admin
  })
});
the vulnerable endpoint only checks if the user exists and their current role, which can be bypassed

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
The secure version prevents privilege escalation by:
using session-based authentication
implementing proper authentication checks
verifying user roles based on session data
using secure cookie settings
separating authentication and authorization logic