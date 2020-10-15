# Step wise access to resource server

Once the KeyCloak Server configured, the below steps 
 
## 1. A get call to the Server to get the Authorization Code. 
    
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
    
## 2. Based on the Authorization Code, we will be calling the below URL to get the access Token

    http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/token
    
![Access Token](https://github.com/arun786/oauthresourceserver/blob/main/src/main/resources/accessToken.png)    


## 3. Based on the Access Token, we can get any resource from the resource server.

    The below are the details of the controller
    
    package com.arun.oauthresourceserver.controller;
    
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    /**
     * @author arun on 10/12/20
     */
    
    @RestController
    @RequestMapping("/users")
    public class UsersController {
    
        @GetMapping("/status/check")
        public String status() {
            return "Working...";
        }
    }

So, when the header has Authorization as Bearer <access token>, we will get the status as Working...

when the end point is hit.
    


    package com.arun.oauthresourceserver.controller;
    
    import org.springframework.security.core.annotation.AuthenticationPrincipal;
    import org.springframework.security.oauth2.jwt.Jwt;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    import java.util.Collections;
    import java.util.Map;
    
    /**
     * @author arun on 10/12/20
     */
    
    @RestController
    @RequestMapping("/token")
    public class TokenController {
    
        @GetMapping
        public Map<String, Object> getToken(@AuthenticationPrincipal Jwt jwt) {
            return Collections.singletonMap("principal", jwt);
        }
    }


The above will give the jwt claim details

    {
        "principal": {
            "tokenValue": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJuTlVOVXdpcVdfbHMxNWxBcVEyZTl4ekFtRXYxUHUxdXFZMnR1Z3dvRnBvIn0.eyJleHAiOjE2MDI1NjY4NDYsImlhdCI6MTYwMjU2NjU0NiwiYXV0aF90aW1lIjoxNjAyNTY2NTIxLCJqdGkiOiI4NzhiZTBlMi03MjY1LTQxMjctYWM0MC0zNmEyZDdlODIwMWIiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvYXBwc2RldmVsb3BlcmJsb2ciLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMTg1MmU3MDAtNTc3Ni00NzhkLTllZTEtZGFlNzFiMmY4OGM0IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoicGhvdG8tYXBwLWNvZGUtZmxvdy1jbGllbnQiLCJzZXNzaW9uX3N0YXRlIjoiYzNiNjEyMGQtOWViMS00NzdkLWIyZjctN2YyZjcyNjA2YzFhIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJvZmZsaW5lX2FjY2VzcyIsInVtYV9hdXRob3JpemF0aW9uIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwibmFtZSI6IkFydW4gU2luZ2giLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhcnVuLnNpbmdoIiwiZ2l2ZW5fbmFtZSI6IkFydW4iLCJmYW1pbHlfbmFtZSI6IlNpbmdoIiwiZW1haWwiOiJhcnVudGNzMjAwNUBnbWFpbC5jb20ifQ.jbB2UJnciyKXMqpK6jWH020abreW_jlgk-G-3VOtsH4fJgTthCQZjIq2tiBhaiOvDzkRoJ1dGo_dYlQ2ld_EisbHa4VLGcv0AXco69s7UEOMMgiimsaTceH7sXrNq7dunXtu_x1yoLSs2iRjZnfkK3jpUk3LA9WN-5OZlo26LlJbGuEFyIDsOswke32MB5Kya_n7LJ1kHF4zAzNVJug4xLYwwGwceZ-mEnXyO2_IX9pG2AaWwB0SnJ4vUduDDlKB_BUNU2392zAjHDpWZrSo9JZta_QS84_VdfCvXzhvpng_UagF_zGKKjyJY094TTo00FmjDkXXNr1scxMeFBgWRg",
            "issuedAt": "2020-10-13T05:22:26Z",
            "expiresAt": "2020-10-13T05:27:26Z",
            "headers": {
                "kid": "nNUNUwiqW_ls15lAqQ2e9xzAmEv1Pu1uqY2tugwoFpo",
                "typ": "JWT",
                "alg": "RS256"
            },
            "claims": {
                "sub": "1852e700-5776-478d-9ee1-dae71b2f88c4",
                "resource_access": {
                    "account": {
                        "roles": [
                            "manage-account",
                            "manage-account-links",
                            "view-profile"
                        ]
                    }
                },
                "email_verified": false,
                "iss": "http://localhost:8080/auth/realms/appsdeveloperblog",
                "typ": "Bearer",
                "preferred_username": "arun.singh",
                "given_name": "Arun",
                "aud": [
                    "account"
                ],
                "acr": "1",
                "realm_access": {
                    "roles": [
                        "offline_access",
                        "uma_authorization"
                    ]
                },
                "azp": "photo-app-code-flow-client",
                "auth_time": 1602566521,
                "scope": "openid profile email",
                "name": "Arun Singh",
                "exp": "2020-10-13T05:27:26Z",
                "session_state": "c3b6120d-9eb1-477d-b2f7-7f2f72606c1a",
                "iat": "2020-10-13T05:22:26Z",
                "family_name": "Singh",
                "jti": "878be0e2-7265-4127-ac40-36a2d7e8201b",
                "email": "aruntcs2005@gmail.com"
            },
            "subject": "1852e700-5776-478d-9ee1-dae71b2f88c4",
            "id": "878be0e2-7265-4127-ac40-36a2d7e8201b",
            "notBefore": null,
            "issuer": "http://localhost:8080/auth/realms/appsdeveloperblog",
            "audience": [
                "account"
            ]
        }
    }