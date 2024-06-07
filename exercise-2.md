# Exercise 2 - Modernize application with OpenRewrite 

1. 
Checkout Spring Petclinic at <TODO: set branch matching solution after previous exercise>

`git checkout <branch>`


2. 
Run once to see version:

`mvn spring-boot:run`

We see its running Spring 2.7.x.

3. 
Add Rewrite Plugin Dependency and Recipe configuration in the pom.xml under <plugins>.

```xml
      <plugin>
        <groupId>org.openrewrite.maven</groupId>
        <artifactId>rewrite-maven-plugin</artifactId>
        <version>5.27.0</version>
        <configuration>
          <activeRecipes>
            <recipe>org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_2</recipe>
          </activeRecipes>
          <activeStyles>
            <style>org.openrewrite.java.SpringFormat</style>
          </activeStyles>
        </configuration>
        <dependencies>
          <dependency>
            <groupId>org.openrewrite.recipe</groupId>
            <artifactId>rewrite-spring</artifactId>
            <version>5.7.0</version>
          </dependency>
        </dependencies>
      </plugin>
```

Note: We added the SpringFormat style, so that OpenRewrite automatically formats in the way Spring expects it. This solves formatting issues after the transformation.

Then run recipe with:

`mvn rewrite:run`

4.

Now let’s verify, if everything works properly:

`mvn verify` 

Oh no, we see an error!

Check error with IDE and fix it.

### Solution:

The XML Binding Dependency is missing as the JavaEE Bindings were deprecated after Java 11. 

We need to add the dependency to the pom: 

```xml
<dependency>
  <groupId>jakarta.xml.bind</groupId>
  <artifactId>jakarta.xml.bind-api</artifactId>
  <version>4.0.2</version>
</dependency>

```

5.

Verify again:

`mvn verify` 

→ No errors


Now execute to see if its actually Spring Boot 3:

`mvn spring-boot:run`

We see Spring 3.x.