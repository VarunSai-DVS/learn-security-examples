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
   
There is a spoofing vulnerability in insecure.ts. This is because of the improper cookie configurations:

a. Since httpOnly is set to false, this gives access to the session cookie, which is a vulnerability. 
A malicious site can steal the session cookie and impersonate the user.

b. Since sameSite is not set, this makes it possible for cookies to be sent along with cross-site requests.
This can be exploited resulting in Cross-Site Request Forgery (CSRF) attacks.

2. Briefly explain different ways in which vulnerability can be exploited.

Cookies: When the user visits a malicious page, JavaScript can access
the session cookie and use it, since httpOnly is not set to true. The stolen cookie can be used to
impersonate the user on the server.

Cross-Site Request Forgery (CSRF): When the user is logged in on insecure.ts and goes to a malicious site on the
same browser which has a hidden form, the malicious site can then submit a request to the server impersonating the user.
Since sameSite is not set, the browser sends the user's session cookie with the request, allowing the attacker to
perform actions on behalf of the user without the user's knowledge.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

httpOnly: true: This setting blocks JavaScript from reading the session cookie, protecting it from being stolen by
malicious scripts.

sameSite: true: This option restricts cookies to same-origin requests only, stopping them from being shared across
sites and reducing CSRF risks.