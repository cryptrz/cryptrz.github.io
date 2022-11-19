---
title: Portswigger writeup - Brute-forcing a stay-logged-in cookie
categories: [writeup,walkthrough,security]
tags: [writeup,walkthrough,portswigger,burp,cookie,bruteforce,login,security]
---

These days I daily work on Web Security Academy, the final goal is becoming a Burp Suite Certified Practitioner. The last lab I was working on is <a href="https://portswigger.net/web-security/authentication/other-mechanisms/lab-brute-forcing-a-stay-logged-in-cookie" target="_blank">Brute-forcing a stay-logged-in cookie</a>, which is not so difficult but I think that a few details are missing in Portswigger's solution. So here's a walkthrough for this lab.

# **Lab: Brute-forcing a stay-logged-in cookie**

1. When the lab is open and Burp Suite is ready (Intercept off), Click “My account”
    
    ![stay-logged-in-1.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.1.png)
    
2. Enter Wiener’s credentials, check “Stay logged in” option and click “Log in” 
    
    ![stay-logged-in-2.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.2.png)
    
3. When logged in, see the title “My account” and the button “Update email” (useful for later, step 10), and click “Log out”
    
    ![stay-logged-in-3.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.3.png)
    
4. Switch to Burp, in Proxy > HTTP History, find the GET request for “my-account” page and check its cookies. Click on the arrow for the “stay-looged-in” cookie to see details
    
    ![stay-logged-in-4.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.4.png)
    
5. The decoded string for this cookie is Base64 encoded and contains “`username:hash`”
    
    ![stay-logged-in-5.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.5.png)
    
6. Copy/paste the hash on CrackStation to verify which algorithm is used (MD5) and its content (Wiener’s password, “peter”).
    
    ![stay-logged-in-6.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.6.png)
    
7. Go back to Burp and send the “my-account” page to Intruder
    
    ![stay-logged-in-7.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.7.png)
    
8. On Positions tab, add a payload position on the “stay-logged-in” parameter
    
    ![stay-logged-in-8.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.8.png)
    
9. On Payloads tab, add the password list provided by Portswigger for this lab, and add the following payload processing:
    1. Hash: MD5
    2. Add prefix: carlos
    3. Encode: base64-encode
        
    ![stay-logged-in-9.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.9.png)
        
10. On Options tab, in the “Grep - Match” section, click “Clear” first, and add a unique element from the “My account” page. It could be the H1 title “My account” (as in this example below), but it could be as well the “Update email” button. Add one of them in the list and choose same options if different (”Flag results”, “Match type” and “Exclude HTTP headers”)
    
    ![stay-logged-in-10.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.10.png)
      
11. Finally, switch back to Payloads tab and click “Start attack”
    
    ![stay-logged-in-11.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.11.png)
    
12. Wait a couple of seconds, when the attack is over, click the tab where is your “Grep - Match” string (”My account” or “Update email”) to see the valid response (which has a longer length and contains a string in the Payload column) to see its content. Then, when sure, right click on it and click “Send response in browser”
    
    ![stay-logged-in-12.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.12.png)
    
13. Copy the link
    
    ![stay-logged-in-13.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.13.png)
    
14. Paste it in the browser, the lab is solved
    
    ![stay-logged-in-14.png](/assets/images/2022-11-19-stay-logged-in/stay-logged-in-1.14.png)