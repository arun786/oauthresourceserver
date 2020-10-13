# OAuth2.0 Resource Server

Once the Authorization server setup done. we get the below urls from the Authorization Server

The OAuth Server is a KeyCloak Server, running on Port 8080.

The below are the details of the KeyCloak Server

    {
      "issuer": "http://localhost:8080/auth/realms/appsdeveloperblog",
      "authorization_endpoint": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/auth",
      "token_endpoint": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/token",
      "introspection_endpoint": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/token/introspect",
      "userinfo_endpoint": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/userinfo",
      "end_session_endpoint": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/logout",
      "jwks_uri": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/certs",
      "check_session_iframe": "http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/login-status-iframe.html",
      "grant_types_supported": [
        "authorization_code",
        "implicit",
        "refresh_token",
        "password",
        "client_credentials"
      ],
      "response_types_supported": [
        "code",
        "none",
        "id_token",
        "token",
        "id_token token",
        "code id_token",
        "code token",
        "code id_token token"
      ],
      "subject_types_supported": [
        "public",
        "pairwise"
      ],
      "id_token_signing_alg_values_supported": [
        "PS384",
        "ES384",
        "RS384",
        "HS256",
        "HS512",
        "ES256",
        "RS256",
        "HS384",
        "ES512",
        "PS256",
        "PS512",
        "RS512"
      ],
      "id_token_encryption_alg_values_supported": [
        "RSA-OAEP",
        "RSA1_5"
      ],
      "id_token_encryption_enc_values_supported": [
        "A256GCM",
        "A192GCM",
        "A128GCM",
        "A128CBC-HS256",
        "A192CBC-HS384",
        "A256CBC-HS512"
      ],
      "userinfo_signing_alg_values_supported": [
        "PS384",
        "ES384",
        "RS384",
        "HS256",
        "HS512",
        "ES256",
        "RS256",
        "HS384",
        "ES512",
        "PS256",
        "PS512",
        "RS512",
        "none"
      ],
      "request_object_signing_alg_values_supported": [
        "PS384",
        "ES384",
        "RS384",
        "HS256",
        "HS512",
        "ES256",
        "RS256",
        "HS384",
        "ES512",
        "PS256",
        "PS512",
        "RS512",
        "none"
      ],
      "response_modes_supported": [
        "query",
        "fragment",
        "form_post"
      ],
      "registration_endpoint": "http://localhost:8080/auth/realms/appsdeveloperblog/clients-registrations/openid-connect",
      "token_endpoint_auth_methods_supported": [
        "private_key_jwt",
        "client_secret_basic",
        "client_secret_post",
        "tls_client_auth",
        "client_secret_jwt"
      ],
      "token_endpoint_auth_signing_alg_values_supported": [
        "PS384",
        "ES384",
        "RS384",
        "HS256",
        "HS512",
        "ES256",
        "RS256",
        "HS384",
        "ES512",
        "PS256",
        "PS512",
        "RS512"
      ],
      "claims_supported": [
        "aud",
        "sub",
        "iss",
        "auth_time",
        "name",
        "given_name",
        "family_name",
        "preferred_username",
        "email",
        "acr"
      ],
      "claim_types_supported": [
        "normal"
      ],
      "claims_parameter_supported": false,
      "scopes_supported": [
        "openid",
        "address",
        "email",
        "microprofile-jwt",
        "offline_access",
        "phone",
        "profile",
        "roles",
        "web-origins"
      ],
      "request_parameter_supported": true,
      "request_uri_parameter_supported": true,
      "code_challenge_methods_supported": [
        "plain",
        "S256"
      ],
      "tls_client_certificate_bound_access_tokens": true
    }
    
 Once the KeyCloak Server configured, the below steps 
 
 1. A get call to the Server to get the Authorization Code. 
    
    ClientId configured on the Server = photo-app-code-flow-client
 
    http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/auth?
    
    client_id=photo-app-code-flow-client
    
    &response_type=code&scope=openid profile
    
    &redirect_uri=http://localhost:8083/callback
    
    &state=abcde
    
    Response from the above , we get the code
    
    http://localhost:8083/callback?
    
    state=abcde&session_state=c3b6120d-9eb1-477d-b2f7-7f2f72606c1a
    
    &code=4e44e265-a424-4a91-8b5b-04200d1eda67.c3b6120d-9eb1-477d-b2f7-7f2f72606c1a.8bb312a1-336f-4ff8-be8a-8acbfe5bd014
    
2. Based on the Authorization Code, we will be calling the below URL to get the access Token

    http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/token
    
![Access Token](https://github.com/arun786/oauthresourceserver/blob/main/src/main/resources/accessToken.png)    