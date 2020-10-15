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
 

By default keyCloak will be having the Scope profile, we have to remove the profile from default.

![Profile as default Scope]()
![Profile without default Scope]()
