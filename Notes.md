# Project Structure: A Maven Multi-Module Guide

This document explains the structure of this `quarkus-langchain4j-workshop` project and provides a simple guide to creating your own multi-module projects using Maven.

---

### Understanding This Project's Structure

This project is a **Maven Multi-Module Project**. Think of it like a big container that holds several smaller, related sub-projects. This is a common setup for large applications or, in this case, a workshop with sequential steps.

Here’s the breakdown:

1.  **The Parent/Aggregator (`pom.xml` in the root folder):**
    *   The `pom.xml` file in the main directory (`/mydata/codes/2025/quarkus-langchain4j-workshop/pom.xml`) is the "parent" or "aggregator" POM.
    *   It defines and manages the entire collection of sub-projects (modules).
    *   It contains a special `<packaging>pom</packaging>` tag, which means this POM itself doesn't build into a JAR or WAR file; its job is to manage the modules listed in its `<modules>` section.
    *   It's the perfect place to define common dependencies and versions in a `<dependencyManagement>` section, so all sub-projects use the same versions, ensuring consistency.

2.  **The Modules (`step-01`, `step-02`, etc.):**
    *   Each `step-xx` directory is a self-contained **module** (a sub-project).
    *   Each module has its own `pom.xml` file.
    *   Inside a module's `pom.xml`, you'll find a `<parent>` section that links it back to the main project's POM. This is how it inherits the common configurations.
    *   Each module can have its own unique dependencies and source code.

When you run a Maven command like `mvn clean install` from the root directory, Maven is smart enough to read the parent `pom.xml`, find all the modules, and build them in the correct order.

---

### How to Create Your Own Multi-Module Project (in Layman's Terms)

Let's say you want to create an application with a `core-logic` module and a `web-interface` module.

#### Step 1: Create the Parent Project

1.  Create a new folder for your project (e.g., `my-awesome-app`).
2.  Inside `my-awesome-app`, create a `pom.xml` file. This will be your parent POM.

**`my-awesome-app/pom.xml`:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Your project's coordinates -->
    <groupId>com.example</groupId>
    <artifactId>my-awesome-app</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <!-- VERY IMPORTANT: This tells Maven it's a parent/container -->
    <packaging>pom</packaging>

    <name>My Awesome App (Parent)</name>

    <!-- List your sub-folders (modules) here -->
    <modules>
        <module>core-logic</module>
        <module>web-interface</module>
    </modules>

</project>
```

#### Step 2: Create the Sub-Projects (Modules)

1.  Inside `my-awesome-app`, create the folders you listed in the `<modules>` section: `core-logic` and `web-interface`.
2.  Each of these folders gets its own `pom.xml` and a `src` directory.

**`my-awesome-app/core-logic/pom.xml`:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Link back to the parent POM -->
    <parent>
        <groupId>com.example</groupId>
        <artifactId>my-awesome-app</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>

    <!-- This module's specific artifactId -->
    <artifactId>core-logic</artifactId>

    <name>Core Logic</name>
    <!-- Default packaging is 'jar', which is what we want -->

    <dependencies>
        <!-- Add dependencies needed ONLY for core-logic -->
    </dependencies>

</project>
```

**`my-awesome-app/web-interface/pom.xml`:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Link back to the parent POM -->
    <parent>
        <groupId>com.example</groupId>
        <artifactId>my-awesome-app</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>

    <!-- This module's specific artifactId -->
    <artifactId>web-interface</artifactId>
    <packaging>war</packaging> <!-- Example: this could be a web application -->

    <name>Web Interface</name>

    <dependencies>
        <!-- This module depends on our core-logic module -->
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>core-logic</artifactId>
            <version>${project.version}</version> <!-- ${project.version} is inherited from parent -->
        </dependency>

        <!-- Add other dependencies needed for the web interface -->
    </dependencies>

</project>
```

#### Step 3: Build Your Project

-   Navigate to the root folder (`my-awesome-app`) in your terminal.
-   Run `mvn clean install`.
-   Maven will first build `core-logic` (because `web-interface` depends on it) and then build `web-interface`. The resulting `.jar` and `.war` files will be in the `target` folder of each respective module.

That's it! You've created a multi-module project. This approach keeps your code organized, reusable, and much easier to manage as it grows.
