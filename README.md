# Xero-Postman OAuth 2.0 via PKCE
A Postman collection for authenticating to the Xero API. 

## Steps to get up and running
Follow these steps to quickly get up and running with the Xero API and Postman with the PKCE Grant Type:

### 1. Import the Xero OAuth 2.0 collection and Xero environment into Postman
Click the button below and select the Desktop version of Postman (Chrome extension doesn't support environment variables). This will also install the Collection and Environment we'll be using.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/ba8b27ad64cda3d47caa#?env%5BXero%20OAuth%202.0%20PKCE%20Environment%5D=W3sia2V5IjoiY2xpZW50X2lkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InJlZnJlc2hfdG9rZW4iLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYWNjZXNzX3Rva2VuIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Inhlcm8tdGVuYW50LWlkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InJlX2RpcmVjdFVSSSIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJjb2RlX3ZlcmlmaWVyIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImNvZGVfY2hhbGxlbmdlIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InN0YXRlIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InRva2VuX2VuZHBvaW50IiwidmFsdWUiOiJodHRwczovL2lkZW50aXR5Lnhlcm8uY29tL2Nvbm5lY3QvdG9rZW4iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImF1dGhvcml6YXRpb25fZW5kcG9pbnQiLCJ2YWx1ZSI6Imh0dHBzOi8vbG9naW4ueGVyby5jb20vaWRlbnRpdHkvY29ubmVjdC9hdXRob3JpemUiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImNvZGVfY2hhbGxlbmdlX21ldGhvZCIsInZhbHVlIjoiUzI1NiIsImVuYWJsZWQiOnRydWV9XQ==)

### 2. Create an OAuth2 app at https://developer.xero.com/myapps
Go to the Xero developer portal and create an OAuth2 app.

If you haven't already signed up for a xero account you can do so [here](https://www.xero.com/signup/api/).

Use the following values:
1. App Name - your choice, but can't contain the word 'Xero'
1. Click ”Auth Code with PKCE”
1. Company or application URL - this needs to be an https address, but isn't used with postman
1. OAuth 2.0 redirect URI - also needs to be https or localhost address 
1. Click Create App

![create an oauth2 app](images/2_1_addApp.PNG)

You'll then be taken to your App's Details page. Keep this page open, and start up Postman.

![your newly created app details](images/2_2_createdAppDetails.PNG)

### 3. Add your Variables to the GET Authorisation Call
Copy the following details into the Params tab of the GET Authorisation Call:
1. Response_type - code
2. code_challenge_method - S256
3. Client ID - The Client ID you’ve just generated from the My Apps Page
4. Scopes - the scopes you’ll use to access the areas of Xero you require. More information on Scopes can be found here (https://developer.xero.com/documentation/oauth2/scopes) For this tutorial, we’d suggest using the following:
'openid profile email offline_access accounting.transactions'
5. OAuth 2.0 Redirect URI - The URI you’ve entered into the My Apps Page, this has to match exactly
6. State - any value you wish. Including the State with this call is optional however we suggest using it for this tutorial. If you’re not including State, untick the checkbox next to the State (you’ll also have to untick it on any calls you make)
7. code_challenge - see the step below

![Environment with some details](images/3_1_addedToEnvironment.PNG)

### 4. Add the scopes for the endpoints you will be accessing.
Our Developer Center lists the available scopes [here](https://developer.xero.com/documentation/oauth2/scopes). For getting started you will need at least:

`offline_access accounting.transactions`

In addition, to make further test calls we would also suggest adding:

`openid profile email accounting.contacts accounting.settings`

Add the scopes required to the `scopes` environment variable.

![Add some Scopes to your Environment](images/4_1_addScopesToEnvironment.PNG)

### 5. Generate your access token
1. Double-click on the GET Get Started request under the Xero OAuth 2.0 Collection
1. Select the Authorization tab
1. Click Get New Access Token

![Click the Get new Access Token Button](images/5_1_generateAccessToken.png)

1. Add the Variable names surronded by {{}} from your Environment into the fields, as shown in the screenshot below
1. Add https://login.xero.com/identity/connect/authorize to the Auth URL field
1. Add https://identity.xero.com/connect/token to the Access Token Field
1. Click Request Token

![Request your Access Token](images/5_2_addTheVariablesAndURLs.PNG)

At this stage you will be prompted to log in to Xero. 

![Login to Xero](images/5_3_askedToLogin.PNG)

If you've included the `openid profile email` scopes, you'll be asked to access your basic profile information.

![Allow Basic Profile Information](images/5_4_basicProfile.PNG)

You'll then be taken through to the Organisation Select window. Select the Organisation you want to connect to. If you want to connect to more than one Organisation, you can repeat the steps above and select another Organisation. 

![Select your Organisation](images/5_5_selectOrganisation.PNG)

Once complete you'll be passed back to Postman.

### 6. Set your Access and Refresh Tokens
We now have the last remaining tokens needed to access the Xero API. These need to be set to the Environment Variables, to do this:
1. Highlight the Access Token
1. Right-click on it
1. Select Set: OAuth 2.0 > access_token

Follow the same process for the Refresh Token.

![Set your Access and Refresh Tokens](images/6_1_setTheAccessAndRefreshTokens.png)

### 7. Find out which tenants (organisations) we are connected to

1. Double-click on the GET Connections request
1. Click Send
1. Like we did for the Access and Refresh Tokens, highlight the tenantId from the response, right click and select Set > OAuth 2.0 > xero-tenant-id

![GET access token](images/7_1_addTheTenantID.PNG)

Congrats! You're now authenticated and can start making API calls. Your access token will last for 12mins, after which time you'll need to refresh the token. 

### 8. Make your first API call!
1. Double-click to load the GET Invoices request
1. Ensure No Auth is set on the Authorization tab
1. Click Send

### 9. Refreshing the token
1. Double-click to load the POST Refresh token request
1. Ensure No Auth is set on the Authorization tab
1. Click Send

### Notes:
* We use the built in OAuth 2.0 support to get the token, however we then set this as an environment variable. So we don't need to use this support when making the normal API calls.
