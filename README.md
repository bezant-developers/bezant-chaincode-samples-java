# Hypledger fabric chaincode samples java
- [Simple](simple): Basic example that uses chaincode to query and execute transaction

## Upload partner admin site
### 1. Download file and upload  
- [simple-java.zip](simple/simple-java.zip): You have to **download zip file** and upload

### 2. Compress source file and upload
- Only **src**, **build.gradle** or **pom.xml** files need to be compressed 
    ``` console
    zip -r chaincode.zip build.gradle src
    ```

## Environment
+ `JDK8+`
+ `Gradle 4.10`
+ `Hyperledger Fabric 1.4`

## Download code
```sh
$ git clone https://github.com/bezant-developers/bezant-chaincode-samples-java.git
```


## gradle (build.gradle)
```groovy
plugins {
    id 'com.github.johnrengelman.shadow' version '2.0.3'
    id 'java'
}

group 'io.bezant.bezant-chaincode-samples-java'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

dependencies {
    compile group: 'org.hyperledger.fabric-chaincode-java', name: 'fabric-chaincode-shim', version: '1.4.0'
    testCompile group: 'junit', name: 'junit', version: '4.12'
}

shadowJar {
    baseName = 'chaincode'
    version = null
    classifier = null

    manifest {
        attributes 'Main-Class': 'io.bezant.SimpleChaincode'
    }
}

```

## maven (pom.xml)
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>io.bezant</groupId>
	<artifactId>bezant-chaincode-samples-java</artifactId>
	<version>1.0-SNAPSHOT</version>
	<properties>

		<!-- Generic properties -->
		<java.version>1.8</java.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

		<!-- fabric-chaincode-java -->
		<fabric-chaincode-java.version>1.4.0</fabric-chaincode-java.version>

		<!-- Test -->
		<junit.version>4.12</junit.version>
	</properties>

	<dependencies>
		<!-- fabric-chaincode-java -->
		<dependency>
			<groupId>org.hyperledger.fabric-chaincode-java</groupId>
			<artifactId>fabric-chaincode-shim</artifactId>
			<version>${fabric-chaincode-java.version}</version>
			<scope>compile</scope>
		</dependency>

		<dependency>
			<groupId>org.hyperledger.fabric-chaincode-java</groupId>
			<artifactId>fabric-chaincode-protos</artifactId>
			<version>${fabric-chaincode-java.version}</version>
			<scope>compile</scope>
		</dependency>

		<!-- Test Artifacts -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit.version}</version>
			<scope>test</scope>
		</dependency>

	</dependencies>
	<build>
		<sourceDirectory>src</sourceDirectory>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.1</version>
				<configuration>
					<source>${java.version}</source>
					<target>${java.version}</target>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-shade-plugin</artifactId>
				<version>3.1.0</version>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>shade</goal>
						</goals>
						<configuration>
							<finalName>chaincode</finalName>
							<transformers>
								<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
									<mainClass>io.bezant.SimpleChaincode</mainClass>
								</transformer>
							</transformers>
							<filters>
								<filter>
									<!-- filter out signature files from signed dependencies, else repackaging fails with security ex -->
									<artifact>*:*</artifact>
									<excludes>
										<exclude>META-INF/*.SF</exclude>
										<exclude>META-INF/*.DSA</exclude>
										<exclude>META-INF/*.RSA</exclude>
									</excludes>
								</filter>
							</filters>
						</configuration>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
</project>
```

## Creating a chain code class

Use the `Simple Chaincode` of the `Java` version (https://hyperledger-fabric.readthedocs.io/en/latest/chaincode4ade.html#simple-asset-chaincode) as an example. This chain code is the `Go to Java` translation of `Simple Asset Chaincode`, which will be explained.

The `ChaincodeBase` class is a *abstract class* that inherits the `Chaincode` form, which contains the `start` method for starting `chaincode`. Therefore, we will create our chain code by extending `ChaincodeBase` instead of `Chaincode`.

First, from a basic beginning, create a `class` file `SimpleChaincode`. Like every chain code, it inherits the [`ChaincodeBase` abstract class] (https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#Chaincode), especially implemented. `Init` and `invoke` functions.

```java
package io.bezant;

import org.hyperledger.fabric.shim.ChaincodeBase;
import org.hyperledger.fabric.shim.ChaincodeStub;

public class SimpleChaincode extends ChaincodeBase {
    public static void main(String[] args) {
        new SimpleChaincode().start(args);
    }

    @Override
    public Response init(ChaincodeStub stub) {
        return newSuccessResponse();
    }

    @Override
    public Response invoke(ChaincodeStub stub) {
        return newSuccessResponse();
    }
}
```

## Local environment test
[bezant-chaincode-test-network link](https://github.com/bezant-developers/bezant-chaincode-test-network)