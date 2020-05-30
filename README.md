# OAuth_Authentication_Flow_With_Fiddler

# Description

OAuth is an open standard to grant access to Application or Websites to their information without providing them password. Oauth uses JWT (JSON Web token) in encrypted form to exchange tokens between serivce provider and identity provider.

To illustrate the authorization and authentication flow, I have chosen LinkedIn as **Service provider(sp)** and Facebook as **Identity provider(idp)**. The motive is to identify the redirections and to understand the authentication flow with Fiddler in action. (*Fiddler intercept network traffic*)

## *OVERVIEW*

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/oauth_authflow.jpg?w=1024)

*Download and install fidder tool and open the LinkedIn signup page and signup with facebook*

*Start capturing session traffic from the fiddler untill the signup is complete then stop capturing and save all sessions.*

**Linkedin Sign-up via Facebook.**

![Dashboard](https://s2.aconvert.com/convert/p3r68-cdx67/ttwkz-jw3sl.png)

## **Fiddler In Action** 

**Step 1:** Open Linkedin URL Status code 200 (ok).

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/4.png?w=1024)

**Step 2:** Oauth process strated **(*facebook.com/v2.12/dialog/oauth? (presented with client id and redirect URI*)**

**Step 3:** Redirect to OAuth Server ***Check the response header (*Transport)**

**Response header** 
*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9*

**Cookies**

**Step 4:** Facebook sents Authorization URI to Linkedin
![Dashboard](https://anishpathan.files.wordpress.com/2020/05/5.png?w=1024)

*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9&ret=login&fbapp_pres=0&logger_id=7d6cb9eb-d86f-41d4-b891-80a3e811a58e&cbt=1590765084307&ext=1590768704&hash=AeZbeAYE7clWLqPD
