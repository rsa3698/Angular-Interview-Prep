HttpRequest

1. Introduction to Http Request. Make a Http Post Request Call through HttpClientModule - Angular.
2. Make Http Get Request and use RxJs Operators to transform the response in the Angular.
4. Define the Types of HttpClient Request Data using the interface and Generic Type in the Angular.
5. Using Services for Http request with HttpClient. Communicate Services and Components in Angular.
6. Implement HTTP Delete Request with HttpClient in Angular.
7. Error Handling in the Http Request Calls with HTTPClient
8. Sending HTTP Headers in the API Request Call with HTTPClient in the angular.
9. Adding Query Params for the Url using HttpParams Object in HttpClient
10. Observe different types of response in HttpClient and Changing the Response Body Type
11. Introducing HTTP Interceptors using HTTP_INTERCEPTORS in Angular.
12. Manipulating Request Objects, headers with Interceptors in the Angular.
13. Accessing Http Response Event Object with Response Interceptors in the Angular.
14. Adding Multiple Interceptors for the HTTP Request. Interceptors executing order in Angular.

All about Angular HTTP:
https://www.tektutorialshub.com/angular-tutorial/#httpnbsp


Angular HTTP Client Tutorial
HTTP GET Example
HTTP POST Example
Passing URL Parameters (Query strings)
HTTP Headers Example


MOST IMP TOPIC: HTTP Interceptor
https://www.tektutorialshub.com/angular/angular-httpclient-http-interceptor/

Angular HTTP interceptors provide a way to intercept and modify HTTP requests and responses globally. Interceptors are a powerful mechanism for adding common functionality to HTTP requests or responses, such as authentication, error handling, or modifying headers. They allow you to centralize certain behaviors that should be applied to every HTTP request or response in your application.

Here's a use case and an example of an Angular HTTP interceptor:

Use Case: Adding an Authorization Token to Requests
Suppose you have an Angular application that communicates with a server that requires an authorization token for authentication. You want to include this token in the headers of every outgoing HTTP request.

Example: Creating an HTTP Interceptor for Authorization
Create the Interceptor:
typescript
Copy code
// auth-interceptor.ts

import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Get the authorization token from your authentication service
    const authToken = 'your-auth-token';

    // Clone the request and add the authorization header
    const authRequest = request.clone({
      setHeaders: {
        Authorization: `Bearer ${authToken}`
      }
    });

    // Pass the cloned request to the next handler
    return next.handle(authRequest);
  }
}
Register the Interceptor:
In your app.module.ts or a dedicated module, you need to provide the interceptor using the HTTP_INTERCEPTORS token.

typescript
Copy code
// app.module.ts

import { NgModule } from '@angular/core';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './path-to-auth-interceptor';

@NgModule({
  imports: [
    HttpClientModule,
    // other module imports
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true
    }
    // other providers
  ],
  bootstrap: [/* your root component */]
})
export class AppModule { }
This tells Angular to use AuthInterceptor as an HTTP interceptor.

Now, every outgoing HTTP request from your Angular application will include the Authorization header with the specified token. You can easily extend this example to handle other scenarios like error handling or modifying responses.

Remember to replace 'your-auth-token' with the actual token obtained from your authentication service. Additionally, consider handling token expiration, refreshing tokens, or other aspects based on your specific authentication requirements.