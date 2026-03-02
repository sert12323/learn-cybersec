# Web开发-JavaEE应用&SpringBoot栈&身份验证&JWT令牌&Security鉴权&安全绕过

```
#开发框架-SpringBoot
参考：https://springdoc.cn/spring-boot/

#身份验证的常见技术：
1、JWT
2、Shiro
3、Spring Security
4、OAuth 2.0
5、SSO
6、JAAS等

#身份验证-JWT技术
JWT(JSON Web Token)是由服务端用加密算法对信息签名来保证其完整性和不可伪造；Token里可以包含所有必要信息，这样服务端就无需保存任何关于用户或会话的信息；
JWT用于身份认证、会话维持等。由三部分组成，header、payload与signature。
1、引入依赖
<dependency>
	<groupId>com.auth0</groupId>
	<artifactId>java-jwt</artifactId>
	<version>3.4.0</version>
</dependency>
2、创建JWT
JWT.create()
3、配置JWT
JWT.create()
//header
.withHeader(map)
//payload
.withClaim("userid",id)
.withClaim("username",user)
.withClaim("password",pass)
//signature
.sign(Algorithm.HMAC256("xiaodisec"));
4、解析JWT
//构建解密注册
JWTVerifier jwt = JWT.require(Algorithm.HMAC256("xiaodisec")).build();
//解密注册数据
DecodedJWT verify = jwt.verify(jwtdata);
//提取解密数据
Integer userid = verify.getClaim("userid").asInt();
5、登录校验
总结：在未知的算法密钥下，即使修改JWT值里的内容去伪造用户，也无法达到认证成功
6、安全问题
参考：https://mp.weixin.qq.com/s/xH_v825bNqDszwmMOe8CBw


#身份验证-Spring Security
Spring Security安全框架，是Spring Boot底层安全模块默认的技术选型，可以实现强大的Web安全控制。
WebSecurityConfigurerAdapter：自定义Security策略
AuthenticationManagerBuilder：自定义认证策略
@EnableWebSecurity：开启WebSecurity模式
"认证"和"授权"(访问控制) 
"认证"(Authentication)
"授权"(Authorization)
这个概念是通用的，而不是只在 Spring Security 中存在。
参考官网：https://spring.io/projects/spring-security

1、新建Spring Security+web+thymeleaf项目
2、配置application.properties模版解析
3、添加前端页面文件到templates目录
4、创建路由控制器并指向前端页面文件
@Controller
public class RouterController {
    @RequestMapping("/index")
    public String index() {
        return "index";
    }

    @RequestMapping("/toLogin")
    public String toLogin() {
        return "views/login";
    }

    @RequestMapping("/level1/{id}")
    public String level1(@PathVariable("id") int id) {
        return "views/level1/"+id;
    }

    @RequestMapping("/level2/{id}")
    public String level2(@PathVariable("id") int id) {
        return "views/level2/"+id;
    }

    @RequestMapping("/level3/{id}")
    public String level3(@PathVariable("id") int id) {
        return "views/level3/"+id;
    }

}

5、创建Security授权文件并开启访问策略
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.authorizeHttpRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");
        http.formLogin();
    }

6、添加认证用户密码并进行密码加密操作
	@Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("admin").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                .and()
                .withUser("xiaodi").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1https://mp.weixin.qq.com/s/5tj6O4TA04QWyWnsd-EmEA
er("xiaodisec").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2")
                .and()
                .withUser("gay").password(new BCryptPasswordEncoder().encode("123456")).roles("vip3");

    }

7、安全问题
参考：
https://mp.weixin.qq.com/s/5tj6O4TA04QWyWnsd-EmEA
https://mp.weixin.qq.com/s/M1FiPKJRAWgwaKCtyNW8eQ
除去本身的代码不安全写法外，还有版本漏洞导致的安全问题
演示：antMatchers 配置认证绕过

```

