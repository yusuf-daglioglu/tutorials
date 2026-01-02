# JAVA SPRING SECURITY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Java EE User's Session Paylaşımı

Tek bir sunucu (farklı cluster'lar olmadan) üzerinde uygulamaların (farklı war uygulamalarının) session paylaşımı için sunulanlar: <https://docs.oracle.com/cd/E28280_01/web.1111/e13712/sessions.htm#WBAPP309>

- Memory (single-server, non-replicated) (Session'ları RAM'de saklıyor)
- `File system` persistence
- `JDBC` persistence
- Cookie-based session persistence:
  
  - cookie desteklemeyen client'lar için 'URL Rewriting' yapılıyor.
  - tüm bilgiler `JWT`'teki gibi token'da saklanıyor. Sunucu tarafta token saklanmıyor.
  - Sadece `Basic Access Authentication` destekleniyor.

Farklı clusterlardaki suncuların birbirleri ile session paylaşımı için sunulanlar:

- In-memory replication (RAM'de session'ları tutup, diğer data center'daki makine ile sürekli sync kalıyor)
- JDBC-based persistence ('In-memory replication' ile aynı. tek farklı RAM yerine DB kullanması)
- Coherence*Web

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring security

Spring'in default destekledikleri:

- `Basic Access Authentication`
- `Digest Access Authentication`
- `LDAP`
- ve dahası.

Bunların içinde `JWT` yok. `JWT` için implementasyonu yazılımcının kendisi yapması gerekiyor. Zaten `JWT` bir security metodu değildir, `JWT` data transferi yöntemidir.

### 📌📌 Spring security modülleri

- `Spring-security-core.jar`
  - `org.springframework.security.core`
  - `org.springframework.security.access`
  - `org.springframework.security.authentication`
  - `org.springframework.security.provisioning`

- `Spring-security-remoting.jar`

  remote method calling için gerekli.

- `Spring-security-web.jar`

  URL based servlet control. filtreler buranın içinde.

- `Spring-security-ldap.jar`

- `Spring-security-oauth2-core.jar`

- `Spring-security-oauth2-client.jar`

  client tarafta Java uygulaması varsa bu client kullanılabilir.

- ve diğer modüller

### 📌📌 foundamental process

As we know, Servlets have feature which called Filter. Merging multiple Filters called a filter chain. Spring has it default chain which is:

> org.springframework.security.web.FilterChainProxy

People also name it as (🔴) `springSecurityFilterChain`. This class is responsible for all security things. This class is also an implementation of servlet-Filter.

When we enable spring-boot security default configuration, it adds the (🔴) `springSecurityFilterChain` to our servlet. As `spring-boot` defaults, it registers to front-controller URL's(servlet). In order to do that, `spring-boot` adds a filter to our servlet which is called (🟢) `DelegatingFilterProxy`.

(🟢) `DelegatingFilterProxy` is a Filter which invokes a Spring managed beans which is also Filter. It named (🟢) `DelegatingFilterProxy`, because it invokes another filter which is called `target Bean` or `delegate`. In this case our target `Bean` is (🔴) `springSecurityFilterChain`.

We can register our filters the (🔴) `springSecurityFilterChain` without (🟢) `DelegatingFilterProxy`. But that will not be recommended. Also in order to do that, we will need extra configurations.

### 📌📌 ayarlar

aşağıdaki sınıfta istediğimiz ayarı belirliyoruz:

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

 @Override
 protected void configure(HttpSecurity http) throws Exception {

  http
    .authorizeRequests()
      .antMatchers("/css/**", "/index").permitAll()
      .antMatchers("/user/**").hasRole("USER")
      .and()
    .formLogin().loginPage("/login").failureUrl("/login-error");
 }

 @Autowired
 public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {

  auth
   .inMemoryAuthentication()
    .withUser(User.
                withDefaultPasswordEncoder()
                .username("user")
                .password("password")
                .roles("USER")
              );
 }
}
```

Artık controller taraflarında ekstra bir ayar yapmaya gerek kalmıyor.

### 📌📌 SecurityContext

bu sınıf tüm login olmuş kullanıcıların listesini tutar. kullanıcıları Authentication objesi ile saklar.

### 📌📌 SecurityContextHolder

SecurityContext'e direk erişim tavsiye edilmez. SecurityContextHolder aracılığı ile erişebiliriz.

### 📌📌 Authentication

(🟡) `org.springframework.security.authentication.Authentication` bir arayüzdür. birçok implementasyonu var. örnek:

(🔵) `UsernamePasswordAuthenticationToken` sınıfı kullanılırsa; login olmaya çalışan kullanıcı, username ve password bilgisini göndermesi yeterlidir.

### 📌📌 authenticationManager

Sadece 1 metodu vardır:

```java
Authentication authenticate(Authentication authentication) throws AuthenticationException
```

(🟡) `Authentication` alır ve (🟡) `Authentication` objesi döndürür. Aldığı (🟡) `Authentication` objesinde, henüz granted authorities yoktur. Fakat döndürdüğünde vardır.

Temel akış:

- (🟡) `Authentication` implementasyonu (yukarıdaki örnekteki (🔵) `UsernamePasswordAuthenticationToken`) (🟣) `AuthenticationManager` sınıfına pass edilir.
- (🟣) `AuthenticationManager` kendi içinde `AuthenticationProvider` aracılığı ile kullanıcıyı login eder yada hata fırlatır.
- (🟣) `AuthenticationManager` içinde birden fazla `AuthenticationProvider` olabilir.
- Kullanıcı login olabilirse; (🟣) `AuthenticationManager` bir Authentication instance'ı döndürür. bu döndürülen instance `SecurityContextHolder.getContext().setAuthentication()` metoduna yollanır. `setAuthentication` metodu o andaki user'ın (🟡) `Authentication` instance'ını değiştirecektir.

`Spring security diagram` internette aratıldığında açıklayıcı görseller bulunabilir.

örnek olarak manuel bir kullanıcının kimliğini onaylayalım:

```java
SecurityContext context = SecurityContextHolder.createEmptyContext();
Authentication authentication = new TestingAuthenticationToken("username", "password", "ROLE_USER");
context.setAuthentication(authentication);
SecurityContextHolder.setContext(context); // use ThreadLocal for it's values.
```

Prod ortamında, `TestingAuthenticationToken` yerine (🔵) `UsernamePasswordAuthenticationToken` kullanmalıyız.

En sık kullanılan (🟣) `AuthenticationManager` implementasyonu:

> org.springframework.security.authentication.ProviderManager

## 📌 AuthenticationProvider

Bazı implementasyonları:

- ActiveDirectoryLdapAuthenticationProvider
- AnonymousAuthenticationProvider
- DaoAuthenticationProvider

  sadece UserDetailsService ve PasswordEncoder kullanarak username ve password onayı ile çalışır.

- org.springframework.security.ldap.authentication.LdapAuthenticationProvider
- OpenIDAuthenticationProvider
- org.springframework.security.authentication.TestingAuthenticationProvide
- org.springframework.security.oauth2.server.resource.authentication.JwtAuthenticationProvider

### 📌📌 UserDetailsService

arayüzdür. birçok implementasyonu var. örnek: InMemoryUserDetailsManager. UserDetailsService sadece şu metodu içerir:

```java
public org.springframework.security.userdetails.UserDetails loadUserByUsername(String username) throws UsernameNotFoundException
```

Spring arka planda loadUserByUsername metodunu çağırır. Kullanıcının gönderdiği credentials bilgileri ile UserDetails'i karşılaştırır. Eğer bilgiler uyuşuyorsa kullanıcıyı login eder.

### 📌📌 Subject vs Principal vs User vs realm vs credentials

Bilişim güvenliği dünyasında;

- subject; bir objeye erişmek için izin isteyen yazılım parçasıdır. web tarayıcısı, thread, process, birer subject'tir.

- user: sadece son kullanıcı (bir insan) hesabından bahsediliyor ise bu "user" olarak da kullanılır.

- Principal: identifiable attribute of the subject. like: username, social security number, user ID... kaynak: https://shiro.apache.org/terminology.html title: "Principal".

- Realm: A Realm is a component that can access application-specific security data such as users, roles, and permissions. kaynak: https://shiro.apache.org/terminology.html title: "Realm".

- credentials: A credential contains information used to authenticate the subject to services.

### 📌📌 SecurityContext metotları

- getAuthentication
- setAuthentication

### 📌📌 Authentication metotları

- getCredentials: o andaki isteği yapan user'ın credential'i döndürür. credential tam olarak şu demek: kullanıcının login olabilmesi için kullandığı token/şifre veya bunların kombinasyonlarının tutulduğu objedir. Bu metot kullanıcı kimliği onaylandıktan sonra boş dönüş yapabiliyor. bu tamamen güvenliği arttırmak için yapılan tercihi bir tekniktir.

- getDetails: o andaki isteği yapan hakkında ekstra bilgiler içerir: örnek: IP.

- getPrincipal: o andaki isteği yapanın hesabı ile ilgili bilgileri içerir. örnek: username, password, roles, email. dönüş değeri object'tir. bazı case'lerde __org.springframework.security.core.userdetails.UserDetails__ arayüzinin instance'ını döndürebilir.

- isAuthenticated: isteği yapan kullanıcının token'ının onaylanmış olup olmadığı (login olup olmadığı)

- setAuthenticated

- getAuthorities: GrantedAuthority'den implemente etmiş obje listesi döner. Her GrantedAuthority bir işlemin yetkisini ifade eder. örnek: READ_AUTHORITY, WRITE_PRIVILEGE.

### 📌📌 Herhangi bir kod satırında kullanıcı bilgileri çekme

herhangi bir satırda;

```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();

auth.getPrincipal();
```

metotları çağrılabilir. getPrincipal'ı çağırmak yerine isteğe bağlı controller request'inde şu parametre eklenebilir:

```java
@RequestMapping("/messages/inbox")

public ModelAndView findMessages(@AuthenticationPrincipal CustomUser customUser) {

     //some code here
}
```

## 📌 org.springframework.security.web.AuthenticationEntryPoint

It is not Security-core's feature. It is security-web feature.

If a client make an unauthenticated request to a resource (which the client is not authorized) AuthenticationEntryPoint step is invoking.

some implementations:

- BasicAuthenticationEntryPoint

  it only sends HTTP 401 response to client.

- DigestAuthenticationEntryPoint
- Http403ForbiddenEntryPoint

  it only sends HTTP 403 response to client.

- LoginUrlAuthenticationEntryPoint

  redirects to Login page (which is spesified as parameter)

AuthenticationEntryPoint implementations should override this method:

```java
commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException)
```

## 📌 AbstractAuthenticationProcessingFilter

Creates Authentication instance from request, and pass it to authenticationManager.

implementations:

- OAuth2LoginAuthenticationFilter
- OpenIDAuthenticationFilter
- Saml2WebSsoAuthenticationFilter
- UsernamePasswordAuthenticationFilter

### 📌📌 role vs GrantedAuthority

Her role bir GrantedAuthority kümesidir. Yazılımcı; role listesi verildiğinde GrantedAuthority listesi dönen bir metot sunmalıdır. Bunu da UserDetails.getAuthorities() metodunu implemente ederek yapmalıdır.

## 📌 Entegrated methods with Servlet

Servlet method calls Spring security's method. Those are explained on this link:

https://docs.spring.io/spring-security/site/docs/5.3.2.RELEASE/reference/html5/#servletapi-25 title: "15.1. Servlet API integration".

### 📌📌 configure metodu içeriği

HTTP objesinin aşağıdaki gibi metotlara ayırmakta fayda var. bu şekilde daha okunaklı oluyor.

```java
@Override
protected void configure(HttpSecurity http) throws Exception {

         // hepsini bir arada yazmamak için metotlara bölüp
         // gruplandırmış olduk
         csrf(http);
         login(http);
         logout(http);
         exceptionHandling(http);
         authorizeRequests(http);
}

protected void csrf(HttpSecurity http) throws Exception {

    http
    .csrf().enable();
    /* banka sitemiz olsun. Kullanıcı login oldu ve logout olmadan başka siteye gitti (X sitesi). X kendi içinde bizim bankamıza istek yapabilir. İstek yaparken cookie'yi barındıramayacağı için (çünkü JSESSIONID yada Session-ID bankamıza ait domain spesifik cookie'ye bağlı saklanıyor) isteği geçersiz olacaktır. fakat cookie'ler her zaman %100 güven sağlayamıyor. bazen farklı sitelerce yada web tarayıcısı dışından da çekilebiliyor. örnek:

    - kötü niyetli web tarayıcı eklentileri ve bug'ları
    - web tarayıcı bug'ları
    - OS'ta kurulu native uygulamanın web tarayıcısından cookie'leri çekmesi
    - X sitesi iframe içinde bankamızı açar ve üzerinde CSS işlemleri ile bir button'una basmamızı sağlayabilir...

    gibi açıklarla cookie çalınabiliyor veya cookie bir şekilde kullanılarak adımıza request atılabiliyor.

    bu durumu engellemek için Spring'in çözümü var. bu çözümde her istek için için sunucu tarafta o anda random bir token üretiliyor. buna csrf-token deniliyor. token client tarafa yollanıyor ve cookie'de saklanmıyor. direk sayfaya gömülü oluyor. bu değeri yazılımcının client tarafta her istekte yollaması gerekiyor. burada yazılımcının da bir şey yapmasına gerek kalmıyor her form alanının içine otomatik Spring hidden-input olarak bu değeri sokuyor eğer sayfa JSP/JSF ise. fakat JS olan sayfalar için iş yazılımcının kendisine düşüyor.
    */
}

protected void login(HttpSecurity http) throws Exception {

  http
  .formLogin() // cookie based form login için gerekli ayarlar

          .loginPage("/login")
          //web sayfası olarak login formunun olduğu sayfa URL'si

          .loginProcessingUrl("/loginservice")
          // /login formu ve diğer login olmak isteyenler buraya istek yapacaklar

          .failureUrl("/loginfailed")
          //login işlemi başarısız olursa, login HTTP isteği buraya yönlendirilir (redirection)

          .successHandler(authenticationSuccessHandler)
          //success işlemleri event'lerinin tanımlamaları

          .failureHandler(authenticationFailureHandler);
          //fail işlemleri event'lerinin tanımlamaları

    // .formLogin(withDefaults());
    // Spring will generate a default login page for us.
}

protected void logout(HttpSecurity http) throws Exception {

  http
      .logout()
      //logout config ayarları

          .logoutUrl("/logout")
          // default: /logout

          .logoutSuccessUrl("/my/index")
          // default: /login?logout

          .logoutSuccessHandler(logoutSuccessHandler)
          // Eğer bu set edilmiş ise logoutSuccessUrl ignore edilir

          .addLogoutHandler(logoutHandler)

          .deleteCookies("JSESSIONID");
          // auto olarak silinmesi istenen cookie'ler
}

protected void exceptionHandling(HttpSecurity http) throws Exception {

  http
     .exceptionHandling()
          .authenticationEntryPoint(new Http403ForbiddenEntryPoint());
          //when someone try to access a URL which is not pemitted
}

protected void authorizeRequests(HttpSecurity http) throws Exception {

  http.
     authorizeRequests()

          .MVCMatchers("/login/impersonate*").hasRole(ADMIN_ROLE)
          // only ADMIN_ROLE users can access these URLs

          .MVCMatchers("/login/anotherpage*").hasAuthority(USER_ROLE_1)

          .MVCMatchers("/logout/impersonate*").authenticated()
          // any authenticated user can  access these URLs

          .MVCMatchers("/**").permitAll();
          // allow all (even anonym) users can access these URL's.
          // MVCMatchers("/**") yerine anyRequest()'de kullanılabilir.

          //not: buradaki config'lerde MVCMatchers yerine antMatchers kullanılabilir. fakat MVC request'i ise MVC tercih edilmelidir.

          //not: MVCMatchers("/**") kısmı, authorizeRequester() 'ın her zaman sonunda çağrılmalıdır. çünkü authorizeRequests() kısmı URL'leri spesifikten genele doğru bir sıra ile ayarlanmalıdır. farklı bir örnek:
          .antMatchers("/auth/admin/*").hasAuthority("ADMIN")
          .antMatchers("/auth/*").hasAnyAuthority("ADMIN", "USER")
}
```

### 📌📌 Session-based Authentication vs Token-based Authentication

Session-based'de bir random bir ID yaratılır ve login olmuş kullanıcının referansı bu olur. kullanıcı bilgileri sunucudadır. Java EE ve Spring'de bu session-ID default olarak "JsessionID" olarak adlandırılır. yani statefull'dır. burada token; session-ID'dir.

Diğer Session-based Authentication token örnekleri:

- `PHPSESSID` (`PHP`)
- `JSESSIONID` (`J2EE`)
- `CFID` and `CFTOKEN` (`ColdFusion`)
- `ASP.NET_SessionId` (`ASP .NET`)

OWASP'ın önerisi; sunucu teknolojisinin ismini ortaya çıkarmamak için bu cookie'lerin ismini "ID" gibi generic bir şey yapmamızdır.

Token-based'de ise token random bir string değildir. şifreli ve kullanıcının session bilgilerini içeren bir string'dir. dolayısı ile bilgiler sunucuda tutulmaz. yani stateless'dır. JWT, Token-based'e en iyi örnektir.

JWT içerisindeki blok şifrelenmiştir ve sadece sunucu tarafında açılıp/okunabilir. sunucu taraf kimlere JWT verdiğini bilmez. JWT bir kere üretilir ve sonra salınır. dolayısı ile JWT her geldiğinde içindeki bilgilerde olan son kullanma tarihine bakılır. son kullanma tarihi geçmişse artık bu token geçersizdir. JWT içinde son kullanma tarihi ve userId olması yeterlidir. zaten data kimse tarafından değiştirilemeyeceği için sunucu bunu güvenle kullanabilir.

JWT'deki sakınca şudur: eğer JWT token'ını hacker çalarsa, o kişi adına JWT'nin son kullanma tarihine kadar işlem yapabilir. hacker'ın çaldığını sunucu ve client bilse de bunu engelleyemezler. çünkü JWT'lerin geçerliliği sunucu tarafta tutulmamaktadır. sunucu tarafta hiçbir bilgi tutulmamaktadır. bu sebeple genelde JWT'nin son kullanma tarihi çok kısa tutulmaktadır. bu sebeple JWT'de refresh token altyapısı mevcuttur. (başka başlıkta anlatılıyor)

Token-based Authentication'ın avantajı sunucu tarafta bilginin tutulmamasıdır. sunucu tarafta birden fazla aynı işi yapan sunucuları load balancer ile bağladığımızı düşünelim. her login bilgisi güncellendiğinde tüm sunucuların senkron olması gerekecek. senkronizasyon işlemi hem sistemi yavaşlatacak hem de sunucu çöküşlerinde sorun yaratacak. cluster'ın olaylarında konfigürasyon gerekecek. JWT bizi bu sorunlardan kurtarıyor. özellikle load balancing yaptığımız sunucular fiziksel olarak uzak mesafelerde ise "Session-based Authentication" facialara sebep olabilir.

### 📌📌 RedirectStrategy

security modülü içinde olan bir arayüzdür. DefaultRedirectStrategy isminde bir implementasyonu mevcut. RedirectStrategy, filter'ların yada authenticationSuccessHandler gibi sınıfların içinde inject edilir. bu inject edilen nesne ile filter içerisinde gelen istekleri farklı bir URL'ye yönlendirmemiz mümkündür. örneğin; login olmuş ve header'ında admin olan kullanıcıyı admin sayfasına yönlendirebiliriz. admin olmayanları ise farklı bir sayfaya yönlendiririz.

DefaultRedirectStrategy, sendRedirect(request, response, URL); metodunu içerir. URL burada yönlendireceğimiz path'tir.

## 📌 forward vs redirect

HTTP isteklerinde forward; sunucuya gelen isteği farklı bir yere yönlendirir. bunu yaparken client'ın bu yönlendirmeden hiçbir haberi olmaz. oysa redirect'te, client'a 30x kodunda response dönülür. aynı response'ta client'ın yapması gereken yeni isteğin nereye olacağı bilgisi de dönülür. burada client ikinci isteği yapar. web tarayıcıları bu isteği otomatik (son kullanıcıya sormadan yapar).

forward vs redirect birçok yerde kullanılan metot isimleridir. örnek:

- response.sendRedirect("login.jsp");

- request.getRequestDispatcher("login.jsp").forward(request, response);

Dispatch Türkçe kelime anlamı: sevk etmek. yazılım dünyasında metoda/fonksiyona mesaj(parametre) yollama(sevk etme) anlamında kullanılır.

## 📌 @EnableResourceServer

bu annotation'un atıldığı service auth2.0 API'si olmaktadır (başka yerde auth2.0 API nedir anlatılıyor.)

## 📌 @EnableOAuth2Sso

bu annotation'un olduğu service auth2.0 client'i oluyor. bu annotation aşağıdaki iki annotation'u içermektedir:

- @EnableOAuth2Client : OAuth2RestTemplate ile yaptığımız isteklerde access-token'ı karşı tarafa otomatik yollamaktadır.

- @EnableConfigurationProperties(OAuth2SsoProperties.class) : eğer yetkisiz istek gerçekleşirse gelen isteği otomatik olarak /login'e (daha doğrusu Authorization Server'a) yönlendirir.

## 📌 Spring security örnek kod

```java
@Service

public class CustomAuthenticationManager implements AuthenticationManager {

 @Override
 public Authentication authenticate(Authentication authentication) throws AuthenticationException {

      //authentication nesnesi otomatik dolduruldu Spring tarafından. içinde user'ın login olmaya çalışırken yaptığı istekteki bilgiler var.

      // Aşağıda username string'i kullanıcının formdan gönderdiği "username" alanıdır.

      // password ise password alanıdır.

      String username = authentication.getPrincipal() + "";

      String password = authentication.getCredentials() + "";

      // fetch from database

      // User user = dbCOnnection.fetch(username);

      // check password

      //get roles of user

      List<GrantedAuthority> grantedAuths = new ArrayList<>();

      grantedAuths.add(new SimpleGrantedAuthority("ROLE_USER"));

      if(username.equals("ahmet")) {

          throw new AuthenticationException("Engellenmiş/bloke kullanıcı");

          //eğer throw edilmeyip null döndürülürse, Spring bir sonraki auth-provider ile işlem yapmaya çalışacaktır.
    }

     return new UsernamePasswordAuthenticationToken(username, password, grantedAuths);
     //bu constructor'ın içinde "super.setAuthenticated(true);" satırı mevcut.
 }
}
```

authenticate metodundan dönen Authentication instance'ı artık o kullanıcın principal ve credentials'ini tutuyor ve her tarafta bu objelere erişebiliyoruz. principal ve credentials birer Java obje sınıfıdır. dolayısı ile buralarda istediğimiz her şeyi tutabiliriz.

CustomAuthenticationManager'ı Spring'e kullanmasını belirtmeliyiz:

```java
@EnableWebSecurity //bu satır zaten vardı

public class SecurityConfig extends WebSecurityConfigurerAdapter { //bu satır zaten vardı

  @Autowired
  private CustomAuthenticationProvider authProvider;

  @Override
  protected void configure(AuthenticationManagerBuilder auth) throws Exception {

         auth.authenticationProvider(authProvider);
 }
}
```

## 📌 AuthenticationException

AuthenticationException fırlatılacak olan sınıftır. BUnun birçok türevi vardır. Fakat tüm exception'lar sadece string error'u döndürür. farklı bir parametre eklemek istersek ya bu string alanına bir JSON yazmamız gerekecek yada daha sonraki aşamalarda devreye girecek kod bloklarına yeni kodlar eklememiz gerekecek.

AuthenticationException'dan türemiş bazı sınıflar: AccountStatusException, AccountExpiredException, DisabledException, LockedException, BadCredentialsException, UserNameNotFoundException

## 📌 client taraftan login isteğinde username vs password haricinde değer yollama

Authenticate metodu sadece Authentication parametresi alır. "Authentication metotları" başlığına bakılırsa görüleceği credential, details ve principal birer objedir. yani custom bir Authentication nesnesi yaratıp buraya request parameter'ları ekleyebiliriz. bunun için bir filter eklememiz gerekli:

Security config sınıfına bunlar eklenmelidir:

```java
@Override // burası zaten olmalı
protected void configure(HttpSecurity http) throws Exception {  //burası zaten olmalı

    http // burası zaten olmalı
      .addFilter(authenticationFilter())

    doOtherThingsHere();

    // addFilter yerine addFilterBefore, addFilterAfter gibi metotlar da mevcut.
}

public MySimpleFilter authenticationFilter() throws Exception {

    MySimpleFilter filter = new MySimpleFilter();
    filter.setAuthenticationManager(authenticationManagerBean());
    return filter;

}
```

Aynı zamanda filter sınıfımızı tanımlayalım:

```java
public class MySimpleFilter extends GenericFilterBean {

  public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)

    throws IOException, ServletException {
      // get params from request
      // and create token object to pass to next filter or auth-provider
      UsernamePasswordAuthenticationToken token = thisIsNotImportantForThisExample();
      chain.doFilter(req, res);
  }
}
```

Yukarıdaki UsernamePasswordAuthenticationToken nesnemizi custom bir obje yaparız ve kendi custom-auth-provider'ımızı da cast ederek kullanabiliriz.

GenericFilterBean'dan türemiş birçok hazır sınıf mevcuttur.

Eğer MySimpleFilter içinde:

```java
token.setAuthenticated = true;
SecurityContextHolder.getContext().setAuthentication(token);
```

yapıp o session'daki user'ın kimliğini doğrularsak, o istek için auth-provider kodu çalışmayacaktır.

Filter'lar, user login olmuşsa bile her istekte çalışır. auth-provider'a sadece login olmamışsa yönlendirilir.

## 📌 Spring'in default Filter'larının bazıları

- ChannelProcessingFilter: en önce olan filter'dır. HTTP'den gelenleri HTTPS'e yönlendirme, sendRedirect ile gelen yeni isteklerin nerelere yönlendirileceğini belirten tanımlar mevcut.

- SecurityContextPersistenceFilter: SecurityContextHolder ve HttpSession'ın o request için tanımlanmasını sağlıyor.

- ConcurrentSessionFilter: aynı anda, aynı kullanıcı iki farklı session ile login olabilir (örneği 2 ayrı web browser'dan). bunların yönetimini sağlayan filter'dır. burada bir önceki session iptal edilebilir yada devam ettirilebilir...

- AbstractAuthenticationProcessingFilter'dan türemiş sınıfların olduğu filter'dır. auth-provider'a Authentication nesnesini hazırlarlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JWT SUPPORT

Spring'in içinde default JWT entegrasyonu yok. elle kod yazmak gerekli.

JWT ekleyeceksek, security filter şart. JWT yapısında bu şekilde kullanıcıyı header'ındaki bilgi ile her istekte login olup olmadığını tespit edebiliriz.

JWT yapacaksak refreshToken, createToken gibi tüm metotlarımızı için REST-controller açmamız gereklidir. bu metotlara tüm doğru user bilgisi olan kişiler erişebilmelidir. bunun için bir ROLE belirlemek gerek. Bu role her user'da olmalı. çünkü herkes bu metotlara erişebilmelidir.

JWT'de login işlemini de REST servisine bağlamamız gerekli. Login tamamen public bir metot/URL olmalı. Login controller'a "@Autowired AuthenticationManager" 'ı inject edip, login metodunda token'ı oluşturup authenticationManager.authenticate(token) metodunu çağırmalıyız. auth-provider'ımızı burada çalışacaktır. ve login metodunun sonunda da kullanıcıya header'a eklemesi gerek JWT token'ımızı dönmeliyiz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Method security

Main sınıfımıza:

```java
@EnableGlobalMethodSecurity(securedEnabled = true)
```

eklenmelidir. Daha sonra herhangi bir metodun yetkisini kısıtlamak istediğimizde o metodun tepesine annotation atarız. örnek:

```java
@Secured("ROLE_USER")
public String myMethod() {
    return "Hello";
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
