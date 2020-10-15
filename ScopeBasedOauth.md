# Scope based Access Token.

## Changes to WebSecurityConfigurerAdaptor

we have to override configure method of WebSecurityConfigurerAdapter and implement Oauth with jwt,
we have configured /users/status/check to authorize when the scope is
profile in the access token, please see the code below.

    package com.arun.oauthresourceserver.security;
    
    import org.springframework.http.HttpMethod;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    
    /**
     * @author arun on 10/14/20
     * <p>
     * Override the configure method of WebSecurityConfigurerAdapter and implement Oauth with jwt
     * for the below method , we have configured /users/status/check to authorize when the scope is
     * profile
     *
     */
    
    @EnableWebSecurity
    public class WebSecurity extends WebSecurityConfigurerAdapter {
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .antMatchers(HttpMethod.GET, "/users/status/check")
                    .hasAuthority("SCOPE_profile")
                    .anyRequest().authenticated()
                    .and()
                    .oauth2ResourceServer()
                    .jwt();
        }
    }
 

Default keyCloak will be having the Scope profile, we have to remove the profile from default.

![Profile as default Scope](https://github.com/arun786/oauthresourceserver/blob/main/src/main/resources/beforeProfileasdefault.png)
![Profile without default Scope](https://github.com/arun786/oauthresourceserver/blob/main/src/main/resources/afterProfileNotDefault.png)


Step 1 : use the below to get the code

http://localhost:8080/auth/realms/appsdeveloperblog/protocol/openid-connect/auth?client_id=photo-app-code-flow-client&response_type=code&scope=openid&redirect_uri=http://localhost:8083/callback&state=abcde

Response will be 

http://localhost:8083/callback?state=abcde&session_state=30c72e58-31c0-4061-bd2e-316a0c0563ed&code=ec939449-0c3a-48e5-8c71-00d55c5826e7.30c72e58-31c0-4061-bd2e-316a0c0563ed.8bb312a1-336f-4ff8-be8a-8acbfe5bd014

Step 2:

Get the code from the response and to get the access token from the below Post method
![]()

Step 3:

When we call the resource server to get the resource, we get 403 error as below.
![]()