# OAuth_Authentication_Flow_With_Fiddler

# Description

OAuth is an open standard to grant access to Application or Websites to their information without providing them password. Oauth uses JWT (JSON Web token) in encrypted form to exchange tokens between serivce provider and identity provider.

To illustrate the authorization and authentication flow, I have chosen LinkedIn as **Service provider(sp)** and Facebook as **Identity provider(idp)**. The motive is to identify the redirections and to understand the authentication flow with Fiddler in action. (*Fiddler intercept network traffic*)

## *OVERVIEW*

![Dashboard](https://anishpathan.files.wordpress.com/2020/05/oauth_authflow.jpg?w=1024)

Download and install fidder tool and open the LinkedIn sinup page. Start capturing session traffic from the fiddler untill the signup is complete then stop capturing and save all sessions.

Step 1: User enters the URL(http://linkedin.com/signup) in the browsers and sign's up with Facebook option. 

![Dashboard](https://s2.aconvert.com/convert/p3r68-cdx67/ttwkz-jw3sl.png)


