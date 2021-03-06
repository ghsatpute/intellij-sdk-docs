---
title: 7. Annotator
---
<!-- Copyright 2000-2020 JetBrains s.r.o. and other contributors. Use of this source code is governed by the Apache 2.0 license that can be found in the LICENSE file. -->

An [Annotator](/reference_guide/custom_language_support/syntax_highlighting_and_error_highlighting.md#annotator) helps highlight and annotate any code based on specific rules.
This section adds annotation functionality to support the Simple Language in the context of Java code.

* bullet list
{:toc} 

## 7.1. Define an Annotator
The `SimpleAnnotator` subclasses [`Annotator`](upsource:///platform/analysis-api/src/com/intellij/lang/annotation/Annotator.java).
Consider a literal string that starts with "simple:" as a prefix of a Simple Language key.
It isn't part of the Simple Language, but it is a useful convention for detecting Simple Language keys embedded as string literals in other languages, like Java.
Annotate the `simple:key` literal expression, and differentiate between a well-formed vs. an unresolved property: 

```java
{% include /code_samples/simple_language_plugin/src/main/java/org/intellij/sdk/language/SimpleAnnotator.java %}
```

## 7.2. Register the Annotator
Using the `com.intellij.annotator` extension point in the plugin configuration file, register the Simple Language annotator class with the IntelliJ Platform:

```xml
  <extensions defaultExtensionNs="com.intellij">
    <annotator language="JAVA" implementationClass="org.intellij.sdk.language.SimpleAnnotator"/>
  </extensions>
```

## 7.3. Run the Project
As a test, define the following Java file containing a Simple Language `prefix:value` pair:

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("simple:website");
    }
}
```

Open this Java file in an IDE Development Instance running the `simple_language_plugin` to check if the IDE resolves a property: 

![Annotator](img/annotator.png){:width="800px"}

If the property is an undefined name, the annotator flags the code with an error.

![Unresolved property](img/unresolved_property.png){:width="800px"}

Try changing the Simple Language [color settings](/tutorials/custom_language_support/syntax_highlighter_and_color_settings_page.md#run-the-project-1) to differentiate the annotation from the default language color settings.