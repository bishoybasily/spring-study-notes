# spring-study-notes


### SpEL Compiled mode unsupported features: 
* Assignments 
* Relying on conversion service 
* Selection or Projection 
* Relying on custom accessors or resolvers 
### Spring-Boot Property sources precedence order: 
* Dev-tools (~/.spring-boot-devtools.properties when devtools is active) 
* @TestPropertySource (for tests) 
* @SpringBootTest#properties (for tests) 
* Command line arguments 
* Properties from SPRING_APPLICATION_JSON (a special env var contains inline json) 
* ServletConfig init Params (for web) 
* ServletContext init Params (for web) 
* JNDI attributes (for web) 
* Java System Properties (System#getProperties()) 
* Environment Variables (other than SPRING_APPLICATION_JSON) 
* RandomValuePropertySource (random.int ...) 
* Profile-specific files Outside the jar (properties and/or yml) 
* Profile-specific files Inside the jar (properties and/or yml) 
* Application properties Outside the jar (properties and/or yml) 
* Application properties Inside the jar (properties and/or yml) 
* @PropertySource on @Configuration annotated classes 
* Default properties set programmatically (SpringApplication#setDefaultProperties()) 
### Most of the annotations can be applied to other annotations to create a composed annotation 
### In declarative transactions (using @Transactional), spring relies on its own AOP using CGLIB and JDK Proxies which means all Proxies limitations are relevant to declarative transactions, this behavior can be altered by switching to AspecJ (@EnableTransactionManagement#mode, AspectJ needs to be configured correctly). 
### Visibility levels supported by CGLIB Proxy are public, protected & package-private 
### AOP predicates (PointCut expressions)
* execution - method execution 
* this - targets a specific proxy (cglib or jdk according to proxyTargetClass enabled or not), it targets the proxy by referencing the taget object class 
* bean - a specific managed spring bean by name 
* within - within a class/es 
* @within - within a class/es annotated with 
* args - method arguments 
* @args - method arguments annotated with 
* target - a specific target class  
* @target - a specific target class annotated with 
* @annotation - method annotated with 
### Transaction propagations 
* Required: supports current, creates if none (default) 
* Supports: supports current, non-transactionally if none 
* Mandatory: supports current, throws if none 
* Requires New: creates new, suspends any 
* Not Supported: non-transactionally, suspends if any 
* Never: non-transactionally, throws if any 
* Nested: nested, supports current, creates if none 
### Transaction isolation (what is bad about each) 
* Serializable: nothing 
* Repeatable Reads: phantom reads 
* Read committed: phantom reads, repeatable reads 
* Read uncommitted: phantom reads, repeatable reads, dirty reads 
### JdbcTemplate callbacks: 
* RowCallbackHandler: Stateful process row by row 
* ResultSetExtractor<T>: Stateless process the entire resultSet 
* RowMapper<T>: Stateless process row by row 
### All actuator endpoints are enabled by default (available through JMX) except “shutdown”, all of them are not exposed over HTTP except “health” with minimal details 
### Health Indicators default severity order: DOWN, OUT_OF_SERVICE, UP, UNKNOWN. This can be altered through management.endpoint.health.status.order. 
### @SpringBootTest default webEnvironment is MOCK if servlet APIs are on the classpath otherwise regular ApplicationContext will be created, if you don’t want to provision a servlet environment set webEnvironment to NONE explicitly, RANDOM_PORT & DEFINED_PORT will provision an embedded servlet environment, the random port can be injected with @LocalServerPort 
### For a faster IntegrationTest you might consider using @AutoConfigure... Composed annotations instead of @SpringBootTest, examples: 
* For Jpa @DataJpaTest is more convenient to test all Jpa related parts, it’s a composed annotation brings @ImportAutoConfiguration, @Transactional & along with other @AutoConfigure... annotations related to Jpa. 
* Other examples like @DataMongoTest, @WebFluxTest … each enables different auto configuration. 
### Excluding auto configuration in a test can be done through @ImportAutoConfiguration#execlude, @EnableAutoConfiguration#execlude 
### Default http status mapping for actuator health indicators is: 
* 200 for UP & UNKNOWN 
* 503 for DOWN & OUT_OF_SERVICE 
### Logging debug and trace can be enabled with: 
* In the root config file debug=true & trace=true 
* Logging properties in the config file logging.level.root=DEBUG, logging.level.root=TRACE 
* Jar arguments “java -jar app.jar --debug”, “java -jar app.jar --trace” 
### META-INF/spring.factories file registers the following: 
* Auto Configuration classes 
* Failure Analyzers 
* Environment Post-Processors 
* Application Event Listeners 
* Filter to limit Auto Configuration classes considered 
* Availability of template view providers (implements TemplateAvailabilityProvider interface) 
### Empty Request Parmas 
* Spring will set empty values for RequestParam controller method args if only the key presented in the url without value exmples: 
  * /api/users?name on @RequestParam String name, name will be set to “” or the default value declared in the annotation 
  * /api/numbers?nums on @RequestParam(name=”nums”) List<Integer> numbers will result in an empty list to be created 
* In case of Map<String,String> params, whatever presented in the url will be injected key without a value or empty map, the map will be initialized anyway. 
* Otherwise, if the method is the only match, the http client will receive bad request 400 
* Setting a default value will result in marking the field as not required 
### Local data validator gets registered using @InitBinder & WebDataBinder, Global data validator using WebMvcConfigurer#getValidator 
### Some view technologies supported by spring FreeMaker, JSP & JSTL, Thymeleaf, GroovyMarkup, Velocity. 
### In MVC Multi-Step form processing use HttpSession#setComplete to mark the request as completed. 
### Both ResponseEntity<T> and HttpEntity<T> can be returned from a controller method to provide headers along with the body, ResponseEntity extends HttpEntity to status object 
### ProviderManager which implements AuthenticationManager responsible for authenticating the user by delegating the authentication to one or more AuthenticationProvider implementations, authenticationProvider decides whether this user can be authenticated or it doesn’t support this kind of authentication which makes the ProviderManager delegates the call to another AuthenticationProvider until one decides that the user is Authenticated by returning an Implementation of Authentication or decides that the credentials are invalid or returns null for another provider to try authenticating the user. 
### @RolesAllowed from jsr250 allows the invocation if any role matches one from the array NOT all of them, it’s incapable of expressing ROLE_X && ROLE_Y 
### AccessDescisionManagers: 
* AffirmativeBased: provides access if at least one access granted vote is received 
* ConsensusBased: provides access if access granted votes >= access denied votes 
* UnanimousBased: provides access if all voters granted access 
### SecurityContextHolder default mode is TREAD_LOCAL (!!!!!! NOT inheritable thread local !!!!!!!!) 
 

