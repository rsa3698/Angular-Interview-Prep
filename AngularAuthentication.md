https://www.youtube.com/playlist?list=PLLhsXdvz0qjJHtgs1b7nyue6GDcSXfJNA
Follow this Playlist for basic apporach + Exmplain Authentication in your project.

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




Authentication

1. Authentication - Design the auth (Login) page in the angular
2. Apply Template Driven Approach and implement Validations for the authentication form - Angular
3. Authentication - Perform Signup Request and get the token & expiresIn as Response - Angular
4. Authentication - Improve Error Messages with CatchError and throwError rxjs operators - Angular
5. Authentication - Send the login request and use Observable for the HTTP in the angular.
6. Authentication - Create and Store the User Token Data in the angular
7. Authentication - Send Auth Token to the outgoing HTTP Requests with behavior Subject - Angular.
8. Authentication - Add the auth token as parameter using Interceptors - Angular
9. Authentication - Adding the Logout Functionality by removing the auth token in Angular
10. Authentication - Saving Token in LocalStorage for the autologin feature - Angular
11. Authentication - Auto-Logout the user when the token expired - Angular.
12. Create Component Dynamically using ComponentFactoryResolver and ViewContainerRef in Angular.
13. Access the Component properties and methods with ComponentRef instance in Angular.
