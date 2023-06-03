---
title: "Journey Into Spring Framework 1"
seoTitle: "Java Spring & Spring Boot"
seoDescription: "Mastering Java Spring & Spring Boot"
datePublished: Sat Jun 03 2023 06:47:11 GMT+0000 (Coordinated Universal Time)
cuid: clifmt4v8000m0amehq62dvel
slug: journey-into-spring-framework-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685026512057/5024150a-f36b-4828-9002-e054ed4062e7.png
tags: spring, springboot, spring-framework

---

The best way to create a spring project is to go to the spring initializer &lt;[https://start.spring.io](https://start.spring.io/)\&gt;. Over here you can give a name to your project. Select the frameworks you want to make use of and create a project quickly to get started. I have used Maven as the build tool, the language is Java 17.0.2 and Spring Boot 3.1.0

We add a few lines of code to the boilerplate to make [A simple Java Application](https://github.com/prasunchakra/Journey-into-Spring-Framework/tree/f465b4588e5402550dda3f18e94985c54614f09a) ([https://github.com/prasunchakra/Journey-into-Spring-Framework/tree/f465b4588e5402550dda3f18e94985c54614f09a](https://github.com/prasunchakra/Journey-into-Spring-Framework/tree/f465b4588e5402550dda3f18e94985c54614f09a))

Here we have four classes for four tournaments *Australian Open*, *French Open*, *Wimbledon* and *US Open*. They all implement the *GrandSlam* interface. A Class *TennisTournament* manages all the tournaments on passing an object of grandSlam type and in the main method, we create an object of the TennisTournament class with one of the four grandSlam classes and call the run method of the TennisTournament class. With this example, we will see how and where spring comes into play.

```java
public interface GrandSlam {
    void founded();
    void timeOfYear();
    void location();
    void venue();
    void prizeMoney();
}

public class Wimbledon implements GrandSlam{
    public void founded(){
        System.out.println(1877);
    }
    public void timeOfYear(){
        System.out.println("late June");
    }
    public void location(){
        System.out.println("London, England, United Kingdom");
    }
    public void venue(){
        System.out.println("All England Lawn Tennis and Croquet          Club");
    }
    public void prizeMoney(){
        System.out.println("£40,350,000  / 4,11,74,18,910.00 Indian     Rupee");
    }
}
```

```java
public class TennisTournament {
    GrandSlam grandSlam;
    public TennisTournament(GrandSlam grandSlam) {
        this.grandSlam = grandSlam;
    }
    public void run() {
        System.out.println("RUNNING Tennis Tournament");
        grandSlam.founded();
        grandSlam.timeOfYear();
        grandSlam.location();
        grandSlam.venue();
        grandSlam.prizeMoney();
    }
}
```

```java
public class Tennis {
    public static void main(String[] args) {
            var tennisGame = new Wimbledon();
//            var tennisGame = new FrenchOpen();
//            var tennisGame = new USOpen();
//            var tennisGame = new AustralianOpen();
            var tennisTournament = new TennisTournament(tennisGame);
            tennisTournament.run();
    }
}
```

So here we are creating an object of the Wimbledon class, and passing that object to create a class of the TennisTournament class. Now the TennisTournament class needs a tennis game to run, so the TennisTournament is dependent on it or in other words, a tennis game is the **dependency** of TennisTorunament. This you can also call as the tennisGame as a dependency is injected or wired into the TennisTournament class.

While this code looks very here, but when you talk about enterprise applications, you will have thousands of classes and you will have thousands of dependencies that are created and thousands of dependencies that are injected wherever they are needed.

Now let us see how spring framework gets to do that and take the burden from us.

### Enter Spring Beans, Annotations and Auto Wiring.

Before directly jumping into the code, let's first get some understanding of basic spring

**Spring container:** It is the core runtime environment of the Spring Framework that manages the creation, configuration, and lifecycle of objects, also known as beans

**Spring IoC (Inversion of Control)**: It is a design pattern and mechanism in the Spring Framework that allows the container to take control of creating and managing object instances and their dependencies. Instead of the application code being responsible for creating objects, the responsibility is "inverted" to the container, which injects dependencies into objects, resulting in looser coupling and easier configuration.

**Spring bean**: An object managed by the Spring IoC (Inversion of Control) container that provides reusable and configurable components in a Spring application. Beans are created, managed, and wired together by the container, allowing for dependency injection and providing a central mechanism for managing object instances and their lifecycle within the application.

**Spring Annotations:** annotations in the Spring Framework are special markers added to code elements that provide instructions to Spring on how to handle and configure those elements automatically. To start with let's look at three that we will be using.

* **@Configuration**: When applied to a class, it designates it as a configuration class within the Spring application context. In other words, it tells Spring that this class will provide bean definitions and other configuration options. Inside a `@Configuration` class, you can define bean methods using the `@Bean` annotation. These methods are responsible for creating and configuring the beans that will be managed by the Spring container.
    
* **@Component**: It serves as a generic stereotype for any Spring-managed component, indicating that the class is a candidate for auto-detection and registration as a bean. With `@Component`, you can create custom reusable components and allow Spring to handle their instantiation, dependency injection, and lifecycle management.
    
* **@ComponentScan**: The `@ComponentScan` annotation is used to enable component scanning in Spring. Component scanning allows Spring to automatically discover and register Spring-managed components, such as beans, within a specified package or set of packages. `@ComponentScan` is typically used at the configuration class level to specify the base packages to scan for components. It eliminates the need for explicit bean declarations and enables a more streamlined and flexible configuration process.
    

With these in hand let's see how they help remove the burden of the developer.

You will find the updated code here ([https://github.com/prasunchakra/Journey-into-Spring-Framework/tree/7f422c76dc594edd9c0bbc09937c918fabc763a4](https://github.com/prasunchakra/Journey-into-Spring-Framework/tree/7f422c76dc594edd9c0bbc09937c918fabc763a4))

```java
@Configuration
@ComponentScan("com.prasunchakra.journeytospringframework.tennis")
public class Tennis {
    public static void main(String[] args) {
        var context = 
                 new AnnotationConfigApplicationContext(Tennis.class);
        context.getBean(GrandSlam.class).founded();
        context.getBean(TennisTournament.class).run();
    }
}
```

```java
@Component
public class TennisTournament {
    GrandSlam grandSlam;
    public TennisTournament(@Qualifier("gameQualifier") GrandSlam    grandSlam) {
        this.grandSlam = grandSlam;
    }

    public void run() {
        System.out.println("RUNNING Tennis Tournament");
        grandSlam.founded();
        grandSlam.timeOfYear();
        grandSlam.location();
        grandSlam.venue();
        grandSlam.prizeMoney();
    }
}
```

```java
@Component
@Qualifier("gameQualifier")
public class FrenchOpen implements GrandSlam{
    public void founded(){
        System.out.println(1891);
    }
    public void timeOfYear(){
        System.out.println("late May");
    }
    public void location(){
        System.out.println("Paris, France");
    }
    public void venue(){
        System.out.println("Stade Roland Garros ");
    }
    public void prizeMoney(){
        System.out.println("€42,661,000 / 3,78,11,72,413.00 Indian Rupee");
    }
}
```

Here we have added @Component annotation in all the tennis game classes. So, what would happen is an instance of the tennis game class will be created for you by spring. Similarly, @Component annotation is also added to the TennisTournament class such that it is also managed by Spring. Lastly, we added @Configuration and @ComponentScan annotations to the Tennis class containing the main method. The configuration annotation is telling Spring that this class will provide the beans and other configurations to run and the component scan searches the package and create beans of all classes with component annotation in them.

So here we didn't have to create or manage any object, all those are taken care of by the spring framework.

This is just the tip of the iceberg, let's dig deeper into the spring framework in the subsequent series  
[https://blog.prasunchakra.com/series/java-spring](https://blog.prasunchakra.com/series/java-spring)