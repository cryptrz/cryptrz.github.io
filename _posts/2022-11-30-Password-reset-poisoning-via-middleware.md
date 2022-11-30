---
title: Portswigger writeup - Password reset poisoning via middleware
categories: [writeup,walkthrough,security]
tags: [writeup,walkthrough,portswigger,burp,login,security,password,token]
---

# **Lab: Password reset poisoning via middleware**

To solve <a href="https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware" target="_blank">this lab</a>, you have to mix different techniques seen in the previous labs of the same series in order to steal, once again, the cookie of this unfortunate Carlos.

1. On the home page, click “My account
    
    ![Password-reset-poisoning-via-middleware-1.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-1.png)
    
2. Then, click “Forgot password”
    
    ![Password-reset-poisoning-via-middleware-2.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-2.png)
    
3. Enter your username, “wiener” and click “Submit”
    
    ![Password-reset-poisoning-via-middleware-3.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-3.png)
    
4. Access the email client through the exploit server (take the opportunity to keep the exploit server's URL on a notepad for later)
    
    ![Password-reset-poisoning-via-middleware-4.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-4.png)
    
    ![Password-reset-poisoning-via-middleware-5.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-5.png)
    
5. Click the link
    
    ![Password-reset-poisoning-via-middleware-5.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-6.png)
    
6. Enter twice a password (kept “peter” in this example) and click “Submit”
    
    ![Password-reset-poisoning-via-middleware-6.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-7.png)
    
7. In Burp Suite > Proxy > HTTP History, find the POST request “/forgot-password” that contains the username “wiener”, send it to Repeater
    
    ![Password-reset-poisoning-via-middleware-7.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-8.png)
    
8. In Repeater, add the header“`X-Forwarded-Host: <your exploit server URL>`”, change “wiener” to “carlos” and send it
    
    ![Password-reset-poisoning-via-middleware-8.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-9.png)
    
9. Go back to your email client, copy the link, paste it in a new tab, delete the token and don’t send it for the moment
    
    ![Password-reset-poisoning-via-middleware-9.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-10.png)
    
    ![Password-reset-poisoning-via-middleware-10.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-11.png)
    
10. Go back to the exploit server, click “Access logs”
    
    ![Password-reset-poisoning-via-middleware-11.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-12.png)
    
11. Find the GET request for “/forgot-password” that contains the token for “carlos“
    
    ![Password-reset-poisoning-via-middleware-12.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-13.png)
    
12. Go back to the new open tab (the one opened step 9), add carlos’s token at the end of the URL, where you deleted yours a few steps ago, and send it
    
    ![Password-reset-poisoning-via-middleware-14.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-14.png)
    
13. Enter twice a password (entered “peter” as well for this account) and submit
    
    ![Password-reset-poisoning-via-middleware-15.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-15.png)
    
14. Go to “My account” again
    
    ![Password-reset-poisoning-via-middleware-16.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-16.png)
    
15. Enter Carlos’s new credentials and click “Submit”
    
    ![Password-reset-poisoning-via-middleware-17.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-17.png)
    
16. The lab is solved
    
    ![Password-reset-poisoning-via-middleware-18.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/Password-reset-poisoning-via-middleware-18.png)