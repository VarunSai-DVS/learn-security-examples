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

- The main problem in `insecure.ts` is **Weak or Missing Authorization**.  
The server checks if the user’s role is `'admin'` based solely on the data provided in the request body, which is
highly insecure. There’s no real authentication mechanism or session validation to confirm the user's identity or
permissions before allowing role updates.

2. Briefly explain how a malicious attacker can exploit them

- **Spoofing**:  
  An attacker can exploit this by sending a crafted request that includes a fake `'admin'` role in the body. Without
real verification, the server accepts it and allows the attacker to update their own role—or someone else’s—even 
without the proper access rights. This opens the door to unauthorized privilege escalation.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability

- **Validating User Roles**:  
  The secure version checks that the user performing the role update actually has admin privileges by verifying their
- session and role. Only authorized admins are allowed to update user roles, effectively preventing unauthorized changes.

- **Secured Session Cookies**:  
  Session cookies are protected using the `httpOnly` and `sameSite` flags. This helps prevent session hijacking and
cross-site request forgery (CSRF) attacks by restricting how cookies are accessed and shared.

- **Hardcoded Secret (Security Risk)**:  
  Although `secure.ts` implements session handling, the session's secret key is **hardcoded in the code**, which is
unsafe. Ideally, this secret should be stored securely in an environment variable or secret manager. Keeping it
hardcoded exposes it to attackers and compromises session security.
