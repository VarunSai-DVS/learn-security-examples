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

The issue with the insecure version is that it doesn’t log any incoming requests or responses. 
This lack of visibility makes it hard to trace what users are doing, spot unusual behavior, or figure out what went
wrong during errors. Without proper logging, debugging, monitoring for attacks, or auditing user activity becomes a
major challenge.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.

The secure version fixes this by adding middleware that logs each request. It captures key info like the HTTP method,
request URL, timestamp, and IP address. It also tracks important operations like message sending and retrieval.
All these logs go into a server.log file, helping with error diagnosis, security audits, and incident tracking. 
This brings better observability and makes handling issues easier.

That said, writing to files can be resource-heavy—especially under high load or when done synchronously.
If the logging blocks the main process or fails, it could cause slowdowns or even denial-of-service (DoS) problems.
To prevent that, logging should be optimized or handled asynchronously.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

The Middleware Pattern is used in the secure version. In Express.js, middleware functions sit in the path of a request
and can perform tasks like logging, authentication, or error handling. Here, the middleware logs important request
details before handing off control to the next handler. This approach keeps logging centralized and clean, without
mixing it into the core route logic.