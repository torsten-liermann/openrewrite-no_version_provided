Hier ist das aktualisierte `README.md` für dein GitHub-Demo-Projekt, angepasst an die tatsächliche Projektstruktur mit den Modulen `props`, `parent` und `consumer`, wie aus der Analyse der `pom.xml`-Dateien hervorgeht:

---

## 📘 `README.md`

```markdown
# openrewrite-no_version_provided

This project demonstrates a Maven multi-module setup involving `dependencyManagement` with `test-jar` dependencies, created to investigate and document a diagnostic limitation in the OpenRewrite Maven Plugin (version 6.8.0).

Although the original issue could not be fully reproduced with this demo, it captures the structural patterns and use cases relevant to the encountered behavior. The repository serves as a minimal reference for discussion and improvement of error diagnostics.

---

## 🧱 Project Structure

```

openrewrite-test/
├── pom.xml               # Aggregator POM (declares all modules)
├── props/                # Provides <properties> like flux.version
├── parent/               # Contains <dependencyManagement> (test-jar)
└── consumer/             # Uses test-jar dependency

````

---

## 💡 Problem Background

In a different real-world project, the following message was produced by OpenRewrite, but without revealing which dependency caused the error:

```xml
<!--~~(No version provided for direct dependency)~~>-->
````

This placeholder was embedded in the serialized XML content of a `pom.xml`, making it impossible to identify the problematic dependency — despite the plugin clearly having access to that data at the time of failure.

---

## 🔧 Plugin Invocation

The Rewrite plugin is executed from the CLI as follows:

```bash
\apache-maven-3.9.9\bin\mvn \
  org.openrewrite.maven:rewrite-maven-plugin:6.8.0:run \
  -Drewrite.configLocation=https://raw.githubusercontent.com/torsten-liermann/openrewrite-pom-manipulation/refs/heads/main/rewrite.yml \
  -Drewrite.activeRecipes=EAP80
```

---

## 🖥 Environment

* **Java:** 17
* **Maven:** 3.9.9
* **OS:** Windows 11
* **OpenRewrite Plugin:** 6.8.0

---

## 🚧 Known Limitation

This project **does not trigger the actual error**, but reflects the structure and resolution model relevant for scenarios involving:

* `test-jar` dependencies
* `<dependencyManagement>` inheritance
* property-based versions

This structure has caused Rewrite failures in enterprise scenarios.

---

## 🐞 Related GitHub Issue

The following issue has been submitted to suggest improvements to OpenRewrite's error reporting:

👉 *\[Link to GitHub Issue – to be added]*
