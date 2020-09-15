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
   
   
                      *****************Lesson no 87********************
*************************Setting up client side load balancing with Ribbon*************************
==================================================================================================













 





  