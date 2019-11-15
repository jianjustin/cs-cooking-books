# 3 - 基于安全认证JWT的RestAPI应用
**安全**，是任何一个服务不能避免的问题；而在本章节中，我们将开发一个基于SpringBoot的权限管理应用程序，该应用程序使用JWT身份验证来保护公开的REST API。



### 创建简单SpringBoot应用（配置SpringBoot依赖）

```java
@SpringBootApplication
@RestController
public class SecurityJWTApp {
    @RequestMapping("/")
    public String home(){
        return " a Rest API based SpringBoot and Spring Security OAuth2";
    }

    public static void main(String[] args) {
        SpringApplication.run(SecurityJWTApp.class,args);
    }
}
```

### 配置H2数据库及数据库实体，创建基本RestAPI应用

* user

| 字段名               | 字段类型      | 字段说明 |
| -------------------- | ------------- | -------- |
| PK_ID                | BIGINT        | 主键ID   |
| MEMBER_USER_NAME     | VARCHAR2(100) | 用户名称 |
| MEMBER_USER_PASSWORD | VARCHAR2(200) | 用户密码 |

* role

| 字段名           | 字段类型      | 字段说明 |
| ---------------- | ------------- | -------- |
| PK_ID            | BIGINT        | 主键ID   |
| MEMBER_ROLE_NAME | VARCHAR2(100) | 角色名称 |

* authority

| 字段名                 | 字段类型      | 字段说明 |
| ---------------------- | ------------- | -------- |
| PK_ID                  | BIGINT        | 主键ID   |
| MEMBER_RESOURCE_ROUTER | VARCHAR2(100) | 资源路由 |

* user_role

| 字段名         | 字段类型 | 字段说明 |
| -------------- | -------- | -------- |
| PK_ID          | BIGINT   | 主键ID   |
| MEMBER_USER_ID | BIGINT   | 用户编号 |
| MEMBER_ROLE_ID | BIGINT   | 角色编号 |

* role_authority

| 字段名              | 字段类型 | 字段说明     |
| ------------------- | -------- | ------------ |
| PK_ID               | BIGINT   | 主键ID       |
| MEMBER_ROLE_ID      | BIGINT   | 角色编号     |
| MEMBER_AUTHORITY_ID | BIGINT   | 权限资源编号 |

### 添加Spring Security依赖和JWT依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-api</artifactId>
  <version>0.10.6</version>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-impl</artifactId>
  <version>0.10.6</version>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-jackson</artifactId>
  <version>0.10.6</version>
</dependency>
```
>Spring Security会自动生成密码，并将链接重定向至*/login*

### 配置JWT

* ```java
  @Component
  public class TokenProvider implements InitializingBean {
      private final Logger log = LoggerFactory.getLogger(TokenProvider.class);
  
      private static final String AUTHORITIES_KEY = "auth";
      private final String base64Secret;
      private final long tokenValidityInMilliseconds;
      private final long tokenValidityInMillisecondsForRememberMe;
  
      private Key key;
  
      public TokenProvider(
              @Value("${jwt.base64-secret}") String base64Secret,
              @Value("${jwt.token-validity-in-seconds}") long tokenValidityInSeconds,
              @Value("${jwt.token-validity-in-seconds-for-remember-me}") long tokenValidityInSecondsForRememberMe) {
          this.base64Secret = base64Secret;
          this.tokenValidityInMilliseconds = tokenValidityInSeconds * 1000;
          this.tokenValidityInMillisecondsForRememberMe = tokenValidityInSecondsForRememberMe * 1000;
      }
  
      @Override
      public void afterPropertiesSet() {
          byte[] keyBytes = Decoders.BASE64.decode(base64Secret);
          this.key = Keys.hmacShaKeyFor(keyBytes);
      }
  
      public String createToken(Authentication authentication) {
          String authorities = authentication.getAuthorities().stream()
                  .map(GrantedAuthority::getAuthority)
                  .collect(Collectors.joining(","));
  
          long now = (new Date()).getTime();
          Date validity = new Date(now + this.tokenValidityInMilliseconds);
  
  
          return Jwts.builder()
                  .setSubject(authentication.getName())
                  .claim(AUTHORITIES_KEY, authorities)
                  .signWith(key, SignatureAlgorithm.HS512)
                  .setExpiration(validity)
                  .compact();
      }
  
      public Authentication getAuthentication(String token) {
          Claims claims = Jwts.parser()
                  .setSigningKey(key)
                  .parseClaimsJws(token)
                  .getBody();
  
          Collection<? extends GrantedAuthority> authorities =
                  Arrays.stream(claims.get(AUTHORITIES_KEY).toString().split(","))
                          .map(SimpleGrantedAuthority::new)
                          .collect(Collectors.toList());
  
          User principal = new User(claims.getSubject(), "", authorities);
  
          return new UsernamePasswordAuthenticationToken(principal, token, authorities);
      }
  
      public boolean validateToken(String authToken) {
          try {
              Jwts.parser().setSigningKey(key).parseClaimsJws(authToken);
              return true;
          } catch (io.jsonwebtoken.security.SecurityException | MalformedJwtException e) {
              log.info("Invalid JWT signature.");
              log.trace("Invalid JWT signature trace: {}", e);
          } catch (ExpiredJwtException e) {
              log.info("Expired JWT token.");
              log.trace("Expired JWT token trace: {}", e);
          } catch (UnsupportedJwtException e) {
              log.info("Unsupported JWT token.");
              log.trace("Unsupported JWT token trace: {}", e);
          } catch (IllegalArgumentException e) {
              log.info("JWT token compact of handler are invalid.");
              log.trace("JWT token compact of handler are invalid trace: {}", e);
          }
          return false;
      }
  }
  ```

  ```java
  public class JWTFilter extends GenericFilterBean{
      private static final Logger LOG = LoggerFactory.getLogger(JWTFilter.class);
  
      public static final String AUTHORIZATION_HEADER = "Authorization";
  
      private TokenProvider tokenProvider;
  
      public JWTFilter(TokenProvider tokenProvider) {
          this.tokenProvider = tokenProvider;
      }
  
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain)
              throws IOException, ServletException {
          HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
          String jwt = resolveToken(httpServletRequest);
          String requestURI = httpServletRequest.getRequestURI();
  
          if (StringUtils.hasText(jwt) && tokenProvider.validateToken(jwt)) {
              Authentication authentication = tokenProvider.getAuthentication(jwt);
              SecurityContextHolder.getContext().setAuthentication(authentication);
              LOG.debug("set Authentication to security context for '{}', uri: {}", authentication.getName(), requestURI);
          } else {
              LOG.debug("no valid JWT token found, uri: {}", requestURI);
          }
  
          filterChain.doFilter(servletRequest, servletResponse);
      }
  
      private String resolveToken(HttpServletRequest request) {
          String bearerToken = request.getHeader(AUTHORIZATION_HEADER);
          if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
              return bearerToken.substring(7);
          }
          return null;
      }
  }
  ```

  ```java
  public class JWTConfigurer extends SecurityConfigurerAdapter<DefaultSecurityFilterChain, HttpSecurity> {
      private TokenProvider tokenProvider;
  
      public JWTConfigurer(TokenProvider tokenProvider) {
          this.tokenProvider = tokenProvider;
      }
  
      @Override
      public void configure(HttpSecurity http) {
          JWTFilter customFilter = new JWTFilter(tokenProvider);
          http.addFilterBefore(customFilter, UsernamePasswordAuthenticationFilter.class);
      }
  }
  ```

### 配置自定义`UserDetailsService`

```java
@Component("userDetailsService")
public class UserModelDetailsService implements UserDetailsService {
    @Autowired
    public BaseRepository baseRepository;
    @Autowired
    public UserRepository userRepository;
    @Autowired
    public UserRoleRepository userRoleRepository;
    @Autowired
    public RoleAuthorityRepository roleAuthorityRepository;
    @Autowired
    public AuthorityRepository authorityRepository;

    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        User user = userRepository.findByMemberUserName(s);
        if (null == user || user.id == null)
            throw new UsernameNotFoundException("用户账户【"+s+"】查询失败");
        List<CustomGrantedAuthority> list = getGrantedAuthorities(user.id);

        return new org.springframework.security.core.userdetails.User(user.memberUserName,user.memberPassword,list);
    }

    public List<CustomGrantedAuthority> getGrantedAuthorities(long id){
        List<UserRole> userRoles = userRoleRepository.findByMemberUserId(id);
        List<RoleAuthority> roleAuthorities = new ArrayList<RoleAuthority>();
        for (int i = 0; i < userRoles.size(); i++)
            roleAuthorities.addAll(roleAuthorityRepository.findByMemberRoleId(userRoles.get(i).memberRoleId));
        List<Optional<Authority>> authorities = new ArrayList<>();
        for (int i = 0; i < roleAuthorities.size(); i++)
            authorities.add(authorityRepository.findById(roleAuthorities.get(i).memberAuthorityId));
        return authorities.stream()
                .map(authority -> new CustomGrantedAuthority(authority.get().memberResourceRouter))
                .collect(Collectors.toList());
    }

    class CustomGrantedAuthority implements GrantedAuthority {
        private String authority;
        public CustomGrantedAuthority(){}
        public CustomGrantedAuthority(String authority){
            this.authority = authority;
        }

        @Override
        public String getAuthority() {
            return authority;
        }
    }
}
```

### 配置入口Controller

```java
@RestController
@RequestMapping("/api")
public class AuthenticationController {
    @Autowired
    public TokenProvider tokenProvider;
    @Autowired
    public AuthenticationManagerBuilder authenticationManagerBuilder;

    @PostMapping("/auth/login")
    public ResponseEntity<String> authorize(@Valid @RequestBody User user) {
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.memberUserName, user.memberPassword);
        Authentication authentication = authenticationManagerBuilder.getObject().authenticate(authenticationToken);
        SecurityContextHolder.getContext().setAuthentication(authentication);

        String jwt = tokenProvider.createToken(authentication);

        HttpHeaders httpHeaders = new HttpHeaders();
        httpHeaders.add(JWTFilter.AUTHORIZATION_HEADER, "Bearer " + jwt);

        return new ResponseEntity<>(jwt, httpHeaders, HttpStatus.OK);
    }
    
}
```

### 配置`WebSecurityConfig`

```java
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    public TokenProvider tokenProvider;
    @Autowired
    public CorsFilter corsFilter;

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    public void configure(WebSecurity web) {
        web.ignoring()
                .antMatchers(HttpMethod.OPTIONS, "/**")
                .antMatchers("/","/*.html","/favicon.ico", "/**/*.html", "/**/*.css", "/**/*.js", "/h2-console/**");
    }

    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception {
        httpSecurity
                // we don't need CSRF because our token is invulnerable
                .csrf().disable()
                .addFilterBefore(corsFilter, UsernamePasswordAuthenticationFilter.class)
                .exceptionHandling()
                .and()
                .headers()
                .frameOptions()
                .sameOrigin()
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                .antMatchers("/api/auth/login").permitAll()
                .anyRequest().authenticated()
                .and()
                .apply(securityConfigurerAdapter());
    }

    private JWTConfigurer securityConfigurerAdapter() {
        return new JWTConfigurer(tokenProvider);
    }
}
```