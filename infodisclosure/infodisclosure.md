# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
NoSQL injection vulnerability in the /userinfo endpoint.
No input validation for the username parameter.
Direct use of user input in database queries.
2. Briefly explain how a malicious attacker can exploit them.
Attacker can send a request like:
GET /userinfo?username[$ne]=null
Then this query becomes:
User.findOne({ username: { $ne: null } })
Which returns all user records since username is not equal to null and exposes sensitive user information.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
Input Type Validation:
if (typeof username !== 'string') {
    return res.status(400).send('Invalid username format');
}
Input Sanitization:
const sanitizedUsername = username.replace(/[^\w\s]/gi, '');

Error Handling:
try {
    // Database query with sanitized input
} catch (error) {
    console.error('Error querying database:', error);
    res.status(500).send('Internal server error');
}