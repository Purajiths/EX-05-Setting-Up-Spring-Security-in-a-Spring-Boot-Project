# EXP05-Setting-Up-Spring-Security-in-a-Spring-Boot-Project
## AIM:
To write a program for setting up Spring Security in a Spring Boot project to secure endpoints with basic authentication and role-based access control.

## ALGORITHM:
Create a Spring Boot Project with the following dependencies:

Spring Web

Spring Security

Spring Boot DevTools (optional)

Add Spring Security dependency in pom.xml (if not using Spring Initializr).

Create a configuration class extending WebSecurityConfigurerAdapter (or using SecurityFilterChain for newer Spring versions).

Define an in-memory user with username, password, and roles using UserDetailsService.

Secure your REST endpoints using annotations or in the security config class.

Run and test the app using a browser or Postman:

Secure endpoints will prompt for username and password.

## PROGRAM CODE:
###pom.xml (Dependencies)
```
<dependencies>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-web</artifactId>
			</dependency>

			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-security</artifactId>
			</dependency>

			<dependency>
				<groupId>io.jsonwebtoken</groupId>
				<artifactId>jjwt-api</artifactId>
				<version>0.12.5</version>
			</dependency>

			<dependency>
				<groupId>io.jsonwebtoken</groupId>
				<artifactId>jjwt-impl</artifactId>
				<version>0.12.5</version>
				<scope>runtime</scope>
			</dependency>

			<dependency>
				<groupId>io.jsonwebtoken</groupId>
				<artifactId>jjwt-jackson</artifactId>
				<version>0.12.5</version>
				<scope>runtime</scope>
			</dependency>

		</dependencies>

```
### SecurityConfig.java (Spring Boot 3.x / Spring Security 6+)
```
package com.example.demo.Config;

//import com.example.demo.security.JwtFilter;
import com.example.demo.Security.JwtFilter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http
                .csrf(csrf -> csrf.disable())

                .sessionManagement(session ->
                        session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                )

                .authorizeHttpRequests(auth -> auth
                        .requestMatchers("/auth/login").permitAll()
                        .anyRequest().authenticated()
                )

                .addFilterBefore(
                        new JwtFilter(),
                        UsernamePasswordAuthenticationFilter.class
                );

        return http.build();
    }
}
```
### AuthController.java
```
package com.example.demo.Controller;

import com.example.demo.LoginRequest;
import com.example.demo.Security.JwtUtil;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/auth")
public class AuthController {

    @PostMapping("/login")
    public String login(@RequestBody LoginRequest req) {

        if ("admin".equals(req.getUsername())
                && "1234".equals(req.getPassword())) {

            return JwtUtil.generateToken(req.getUsername());
        }

        return "Invalid Credentials";
    }

    @GetMapping("/hello")
    public String hello() {
        return "Hello JWT Secure API";
    }
}
```
### LoginRequest.java
```
package com.example.demo;

public class LoginRequest {

    private String username;
    private String password;

    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }

    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
}
```
### Output

<img width="1495" height="928" alt="image" src="https://github.com/user-attachments/assets/49dc823a-fdf6-44c5-aed2-db0106367d4a" />
<img width="1640" height="910" alt="image" src="https://github.com/user-attachments/assets/b4c5b413-b1fd-4b1c-9405-75595ee0f618" />

## RESULT 

Thus,the program for setting up Spring Security in a Spring Boot project to secure endpoints with basic authentication and role-based access control implemented and executed successfully.
