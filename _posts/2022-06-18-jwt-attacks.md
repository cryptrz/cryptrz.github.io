---
title: Portswigger writeup - JWT Attacks
categories: [writeup,walkthrough,security]
tags: [writeup,walkthrough,portswigger,burp,jwt,json,security]
---

Since I've created this website, I didn't write a "real" post yet. So, as a first start, I'll talk about the new labs provided by Portswigger's <a href="https://portswigger.net/web-security/dashboard" target="_blank">Web Security Academy</a> few days ago: <a href="https://portswigger.net/web-security/jwt" target="_blank">JWT Attacks</a>). This is also my very first writeup ever, I hope everything will be clear (Let me know in a private message on <a href="https://www.linkedin.com/in/franck-ridel/" target="_blank">Linkedin</a> if you see something wrong, I'll fix it). 

As I write this post, I solved only the 3 first labs. The next ones will be added in an update to this post or in a "part 2", soon.

# Preparation

Before starting, install the <a href="https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd" target="_blank">JWT Editor</a> extension on <a href="https://portswigger.net/burp" target="_blank">Burp Suite</a> (available for <a href="https://portswigger.net/burp/communitydownload" target="_blank">Community</a> and <a href="https://portswigger.net/burp/pro" target="_blank">Professional</a> versions), it will be used for all labs.


# Lab 1: JWT authentication bypass via unverified signature

1. Go to My Account
    
    ![jwt-attacks-1-1](/assets/images/2022-06-18-jwt-attacks/1-1.png)
    
2. Enter the credentials “wiener” and “peter”
    
    ![jwt-attacks-1-2](/assets/images/2022-06-18-jwt-attacks/1-2.png)
    
3. From the HTTP History, send the “/my-account” page to repeater
    
    ![jwt-attacks-1-3](/assets/images/2022-06-18-jwt-attacks/1-3.png)
    
4. Highlight the payload part of the token and go to the “Decoded from” section on the right side
    
    ![jwt-attacks-1-4](/assets/images/2022-06-18-jwt-attacks/1-4.png)
    
5. Change “wiener” to “administrator” and click on “Apply changes”
    
    ![jwt-attacks-1-5](/assets/images/2022-06-18-jwt-attacks/1-5.png)
    
6. Send the modified page, if successful the response will be a code 200
    
    ![jwt-attacks-1-6](/assets/images/2022-06-18-jwt-attacks/1-6.png)
    
7. Get the link to the response and paste it in the web browser
    
    ![jwt-attacks-1-7-1](/assets/images/2022-06-18-jwt-attacks/1-7-1.png)
    
    ![jwt-attacks-1-7-2](/assets/images/2022-06-18-jwt-attacks/1-7-2.png)
    
8. When the admin account is displayed, click on “Admin panel” link
    
    ![jwt-attacks-1-8](/assets/images/2022-06-18-jwt-attacks/1-8.png)
    
9. Find the “/admin” page in the HTTP History and repeat the previous steps from 3 to 7
    
    ![jwt-attacks-1-9-1](/assets/images/2022-06-18-jwt-attacks/1-9-1.png)
    
    ![jwt-attacks-1-9-3](/assets/images/2022-06-18-jwt-attacks/1-9-3.png)
    
    ![jwt-attacks-1-9-4](/assets/images/2022-06-18-jwt-attacks/1-9-4.png)
    
    ![jwt-attacks-1-9-5](/assets/images/2022-06-18-jwt-attacks/1-9-5.png)
    
    ![jwt-attacks-1-9-6](/assets/images/2022-06-18-jwt-attacks/1-9-6.png)
    
    ![jwt-attacks-1-9-7](/assets/images/2022-06-18-jwt-attacks/1-9-7.png)
    
10. When the “Admin panel” is displayed, click on the “delete” link for “carlos”
    
    ![jwt-attacks-1-10](/assets/images/2022-06-18-jwt-attacks/1-10.png)
    
11. Repeat the same steps again, from 3 to 7 (except the time, the final response will be a 302 and not a 200), for the “/admin/delete?username=carlos” page
    
    ![jwt-attacks-1-11-1](/assets/images/2022-06-18-jwt-attacks/1-11-1.png)
    
    ![jwt-attacks-1-11-2](/assets/images/2022-06-18-jwt-attacks/1-11-2.png)
    
    ![jwt-attacks-1-11-3](/assets/images/2022-06-18-jwt-attacks/1-11-3.png)
    
    ![jwt-attacks-1-11-4](/assets/images/2022-06-18-jwt-attacks/1-11-4.png)
    
12. Go back to the browser, the lab is solved
    
    ![jwt-attacks-1-12](/assets/images/2022-06-18-jwt-attacks/1-12.png)
    

# Lab 2: JWT authentication bypass via flawed signature verification

1. Go to My Account
    
    ![jwt-attacks-2-1](/assets/images/2022-06-18-jwt-attacks/2-1.png)
    
2. From the HTTP History, send the “/my-account” page to Repeater
    
    ![jwt-attacks-2-2](/assets/images/2022-06-18-jwt-attacks/2-2.png)
    
3. Highlight the Header part of the token and go to the “Decoded from” section on the right side
    
    ![jwt-attacks-2-3](/assets/images/2022-06-18-jwt-attacks/2-3.png)
    
4. Change “RS256” to “none” and click on “Apply changes”
    
    ![jwt-attacks-2-4](/assets/images/2022-06-18-jwt-attacks/2-4.png)
    
5. Highlight the Payload part of the token and go to the “Decoded from” section on the right side
    
    ![jwt-attacks-2-5](/assets/images/2022-06-18-jwt-attacks/2-5.png)
    
6. Change “wiener” to “administrator” and click on “Apply changes”
    
    ![jwt-attacks-2-6](/assets/images/2022-06-18-jwt-attacks/2-6.png)
    
7. Highlight the Signature of the token and delete it
    
    ![jwt-attacks-2-7](/assets/images/2022-06-18-jwt-attacks/2-7.png)
    
8. Send the modified request, if successful, the response will be a code 200
    
    ![jwt-attacks-2-8](/assets/images/2022-06-18-jwt-attacks/2-8.png)
    
9. Get the link to the response and paste it in the browser
    
    ![jwt-attacks-2-9-1](/assets/images/2022-06-18-jwt-attacks/2-9-1.png)
    
    ![jwt-attacks-2-9-2](/assets/images/2022-06-18-jwt-attacks/2-9-2.png)
    
10. When the admin account is displayed, click on “Admin panel”
    
    ![jwt-attacks-2-10](/assets/images/2022-06-18-jwt-attacks/2-10.png)
    
11. Find the “/admin” page in the HTTP History and repeat the same steps from 2 to 9
    
    ![jwt-attacks-2-11-1](/assets/images/2022-06-18-jwt-attacks/2-11-1.png)
    
    ![jwt-attacks-2-11-2](/assets/images/2022-06-18-jwt-attacks/2-11-2.png)
    
    ![jwt-attacks-2-11-3](/assets/images/2022-06-18-jwt-attacks/2-11-3.png)
    
    ![jwt-attacks-2-11-4](/assets/images/2022-06-18-jwt-attacks/2-11-4.png)
    
    ![jwt-attacks-2-11-5](/assets/images/2022-06-18-jwt-attacks/2-11-5.png)
    
    ![jwt-attacks-2-11-6](/assets/images/2022-06-18-jwt-attacks/2-11-6.png)
    
    ![jwt-attacks-2-11-7](/assets/images/2022-06-18-jwt-attacks/2-11-7.png)
    
    ![jwt-attacks-2-11-8](/assets/images/2022-06-18-jwt-attacks/2-11-8.png)
    
    ![jwt-attacks-2-11-9](/assets/images/2022-06-18-jwt-attacks/2-11-9.png)
    
12. When the Admin panel is displayed, click on the “delete” link for “carlos”
    
    ![jwt-attacks-2-12](/assets/images/2022-06-18-jwt-attacks/2-12.png)
    
13. Find the “/admin/delete?username=carlos” page in the HTTP History and repeat the same steps from 2 to 9 (The final response will be a code 302, not 200)
    
    ![jwt-attacks-2-13-1](/assets/images/2022-06-18-jwt-attacks/2-13-1.png)
    
    ![jwt-attacks-2-13-2](/assets/images/2022-06-18-jwt-attacks/2-13-2.png)
    
    ![jwt-attacks-2-13-3](/assets/images/2022-06-18-jwt-attacks/2-13-3.png)
    
    ![jwt-attacks-2-13-4](/assets/images/2022-06-18-jwt-attacks/2-13-4.png)
    
    ![jwt-attacks-2-13-5](/assets/images/2022-06-18-jwt-attacks/2-13-5.png)
    
    ![jwt-attacks-2-13-6](/assets/images/2022-06-18-jwt-attacks/2-13-6.png)
    
    ![jwt-attacks-2-13-7](/assets/images/2022-06-18-jwt-attacks/2-13-7.png)
    
14. Go back to the browser, the lab is solved
    
    ![jwt-attacks-2-14](/assets/images/2022-06-18-jwt-attacks/2-14.png)
    

# Lab 3: JWT authentication bypass via weak signing key

1. Save this JWT secrets list: [https://raw.githubusercontent.com/wallarm/jwt-secrets/master/jwt.secrets.list](https://raw.githubusercontent.com/wallarm/jwt-secrets/master/jwt.secrets.list)
2. Enter the credentials “wiener:peter”
    
    ![jwt-attacks-3-2](/assets/images/2022-06-18-jwt-attacks/3-2.png)
    
3. Go to “My account” page
    
    ![jwt-attacks-3-3](/assets/images/2022-06-18-jwt-attacks/3-3.png)
    
4. In the HTTP History, send the page to the Repeater
    
    ![jwt-attacks-3-4](/assets/images/2022-06-18-jwt-attacks/3-4.png)
    
5. Copy the token
    
    ![jwt-attacks-3-5](/assets/images/2022-06-18-jwt-attacks/3-5.png)
    
6. Use hashcat to get the secret, using the token copied above and the JWT Secret List downloaded at step 1: `hashcat -a 0 -m 16500 <token> <JWT secrets list>` 
    
    ![jwt-attacks-3-6](/assets/images/2022-06-18-jwt-attacks/3-6.png)
    
7. Use the upper arrow (↑) the go back to the previous command and add `—show` at the end to get the secret key (the result will be “secret1”)
    
    ![jwt-attacks-3-7](/assets/images/2022-06-18-jwt-attacks/3-7.png)
    
8. On [jwt.io](https://jwt.io), paste the token in the “Encoded” section, replace “wiener” with “administrator” and add “secret1” as a secret
    
    ![jwt-attacks-3-8](/assets/images/2022-06-18-jwt-attacks/3-8.png)
    
9. Copy the new token generated on [jwt.io](http://jwt.io), paste it in the Repeater and send the request, you’ll get code 200
    
    ![jwt-attacks-3-9-1](/assets/images/2022-06-18-jwt-attacks/3-9-1.png)
    
    ![jwt-attacks-3-9-2](/assets/images/2022-06-18-jwt-attacks/3-9-2.png)
    
10. Get the link to the response and paste it in the browser
    
    ![jwt-attacks-3-10-1](/assets/images/2022-06-18-jwt-attacks/3-10-1.png)
    
    ![jwt-attacks-3-10-2](/assets/images/2022-06-18-jwt-attacks/3-10-2.png)
    
11. When the admin account is displayed, go to the “Admin panel” 
    
    ![jwt-attacks-3-11-1](/assets/images/2022-06-18-jwt-attacks/3-11-1.png)
    
    ![jwt-attacks-3-11-2](/assets/images/2022-06-18-jwt-attacks/3-11-2.png)
    
12. In the HTTP History, find the “/admin” page and send it to the Repeater
    
    ![jwt-attacks-3-12](/assets/images/2022-06-18-jwt-attacks/3-12.png)
    
13. Repace the JWT token with the one previously generated on [jwt.io](http://jwt.io) and send the request, you’ll get a code 200
    
    ![jwt-attacks-3-13](/assets/images/2022-06-18-jwt-attacks/3-13.png)
    
14. Again, get the link to the response and paste it in the browser
    
    ![jwt-attacks-3-14-1](/assets/images/2022-06-18-jwt-attacks/3-14-1.png)
    
    ![jwt-attacks-3-14-2](/assets/images/2022-06-18-jwt-attacks/3-14-2.png)
    
15. Finally, click on the “delete” link for “carlos”
    
    ![jwt-attacks-3-15-1](/assets/images/2022-06-18-jwt-attacks/3-15-1.png)
    
    ![jwt-attacks-3-15-2](/assets/images/2022-06-18-jwt-attacks/3-15-2.png)
    
16. In the HTTP History, find the “/admin/delete?username=carlos” page and send it to the Repeater
    
    ![jwt-attacks-3-16](/assets/images/2022-06-18-jwt-attacks/3-16.png)
    
17. Replace the token with the modified previous one (the same previously generated on [jwt.io](http://jwt.io)). Send the request, you’ll get a code 302
    
    ![jwt-attacks-3-17](/assets/images/2022-06-18-jwt-attacks/3-17.png)
    
18. Go back to the browser, the lab is solved 
    
    ![jwt-attacks-3-18](/assets/images/2022-06-18-jwt-attacks/3-18.png)