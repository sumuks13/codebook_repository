---
{"publish":true,"title":"AuthN & AuthZ","cssclasses":""}
---

## Definitions

**OAuth2**: Authorization framework enabling third-party applications accessing user resources. Separates authentication from authorization via tokens.

**JWT (JSON Web Token)**: Compact token format containing user claims. Self-contained; server doesn't need session storage. Format: header.payload.signature.

**Bearer Token**: Authorization header value format. Syntax: `Authorization: Bearer <token>`. Used for token-based authentication (JWT, OAuth2).

**Scope**: Permission level in OAuth2. Examples: read, write, admin. Limits what client application can access on behalf of user.

**Refresh Token**: Long-lived token generating new access tokens. Access token short-lived; refresh token enables continuous access without re-login.

**OIDC (OpenID Connect)**: Layer on OAuth2 adding identity verification. Returns ID token with user information plus access token.

---

## Q&A

**How does OAuth2 flow work?**

User redirects to authorization server, logs in, approves application permissions. Authorization server redirects back with authorization code. Application exchanges code for access token via back-channel. Application uses token accessing protected resources. Token contains no session; server validates signature.

**What are the differences between OAuth2 and JWT?**

OAuth2 is framework/protocol for authorization delegation. JWT is token format. OAuth2 can use JWT tokens but can also use other token types. OAuth2 defines authorization flow; JWT defines token structure and verification.

**When should you use JWT instead of sessions?**

JWT tokens stateless; scalable across multiple servers without shared session storage. Useful for microservices, APIs, mobile apps. Trade-off: tokens can't be revoked immediately (expiration delay). Sessions better for security-sensitive apps; JWT better for scalability.

**How do you implement JWT authentication in Spring Boot?**

Create JwtTokenProvider generating/validating tokens. Implement JwtTokenFilter intercepting requests, extracting token, validating, setting authentication in SecurityContext. Configure SecurityFilterChain adding filter before authentication. On login, generate token, return to client.

**What are the advantages and disadvantages of token-based auth?**

Advantages: stateless (scalable), works across domains, suitable for mobile/SPAs, enables API authorization. Disadvantages: token revocation delayed (until expiration), larger request size, client must store securely, susceptible to XSS if stored in localStorage.

**How do you refresh access tokens in OAuth2/JWT?**

Client receives refresh token on login. When access token expires, client sends refresh token to authorization server for new access token. Authorization server validates refresh token, returns new access token. Keep refresh token expiration longer than access token for flexibility.

---

## Code Examples

**Example - JWT Token Provider:**
```java
@Component
public class JwtTokenProvider {
    @Value("${app.jwtSecret:mySecretKey}")
    private String jwtSecret;
    
    @Value("${app.jwtExpiration:86400000}")
    private long jwtExpiration;
    
    public String generateToken(Authentication auth) {
        UserPrincipal userPrincipal = (UserPrincipal) auth.getPrincipal();
        
        return Jwts.builder()
            .setSubject(userPrincipal.getUsername())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + jwtExpiration))
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }
    
    public String getUsernameFromToken(String token) {
        return Jwts.parser()
            .setSigningKey(jwtSecret)
            .parseClaimsJws(token)
            .getBody()
            .getSubject();
    }
}
```

**Example - JWT Token Filter:**
```java
@Component
public class JwtTokenFilter extends OncePerRequestFilter {
    @Autowired
    private JwtTokenProvider tokenProvider;
    
    @Autowired
    private CustomUserDetailsService userDetailsService;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                   HttpServletResponse response, 
                                   FilterChain filterChain) 
            throws ServletException, IOException {
        try {
            String jwt = getJwtFromRequest(request);
            if (jwt != null && tokenProvider.validateToken(jwt)) {
                String username = tokenProvider.getUsernameFromToken(jwt);
                UserDetails userDetails = userDetailsService
                    .loadUserByUsername(username);
                UsernamePasswordAuthenticationToken auth = 
                    new UsernamePasswordAuthenticationToken(userDetails, null, 
                        userDetails.getAuthorities());
                SecurityContextHolder.getContext().setAuthentication(auth);
            }
        } catch (Exception ex) {
            logger.error("Could not set user authentication", ex);
        }
        filterChain.doFilter(request, response);
    }
    
    private String getJwtFromRequest(HttpServletRequest request) {
        String header = request.getHeader("Authorization");
        if (header != null && header.startsWith("Bearer ")) {
            return header.substring(7);
        }
        return null;
    }
}
```

---

## Key Points

- **Token Storage**: Never store JWT in localStorage for security; use httpOnly cookies or memory (requires re-login on page refresh).
- **Token Expiration**: Short expiration (15 min) for access tokens, longer for refresh (7 days). Balance security vs. user experience.
- **Signature Validation**: Always validate token signature; prevents token tampering. Use strong secret keys.
