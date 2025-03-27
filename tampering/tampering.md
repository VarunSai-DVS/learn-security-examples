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

The insecure.ts server is open to Cross-Site Scripting (XSS) risks because it doesn't clean up user input (trim())
before saving it in the session. It also directly injects user-provided input into the HTML
using res.send, which lets attackers slip in harmful JavaScript. Even though the server seems to expect alphanumeric
input, it doesn’t actually enforce that anywhere.

2. Briefly explain how a malicious attacker can exploit them

Cross-Site Scripting (XSS): They can enter harmful scripts in the name field—something like
which will run as soon as someone visits the page.

Malicious Redirects: An attacker could insert something to mislead
users to dangerous websites.

Phishing Attacks: An attacker might insert a fake login form to trick users into entering their credentials.

3. Briefly explain why secure.ts does not have the same vulnerabilities?

The secure.ts server avoids these issues by properly sanitizing any input before it’s saved or shown on the page.

Input Sanitization with escapeHTML()

The escapeHTML() function replaces characters with their safe HTML equivalents,
making sure that the browser treats input as plain text. This approach blocks XSS attempts by ensuring any
injected HTML or scripts are displayed as text, not executed.