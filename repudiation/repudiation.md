# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
The vulnerability is insufficient logging and monitoring. The application lacks request logging, error logging, activity tracking, and input validation logging.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
The secure.ts logs all HTTP requests with timestamp, method, URL, application errors with stack traces and IP. It also logs message sending activities with user details and retrieval attempts. And uses a dedicated log file (server.log) for persistence. 

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
The secure.ts implements the observer pattern through middleware for logging. This pattern works by the middleware acts as an observer that monitors all requests, and each request triggers the logging mechanism automatically.