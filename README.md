# test-express-plugin for various types of oauth-2

Prerequisites:
•	A developer account.

Steps:
1.	Create an OAuth App:
o	Provide a name, homepage URL, (e.g http://localhost:3000)
and callback URL (e.g., http://localhost:3000/auth/github/callback).
o	Note down the Client ID and Client Secret that are generated.

2.	Set up Environment Variables:
o	open .env file in your project's root directory.
o	Add the following lines, replacing the placeholders with your actual GitHub credentials:

#jwt:
JWT_SECRET= mysecret         #the secret
JWT_AUDIENCE= mycompany      #aud in jwt token
JWT_ISSUER= mycompany        #iss in jwt token

# for Github-oauth2
GITHUB_CLIENT_ID=  
GITHUB_CLIENT_SECRET=   
GITHUB_CALLBACK_URL= e.g http://localhost:4000/auth/github/callback
GITHUB_AUTH_ROUTE   = /auth/github
GITHUB_CALLBACK_ROUTE = /auth/github/callback
GITHUB_SUCCESS_REDIRECT_URL = /verify/user
GITHUB_FAILURE_REDIRECT_URL = /error

# for google- oauth2
GOOGLE_CLIENT_ID= 
GOOGLE_CLIENT_SECRET= 
GOOGLE_CALLBACK_URL= e.g http://localhost:4000/auth/google/callback
GOOGLE_AUTH_ROUTE = /auth/google
GOOGLE_SUCCESS_REDIRECT_URL = /verify/user
GOOGLE_FAILURE_REDIRECT_URL = /error

# for linkedin-oauth2
LINKEDIN_CLIENT_ID=
LINKEDIN_CLIENT_SECRET= 
LINKEDIN_CALLBACK_URL= e.g http://localhost:4000/auth/linkedin/callback 
LINKEDIN_AUTH_ROUTE = e.g. /auth/linkedin
LINKEDIN_SUCCESS_REDIRECT_URL = /verify/user
LINKEDIN_FAILURE_REDIRECT_URL = /error

# for passport session
SESSION_SECRET = your secret

3.	Configure http.yaml file like this
o	Add the following configuration under the authn section in src/eventsources/http.yaml 
authn:
  oauth2:
    github:
      client_id: <% process.env.GITHUB_CLIENT_ID %>
      client_secret: <% process.env.GITHUB_CLIENT_SECRET %>
      callback_url: <% process.env.GITHUB_CALLBACK_URL  %>
