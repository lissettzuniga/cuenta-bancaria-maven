# Errores de Maven que encontré

| Queja | La línea clave que dijo Maven | La causa | Cómo lo arreglé |
|---|---|---|---|
| 1 | `[FATAL] Non-parseable POM pom.xml: end tag name </dependencia> must match start tag name <dependency>` | Etiqueta de cierre XML escrita en español (`</dependencia>`). | Se cambió por `</dependency>`. |
| 2 | `Fatal error compiling: error: release version 21 not supported` | La propiedad `<maven.compiler.release>` especificaba Java 21, no soportado por el JDK activo. | Se cambió el valor a `17`. |
| 3 | `Could not resolve dependencies [...] com.fasterxml.jackson.core:jackson-databind:jar:2.22.30 was not found` | La versión de Jackson en `<properties>` tenía un cero de más (`2.22.30`). | Se corrigió la versión a `2.22.3`. |
| 4 | `package com.fasterxml.jackson.core does not exist` | La dependencia de Jackson tenía asignado el alcance `<scope>test</scope>`, no disponible en `src/main`. | Se eliminó la etiqueta `<scope>` de Jackson. |
| 5 | `Error: Could not find or load main class com.academia.banco.Aplicacion` | El `maven-jar-plugin` apuntaba a una clase principal inexistente (`Aplicacion`). | Se corrigió `<mainClass>` a `com.academia.banco.App`. |