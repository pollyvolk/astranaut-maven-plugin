# Maven Plugin for Astranaut

![Build and test](https://github.com/unified-ast/astranaut-maven-plugin/workflows/Build%20and%20test/badge.svg)
[![Codecov](https://codecov.io/gh/unified-ast/astranaut-maven-plugin/branch/master/graph/badge.svg)](https://codecov.io/gh/unified-ast/astranaut-maven-plugin)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/unified-ast/astranaut-maven-plugin/blob/master/LICENSE.txt)
___

## About

Astranaut Maven Plugin generates source Java files that describe an abstract syntax tree (AST) model.
The plugin works in the Maven compile stage.

## Requirements

* Java 1.8
* Maven 3.6.3+ (to build)

Add the `astranaut-core` dependency to use files generated by Astranaut plugin:

~~~xml
<dependencies>
  <dependency>
    <groupId>org.cqfn</groupId>
    <artifactId>astranaut-core</artifactId>
    <version>1.0.1</version>
  </dependency>
</dependencies>
~~~

## How to use

Add Astranaut plugin to your project's POM file:

~~~xml
<plugin>
    <groupId>org.cqfn</groupId>
    <artifactId>astranaut-maven-plugin</artifactId>
    <version>0.1</version>
    <executions>
        <execution>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
</plugin>
~~~

You can find the latest release version here <TODO after release>.

Astranaut plugin uses the following variable data to generate source files:
- `dsl` - the name (path) of the file that contains DSL rules describing an AST model and tree transformations
- `output` - the name (path) of the output directory where the Java files should be generated -
  it must be a directory that is marked as a **Sources Root** in Maven project
- `pkg` - the package of the generated Java source files - the directory inside `output` which is found in generated files
- `license` - the name (path) of the file that contains a license header. 
  It is required to add a license to each Java file, therefore the plugin needs this information.
- `version` - the version of the implementations which is added to the `@since` tag in a Javadoc of each generated class

You can specify all or some of these parameters manually in the plugin's configuration property.

#### Example

~~~xml
<plugin>
    <groupId>org.cqfn</groupId>
    <artifactId>astranaut-maven-plugin</artifactId>
    <version>0.1</version>
    <configuration>
        <dsl>${basedir}/data/dsl.txt</dsl>
        <license>${basedir}/licenses/LICENSE.txt</license>
        <output>${basedir}/src/main/java</output>
        <version>1.2.3</version>
        <pkg>org.uast.generated</pkg>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
</plugin>
~~~

**By default** when the corresponding parameters are not specified in the configuration, Astranaut project will:

- try to find DSL in `${basedir}/src/main/dsl/rules.dsl` path and throw an exception if not found;
- try to find license in `${basedir}/LICENSE.txt` path and throw an exception if not found;
- generate sources in the `${project.build.directory}/generated-sources/astranaut` directory marked 
  as `Generated sources root`;
- generate sources in the `org.cqfn.astranaut.generated.tree` Java package;
- take the version from the Maven project, which is `0` if the project is empty.

### Contributors

* Ivan Kniazkov, @kniazkov
* Polina Volkhontseva, @pollyvolk

See our [Contributing policy](CONTRIBUTING.md).