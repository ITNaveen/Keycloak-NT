# keycloack support oauth2. OAuth 2.0 is a security standard that lets one app access your data in another app — without giving away your password.

Process - 
1. Token Request Process

Client sends a request to Keycloak's token endpoint
Request includes authentication info: client ID, username, and password
Keycloak verifies if the client is registered and credentials are valid
If verification passes, Keycloak issues an access token in JWT format
This token gives the client authorization to access protected resources

2. API Access Process

Client includes the JWT token when requesting access to protected resources
The resource server verifies the token before allowing access
Spring Security handles token verification on the server side
Verification checks if the JWT token is valid and trustworthy
After successful verification, access to the API or resource is granted

3. Keycloak Role Configuration

Configure user roles in Keycloak (example: "premium_material_access")
Assign these roles to specific users who should have premium access
These roles are included in the JWT token when users authenticate
This allows the resource server to know what permissions the user has

4. Role-Based Access Control (RBAC)

The API server extracts the user's role from the JWT token
Based on the role information, the server allows or denies access
Example: Only users with "premium_material_access" role can access premium content

This process ensures that only authenticated users with proper authorization can access protected resources.

# Keycloak Basic Terms
1. Client - Any application or service registered with Keycloak. Requests authentication and        authorization from Keycloak for users.

2. User - The user of the Client. The subject who receives authentication and authorization through the Client.

3. Group - A mechanism for managing multiple Users together. Allows for bulk permission settings for multiple Users.

4. Role	- Permissions assigned to Clients or Users. Can specify what kind of operations can be performed on which resources.

5. Realm - A mechanism for collectively managing security information in Keycloak. Allows management according to individual security requirements by separating Realms for different organizations.
Each company = a Realm
