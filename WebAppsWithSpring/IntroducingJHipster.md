## Introducing JHipster  
[JHipster](https://spring.io/blog/2015/02/10/introducing-jhipster), or `Java Hipster`, is a handy application generator that will create for you a Spring Boot (that’s the Java part) and AngularJS (that’s the hipster part) application.

#### What Is JHipster?
A tool to generate a complete and modern Web app

#### a Yeoman generator
Yeoman workflow is: (1) `yo` to scaffold out application with Maven/Gradle build config (2) `Gulp & Grunt` to automate repetitive tasks with Javascript, and (3) `npm and bowman` package managers for dependency management.

#### a Spring Boot
Build a high-performance and robust Java stack on the server side with Spring Boot.

#### an Angular project
Modern, mobile-first front-end with Angular and Bootstrap.

#### Details:
- can include an extensive set of Spring technologies; Spring Boot, Spring Security, Spring Data, Spring MVC, Websockets, REST
- creates a set of pre-defined screens for user management, monitoring, and logging
- specifically tailored to make working with Angular.js a smoother experience
- you will need to add custom coding
- ultimately just simple Spring Boot-based Maven and Gradle-based projects that can be imported into any IDE that knows about Maven (or Gradle) and Java.

### [Installing JHipster](https://jhipster.github.io/installation/)
- Install Node.js from the [Node.js website](http://nodejs.org/) (prefer an LTS version “Long Term Support”). This will also install npm.  #Why LTS? “..to reduce risk, expense, and disruption of software deployment”
  # `Node.js` a JavaScript runtime built on Chrome's V8 JavaScript engine
- Install Yeoman: npm install -g yo   #[yeoman](http://yeoman.io/) is a web scaffolding tool
- Install Bower: npm install -g bower  #[bower](https://bower.io/) is a package manager for the web
- check first:
$ which gulp
- Install Gulp: npm install -g gulp-cli  #(If you have previously installed a version of gulp globally, please run npm rm -g gulp to make sure your old version doesn’t collide with gulp-cli)
  #[Gulp](http://gulpjs.com) is a streaming build system; a task/build runner for development. Gulp uses Node streams.
- Install JHipster: npm install -g generator-jhipster


**Create an application** named GameSales:  
$ cd ~/workspace  
$ mkdir GameSales  
$ cd GameSales  
$ yo jhipster  

NOTE: If you selected “Java 8” instead of Java 7, you will have lambda expressions in the generated code.

Now just answer questions:
- choose either monolithic or microservices app  (try monolithic 1st)
- enter PROJECT_NAME
- package name: com.theironyard.jastrow
- Authentication: stateful
- Social login: no (skip for now)
- Database type: SQL
- Database engine Production: PostgresSQL
- Database engine Development: H2
- Hibernate cache: yes
- Build tool: Gradle
- Social / Search / etc: skip
- CSS preprocessor: no
- internationalization: yes
- native language: English
- add’l languages: French and Japanese
- testing framework: Gatling (or perhaps Cucumber later)
- Clustered HTTP: no
- Websockets: no  
**Time for a break?** (installation may take awhile)

Look in directory for Gradle files and run:  
$ ./gradlew bootrun

Sign in with Login/Password: user/user

Break out with ^C and create / define data:  
$ yo jhipster:entity customer

Add fields: 
- String firstName
- String lastName (Required with minimum length 3)
- LocalDate effectiveStartDate (Required)
- Add another field: no
- Relationships: no
- DTO: no
- service classes: no
- pagination: yes with infinite scroll
- Overwrite files: yes to all..

Take a look at the Entity info in the JSON file:  
$ cat .jhipster/Customer.json

Now run it:  
$ ./gradlew bootrun

- Log in as: user / user
- Create new customer with the form.
- Log out
- Log in as: admin / admin
- Create a new customer using the REST Service API:
  - POST 
  - Model Schema
  - remove ID
  - edit pre-populated field data
  - Click “Try it out!” button
  - Look at: Response Body & Response Code .. should be 201 (Created)

Now close with ^C and with IntelliJ open file: build.gradle  
- Edit file: src/main/webapp/i18n/en/home.json
- change "title", "subtitle"
- Edit the file: src/main/java/com/theironyard/jastrow/web/rest/CustomerResource.java
- you can see the routes like: getAllCustomers()

If you want to call the APIs using Postman:
- Regenerate the app WITHOUT Basic Auth
- Or, Regenerate the app using OAuth2 (might be tricky)
- OAuth2 is a stateless security mechanism. Spring Security provides an OAuth2 implementation. This solution uses a secret key, which should be configured in your application.yml file, as the “authentication.oauth.secret” property.


## README.md for more info
And find documentation and help at [https://jhipster.github.io/documentation-archive/v3.9.0](https://jhipster.github.io/documentation-
archive/v3.9.0).


### Building for production

To optimize the jhipsterSampleApplication application for production, run:

    ./mvnw -Pprod clean package

This will concatenate and minify the client CSS and JavaScript files. It will also modify `index.html` so it references these new files.
To ensure everything worked, run:

    java -jar target/*.war

Then navigate to [http://localhost:8080](http://localhost:8080) in your browser.


### Testing

To launch your application's tests, run:

    ./mvnw clean test

### Client tests

Unit tests are run by Karma and written with Jasmine. They're located in `src/test/javascript/` and can be run with:

    gulp test

UI end-to-end tests are powered by [Protractor][], which is built on top of WebDriverJS. They're located in `src/test/javascript/e2e`
and can be run by starting Spring Boot in one terminal (`./mvnw spring-boot:run`) and running the tests (`gulp itest`) in a second one.
### Other tests

Performance tests are run by Gatling (written in Scala). They're located in `src/test/gatling` and can be run with:

    ./mvnw gatling:execute

<hr/>

see also: [Bootstrapping Your Microservices Architecture with JHipster and Spring](https://blog.heroku.com/bootstrapping_your_microservices_architecture_with_jhipster_and_spring)

