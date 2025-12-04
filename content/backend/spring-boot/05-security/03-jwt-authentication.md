---
{"publish":true,"title":"JWT Authentication","cssclasses":""}
---

## Definitions

**JWT (JSON Web Token)**: Self-contained token format encoding user claims. Structure: header.payload.signature (Base64URL encoded). Stateless; server validates signature without session storage.

**Header**: JWT section containing token type and signing algorithm. Example: `{"alg":"HS256","typ":"JWT"}`.

**Payload (Claims)**: Section containing user data and metadata. Standard claims: `sub` (subject), `exp` (expiration), `iat` (issued at). Custom claims application-specific.

**Signature**: Cryptographic signature verifying token integrity. Created by signing encoded header and payload with secret key. Prevents tampering.

**Secret Key**: Symmetric key signing and verifying JWT. Must be kept secure; compromise allows token forgery.

**Token Expiration**: Time limit for token validity. Short-lived access tokens (15 min) require frequent renewal; refresh tokens longer-lived (7 days).

**Refresh Token**: Long-lived token obtaining new access tokens. Stored securely; enables continuous authentication without re-login.

---

## Q&A

**How does JWT authentication flow work?**

User logs in with credentials; server validates, generates JWT containing user info, returns token. Client stores token (memory, cookie), includes in Authorization header for subsequent requests. Server validates token signature, extracts claims, grants access. No server-side session needed; scalable across multiple servers.

**What claims should you include in JWT payload?**

Include minimal necessary claims: user ID (`sub`), expiration (`exp`), issued at (`iat`), issuer (`iss`). Avoid sensitive data (password, SSN); JWT readable by anyone with token. Custom claims for roles, permissions. Keep payload small; sent with every request.

**How do you secure JWT tokens?**

Use strong secret keys (256-bit minimum), short expiration times, HTTPS only, validate signature on every request. Store tokens securely (httpOnly cookies preferred over localStorage). Implement token blacklist for revocation. Rotate secret keys periodically.

**What's the difference between access tokens and refresh tokens?**

Access tokens short-lived (15 min), included in every API request. Refresh tokens long-lived (7 days), used only to obtain new access tokens. If access token compromised, limited damage window. Refresh token compromise requires immediate invalidation. Separate expiration times balance security and user experience.

**How do you handle token expiration on client side?**

Check token expiration before requests; if expired, use refresh token obtaining new access token. Implement interceptor catching 401 errors, attempting refresh automatically. If refresh fails, redirect to login. Store expiration time with token for proactive checking.

**Can you revoke JWT tokens before expiration?**

JWT stateless; immediate revocation requires server-side tracking (defeats stateless benefit). Solutions: maintain token blacklist (check on each request), use short expiration forcing frequent renewal, implement token versioning (increment user's token version on logout, reject old versions).

---

## Code Examples

**Example - JWT Generation:**
```java
@Component
public class JwtTokenProvider {
    @Value("${jwt.secret}")
    private String jwtSecret;
    
    @Value("${jwt.expiration:900000}") // 15 minutes
    private long jwtExpiration;
    
    public String generateToken(UserPrincipal userPrincipal) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpiration);
        
        Map<String, Object> claims = new HashMap<>();
        claims.put("userId", userPrincipal.getId());
        claims.put("roles", userPrincipal.getAuthorities().stream()
            .map(GrantedAuthority::getAuthority)
            .collect(Collectors.toList()));
        
        return Jwts.builder()
            .setClaims(claims)
            .setSubject(userPrincipal.getUsername())
            .setIssuedAt(now)
            .setExpiration(expiryDate)
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }
    
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
            return true;
        } catch (SignatureException | MalformedJwtException | 
                 ExpiredJwtException | UnsupportedJwtException | 
                 IllegalArgumentException ex) {
            return false;
        }
    }
    
    public String getUsernameFromToken(String token) {
        Claims claims = Jwts.parser()
            .setSigningKey(jwtSecret)
            .parseClaimsJws(token)
            .getBody();
        return claims.getSubject();
    }
}
```

**Example - Login Endpoint:**
```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    @Autowired
    private AuthenticationManager authenticationManager;
    
    @Autowired
    private JwtTokenProvider tokenProvider;
    
    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest request) {
        Authentication authentication = authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(
                request.getUsername(), 
                request.getPassword()
            )
        );
        
        SecurityContextHolder.getContext().setAuthentication(authentication);
        String jwt = tokenProvider.generateToken(
            (UserPrincipal) authentication.getPrincipal()
        );
        
        return ResponseEntity.ok(new JwtAuthResponse(jwt));
    }
}
```

**Example - JWT Filter:**
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    @Autowired
    private JwtTokenProvider tokenProvider;
    
    @Autowired
    private CustomUserDetailsService userDetailsService;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                   HttpServletResponse response, 
                                   FilterChain filterChain) 
            throws ServletException, IOException {
        String jwt = extractJwtFromRequest(request);
        
        if (jwt != null && tokenProvider.validateToken(jwt)) {
            String username = tokenProvider.getUsernameFromToken(jwt);
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            
            UsernamePasswordAuthenticationToken authentication = 
                new UsernamePasswordAuthenticationToken(
                    userDetails, null, userDetails.getAuthorities()
                );
            authentication.setDetails(
                new WebAuthenticationDetailsSource().buildDetails(request)
            );
            
            SecurityContextHolder.getContext().setAuthentication(authentication);
        }
        
        filterChain.doFilter(request, response);
    }
    
    private String extractJwtFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```

---

## Key Points

- **Secret Key Security**: Store in environment variables, not code. Use strong random keys (256-bit). Rotate periodically.
- **HTTPS Required**: Always use HTTPS; JWT in HTTP exposes tokens to interception.
- **Token Storage**: Use httpOnly cookies (prevents XSS) or memory (requires re-login on page refresh). Avoid localStorage (vulnerable to XSS).
- **Claims Validation**: Always validate expiration, issuer, audience. Don't trust client-provided tokens without verification.
- **Token Size**: Keep payload small; sent with every request. Large tokens increase bandwidth and latency.
