# gradle-basics
Understanding basic of gradle

# **Gradle Build System Overview**

## **1. Understanding Build Scripts in Gradle**

Gradle provides a flexible build system, and the build configuration is defined in the `build.gradle` file. There are two possible variants of this file:

- **Groovy-based**: `build.gradle`
- **Kotlin-based**: `build.gradle.kts`

The `build.gradle` file is present inside the `app` folder because it is generated when we select the application type during project setup. It serves as the configuration file that defines how Gradle will build, test, and package the application.

---

## **2. Plugins Section**

### **What Are Plugins?**

The **plugins section** in Gradle allows you to extend its functionality. Plugins introduce additional capabilities such as Java compilation, Kotlin support, and code coverage.

### **Example Plugins Section**

```kotlin
plugins {
    id("java") // Enables Java support
    id("org.jetbrains.kotlin.jvm") // Enables Kotlin support
}
```

### **Understanding `id` Keyword**

- The `id` keyword specifies the plugin to be applied.
- Core Gradle plugins (e.g., `java`) are built into Gradle.
- External plugins (e.g., `org.jetbrains.kotlin.jvm`) are provided by third-party developers.

### **Difference Between Core and External Plugins**

| Type | Example | Description |
|------|---------|-------------|
| **Core Plugins** | `id("java")` | Provided by Gradle by default |
| **External Plugins** | `id("org.jetbrains.kotlin.jvm")` | Must be added via repositories |

---

## **3. Difference Between Gradle Plugins and Dependencies**

### **Plugins vs. Dependencies**

| Aspect | Plugins | Dependencies |
|--------|---------|-------------|
| Purpose | Extends Gradle functionality | Adds external libraries to the project |
| Affects Build? | Yes | No |
| Example | `id("jacoco")` | `implementation("org.junit.jupiter:junit-jupiter")` |

---

## **4. Understanding Plugin Aliases and Configuration**

Gradle allows the use of **aliases** to manage plugin versions centrally via `libs.versions.toml`.

```kotlin
plugins {
    alias(libs.plugins.org.jetbrains.kotlin.jvm)
    alias(libs.plugins.org.jetbrains.kotlin.plugin.spring)
    alias(libs.plugins.org.jlleitschuh.gradle.ktlint) apply false
}
```

- `apply false`: Declares the plugin but does not apply it automatically.
- `alias(libs.plugins.org.jetbrains.kotlin.jvm)`: Fetches the plugin from a centralized dependency catalog.
- Using aliases helps in managing multiple versions of plugins effectively.

---

## **5. Repositories and Dependencies**

### **Repositories Section**

Repositories tell Gradle where to fetch dependencies from.

```kotlin
repositories {
    mavenCentral()
    google()
}
```

### **Dependencies Section**

Dependencies define which external libraries are used in the project. Gradle downloads them and places them in the local project archive.

```kotlin
dependencies {
    implementation("com.google.guava:guava:29.0-jre")
    testImplementation("junit:junit:4.13")
}
```

Each project should have at least one section for **testing dependencies**.

---

## **6. Application Section (Defining Main Class)**

Gradle’s `application` plugin allows specifying the **main class** for the application.

```kotlin
application {
    mainClass.set("com.example.Main")
}
```

If this section is missing, Gradle will not know the application entry point when running or packaging the JAR.

---

## **7. Understanding `gradle jar` and `gradle clean`**

### **Generating JAR Files**

```sh
./gradlew jar
```

- This command creates a JAR file and stores it in `app/build/libs`.

### **Cleaning Build Artifacts**

```sh
./gradlew clean
```

- Removes all generated artifacts, ensuring a fresh build.

### **Running the JAR File**

```sh
java -jar app/build/libs/app.jar
```

This will **fail** if `MANIFEST.MF` does not specify the main class. To fix this, add the following:

```kotlin
tasks.jar {
    manifest {
        attributes("Main-Class" to "org.hyperskill.gradleapp.AppKt")
    }
}
```

Once this is added, running the JAR should work correctly.

---

This document provides a comprehensive overview of Gradle build scripts, including **plugins, dependencies, application configuration, and JAR generation**. Let me know if you need any modifications! 🚀


