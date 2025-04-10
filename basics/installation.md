# to install keycloack, there are 2 ways - 
1. download installer and then install manually.
2. start a container with keycloack already installed in that.

# using docker - 
```yml
docker run -d --name my-keycloak \
  -p 8080:8080 \
  -e KC_BOOTSTRAP_ADMIN_USERNAME=admin \
  -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:26.1.4 start-dev
```
- username and password is admin.

# create realm - 
- now we will create a realm, which is a management unit of users and client.
- open in UI top left, where it says keyclock (it should be set on master by default).

- on the same spot on drop down, click on create releam. 
1. create using existing json keycloak file - When you import an existing JSON file in Keycloak, you're not just creating a new realm. The file can contain the complete configuration for the realm, including:
Users, Clients, Roles, Identity providers, Groups, Authentication flows and other settings.
2. create new on your own.
make this enable - It activates the realm, making it accessible for:
Login for users in that realm.
Clients (apps) to authenticate and authorize through it.
Admin APIs specific to that realm.

# creating clients - 
- we will create clients now for the realm we created (news_realm).

- pick our realm and then select client and click. there are default clients created.
```yml
🛠️ Here's a breakdown of those default clients and what they do:
Client ID	              Purpose
account	                  Allows users to manage their account (e.g. change password, view sessions)
account-console	          UI for the account management interface
admin-cli	              Command-line interface for admin tasks (like using kcadm.sh)
broker	                  Used for identity brokering (e.g. logging in via Google, Facebook)
realm-management	      Used by the admin console for realm-level management
security-admin-console	  For internal security-related console functions (rarely used directly)
web-origins	For defining  allowed web origins (CORS), often placeholder or blank
login-theme (optional)	  If you see this, it's related to login UI customization

OpenID Connect is an identity layer built on top of OAuth 2.0. It allows clients — like web or mobile apps — to authenticate users without handling their passwords directly. Instead, the authentication happens via a trusted identity provider like Keycloak or Google.

# Keycloack auth process - 
🔄 Authentication Flow in Keycloak:

User enters credentials:
The user visits your website (the client) and enters their username and password into the login form.

Request sent to Keycloak:
Once the user submits the form, the browser sends the login request to Keycloak (which is acting as the identity provider).
The request is sent to the Keycloak Authorization Server to verify the credentials.

Keycloak checks credentials:
Keycloak checks the username and password against its user database (user pool).
If the credentials are correct, it proceeds to authenticate the user.

Keycloak issues tokens:
If authentication is successful, Keycloak will generate a JWT token (usually an ID token and an access token).
The ID token contains information about the authenticated user (e.g., username, email, roles), and the access token grants the app permission to access certain resources (like APIs).

Redirect back to your application:

After issuing the tokens, Keycloak redirects the user back to your application (using the redirect URI you’ve defined when setting up the client in Keycloak).
The redirect will include the tokens and other information needed for your app to identify the user.

User can now access the app:
The application receives the JWT tokens and uses them to authenticate the user on the client side.
From here, the user can now interact with the application, and the access token can be used to make API calls or access protected resources.

If the JWT token expires, the user may be required to re-authenticate, or the app can use a refresh token to get a new access token without needing the user to log in again.

# back to adding client - 
1. we have first realm called "news_realm".
2. then we add client in this realm "news_app_client".

3. now we set authentication flow - 
    - client authentication - we identify the type of this client. 2 types of clients confidentials and public. confidentials normally mean server side app, public - client side application.
    - authorization - here we can modify fine grained auth for this client. it means what can be done with which resources, wheather its read only or read-write.
    - Authentication flow - auth flow for users using this client. select direct access (username and password based).

4. click save (root address doesnt required), see client saved successfully. client id is visible - General settings. you are on client - client_details - client name.
5. now we need to set up roles for this client, so rules can be set for users of this client. now from this current location go to roles.
6. create roles - we will create 2 roles.
    - basic_access
    - premium_access


