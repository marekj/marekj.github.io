---
layout: post
title: Getting started with Java, Maven, Eclipse
---
Opinionated way of setting up Eclipse, Maven, jUnit etc... for TDD style learning of Java 8.

The tutorial is quite primitive. A draft. I hope to point you to the right direction in setting up your eclipse for the first time and getting java going TDD Style.

## Install Eclipse

Download Eclipse Neon IDE for Java EE Developers (on Mac OS X). This was [recommended by Marty Hall in Java 8 crash course](https://www.safaribooksonline.com/library/view/learning-modern-java/9780134383613/)


## Install CleanSheets Eclipse Theme

http://fappel.github.io/xiliary/

Frank Appel wrote a great book ["Testing with JUnit"](https://www.packtpub.com/application-development/testing-junit) and he is also one of the authors of CleanSheets Eclipse theme.

## Install Google Java style

Coming from Ruby I am used to 2 space indentation, however natively Eclipse uses tabs at 4 and 8 indentation.
I have found out that [Google Java style guide](https://github.com/google/styleguide) has a eclipse xml file you can install to fix that. Download it: https://github.com/google/styleguide/blob/gh-pages/eclipse-java-google-style.xml and import it in Preferences Java Code Style Formatter and it will be your profile

## Maven

Eclipse comes with Maven. If you are coming from Ruby I can say that Maven is like 3 things in Ruby in one package: gem ,bundler and rake.

## Create Maven Java Project

Eclipse provides you with a way to create a new project (DO NOT USE IT) if you select 'File > New > Java Project'

Use Maven instead...

In Eclipse select 'File > New > Other' and find a Maven Project wizard.

Check box check 'Create a simple project (skip archetype selection)'. We don't want that. Why? cause it sucks. Unless you know exactly the archetype you want to use as a template. For now just create a file structure and a pom.xml file.

If all of this confuses you go read [Maven Eclipse tutorial from Vogella](http://www.vogella.com/tutorials/EclipseMaven/article.html)

OK, back to the dialog for new maven project: you need groupId and artifactId: Enter 'personal.example' for groupId and 'personal.example.learn' for artifactId

And you got a project: it's a folder named "personal.example.learn" that has a convention based file structure with pom.xml and src/main/java and src/test/java folders

### Add Junit dependency to your pom.xml

In Project Explorer view double click pom.xml. You should be looking at Overview tab of your pom file. Click dependencies tab and click Add button. For groupId enter 'junit', for artifactId eneter 'junit'  and for version '4.12', click OK and SAVE file.


## Create a JUnit Test Case

Select 'File > New > Other' and search for JUnit Test Case

Make our source folder "personal.example.learn/src/test/java" (notice Eclipse suggest src/main/java which is wrong)

Your package "personal.example.learn" (it's just a folder structure if you are confused about packages) and enter name 'ExampleTest'

for Class under test don't select anything

ok , click Finish. (you may be asked to add Junit 4 to the build path. Yes, do it)

You should see a file created with the following content


```java
package personal.example.learn;

import static org.junit.Assert.*;

import org.junit.Test;

public class ExampleTest {

  @Test
  public void test() {
    fail("Not yet implemented");
  }

}
```

click 'run' button and you should have a failing test. Congrats.


## Install Infinitest plugin

https://infinitest.github.io/

When you make changes and save the tests will run automatically on save.
