# openrewrite-no_version_provided

This project demonstrates a Maven multi-module setup involving `dependencyManagement` with `test-jar` dependencies, created to investigate and document a diagnostic limitation in the OpenRewrite Maven Plugin (version 6.8.0).

Although the original issue could not be fully reproduced with this demo, it captures the structural patterns and use cases relevant to the encountered behavior. The repository serves as a minimal reference for discussion and improvement of error diagnostics.

## 🧱 Project Structure

```

openrewrite-test/
├── pom.xml               # Aggregator POM (declares all modules)
├── props/                # Provides <properties> like flux.version
├── parent/               # Contains <dependencyManagement> (test-jar)
└── consumer/             # Uses test-jar dependency

```

## 💡 Problem Background

In a different real-world project, the following message was produced by OpenRewrite, but without revealing which dependency caused the error:

```xml
<!--~~(No version provided for direct dependency)~~>-->

[ERROR] Failed to execute goal org.openrewrite.maven:rewrite-maven-plugin:6.8.0:run (default-cli) on project demo-container-core:
Failed to parse or resolve the Maven POM file or one of its dependencies;
We can not reliably continue without this information.
[ERROR] <!--~~(No version provided for direct dependency)~~>--><?xml version="1.0" encoding="UTF-8"?>
[ERROR] <project xmlns="http://maven.apache.org/POM/4.0.0"
[ERROR]          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
[ERROR]          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
[ERROR]     <modelVersion>4.0.0</modelVersion>
[ERROR]     <groupId>com.example.enterprise</groupId>
[ERROR]     <artifactId>enterprise-database-plugins</artifactId>

[ERROR]     <parent>
[ERROR]         <groupId>com.example.enterprise</groupId>
[ERROR]         <artifactId>enterprise-core</artifactId>
[ERROR]         <version>2.0.0-master-SNAPSHOT</version>
[ERROR]     </parent>

[ERROR]     <dependencies>
[ERROR]         <dependency>
[ERROR]             <groupId>com.example.maven-extensions</groupId>
[ERROR]             <artifactId>enterprise-maven-exts</artifactId>
[ERROR]             <type>pom</type>
[ERROR]         </dependency>
[ERROR]         <dependency>
[ERROR]             <groupId>com.example.enterprise</groupId>
[ERROR]             <artifactId>enterprise-utils</artifactId>
[ERROR]             <version>${project.version}</version>
[ERROR]         </dependency>
```

This placeholder was embedded in the serialized XML content of a `pom.xml`, making it impossible to identify the problematic dependency — despite the plugin clearly having access to that data at the time of failure.

## 🔧 Plugin Invocation

The Rewrite plugin is executed from the CLI as follows:

```bash
mvnw \
  org.openrewrite.maven:rewrite-maven-plugin:6.8.0:run \
  -Drewrite.activeRecipes=EAP80
```

## 🖥 Environment

* **Java:** 17
* **Maven:** 3.9.9
* **OS:** Windows 11
* **OpenRewrite Plugin:** 6.8.0

## 🚧 Known Limitation

This project **does not trigger the actual error**, but reflects the structure and resolution model relevant for scenarios involving:

* `test-jar` dependencies
* `<dependencyManagement>` inheritance
* property-based versions

This structure has caused Rewrite failures in enterprise scenarios.

## 🐞 Related GitHub Issue

The following issue has been submitted to suggest improvements to OpenRewrite's error reporting:

👉 [OpenRewrite](https://github.com/openrewrite/rewrite-maven-plugin/issues/983)
