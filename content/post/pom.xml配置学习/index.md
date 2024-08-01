---
title: pom.xml配置学习
description: 
date: 2024-04-15
image: https://www.bing.com/th?id=OHR.ChambordCastle_ZH-CN0930093515_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp
draft: false
tags:
  - Maven
categories:
  - Java
---
`pom.xml` 文件是 Maven 项目的核心配置文件，全称为 Project Object Model (POM)，它以 XML 格式定义了项目的构建、依赖、插件、属性、构建生命周期等关键信息。以下是 `pom.xml` 文件的详细配置说明：

### 1. **基本结构**

一个典型的 `pom.xml` 文件结构如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <!-- 元素配置 -->
</project>
```

- `<?xml ... ?>`：XML 声明，指定了文件的版本和编码。
- `<project>` 根元素，包含了所有 POM 相关配置。
- `xmlns` 和 `xsi` 属性定义了 XML 命名空间和对应的 XML 架构位置，确保 Maven 能正确解析 POM 文件。

### 2. **核心配置元素**

#### 2.1 **`modelVersion`**

```xml
<modelVersion>4.0.0</modelVersion>
```

指定 POM 的模型版本，通常固定为 `4.0.0`，表示遵循 Maven 2 或 3 的 POM 规范。

#### 2.2 **`groupId`**

```xml
<groupId>com.example.myproject</groupId>
```

标识项目的唯一性，通常采用反向域名形式，对应项目的组织或公司域名。例如，公司域名为 `example.com`，项目的 groupId 可设置为 `com.example`。

#### 2.3 **`artifactId`**

```xml
<artifactId>my-project</artifactId>
```

定义项目的唯一标识符，通常与项目文件夹名称或主 JAR/WAR 文件名相同。在同一 `groupId` 下，不同项目应有不同的 `artifactId`。

#### 2.4 **`version`**

```xml
<version>1.0.0-SNAPSHOT</version>
```

设定项目的版本号，遵循语义化版本控制（Semantic Versioning, SemVer）。SNAPSHOT 表示开发版本，发布版则去掉 `-SNAPSHOT`。

#### 2.5 **`packaging`**

```xml
<packaging>jar</packaging>
```

指定项目打包类型，默认为 `jar`（Java 库）。其他常见类型包括 `war`（Web 应用）、`pom`（聚合项目）等。

### 3. **依赖管理**

#### 3.1 **`dependencies`**

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>1.0.0</version>
    </dependency>
</dependencies>
```

定义项目依赖的其他库或模块。每个 `dependency` 元素包含 `groupId`、`artifactId` 和 `version`，用于唯一确定依赖项。

#### 3.2 **`properties`**

```xml
<properties>
    <java.version>1.8</java.version>
    <spring-boot.version>2.7.0</spring-boot.version>
</properties>
```

定义项目中可复用的属性值，通常用于存放版本号、路径、变量等，方便在整个 POM 文件中引用。
#### 3.3 **`dependencyManagement`**

```xml
<dependencyManagement>
    <dependencies>
        <!-- 依赖版本统一管理 -->
    </dependencies>
</dependencyManagement>
```

用于集中管理项目及其子模块中依赖的版本。在此处声明的依赖版本会被子模块继承，但不会直接引入依赖。

#### 3.4 **`exclusions`**

```xml
<dependency>
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <exclusions>
        <exclusion>
            <groupId>...</groupId>
            <artifactId>...</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

在某个依赖中排除特定的传递依赖。

### 4. **构建配置**

在 Maven 的 `pom.xml` 文件中，`<build>` 元素用于定义项目的构建过程和相关配置。以下是 `<build>` 元素及其子元素的详细说明：

```xml
<build>
    <pluginManagement>
        <plugins>
            <!-- ... -->
        </plugins>
    </pluginManagement>
    <plugins>
        <plugin>
            <groupId>...</groupId>
            <artifactId>...</artifactId>
            <version>...</version>
            <executions>
                <!-- 插件执行配置 -->
            </executions>
            <configuration>
                <!-- 插件具体配置 -->
            </configuration>
        </plugin>
    </plugins>

    <resources>
        <!-- ... -->
    </resources>
    <testResources>
        <!-- ... -->
    </testResources>

    <directory>...</directory>
    <outputDirectory>...</outputDirectory>
    <testOutputDirectory>...</testOutputDirectory>
    <sourceDirectory>...</sourceDirectory>
    <scriptSourceDirectory>...</scriptSourceDirectory>
    <testSourceDirectory>...</testSourceDirectory>
    <resourcesDirectory>...</resourcesDirectory>
    <testResourcesDirectory>...</testResourcesDirectory>

    <filters>
        <!-- ... -->
    </filters>

    <extensions>
        <!-- ... -->
    </extensions>

    <finalName>${artifactId}-${version}</finalName>
    <defaultGoal>...</defaultGoal>
    <sourceEncoding>...</sourceEncoding>
    <targetEncoding>...</targetEncoding>
    <encoding>...</encoding>
    <finalName>...</finalName>
    <filtering>...</filtering>
    <!-- ... -->
</build>
```

#### 4.1 **Plugin Management**

- `<pluginManagement>`：集中管理项目及其子模块中插件的版本和配置。在此处声明的插件配置会被子模块继承，但不会直接执行。子模块仍需在 `<plugins>` 中声明要使用的插件。
#### 4.2 **Plugins**

- `<plugins>`：配置项目使用的 Maven 插件及其执行细节。插件用于扩展 Maven 的构建功能，如编译、打包、测试、代码检查、部署等。

#### 4.3 **Directory Settings**

- `<directory>`：指定项目构建生成文件的根目录。
- `<outputDirectory>`：指定项目主构建结果（如 JAR、WAR 文件）的输出目录。
- `<testOutputDirectory>`：指定项目测试构建结果（如测试报告、测试覆盖率报告）的输出目录。
- `<sourceDirectory>`：指定项目主源代码目录。
- `<scriptSourceDirectory>`：指定项目脚本源代码目录（如 Groovy、Ruby 等）。
- `<testSourceDirectory>`：指定项目测试源代码目录。
- `<resourcesDirectory>`：指定项目主资源文件目录。
- `<testResourcesDirectory>`：指定项目测试资源文件目录。

#### 4.4 **Filters**

- `<filters>`：定义过滤器列表，这些过滤器在资源文件复制过程中用于替换资源文件中的占位符（如 `${property}`）。

#### 4.5 **Build Extensions**

- `<extensions>`：配置 Maven 扩展插件，这些插件可以扩展 Maven 的核心功能，如支持新的打包类型、构建生命周期等。

#### 4.6 **Other Settings**

- `<finalName>`：指定项目构建产物（如 JAR、WAR 文件）的最终文件名，默认值是 `${artifactId}-${version}。
- `<defaultGoal>`：指定项目默认的构建目标（Maven 命令中的 `mvn [defaultGoal]`）。
- `<sourceEncoding>`、`<targetEncoding>`、`<encoding>`：指定源代码、构建产物、资源文件的字符编码。
- `<filtering>`：在 `<resources>` 或 `<testResources>` 中使用，指定是否对资源文件进行过滤（即替换占位符）。

#### 4.7 **Resources**

- `<resources>`：定义项目主资源文件，如 properties 文件、配置文件、静态资源等。Maven 在构建过程中会将这些资源复制到输出目录中。
- `<testResources>`：定义项目测试资源文件，如测试用到的配置文件、数据文件等。Maven 在构建过程中会将这些资源复制到测试输出目录中。

`<build>` 元素包含了项目构建过程的方方面面，包括资源文件管理、插件配置、构建目录设置、过滤器定义、扩展插件配置等。合理配置 `<build>` 元素，可以定制项目的构建过程，满足特定的构建需求。

### 5. **其他重要元素**

#### 5.1 **`profiles`**

```xml
<profiles>
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <!-- 环境特定配置 -->
    </profile>
    <!-- 更多环境配置 -->
</profiles>
```

定义不同构建环境（如开发、测试、生产）的配置，通过激活特定 profile 来切换环境。

#### 5.2 **`repositories` & `pluginRepositories`**

```xml
<repositories>
    <repository>
        <id>...</id>
        <url>...</url>
    </repository>
</repositories>

<pluginRepositories>
    <pluginRepository>
        <id>...</id>
        <url>...</url>
    </pluginRepository>
</pluginRepositories>
```

配置项目所需的远程仓库地址，用于下载依赖和插件。

### 总结

`pom.xml` 文件是 Maven 项目的核心配置文件，涵盖了项目基本信息、依赖管理、构建过程、环境配置等多个方面。通过合理配置 `pom.xml`，可以有效地管理项目依赖、定制构建过程、适应不同环境需求，确保项目的构建、测试、部署等工作高效、规范地进行。