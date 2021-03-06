##Creating a service for currency conversion

=> CurrencyCalculationService will call the CurrencyExchangeService and will ask which the conversion value

                      *****************Lesson no 85********************
**********Invoking "CurrencyExchangeMicroservice" from "CurrencyConversionMicroservice"**********
=================================================================================================

On invoking the CurrencyExchangeMicroservice there will be a response in json, we will try to map the response with 
"CurrencyConversionBean" which is exactly of the same format, and then take the conversion multiple and calculate total
calculated amount.

We use the RestTemplate to map the response to the Entity, and for that we use a method 
getForEntity(url,"respType",uriVariables)

now hitting http://localhost:8100/currency-converter/from/USD/to/INR/quantity/100 we get the converted values


                      *****************Lesson no 86********************
*************************Using Feign REST Client for Service Invocation*************************
==================================================================================================
1st use of Feign: it makes much simpler to call/invoke other microservices
2nd use of Feign: it provides integration with Ribbon, i.e, a client side load balancing framework

1. Feign is the component that spring cloud inherits from Netflix, so we need to add the dependency. 
2. Enable Feign in main class
3. Just like we use repository to talk to JPA we use Feign proxy to talk to external microservices, hence we create a 
   proxy
4. Using proxy reduces the code to just  < CurrencyConversionBean response = proxy.retrieveExchangeValue(from, to); >
   instead of using the complete rest template and response entity code. 
   
   
                      *****************Lesson no 87/88********************
*************************Setting up & running client side load balancing with Ribbon*************************
==================================================================================================
1. When CurrencyExchangeService has multiple instances and CurrencyConversionService has single instance and we want
   to distribute the load among different instances of CurrencyExchangeService we use client side load balance "Ribbon".
   
2. Ribbon makes use of feign configuration and helps us distribute the load between different instance of 
   CurrencyExchangeService
   
3. Add new dependency in pom.xml file and enable ribbon on proxy. using ribbon clint we don't need to use the hard 
   coded url, as we configure urls saperately in application.properties using property 
   currency-exchange-service.ribbon.listOfServers=http://localhost:8000,http://localhost:8001
   
4. Now launch two insances of CurrencyExchangeService and then call the feign url of currency-converter the load is 
   going to different port every time


					  *****************Lesson no 90/91/92/93/95********************
*************************Understanding all about naming server "Eureka"*************************
==================================================================================================
1. All the instances of all the microservices would register themselves with Naming Server whenever they comes up

2. Service Discovery: 
   If "CurrencyCalculationServices" wants to talk to "CurrencyExchangeService" then it would talk to "EurekaNamingServer"
   to see what are all the instances of "CurrencyExchangeService" that are currently running
   
3. Two imp features of the naming server: 
   a) Service Registration:
   		At the startup of every application they would register with Naming Server
   b) Service Discovery
   		Whenever the details of another microservices is required, there is service discovery
   		
4. Setting up Eureka Naming Server 
   a) Creating a component for EurekaNamingServer
   b) Updating the CurrencyCalculationService to connect to EurekaNamingServer
   c) Connecting CurrenceExchangeService also to talk to the EurekaNamingServer
   b) Configuring Ribbon to find the details from EurekaNamingServer

5. Connecting CurrencyConversionService with Eureka
	a) adding a dependency spring-cloud-starter-netflix-eureka-client and adding annotation "EnableDiscoveryClient" in
	main class of currencyconversionservice
	b) now configure in application.properties the url for eureka i.e, eureka.clien.service-url.default-zone=http://localhost:8761/eureka
	
6. Connecting CurrencyExchangeService microservice with Eureka
	a) Same steps as above
	
7. Execution by hitting url


					  *****************Lesson no 98/99/100********************
*************************Understanding all about API Gateway - Zuul*************************
==================================================================================================
1. There are few common features across all the microservices, eg:
	a) Authentication, Authorization & security
	b) Rate Limits
	c) Fault Tolerance
	d) Service Aggregation
These features are implemented at the level of API Gateway.

2. Instead of allowing microservices to call each other directly, we would make all the call go through a API
   Gateway. This api gateway make sure that all the above common features are adhered to. API gateway also play
   a great role for debugging and analytics
   
A) Setting up Zuul API Gateway server
	a) in main class after extrating and importing a zuul-api-gateway-server project add two annotations
	   @EnableZuulProxy & @EnableDiscoveryClient

B) Implementing Logging in API gatway after setup. It will logg all the api coming through the gateway
	a) We will impletement a ZuulLoggingFilter and will extend a ZuulFilter class which brings some methods to
	   override. 
	b) inside run() method we will write the actual logic of the filter for logging
	









   
   






















 





  