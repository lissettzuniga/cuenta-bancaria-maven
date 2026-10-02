cat << 'EOF' > README.md
# Cuenta bancaria — Maven

**Autor:** Lissett Zuñiga Reyes

## Cómo construir y correr

```bash
./mvnw clean package
java -jar target/cuenta-bancaria-maven-1.0.0.

## Qué hay en este proyecto

| Qué | Dónde |
|---|---|
| El programa (estado de cuenta en texto y en JSON) | `src/main/java/com/academia/banco/App.java` |
| Las pruebas | `src/test/java/com/academia/banco/` |
| La evidencia de cada mini-práctica | `evidencia/` |
| Los 5 errores de Maven que encontré | `evidencia/errores-maven.md` |

## Boleto de salida

1. **¿Qué diferencia hay entre `./mvnw package` y `./mvnw install`?**
   - `package` ejecuta el ciclo de vida de compilación y pruebas hasta empaquetar el código compilado (ej. archivo `.jar`) dentro del directorio `target/` del proyecto local.
   - `install` ejecuta todas las fases anteriores de `package` y además copia el archivo `.jar` resultante dentro del repositorio local de Maven (`~/.m2/repository/`), haciéndolo disponible para que otros proyectos Java locales puedan usarlo como dependencia.

2. **¿Por qué `compile` pasó pero `java -jar` falló en la MP-3?**
   - `compile` verificó que el código fuente no tuviera errores de sintaxis y que las librerías necesarias (como Jackson) estuvieran presentes en el *classpath* de compilación.
   - Sin embargo, el archivo JAR generado por defecto por `maven-jar-plugin` solo incluye las clases compiladas propias del proyecto (el JAR "flaco"), sin incluir el código de las dependencias externas. Al ejecutar `java -jar`, la Máquina Virtual de Java (JVM) no encontró las clases de Jackson en tiempo de ejecución (`NoClassDefFoundError`), lo que requirió la adición de `maven-shade-plugin` para construir el "Fat JAR" con todas las dependencias empaquetadas.

3. **En tu proyecto de Spring Boot, ¿de dónde sale la versión de una dependencia que no tiene `<version>`?**
   - Sale del proyecto padre (`spring-boot-starter-parent`), el cual define las versiones recomendadas y compatibles de cientos de librerías dentro de su sección `<dependencyManagement>`. Al heredar de este padre, Maven resuelve la versión automáticamente sin necesidad de especificarla en el `pom.xml` del proyecto.

4. **¿Qué va en `settings.xml` y no en `pom.xml`? ¿Por qué?**
   - En `settings.xml` se configuran aspectos específicos del entorno local del desarrollador o máquina de construcción: URLs de repositorios privados corporativos (como Nexus o Artifactory), credenciales de acceso (usuarios/contraseñas), configuraciones de proxy y perfiles locales.
   - No se colocan en `pom.xml` porque el `pom.xml` describe el proyecto y se sube al repositorio de control de versiones (Git), por lo que incluir credenciales o configuraciones locales en él expondría datos sensibles y rompería la portabilidad del código. `settings.xml` reside únicamente en la máquina local (`~/.m2/settings.xml`).