---
title: Portswigger writeup - Password reset Broken Logic
categories: [writeup,walkthrough,security]
tags: [writeup,walkthrough,portswigger,burp,login,security,password,token]
---

# **Lab: Password reset broken logic**

In <a href="https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic" target="_blank">this lab</a>, we'll bypass the "Forgot password" feature to get access on Carlos's account.

1. On the home page, click “My account
	
	![password-reset-broken-logic-1.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-1.png)
	
2. Next page, click “Forgot password”
	
	![password-reset-broken-logic-2.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-2.png)
	
3. To get Wiener’s email address, click “Email client”
	
	![password-reset-broken-logic-3.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-3.png)
	
4. In the new open tab, copy Wiener’s email address
	
	![password-reset-broken-logic-4.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-4.png)
	
5. Go back to the previous tab, paste the email address and click “Submit”
	
	![password-reset-broken-logic-5.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-5.png)
	
6. Go to the email client and refresh the page, a new email appears. Click the link
	
	![password-reset-broken-logic-6.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-6.png)
	
7. In the new open page, type and confirm a password and click “Submit”
	
	![password-reset-broken-logic-7.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-7.png)
	
8. On Burp, in Proxy > HTTP History, find the POST reset that contains twice the “temp-forgot-password-token” (at the top and after “connection close”) and send it to Repeater
	
	![password-reset-broken-logic-8.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-8.png)
	
9. Delete the 2 tokens and replace “wiener” with “carlos”
	
	![password-reset-broken-logic-9.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-9.png)
	
10. When it’s done, send the reset, you get a 302 response
	
	![password-reset-broken-logic-10.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-10.png)
	
11. Go back to the lab and click “My account”
	
	![password-reset-broken-logic-11.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-11.png)
	
12. Enter Carlos’s credentials (still “peter” in this example, adapt if you have chosen something else) and click “Submit”
	
	![password-reset-broken-logic-12.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-12.png)
	
13. Carlos’s username and email address appear, the lab is solved
	
	![password-reset-broken-logic-13.png](/assets/images/2022-11-28-password-reset-broken-logic/password-reset-broken-logic-13.png)