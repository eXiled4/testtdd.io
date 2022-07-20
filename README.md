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

