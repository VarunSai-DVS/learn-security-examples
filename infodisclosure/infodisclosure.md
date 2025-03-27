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

1. Briefly explain the potential vulnerabilities in `insecure.ts`

- **Exposing Sensitive Information**:  
  The server logs sensitive data like the username using `console.log()`, which could leak important system or user details that attackers might exploit.

- **NoSQL Injection Vulnerability**:  
  The query in the route is built directly from user input without proper validation or sanitization. This opens the door to NoSQL injection attacks where an attacker can manipulate the query to access unauthorized data.

- **Lack of Access Control**:  
  There's no restriction on who can access the `/userinfo` route. Anyone who supplies a valid username can get user information, regardless of authentication.

2. Briefly explain how a malicious attacker can exploit them

- **Exploiting Lack of Access Control**:  
  Since access to `/userinfo` isn’t restricted, an attacker can submit requests with arbitrary usernames to retrieve sensitive information about other users.

- **Exploiting NoSQL Injection Vulnerability**:  
  An attacker can craft malicious input like `?username[$ne]=` to alter the MongoDB query logic. For example: `/userinfo?username={"$ne":""}`
- This could return all users from the database, exposing private data such as usernames and passwords.

3. Briefly explain the defensive techniques used in `secure.ts` to prevent the information disclosure vulnerability

- **Input Validation and Sanitization**:
- First, it checks if `username` is a valid string. If not, it responds with a `400` status and a message: `Invalid username format`.
- Then it sanitizes the input by stripping out all non-alphanumeric characters using a regular expression. This reduces the risk of NoSQL injection.
