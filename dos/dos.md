# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack

In **insecure.ts**, there are several vulnerabilities that could allow an attacker to perform a Denial of Service (DoS) attack:

- **Invalid User Input**:  
  The `id` parameter in the query is not validated. If a non-MongoDB ObjectId is passed, it causes the server to crash since MongoDB cannot process the malformed ID.

- **No Try-Catch for Error Handling**:  
  The application lacks proper error handling. If the database throws an error (e.g., due to invalid input), the server doesn't catch it, potentially leading to a crash.

- **No Rate Limiting**:  
  Without any request throttling, attackers can flood the server with repeated requests, exhausting server resources and causing downtime.

- **Lack of Input Sanitization**:  
  The server does not sanitize the `id` input, making it susceptible to NoSQL injections and unpredictable query behavior, which can degrade performance or lead to crashes.

2. Briefly explain how a malicious attacker can exploit them

A malicious actor can take advantage of the above vulnerabilities in several ways:

- **Exploiting Invalid IDs**:  
  By sending requests with invalid or malformed `id` values (e.g., `?id=123`), the attacker can cause MongoDB to throw errors. If done repeatedly, this could crash the server and cause a denial of service.

- **Flooding the Server with Requests**:  
  Since there's no rate limiting, the attacker can spam the server with requests, overwhelming it and making it unresponsive for legitimate users.

- **Triggering Unhandled Errors**:  
  Without try-catch blocks, database errors caused by invalid input can crash the app. An attacker could exploit this by repeatedly sending bad requests to keep the service down.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability

**secure.ts** addresses the DoS vulnerabilities using several strategies:

- **Rate Limiting**:  
  The `express-rate-limit` middleware is used to cap the number of requests an IP can make in a short time window (e.g., 5 seconds). This helps defend against request spamming and reduces the risk of DoS.

- **Error Handling**:  
  Database operations are wrapped in a try-catch block. Any errors thrown during the query are caught and handled gracefully, preventing unexpected crashes and improving stability.

- **Input Sanitization**:  
  Sanitizing the `id` before querying the database is recommended. This would prevent malformed inputs or injection attempts, improving overall security and reliability.

```ts
try {
  const user = await db.findById(sanitizeId(req.query.id));
  // ...
} catch (err) {
  res.status(500).send('Something went wrong');
}
