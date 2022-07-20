# Testtdd

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 14.0.6.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

## TESTTDD NOTES##

<!-- Add Jasmine to your package.json
npm install --save-dev jasmine

Initialize Jasmine in your project
npx jasmine init

Set jasmine as your test script in your package.json
"scripts": { "test": "jasmine" }

Run your tests
npm test -->

ng test will run all tests and spec files, provide with set of results showing what is passing and failing
<!-- Angular CLI app comes with special Testbed module - to test the app, in the same way in production -->

Apps are made up of different parts and can be brought together by DEPENDENCY INJECTION during testing as 
<!-- In Testing environment we dont always want to run all components -->

Ergo, we are able to test specific parts we intend on testing on

<!-- In example video - the injection goes where the explicit test is written -->

beforeEach(() => {
    TestNed.configureTestingModule({
        providers: [MyDemoService] 
        <!-- First, provider is declared as the part to be tested-->
    })
})

it('should create', inject([MyDemoService], (service: MyDemoService) => {
    expect(service).tobeTruthy();
    <!-- Then in the actual test, it is injected -->
    <!-- first parameter in the inject line [] - is what we want to inject, the second parameter is the assertion function -->
}))

## Another wasy to inject service into assertion test is by calling the TestBed.get() through a variable inside an async test:

it('#getFullName_p will return Randy Marsh (async)', async( () => {
    let asyncserv = TestBed.get(UserService)
    asyncserv.getUserName_p().subscribe ( nameValue() => {
        expect(nameValue).toBe('Randy Marsh')
    })
}))


## TESTING SERVICE CLASS

SERVICE FILE SYNTAXES
<!-- File name is UserService -->

user = {
    fname:"Randy"
    lname:"Marsh"
}

constructor() { }

getFullName():string {
    return `${this.fname} ${this.lname}`
}

SERVICE SPEC FILE

beforeEach(() => {
    serv = new UserService();
})

it('will show stan's dad full name', () => {
    expect(service.getFullName()).toBe('Randy Marsh')
})

it('#getFullName_p will return Randy Marsh', (done:DoneFn) => {
    service.getFullName_p().then(nameValue => {
        expect(nameValue).toBe('Randy Marsh')
        done();
        <!-- DONE IS A METHOD WE MANUALLY CALL FROM DONEFN, AFTER ANY ASYNC CALLS THAT HAVE COMPLETED... -->
        <!-- ...We call Done to indicate that the test is complete. -->
    })
})

## TESTING A COMPONENT

testing code:

describe('UserProfComponent', () => {
    let comp: UserProfComponent;
    let fixt: ComponentFixture<UserProfComponent>
    <!-- This line allows debugging and testing a component, -with the presence of the <UserProfComponent> .. -->
    <!-- ..Allows us to test the DOM elements of the component -->

    beforeEach (async(() => {
        TestBed.configureTestingModule({
            declarations: [UserProfComponent]
        }).compileComponents();
    }))
})


<!-- compileComponents() is an async operation, only when it is complete, we will have access to all external files or stylesheets relating to the delcared components-->

beforeEach(() => {
    fixture = TestBed.createComponent(UserProfComponent);
    comp = fixture.componentInstance
    fixture.detectChanges();
})

<!-- This sync testing fx above creates a new instance for testing purposes and then binds the value to be tested via fixture.detectChanges() -->

## TESTING A DIRECTIVE

Similiar to testinf componenents, we declare variables to inherit the DOM elements from component by fixt below
<!-- HOWEVER NOW WE ARE USING THE DEBUGELEMENT TOO HERE -->
<!-- debugElements, which is going to be an array of type DebugElement. -->
<!-- debugElement is angular abstraction around the actual HTML Element -->
<!-- AND PROVIDES EXTRA FUNCTIONALITY for testing and working with it -->

describe('testing directive rabble rabble', () => {
    let fixt: ComponentFixture<UserProfComponent>
    let debugElements: DebugElement[]

    beforeEach(() => {
        TestBed.configureTestingModule({
            declarations: [UserProfComponent, UserDirective]
        })
    })
})
<!-- AS ABOVE, THE DIRECTIVE TO BE TESTED ON IS ADDED TO DECLARATIONS -->

inside assertive test - example of how to test css styles

it('should have a border and background color', () => {
    let box = fixture.debugElement.query(By.css('#box4'))
    let borderColor =  box.nativeElement.style.borderColor;
    let backgroundColor = box.nativeElement.style.backgroundColor

    expect(borderColor).toBe('brown')
    expect(backgroundColor).toBe(DEFAULT_BG_COLOR)
})

## TESTING PIPES

Testing pipes is straightforward using transform method

Example code below -

Pipe TS:

export class BlahPipe implements transform PipeTransform {

    transform(value: string):string {
        if(value.length == 0){
            return ""
        }

            let chars = value.split("")
            let newValue = ""
            chars.forEach((c, i) => {
                let char = (i%2 == 0) ? c.toLowerCase() : c.toUpperCase()
                newValue = newValue + char
            })

        return newValue
    }

}

Pipe TS Spec:

describe('AlternatingCasePipe', () => {
    it('create an instance', () => {
        const pipe = new AlternatingCasePipe();
        expect(pipe).toBeTruthy();
    });

    it('alternate lower and upper cases', () => {
        const pipe = new AlternatingCasePipe();
        expect(pipe.transform('Andy')).toBe('aNdY')
    })
})

<!-- in assertion test, we can see here that we are creating an instance of the pipe through the AlternatingCasePipe() -->
<!-- Then we make use of the transform method that asserts the function and as usual, state expected outcomes by chaining the toBe() -->

## CREATING TESTS USING MOCKS

Common to have components/services to depend on other services

Several ways to test elements with dependencies

One of them is by mocking dependencies so we don't have to rely on actual implementations to test the component they are inject to

EXAMPLE COMPONENT TS FILE:

export class ... implements OnInit {

    user: any
    friendsOnLine:Observable<string[]>

    constructor(private userServ: Userservice) { }

    ngOnInit() {
        this.user = this.userServ.getUserData()
    }
}

EXAMPLE COMPONENT TS SPEC FILE:

describe('insert description here' () => {
    let comp: UserProfComponent
    let fixt: ComponentFixture<UserProfComponent>

    beforeEach(async(() => {
        TestBed.configureTestModule({
            declarations: [ UserProfComponent ],
            providers: [{
                provide: UserService, useClass: MockService
                <!-- NOTE ABOVE THAT WHAT THE PROVIDERS DECLARTION MEANS IS THAT WE ARE declaring and using -->
                <!-- the component inside useClass instead of the actual service -->
            }]
        })
    }))

})

<!-- As you can see, UserProfileWithDependencyComponent has a service UserService -->

## Creating Tests Using Spies

When setting up component testing, where our components also have dependencies on async services, we can use JASMINE SPIES to intercept these calls
And return values right away that we can work with

inside the beforeEach() - we can first create dummy data to be tested lke this:

let spy_getOnlineFriends
let spy_getUserDetails

beforeEACH(() => {



let dummyDetails = {
    first:"Eric",
    last:"Cartman",
    friends:[
        "Kenny", "Stan", "Kyle"
    ]
}

const usService = jasmine.createSpyObj('UserService', ['getOnlineFriends], 'getUserDetails')

spy_getOnlineFriends = usService.getOnlineFriends.and.returnValue(of(dummyDetails.friends))
spy_getUserDetails

})

<!-- IN ASSERTION TESTS, WE CAN SPECIFY THE SPECIFIC ELEMENT THAT WE WANT TO TEST FOR -->

fixture.detectChanges();
it("desciption here", () => {
    fixture.detectChanges();
    expect(debugElements[0].nativeElement.innerText).toContain('Kenny')
    expect(debugElements[1].nativeElement.innerText).toContain('Stan')
})

## Testing Components with Input

When creating components in our Angular 6 apps, some of them will be intended to be used as child components that go into parent or host components.
As such, these components are going to have inputs that are passed in from their hosts and used internally in some way.


## Testing Components with Output

When creating components in our Angular 6 apps, some of them will be intended to be used as child components and parent, or host, components.
As such, these child components are going to have outputs that the host components can subscribe to in order to receive events from that child.

we can see that it's these two methods startUp and shutDown that actually emit events by using the appropriate event emitters.

@Output('started')started = new EventEmitter<any>()
@Output('stopped')stopped = new EventEmitter<any>()

isStarted:boolean = false

startup() {
    this.isStarted = true
    this.started.emit({make:this.make, model:this.model})
}

<!-- CREATE HTML ELEMENTS WITH THESE EVENT BINDINGES HERES -->


## Testing Services That Use HttpClient

It's a common scenario to have a custom data service created as part of our Angular app 
that makes a network call to get or set some data.

In ng, best way to make a HTTP call from a service is through the HttpClientService

        //First we inject the HttpClient dependency

        EXAMPLE COMP TS

        import { HttpClient } from '@angular/common/http'


        constructor(private http:HttpClient) {  }

        //Then create a function with http get method that takes any data type

        getFoods():Observable<any> {
            return this.http.get(this.endpoint)
        }

        //


## Testing with Automatic Change Detection

