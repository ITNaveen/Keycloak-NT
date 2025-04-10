# To retrieve the endpoints for a specific realm in Keycloak, I use the OpenID Connect Discovery URL, which follows the standard .well-known/openid-configuration path. For example: http://<host>:<port>/realms/<realm-name>/.well-known/openid-configuration. This returns a JSON document containing all the necessary endpoints like the authorization endpoint, token endpoint, and public keys (jwks_uri). It's useful for configuring OIDC clients dynamically and ensures compatibility with various authentication libraries.

- http://localhost:8080/realms/news_realm/.well-known/openid-configuration .

- to see in better view try - https://jsonformatter.curiousconcept.com/# and then paste json output.

- we can see token_endpoint, so when user try to access resource then it sends request first to this endpoint and then receive jwt back and then he use url or resource along with this jwt to access resource.


# we need postman to check this endpoint - 
1. select workspace in postman, it can be used to send request to any URL.
- select new from top then HTTP, now we need to set up post request content. then may it post and paste target_endpoint in URL spot in postman.
- then from header (below URL section), change body to - url_encoded.
  - we need to add these field under this - 
  - client_id = news_app_client
  - grant_type = password
  - username = basic_user
  - password = ££££££££££

2. now we should check what kind of rights i get with this token, so copy this token and then open JWT.IO. based on JWT.IO outcome we can see we have set up basic_user with basic access and we can read the same from this token decode value.
3. lets do the same for premium_user, so now our access is good.
