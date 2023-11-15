1. Explain use of Angular route guards?
Angular Route Guards
We use the Angular Guards to control, whether the user can navigate to or away from the current route.

Why Guards
We looked at how to configure our routes and navigate to the different parts of our application in our Angular Router Tutorial. Allowing the user to navigate all parts of the application is not a good idea. We need to restrict the user until the user performs specific actions like login. Angular provides the Route Guards for this purpose.

One of the common scenario, where we use Route guards is authentication. We want our App to stop the unauthorized user from accessing the protected route. We achieve this by using the CanActivate guard, which angular invokes when the user tries to navigate into the protected route. Then we hook into the CanActivate guard and use the authentication service to check whether the user is authorized to use the route and if not we can redirect the user to the login page.

Uses of  Angular Route Guards
To Confirm the navigational operation
Asking whether to save before moving away from a view
Allow access to certain parts of the application to specific users
Validating the route parameters before navigating to the route
Fetching some data before you display the component.


2. What are type of Route guards in Angular?

Types of Route Guards
The Angular Router supports Five different guards, which you can use to protect the route

CanActivate
CanDeactivate
Resolve
CanLoad
CanActivateChild
CanActivate
This guard decides if a route can be activated (or component gets used). This guard is useful in the circumstance where the user is not authorized to navigate to the target component. Or the user might not be logged into the system

Read: Angular CanActivate Guard Example

CanDeactivate
This Guard decides if the user can leave the component (navigate away from the current route). This route is useful in where the user might have some pending changes, which was not saved. The CanDeactivate route allows us to ask user confirmation before leaving the component.  You might ask the user if it’s OK to discard pending changes rather than save them.

Read: Angular CanDeactivate Guard Example

Resolve
This guard delays the activation of the route until some tasks are complete. You can use the guard to pre-fetch the data from the backend API, before activating the route

CanLoad
The CanLoad Guard prevents the loading of the Lazy Loaded Module. We generally use this guard when we do not want to unauthorized user to be able to even see the source code of the module.

This guard works similar to CanActivate guard with one difference. The CanActivate guard prevents a particular route being accessed. The CanLoad prevents entire lazy loaded module from being downloaded, Hence protecting all the routes within that module.

Read: Angular CanLoad Guard Example

CanActivateChild
This guard determines whether a child route can be activated. This guard is very similar to CanActivateGuard. We apply this guard to the parent route. The Angular invokes this guard whenever the user tries to navigate to any of its child route. This allows us to check some condition and decide whether to proceed with the navigation or cancel it.

3. How to implement route guard?

How to Build Angular Route Guards
Building the Guards are very easy.

Build the Guard as Service.
Implement the Guard Method in the Service
Register the Guard Service in the Root Module
Update the Routes to use the guards
1. Build the Guard as Service
Building the Guard Service is as simple as building any other Angular Service. You need to import the corresponding guard from the Angular Router Library using the Import statement. For Example to use CanActivate Guard import the CanActivate in the import  the CanActivate in the import statement


 
import { CanActivate } from '@angular/router';
 
Next, create the Guard class which implement the selected guard Interface as shown below.


 
@Injectable()
export class ProductGuardService implements CanActivate {}
 
You can also inject other services into the Guards using the Dependency Injection

2. Implement the Guard Method
The next step is to create the Guard Method. The name of the Guard method is same as the Guard it implements. For Example to implement the CanActivate guard, create a method CanActivate


 
canActivate(): boolean {    
    // Check weather the route can be activated;    
    return true;     
    // or false if you want to cancel the navigation; 
}
 
Return value from the Guard
The guard method must return either a True or a False value.

If it returns true, the navigation process continues. if it returns false, the navigation process stops and the user stays put.

The above method returns a True value. The Guard can also return an Observable or a Promise which eventually returns a True or false. The Angular will keep the user waiting until the guard returns true or false.

The guard can also tell the router to navigate elsewhere, effectively canceling the current navigation.

3. Register the Guard as Service in Module
As mentioned earlier, guards are nothing but services. We need to register them with the Providers array of the Angular Module as shown below


 
providers: [ProductService,ProductGuardService]
 
The Angular router requires the Guards and all other services that guard depends on available during the navigation. Hence the guards must be provided at the module level. This allows the router to access the guards using the Dependency Injection.

4. Update the Routes
Finally, we need to add the guards to the routes array as shown below


 
{ path: 'product', component: ProductComponent, canActivate : [ProductGuardService] 
}
 
The above code adds the canActivate guard (ProductGuardService) to the  Product route.

When the user navigates to the Product route the Angular calls the canActivate method from the ProductGuardService. If the method returns true then the ProductComponent is rendered.

You can add more than one guard as shown below


 
{ path: 'product', 
  component: ProductComponent, 
  canActivate : [ProductGuardService, AnotherProductGuardService ] 
}
 
The syntax for adding other guards are also similar


{ path: 'product', component,        
    canActivate : any[],        
    canActivateChild: any[],       
    canDeactivate: any[],       
    canLoad: any[],       
    resolve: any[] 
}
 
Order of execution of route guards
A route can have multiple guards and you can have guards at every level of a routing hierarchy.

CanDeactivate() and CanActivateChild() guards are always checked first. The checking starts from the deepest child route to the top.

CanActivate() guard is checked next and checking starts from the top to the deepest child route.

CanLoad() is invoked next,  If the feature module is to be loaded asynchronously.

Resolve() Guard is invoked last.

The Angular Router cancels the navigation If any of the guards return false.


5. Example of CanActivate() Guard?
https://www.tektutorialshub.com/angular/angular-canactivate-guard-example/


6. Example of CanActivateChild Guard?
https://www.tektutorialshub.com/angular/angular-canactivatechild-example/

7. Example of CanDeactivate() Guard?
https://www.tektutorialshub.com/angular/angular-candeactivate-guard/
import {ActivatedRouteSnapshot, CanDeactivate, RouterStateSnapshot} from '@angular/router'
import { Observable } from 'rxjs'

export interface IDeactiveateGuard{

    canExit:()=> boolean | Promise<boolean> | Observable<boolean>;
}

export class DeactivateGuardService implements CanDeactivate<IDeactiveateGuard>{

    canDeactivate(
        component: IDeactiveateGuard,
        route: ActivatedRouteSnapshot,
        currentState : RouterStateSnapshot,
        nextState: RouterStateSnapshot
    ):boolean | Promise<boolean> | Observable<boolean>{
        return component.canExit()
    }
}
const routes: Routes = [
  {
    path: 'users',
    component: UsersComponent,
    canActivate: [AuthGuard],
    children: [
      { path: ':id/:name', component: UserComponent },
      {
        path: ':id/:name/edit',
        component: EditUserComponent,
        canDeactivate: [DeactivateGuardService],
        resolve: { user: UserResolveService },
      },
    ],
  },
];


8. Explain Angular Resolve Guard

https://www.tektutorialshub.com/angular/angular-resolve-guard/
import { Injectable } from "@angular/core";
import {  ActivatedRouteSnapshot, Resolve, RouterStateSnapshot } from "@angular/router";
import { Observable } from "rxjs";
import { UserService } from "../user.service";

interface User{
    id: string,
    name: string
}

@Injectable()
export class UserResolveService implements Resolve<User>{
    constructor( private userService : UserService){

    }
    resolve(
        route: ActivatedRouteSnapshot,
        state: RouterStateSnapshot
      ): User | Observable<User> | Promise<User>{
    let id = route.params['id'];
    let details = this.userService.getUser(id);

    return details;

    }
}
See module part at above code


Full Implemenation:

In Angular, route resolvers are used to fetch some data before navigating to a route. Resolvers are services that help to pre-fetch data and ensure that the route is activated only when the necessary data is available. This can be useful when you want to resolve data dependencies before rendering the component associated with a route.

Here's a simple example to illustrate the use of route resolvers in Angular:

Let's assume you have a service (DataService) that fetches user details from an API:

typescript
Copy code
// data.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  constructor(private http: HttpClient) {}

  getUserDetails(userId: string): Observable<any> {
    // Assume there is an API endpoint that fetches user details by ID
    return this.http.get(`api/users/${userId}`);
  }
}
Now, let's set up a route resolver to fetch user details before activating a route:

typescript
Copy code
// user-resolver.service.ts
import { Injectable } from '@angular/core';
import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs';
import { DataService } from './data.service';

@Injectable({
  providedIn: 'root'
})
export class UserResolver implements Resolve<any> {
  constructor(private dataService: DataService) {}

  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<any> {
    const userId = route.paramMap.get('id');
    return this.dataService.getUserDetails(userId);
  }
}
Now, configure your route in the module, specifying the resolver:

typescript
Copy code
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { UserComponent } from './user.component'; // Assume you have a UserComponent to display user details
import { UserResolver } from './user-resolver.service';

const routes: Routes = [
  {
    path: 'users/:id',
    component: UserComponent,
    resolve: {
      userData: UserResolver
    }
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
In this configuration:

The UserResolver is specified in the resolve property of the route.
The resolver is responsible for calling the getUserDetails method from the DataService and returning the result as an observable.
The resolved data is then made available to the component through the route's data property.
Finally, in your UserComponent, you can access the resolved data like this:

typescript
Copy code
// user.component.ts
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-user',
  template: `
    <div *ngIf="userData">
      <!-- Display user details -->
      <h2>{{ userData.name }}</h2>
      <p>Email: {{ userData.email }}</p>
    </div>
  `,
})
export class UserComponent {
  userData: any;

  constructor(private route: ActivatedRoute) {
    this.userData = this.route.snapshot.data['userData'];
  }
}
With this setup, Angular will ensure that the UserResolver is executed before activating the UserComponent, and the resolved data will be available in the component. This helps in managing data dependencies when navigating between routes.

9. Explain passing data using Routes? Static vs Dynamic?
https://www.tektutorialshub.com/angular/angular-pass-data-to-route/

10. Explain RouterLinkActive ?
https://www.tektutorialshub.com/angular/routerlinkactive-router-outlet-styling/

11. Explain ROuter Events?
Summary
The Router events allow us to watch for the router state changes and run some custom logic. One of the use case scenarios is to show the loading indicator when the user navigates from one route to another. You can listen to NavigationStart and NavigationEnd events to achieve this result

https://www.tektutorialshub.com/angular/angular-router-events/

12. Explain activated route?
https://www.tektutorialshub.com/angular/activatedroute-in-angular/