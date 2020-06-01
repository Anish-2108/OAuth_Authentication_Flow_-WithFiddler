# OAuth 2.0 Authentication and Authorization Flow with Fiddler

## *Description*
OAuth 2.0 is very widely used terminology on internet. It’s an open standard authorization and authentication protocol to grant access to Application or Websites and to users’ information without providing the password of them. OAuth uses JWT (JSON Web token) in encrypted form to exchange tokens between service provider and identity provider to access user resources and Web API.

To illustrate the authorization and authentication flow, I have chosen **LinkedIn** as *Service provider(sp)* and **Facebook** as *Identity provider(idp)*. The motive is to identify the redirections and to understand the authentication flow with Fiddler in action. (*Fiddler intercept session traffic*)

## *Overview*

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/oauth_authflow.jpg?w=1024)

## *Fiddler Prep*

* *Download and install fiddler tool and open the LinkedIn signup page and signup with facebook*

* *Start capturing session traffic from the fiddler untill the signup is complete then stop capturing and save all sessions.*
* ***Status codes: 100 (Continue) Status codes: 200 (Ok) Status codes: 300 (Redirections) Status codes: 400 (Client Error) Status codes: 500 (Server Error)***

**(Note: - All the sessions traffic is encrypted in https so allow https traffic from fiddler)**

## *Linkedin Sign-up via Facebook.*

![Dashboard](https://s2.aconvert.com/convert/p3r68-cdx67/ttwkz-jw3sl.png)

## *Fiddler In Action*

**Step 1:** LinkedIn URL Status code 200 (ok). User clicks sign up with Facebook and couple of redirects between LinkedIn and Facebook requesting OAuth server URI.

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/4.png?w=1024)

**Step 2:** OAuth process gets started and in redirect it is asking for OAuth server information. 
(www.linkedin.com/xauth/startauth?) 
*	LinkedIn in first redirect get the OAuth server redirect URL to get the Authorization URI

![Dashboard](https://anishpathan.files.wordpress.com/2020/06/2.1.png?w=1024)


**Step 3:** LinkedIn get the OAuth server URL and opens the redirected URL and request Authorization server with client Id, redirection URI and Scope of access required of user. (In this case It is asking for the profile information to Authorization server)

**Response header** 
*facebook.com/v2.12/dialog/oauth?client_id=161320853908703&redirect_uri=https%3A%2F%2Fwww.linkedin.com%2Fgenie%2Ffinishauth&scope=email&display=popup&state=2309982a-87c5-4330-b4d1-d0687f421dd9*

![Dashboard](https://anishpathan.files.wordpress.com/2020/06/3.1.png?w=1024)


**Step 4:** In response to the client request of profile scope. Authorization server provides the Authorization URI with redirect URL back to LinkedIn with Facebook login page pop-up. 

(**Note: - It’s on Facebook login Landing page. So, user it is getting authenticated to Facebook and not at client application page this is where OpenID connect comes into the picture**)

**Request Header of Facebook login Page**

*GET /login.php?
skip_api_login=1&api_key=161320853908703&kid_directed_site=0&app_id=161320853908703&signed_next=1&next=https%3A%2F%2Fwww.facebook.com%2Fv2.12%2Fdialog%2Foauth%3Fclient_id%3D161320853908703%26redirect_uri%3Dhttps%253A%252F%252Fwww.linkedin.com%252Fgenie%252Ffinishauth%26scope%3Demail%26display%3Dpopup%26state%3D2309982a-87c5-4330-b4d1-d0687f421dd9%26ret%3Dlogin%26fbapp_pres%3D0%26logger_id%3D7d6cb9eb-d86f-41d4-b891-80a3e811a58e


![Dashboard](https://anishpathan.files.wordpress.com/2020/06/4.1.png?w=1024)


**Step 5:** OpenId Connecct
* User post the credentials and hits login and OpenID connect starts. Open ID connect authenticates in secure session between client and server and obtains basic profile information about the end user using REST API.
* OpenID provides grant or licenses to access resources rather than provide the information of authentication to service provider.
* In other words, Facebook validates the credentials securely and then request the user consent to authorize service provider to access end users profile information or as per scope. 
* Once end user click **OK** on user consent. Authorization server generate the Authorization code and access token that is sent to LinkedIn to access the users profile information
* As shown in the below diagram 

![Dashboard](https://anishpathan.files.wordpress.com/2020/06/5.1.jpg?w=1024)

*	**Cookies** are stored at browsers with session id and expiry date and time stamp on it to keep session active and remembers the Authorization URI to save some hassle for the next time.

![Dashboard](https://anishpathan.files.wordpress.com/2020/06/6.1.png?w=1024)

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
