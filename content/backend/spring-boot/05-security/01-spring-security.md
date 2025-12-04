---
{"publish":true,"title":"Spring Security","cssclasses":""}
---

## Definitions

**Spring Security**: Framework providing authentication and authorization. Protects resources, manages user credentials, enforces security policies.

**Authentication**: Verifying user identity. Answer "Who are you?" Involves credentials (username/password), tokens, certificates.

**Authorization**: Determining access permissions. Answer "What can you do?" Based on roles, authorities, permissions.

**Principal**: User identity; entity making request. Can be authenticated or anonymous.

**Grant Authority**: Permission granted to principal. Examples: ROLE_ADMIN, ROLE_USER, CREATE_RESOURCE.

**SecurityContext**: Holds authentication information for current thread/request. Accessible via SecurityContextHolder.

**Filter Chain**: Chain of security filters processing requests. Authenticate, check authorization, enforce security policies.

---

## Q&A

**How does Spring Security work?**

Spring Security intercepts requests via FilterChain, determines authentication status, checks authorization. If unauthenticated, redirects to login. If authenticated, checks if principal has required authorities. If authorized, allows request; if not, denies with 403. Manages session after login.

**What's the difference between authentication and authorization?**

Authentication verifies identity (who are you?), typically via username/password login, OAuth, JWT tokens. Authorization determines permissions (what can you do?), based on roles or authorities. Authentication must happen first; authorization checks against authenticated principal.

**How do you configure security in Spring Boot?**

Extend WebSecurityConfigurerAdapter (deprecated in 6.0, use SecurityFilterChain bean) to configure authentication, authorization. Define AuthenticationProvider (usually UserDetailsService), configure authorization rules via authorizeHttpRequests(). Configure login page, logout, CSRF protection.

**What are roles vs authorities in Spring Security?**

Authorities are granular permissions (READ_RESOURCE, WRITE_RESOURCE). Roles are groups of authorities (ROLE_ADMIN, ROLE_USER). Authority has prefix "ROLE_" when treated as role. Use authorities for fine-grained control; roles for simpler cases.

**How do you implement remember-me functionality?**

Spring Security provides remember-me out-of-the-box. Configure rememberMe() in SecurityConfig. User session persists across browser restarts via persistent token/cookie. Useful for convenience; trade-off with security. Configure with custom token repository for database persistence.

**How do you handle authorization for REST APIs with tokens?**

Use JWT or OAuth2 tokens. Send token in Authorization header (Bearer token). Custom filter (e.g., JwtTokenFilter) extracts token, validates, sets authentication in SecurityContext. Unlike session-based auth, stateless; each request validated independently.

---

## Code Examples

**Example - Basic Security Configuration:**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(login -> login
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
            )
            .logout(logout -> logout.logoutSuccessUrl("/"));
        
        return http.build();
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
            .username("user")
            .password("{bcrypt}$2a$10$...")
            .roles("USER")
            .build();
        
        UserDetails admin = User.builder()
            .username("admin")
            .password("{bcrypt}$2a$10$...")
            .roles("ADMIN")
            .build();
        
        return new InMemoryUserDetailsManager(user, admin);
    }
}
```

**Example - Custom UserDetailsService:**
```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) 
            throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException(username));
        
        return User.builder()
            .username(user.getUsername())
            .password(user.getPassword())
            .authorities(user.getAuthorities())
            .accountNonLocked(true)
            .build();
    }
}
```

---

## Key Points

- **Password Encoding**: Always encode passwords with BCrypt, PBKDF2, or Argon2; never store plain text.
- **CSRF Protection**: Enabled by default for form-based applications; required for state-changing operations.
- **CORS**: Configure explicitly for REST APIs; browser security restriction; server must allow cross-origin requests.
- **Session Fixation**: Spring Security prevents session fixation attacks by default; regenerates session on login.
