 A basic Spring Boot application
===============================

This tutorial will show you how to build a basic web application backed by an H2 database, using Spring Boot.

The technologies we will use are:

* `Spring Boot <http://projects.spring.io/spring-boot/>`_: a library which helps build `Spring <https://projects.spring.io/spring-framework/>`_ applications by setting up the basics in a standardized way.
* `Maven <https://maven.apache.org/>`_: a Java build system. It gives you a standardized way to structure a Java project, locating dependencies and building, by using a declarative build definition.
* `H2 <http://h2database.com/>`_: a Java embedded relational database. It lets you incorporate an SQL database to your application without installing a separate piece of software.
* `Thymeleaf <http://www.thymeleaf.org/>`_: an HTML templating system.

The instructions are for following the tutorial using the `Eclipse IDE <http://www.eclipse.org/>`_ (tested on the Neon.2 version).

Building the basic project
--------------------------

Spring Boot provides the http://start.spring.io/ website, which builds an empty project for you, simplifying greatly the setup process.

 Fill in the following information:

* Group: this is the Maven identifier for the "party" which owns the code. Technically, this should be an Internet domain name reversed (e.g.: `com.example` for the `example.com` domain). If you do not own a suitable domain name, write something you fancy in dotted notation such as `foo.bar` [#groupdomain]_.
* Artifact: this is the Maven identifier for the project. Artifact names should be unique within a Group, for very much the same reasons.
* Dependencies: add `Web`, `JDBC`, `Thymeleaf`, `H2`. Cuttingtand pasting won't work; you need to introduce them one by one and ensure that they appear below in the section "Selected Dependencies".

Then click on "Generate Project". This will prompt you to download a zip file which will contain your skeleton project.

You then should unzip this project somewhere in your system and import it to Eclipse. In general, Eclipse creates new projects within the user's `workspace` directory, which in Windows is located in `C:\\Users\\<username>\\workspace`. If you wish, unzip it there; you should now have a file with full path `C:\\Users\\<username>\\workspace\\<artifact name>\\pom.xml` among other files.

Now, to add this project to Eclipse, you will need to select the "File", "Import..." menu option, choose "Existing Maven Projects" from the "Maven" section and select the folder where the `pom.xml` file resides as the "Root Directory". A checkbox next to `pom.xml` should appear- ensure it is checked. Click on "Finish", and your project should appear in the "Package Explorer".

This will take a while, as Eclipse sets up the project and downloads all dependencies. When Eclipse stops doing things, you should locate a `<Artifact>Application.java` file within `src/main/java`, right-click on it, choose "Run As", "Java Application" and it will be started. The Console view in Eclipse should show a bunch of text and one of the last lines should be::

    2017-01-22 13:18:57.811  INFO 9008 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)

, this means that the base application has been started and tells you which TCP port it's listening at. You should go into your browser and visit http://localhost:8080 to see it working. At the moment this should print an error page as your application does not have any content yet.

Let's have a quick overview of the project as viewed in Eclipse's package explorer:

* `<Artifact>`: your project will be named like the Artifact name you chose in the Spring Initializr website

  * `src/main/java`: this is the folder where your application's main code resides

    * `group.name`: a Java package matching the group name you chose

      * `<Artifact>Application`: a Java class which is the main entry point of your application, whose name is based on the Artifact name you chose

  * `src/main/resources`: a folder which contains non-Java files useful for your application, we will use it below
  * `src/test/java`: a folder where Java test classes should reside. We will not use this in this tutorial, but you can put tests here and Maven and Eclipse will be able to execute them
  * "JRE System Library": a folder where you can browse the libraries included in the Java Standard Platform
  * "Maven Dependencies": a folder where you can browse the libraries pulled in by Maven
  * "src": Eclipse shows you the interesting bits of this folder above; you should basically ignore this folder for now
  * "target": where Maven will put compiled classes and other artifacts; this should also be ignored
  * `mvnw`, `mvnw.cmd`: some scripts to run Maven from a Unix shell or Windows command prompt. We will run Maven with Eclipse so you can also ignore them
  * `pom.xml`: `Maven's "Project Object Model" <https://maven.apache.org/pom.html#What_is_the_POM>`_, the file which defines your project; which dependencies it has, etc.

Adding some simple content to `/`
---------------------------------

Let's remove the error page and make your application show something. Edit your `Application` class, which should look a bit like::

    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class DemoApplication {

            public static void main(String[] args) {
                    SpringApplication.run(DemoApplication.class, args);
            }
    }

edit it so::

    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;

    @SpringBootApplication
    @Controller
    public class DemoApplication {

            @RequestMapping("/")
            String home() {
                    return "index";
            }
            
            public static void main(String[] args) {
                    SpringApplication.run(DemoApplication.class, args);
            }
    }

, adding:

* A `@Controller` annotation and its import
* The `home` method annotated with `@RequestMapping`.

What it does is indicate Spring that this class is a Controller, meaning a class which specifies how certain requests should be handled. The method `home` is mapped to the `/` URI and what it does is that when `/` is served by the application, the template named `index` should be rendered.

Let's create the `index` template in `src/main/resources/templates` as `index.html`::

    <html>
            <body>
                    Hello, world
            </body>
    </html>

. If you re-run your application now and reload your browser, the HTML above should be shown back to you.

Additional exercises
~~~~~~~~~~~~~~~~~~~~

Map another URI which renders a different template and make `/` contain a link to this URI.

Creating a simple database schema
---------------------------------

Let's make this application have a database.

Create a `schema.sql` file within the `src/main/resources` folder::

    CREATE TABLE cats (
            id                       serial PRIMARY KEY,
            name                     varchar(100)
    );

and a `data.sql` file within the same folder::

    INSERT INTO cats (name) VALUES ('Cleo');
    INSERT INTO cats (name) VALUES ('Luna');

. With the current setup, each time you run the application, Spring Boot will create a throwaway H2 database, run the `schema.sql` SQL script to create your database schema and `data.sql` to fill it with sample data. Note that this is done on startup, so any changes you make to the database will be lost when you restart!

Modifying `/` so it shows information from the database
-------------------------------------------------------

In this step we will display the information from the database in the web application.

Modify your `Application` class like this::

    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.jdbc.core.JdbcTemplate;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.ModelMap;
    import org.springframework.web.bind.annotation.RequestMapping;

    @SpringBootApplication
    @Controller
    public class DemoApplication {

            private JdbcTemplate jdbcTemplate;

            @Autowired
            public DemoApplication(JdbcTemplate jdbcTemplate) {
                    this.jdbcTemplate = jdbcTemplate;
            }
            
            @RequestMapping("/")
            String home(ModelMap model) {
                    model.addAttribute("cats", jdbcTemplate.queryForList("select * from cats"));
                    return "index";
            }
            
            public static void main(String[] args) {
                    SpringApplication.run(DemoApplication.class, args);
            }
    }

, adding:

* An `@Autowired` constructor which receives a `JdbcTemplate` and stores it in a field. `JdbcTemplate` is an object which provides methods to access your application's database. By autowiring it, you are requesting Spring to set up one configured to access the database you created in the previous step and provide it to you via constructor. This is stored in a field so it can be used from other methods.
* Extend the `home` method to receive a `ModelMap` object. `ModelMap` is the class used to feed the template data
* Use the `jdbcTemplate` field to execute a simple SQL query and put it in the `model` to be used by the template.

Then modify your `index.html` template to look like this::

    <html xmlns:th="http://www.thymeleaf.org">
            <body>
                    Hello, world

                    <ul>
                            <li th:each="cat : ${cats}" th:text="${cat.name}">Cat name</li>
                    </ul>		
            </body>
    </html>

* First set up the `th` Thymeleaf namespace; this is required for Thymeleaf to work correctly
* Add an *u* nordered *l* ist tag
* Generate *l* ist *i* tem tags by iterating over the `cats` model attribute, assigning each `cats` element to the `cat` variable. Replace each `li`'s text with `cat`'s `name` attribute.

Adding cats
-----------

Now let's create a form to add new cats. Modify your `index.html` template like this::

    <html xmlns:th="http://www.thymeleaf.org">
            <body>
                    Hello, world

                    <ul>
                            <li th:each="cat : ${cats}" th:text="${cat.name}">Cat name</li>
                    </ul>

                    <form method="POST" action="/add">
                            <label>New cat name <input type="text" name="name"/></label>
                            <input type="submit"/>
                    </form>		
            </body>
    </html>

, adding a `form` which `POST`s a cat name to the  `/add` URI. To handle it, add a new request mapping method to your `Application` class::

	@RequestMapping("/add")
	String add(String name) {
		jdbcTemplate.update("insert into cats(name) values (?)", name);
		return "redirect:/";
	}

, which simply inserts the data posted and redirects to `/` again.

Additional exercises
--------------------

* Add a functionality to edit an existing cat's name
* Add a functionality to delete cats
* Add new attributes to cats, such as birth date, weight, etc.

.. [#groupdomain] This is used to avoid collisions. If everyone uses a domain they own, no two projects will ever have the same group name and thus the Group can be used as a namespacing identifier. It is also used to create a package name for the project, which has the same "non-collision" requirements.
