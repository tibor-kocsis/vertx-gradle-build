```
buildscript {
	repositories {
		jcenter()
	}
	
	ext {
		shadowVersion = "1.2.4"
		vertxVersion = "3.4.2"
	}
	
	dependencies {
		classpath "com.github.jengelman.gradle.plugins:shadow:$shadowVersion"
	}
}

apply plugin: 'java'
apply plugin: 'com.github.johnrengelman.shadow'

repositories {
	jcenter()
}

sourceCompatibility = "1.8"
targetCompatibility = "1.8"
	
dependencies {
	compile "io.vertx:vertx-core:$vertxVersion"
	compile "io.vertx:vertx-web:$vertxVersion"
	testCompile 'junit:junit:4.12'
}

shadowJar {
	classifier = "fat"
	dependencies {
		exclude(dependency("io.vertx:codegen"))
		exclude(dependency("junit:junit"))
		exclude(dependency("log4j:log4j"))
	}
}

tasks.withType(JavaExec) {
	if (System.getProperty('DEBUG', 'false') == 'true') {
		jvmArgs '-Xdebug', '-Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=9099'
	}
}

task run(type:JavaExec) {
	main = 'com.github.tkocsis.MainVerticle'
	classpath = sourceSets.main.runtimeClasspath
}

```
