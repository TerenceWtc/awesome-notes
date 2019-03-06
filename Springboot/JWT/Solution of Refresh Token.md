# Solution of Refresh Token

## Failure Experence
In my first try, there's only an access token.  
I would generate a new access token when the current one is expired.  
But once a request with the expired access token was sent to backend service, a `ExpiredJwtException` exception was thrown and it was handled by Spring Framework itself.  
I can not get the infomation which i need to generate a new token from the expired token.  
Finally a `403 Forbidden` response returned.

## What is Refresh Token?
I realized that i thought it too simple.  
I search the reference infomation online, and found the solution from the [blog](https://blog.csdn.net/qq_24127857/article/details/80818742).

In the blog owner's solution, there's two tokens.  
One is `access token`, the other one is `refresh token`.  
Usually the access token will be active for less than an hour, it is unbearable to sign in with account & password in such a frequency.  
So we have to use refresh token to re-generate an access token for extending the time of authentication.  
The refresh should have a much longer active period, may be 7 days or longer, so that we can use it to re-generate a new access token.  
This frequency should be acceptable, and the solution is also effective on preventing the privacy.

## How to
Once a customer signed in, i will generate both an access token and a refresh token in the backend service, and send it to the frontend as response.  
Storing both token in local storage, and if access token expired, call an api with refresh token to re-generate.

## Expiration Problem Again
Using refresh token, I solve the problem of entending the token expiration.  
I had a new try:  
When a customer raise a request and the access token is expired, I can catch the exception and re-generate a new token and use the new access token to authenticate the request, finally return the result with the new access token.

I believed that it would be perfect, but failed.
Nothing would happen, the customer would feel the request was missing.  

The reason is that the request raised by customer would not invoke the method generating token.  
It is intercepted in the filter, thrown an expired exception, and handled by framework again! ~~(WHAT THE FXXK!)~~

## Frontend
I found i should judge the expiration before the request was raised.  
I use `jwt-decode` to judge the expiration, and `axios` for the `ajax` request in frontend service.  
Using axios' interceptor, i can set access token into the request header, also, i can judge the token and raise a request to get new access token before the customer's request.  

But axios does not support an asynchronous request in the interceptor.  
It means the customer raise an expired request, you intercepted it and raise a new asynchronous request.  
Before your request has responsed, the customer's request will be over.

So I use the new feature in ES7, `async` and `await` to make my request synchroniezd.

That's great!  
When I raised an expired request, the code judged the token expired, and sent a new request to get new token, and my request was blocked until the new token returned, and then, awake my request with new token.  

The whole process is successful!
