# OAuth 2.0 Authentication and Authorization Flow with Fiddler

## *Description*
OAuth 2.0 and OpenId Connect is very widely used terminology on internet. It’s an open standard authorization and authentication protocol to grant access to Application or Websites and to users’ information without providing the password of them. OAuth uses JWT (JSON Web token) in encrypted form to exchange tokens between service provider and identity provider to access user resources and Web API.

To illustrate the authorization and authentication flow, I have chosen **LinkedIn** as *Service provider(sp)* and **Facebook** as *Identity provider(idp)*. The motive is to identify the redirections and to understand the authentication flow with Fiddler in action. (*Fiddler intercept session traffic*)

## *Overview*

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/oauth_authflow.jpg?w=1024)

## *Fiddler Prep*

* *Download and install fiddler tool and open the LinkedIn signup page and signup with facebook*

* *Start capturing session traffic from the fiddler untill the signup is complete then stop capturing and save all sessions.*
* ***Status codes: 100 (Continue) Status codes: 200 (Ok) Status codes: 300 (Ridrections) Status codes: 400 (Client Error) Status codes: 500 (Server Error)***

**(Note:- All of the sessions traffic are encrypted in https so allow https traffic from fiddler)**

## *Linkedin Sign-up via Facebook.*

![Dashboard](https://s2.aconvert.com/convert/p3r68-cdx67/ttwkz-jw3sl.png)

## *Fiddler In Action*

**Step 1:** LinkedIn URL Status code 200 (ok). User clicks sign up with Facebook and couple of redirects between LinkedIn and Facebook requesting OAuth server URI.

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/4.png?w=1024)

**Step 2:** This is where OAuth process gets started and in redirect its asking for OAuth server and  users read only access information.**(*facebook.com/v2.12/dialog/oauth? (presented with client id and redirect to OAuth server with URI*)**
* Access scope are previously defined between Idp/Authorization server which understand the scope of access like whether to read or write access to be provided based on the request.


**Step 3:** LinkedIn request for Authorization URI for the user with client id.
* In response facebook opens the redirected URI as shown in below (Headers).  
* **Check the response header (*Transport*)**

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/5.png?w=1024)

* **Response header** 
*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9*

**Note: -** (Authorization URI are the access and scope request of resource made by LinkedIn to Facebook)

**Step 4:** Facebook checks the client id, request and scope. 
* It then redirects Authorization URI back to LinkedIn with a Facebook pop-up requesting to sign in to verify user’s identity on the Facebook login page itself. (So now the user is getting authenticated on Facebook and not on LinkedIn)

* Once user post the credentials on login page Userid and password gets encrypted and sent to verify the credentials at idp(Facebook)

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/7.png?w=1024)

* **Response header** 
*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9&ret=login&fbapp_pres=0&logger_id=7d6cb9eb-d86f-41d4-b891-80a3e811a58e&cbt=1590765084307&ext=1590768704&hash=AeZbeAYE7clWLqPD*

* **Cookies** are stored at browsers with session id and expiry date and time stamp on it to keep session active and remembers the Authorization URI.

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/7.png?w=1024)

**Step 5:** Once Idp verify the authorization URI (Idp will provide Authorization code and access token in encrypted form to access user’s information in read-only access.

* Authorization code contains requested users Metadata and access information of what all information service provider can access) see below fiddler trace.
* Access token is encrypted in SSL bindings, which has the access scope defined for user's information. In this case its read-only access to the users resources.

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/6.png?w=1024)

* **Response header** 
*linkedin.com/genie/finishauth?code=AQAEYHiJ_ebXC0g0zgIBTJIppYKP_jyNufxyccvVU0I3wR596EVu8ubbgHdT0jwT1vxoTE3fQ1sE3xBVOpHmW44GZN64B1-tlWkgUU6FJsrQuF2u803jB_GtzFgUz5yO2uUs4dzpI-a9JPuO-Dm1E7CDNq2rVpRWYJq0Mw7C25ZISuFpjaIln-K5WGyFICE34WVBhjpWYCfa1McgA4Y0HaMiwH20ejr-vF3rMrba4OeqsI-CcCnLuTH7Da46KDlBccU9wpEOgCNJdwn83r-9He3luNCYyyW7eTAF0AEC3heliVVHnO8WAN07LrjsRBjdcmenGEZgu3wMyuGSbOz4lXp7&state=2309982a-87c5-4330-b4d1-d0687f421dd9#_=_*

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/8.png?w=1024)

* **Access Token** 
*/xauth/scb?_authEd=AgHk5vUHUi64VwAAAXJg_TBlbWTfxH_-3T0n_Lr52QUE0m9scLLhPEF_k-ZdpArVgbWH4K-I5kFc6-45M80VrhvKAXAJZeE7MmUKa1FjvrgBCAiURWVhpKwCOE8-KP1Llhv2w6C047KXGMG2PHPofDmvUFhdoouJKlAwNGkky0MaFnltpSfVt6ZwrgwzsMPx2Xzu90jwd3CVqcefU63dXWaA5N3lntiLgEQerrwoNzMXt59qYJ2WNI0_hJIrOb4siDNOuipYbB9VkW7xDpfBeFvS5uUUlACn3JKGH8S320GlkSWPuOh2E5XWTprDcGQCuqVHESUa2HuxW4BoKAUThEmwhti3I-W3B5K1c49SebL8BSz8Gbh8Mqeg61lbFRmvfgg*

**Step 6:** 
* Service provider (LinkedIn) calls for protected resources with access token and authorization code. 
* In response resource server provides the access requested resource information in read only access. See the trace it hold the user access information. see below it has FristName, LastName, ProfilePicUrl, Asid and etc..
* Facebook has Authorized LinkedIn to access the user resources without providing password to LinkedIn with user consent.

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/9.png?w=1024)


* **Step 7:**
OAuth successfully completed authorization and Authentication begins. 
