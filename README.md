# OAuth_Authentication_Flow_With_Fiddler

## *Description*

OAuth/OpenId connect is an open standard to grant access to Application or Websites to their information without providing them password. Oauth uses JWT (JSON Web token) in encrypted form to exchange tokens between serivce provider and identity provider.

To illustrate the authorization and authentication flow, I have chosen **LinkedIn** as *Service provider(sp)* and **Facebook** as *Identity provider(idp)*. The motive is to identify the redirections and to understand the authentication flow with Fiddler in action. (*Fiddler intercept session traffic*)

## *Overview*

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/oauth_authflow.jpg?w=1024)

*Download and install fiddler tool and open the LinkedIn signup page and signup with facebook*

*Start capturing session traffic from the fiddler untill the signup is complete then stop capturing and save all sessions.*

**(Note:- All of the sessions traffic are encrypted in https so allow Https traffic from fiddler)**

## **Linkedin Sign-up via Facebook.**

![Dashboard](https://s2.aconvert.com/convert/p3r68-cdx67/ttwkz-jw3sl.png)

## **Fiddler In Action** 

**Step 1:** Open Linkedin URL Status code 200 (ok).

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/4.png?w=1024)

**Step 2:** Oauth process strated **(*facebook.com/v2.12/dialog/oauth? (presented with client id and redirect to OAuth server with URI*)**

**Step 3:** Idp Opens the redirected URI with client id.
**Check the response header (*Transport*)**

**Response header** 
*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9*


**Step 4:** Facebook sents Authorization URI to Linkedin/Browser In responses to Authorization URI (user will be prompted to enter the credentials) Once user post the creds it will be encrypted to be sent to verify at idp(Facebook)

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/5.png?w=1024)

*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9&ret=login&fbapp_pres=0&logger_id=7d6cb9eb-d86f-41d4-b891-80a3e811a58e&cbt=1590765084307&ext=1590768704&hash=AeZbeAYE7clWLqPD*

**Cookies**
Cookies stored at browsers with expiry date&time stamp on it to keep session active and remebers the URI

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/7.png?w=1024)

**Step 5:** Based on the credentials provided by the user a secure encrpyted connection will be establised to authenticate the user and it will be either authorized or denied.

Once Idp verify the authorization URI (Idp will provide Authorization code in encrypted form. This code contains users Meta Data and access information of what all information service provider can access) see below fiddler trace.

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/6.png?w=1024)

*linkedin.com/genie/finishauth?**code=AQAEYHiJ_ebXC0g0zgIBTJIppYKP_jyNufxyccvVU0I3wR596EVu8ubbgHdT0jwT1vxoTE3fQ1sE3xBVOpHmW44GZN64B1-tlWkgUU6FJsrQuF2u803jB_GtzFgUz5yO2uUs4dzpI-a9JPuO-Dm1E7CDNq2rVpRWYJq0Mw7C25ZISuFpjaIln-K5WGyFICE34WVBhjpWYCfa1McgA4Y0HaMiwH20ejr-vF3rMrba4OeqsI-CcCnLuTH7Da46KDlBccU9wpEOgCNJdwn83r-9He3luNCYyyW7eTAF0AEC3heliVVHnO8WAN07LrjsRBjdcmenGEZgu3wMyuGSbOz4lXp7&state=2309982a-87c5-4330-b4d1-d0687f421dd9#_=_***

**Step 6:** 

**Step 7:**




