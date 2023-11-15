1. Angular Lifecycle Hook?

Here is the complete list of life cycle hooks, which angular invokes during the component life cycle. Angular invokes them when a specific event occurs.
Constructor
ngOnChanges
ngOnInit
ngDoCheck
ngAfterContentInit
ngAfterContentChecked
ngAfterViewInit
ngAfterViewChecked
ngOnDestroy

Constructor: whenever it is instantiated
ngOnChanges: Called every time a data-bound input property changes. It’s called a first time before the ngOnInit hook. The hook receives a SimpleChanges object that contains the previous and current values for the data-bound inputs properties. This hook gets called often, so it’s a good idea to limit the amount of processing it does.
ngOnInit: Called once upon initialization of the component.
ngDoCheck: Use this hook instead of ngOnChanges for changes that Angular doesn’t detect. It gets called at every change detection cycle, so keeping what it does to a minimum is important for performance.
ngAfterContentInit: Called after content is projected in the component.
ngAfterContentChecked: Called after the projected content is checked.
ngAfterViewInit: Called after a component’s view or child view is initialized.
ngAfterViewChecked: Called after a component’s view or child view is checked.

Change detection Cycle
Before diving into the lifecycle hooks, we need to understand the change detection cycle.

Change detection is the mechanism by which angular keeps the template in sync with the component

Consider the following code.

<div>Hello {{name}}</div>
 
 
Angular updates the DOM whenever the value of the name changes. And it does it instantly.

How does angular know when the value of the name changes? It does so by running a change detection cycle on every event that may result in a change. It runs on every input change, DOM event, and timer event like setTimeout(), setInterval(), HTTP requests, etc. 

During the change detection cycle, angular checks every bound property in the template with that of the component class. If it detects any changes, it updates the DOM. 

Angular raises the life cycle hooks during the critical stages of the change detection mechanism.

2. What is Role of Constructor in Angular Life Cycle Hooks?

The life cycle of a component begins when Angular creates the component class. The first method that gets invoked is class Constructor.
Constructor is neither a life cycle hook nor is it specific to Angular.  It is a Javascript feature. It is a method that is invoked when a class is created. 
Angular makes use of a constructor to inject dependencies.
At this point, none of the component’s input properties are available. Neither its child components are constructed. Projected contents are also not available. 
Hence there is little you can do with this method. And also, it is recommended not to use it 
Once Angular instantiates the class, It kick-starts the first change detection cycle of the component.

3. Explain ngOnChanges?

The Angular invokes the ngOnChanges life cycle hook whenever any data-bound input property of the component or directive changes. Initializing the Input properties is the first task angular carries during the change detection cycle. And if it detects any change in property, then it raises the ngOnChanges hook. It does so during every change detection cycle. This hook is not raised if change detection does not detect any changes.

Input properties are those properties which we define using the @Input decorator. It is one of the ways by which a parent communicates with the child component.

In the following example, the child component declares the property message as the input property

@Input() message:string
 
The parent can send the data to the child using the property binding, as shown below.

<app-child [message]="message">
</app-child>
 
The change detector checks if the parent component changes such input properties of a component. If it is, then it raises the ngOnChanges hook.

4.  Explain ngOnInit?
The Angular raises the ngOnInit hook after it creates the component and updates its input properties. It raises it after the ngOnChanges hook.

This hook is fired only once and immediately after its creation (during the first change detection).

This is a perfect place where you want to add any initialization logic for your component.  Here you have access to every input property of the component. You can use them in HTTP get requests to get the data from the back-end server or run some initialization logic etc.

But note that none of the child components or projected content are available at this juncture. Hence any properties we decorate with @ViewChild, @ViewChildren, @ContentChild & @ContentChildren will not be available to use.

5. ngDoCheck?
The Angular invokes the ngDoCheck hook event during every change detection cycle. This hook is invoked even if there is no change in any of the properties.

Angular invoke it after the ngOnChanges & ngOnInit hooks.

Use this hook to Implement a custom change detection whenever Angular fails to detect the changes made to Input properties. This hook is convenient when you opt for the Onpush change detection strategy.

The Angular ngOnChanges hook does not detect all the changes made to the input properties.

6.  ngAfterContentInit?

ngAfterContentInit Life cycle hook is called after the Component’s projected content has been fully initialized. Angular also updates the properties decorated with the ContentChild and ContentChildren before raising this hook. This hook is also raised, even if there is no content to project.

The content here refers to the external content injected from the parent component via Content Projection. 

The Angular Components can include the ng-content element, which acts as a placeholder for the content from the parent as shown below

 
<h2>Child Component</h2>
<ng-content></ng-content>   <!-- placehodler for content from parent -->
 
 
The parent injects the content between the opening & closing element.  Angular passes this content to the child component

<h1>Parent Component</h1>
<app-child> This <b>content</b> is injected from parent</app-child>
 
 
During the change detection cycle, Angular checks if the injected content has changed and updates the DOM.

This is a component-only hook.

7. Explain ngAfterContentChecked , ngAfterViewInit , ngAfterViewChecked ?

ngAfterContentChecked
ngAfterContentChecked Life cycle hook is called during every change detection cycle after Angular finishes checking of component’s projected content. Angular also updates the properties decorated with the ContentChild and ContentChildren before raising this hook. Angular calls this hook even if there is no projected content in the component.

This hook is very similar to the ngAfterContentInit hook. Both are called after the external content is initialized, checked & updated. The only difference is that ngAfterContentChecked is raised after every change detection cycle. While ngAfterContentInit during the first change detection cycle.

This is a component-only hook.

ngAfterViewInit
ngAfterViewInit hook is called after the Component’s View & all its child views are fully initialized. Angular also updates the properties decorated with the ViewChild & ViewChildren properties before raising this hook. 

The View here refers to the template of the current component and all its child components & directives. 

This hook is called during the first change detection cycle, where angular initializes the view for the first time.

At this point, all the lifecycle hook methods & change detection of all child components & directives are processed & Component is entirely ready. 

This is a component-only hook.

ngAfterViewChecked
The Angular fires this hook after it checks & updates the component’s views and child views. This event is fired after the ngAfterViewInit and after that, during every change detection cycle.

This hook is very similar to the ngAfterViewInit hook. Both are called after all the child components & directives are initialized and updated. The only difference is that ngAfterViewChecked is raised during every change detection cycle. While ngAfterViewInit during the first change detection cycle.

This is a component-only hook.

8. Explain ngOnDestroy?

This hook is called just before the Component/Directive instance is destroyed by Angular
You can Perform any cleanup logic for the Component here. This is where you would like to Unsubscribe Observables and detach event handlers to avoid memory leaks.

9. Explain order of execution of life-cycle hooks?

The Order of Execution of Life Cycle Hooks
The Angular executes the hooks in the following order

On Component Creation

OnChanges
OnInit
DoCheck
AfterContentInit
AfterContentChecked
AfterViewInit
AfterViewChecked
When the Component with Child Component is created

OnChanges
OnInit
DoCheck
AfterContentInit
AfterContentChecked
Child Component -> OnChanges
Child Component -> OnInit
Child Component -> DoCheck
Child Component -> AfterContentInit
Child Component -> AfterContentChecked
Child Component -> AfterViewInit
Child Component -> AfterViewChecked
AfterViewInit
AfterViewChecked
After The Component is Created

OnChanges
DoCheck
AfterContentChecked
AfterViewChecked
The OnChanges hook fires only if an input property is defined in the component and it changes. Otherwise, it will never fire.

10. 