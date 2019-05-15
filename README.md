# java-common

Reusable build configuration for for BorderTech open source projects.

## Status

[![Build Status](https://travis-ci.com/BorderTech/java-common.svg?branch=master)](https://travis-ci.com/BorderTech/java-common)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/c7a2226acd574943af9ae966c54b05e6)](https://app.codacy.com/app/BorderTech/java-common?utm_source=github.com&utm_medium=referral&utm_content=BorderTech/java-common&utm_campaign=Badge_Grade_Dashboard)
[![Maven Central](https://img.shields.io/maven-central/v/com.github.bordertech.common/bordertech-parent.svg?label=Maven%20Central)](https://search.maven.org/search?q=g:%22com.github.bordertech.common%22%20AND%20a:%22bordertech-parent%22)

## qa-parent

BorderTech java projects should generally use this as their parent POM.

It runs quality assurance checks on your java code using tools such as checkstyle, pmd and findbugs.

By default qa checks do not run, you must enable them on a per-module basis in the pom.xml like so:

``` xml
<properties>
  <!-- Set bt.qa.skip to false to run QA checks. -->
  <bt.qa.skip>false</bt.qa.skip>
</properties>
```

Refer to qa-parent's [pom.xml](https://github.com/BorderTech/java-common/blob/master/qa-parent/pom.xml) for other project properties.

The qa-parent inherits all of the release functionality from bordertech-parent, discussed below.

## bordertech-parent

This is the top-level pom.xml file.
It configures the maven release plugin for open source BorderTech projects to release to Maven Central.

_Note that java projects should generally not consume this directly but instead should use qa-parent as a parent POM instead._

Projects using this must ensure the necessary POM sections are overriden - these are marked in the bordertech-parent pom, for example:

``` xml
<!--
  Descendants SHOULD override the url.
-->
<url>https://github.com/bordertech/java-common/</url>
```

Once you have configured your project and environment you can release to Maven Central. It may look a little something like the examples below.

### Releasing

The golden rule is ALWAYS do the release on a separate branch (it makes [backing out](https://github.com/BorderTech/java-common/wiki/Releasing#dealing-with-failure) much easier when problems arise).

Full documentation is available in the wiki under [Releasing](https://github.com/BorderTech/java-common/wiki/Releasing).

## build-tools

This is primarily a shared resources module used by qa-parent and potentially other BorderTech maven modules.

## qa-parent overrides

### Enforcer Plugin

Refer to [enforcer plugin](https://maven.apache.org/enforcer/maven-enforcer-plugin/enforce-mojo.html) for all override details.

#### Skip enforcer

``` xml
<property>
	<!-- Report issues but dont fail -->
	<enforcer.fail>false</enforcer.fail>
	<!-- Skip -->
	<enforcer.skip>true</enforcer.skip>
</property>
```

### Skip ALL Static Analysis

The `bt.qa.skip` property can be used to skip all QA checks.

``` xml
<property>
	<bt.qa.skip>true</bt.qa.skip>
</property>
```

### Checkstyle

Refer to [Checkstyle plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/checkstyle-mojo.html) for all override details.

#### Skip Checkstyle

``` xml
<property>
	<checkstyle.skip>true</checkstyle.skip>
</property>
```

#### Change or Add Checkstyle Rules

To add or change a Checkstyle rule you are required to define your own [config.xml](http://checkstyle.sourceforge.net/config.html) file.

``` xml
<property>
	<checkstyle.config.location>your-checkstyle-config.xml</checkstyle.config.location>
</property>
```

#### Ignore Checkstyle Rule

Define a comma-separated list, each value being either a rule name, a rule category or a java package name of rule class.

``` xml
<property>
	<!-- List of rules to ignore -->
	<checkstyle.violation.ignore></checkstyle.violation.ignore>
</property>
```

### PMD and CPD

Refer to [PMD plugin](https://maven.apache.org/plugins/maven-pmd-plugin/) for all override details.

#### Skip PMD and CPD

``` xml
<property>
	<pmd.skip>true</pmd.skip>
	<cpd.skip>true</cpd.skip>
</property>
```

#### Change PMD Rule set

Override bordertech default file property with your custom [rule set](https://pmd.github.io/latest/pmd_userdocs_making_rulesets.html).

``` xml
<property>
	<bt.pmd.rules.file>your-rules.xml</bt.pmd.rules.file>
</property>
```

#### Add extra PMD Rule set

An extra ruleset can be added via the plugin configuration.

``` xml
<plugin>
	<configuration>
		<rulesets>
			<ruleset>${bt.pmd.rules.file}</ruleset>
			<ruleset>your-rules.xml</ruleset>
		</rulesets>
	</configuration>
</plugin>
```

#### Ignore PMD Rule

Create an [excludes XML](https://pmd.github.io/latest/pmd_userdocs_suppressing_warnings.html#xpath-and-regex-message-suppression) file that lists classes and rules to be excluded from failures.

``` xml
<property>
	<pmd.excludeFromFailureFile>my-excludes.properties</pmd.excludeFromFailureFile>
</property>
```

### Spotbugs

Refer to [spotbugs plugin](https://spotbugs.github.io/spotbugs-maven-plugin/spotbugs-mojo.html) for all override details or [doco](https://spotbugs.readthedocs.io/en/latest/index.html).

#### Skip spotbugs

``` xml
<property>
	<spotbugs.skip>true</spotbugs.skip>
</property>
```

#### Ignore Spotbugs Rule

Create a [filter XML](https://spotbugs.readthedocs.io/en/latest/filter.html) file and add it to the bordertech defaults.

``` xml
<property>
	<!-- List of exclude files -->
	<excludeFilterFile>your-exclude-file.xml, ${bt.spotbugs.exclude.file}</excludeFilterFile>
</property>
```

### OWASP

Refer to [OWASP plugin](https://jeremylong.github.io/DependencyCheck/dependency-check-maven/index.html) for all override details.

#### Skip OWASP

``` xml
<property>
	<dependency-check.skip>true</dependency-check.skip>
</property>
```

#### Ignore OWASP Rule

Create a [suppression XML](https://jeremylong.github.io/DependencyCheck/general/suppression.html) file add set the following property:-

``` xml
<property>
	<suppression.file>my-suppression.xml</suppression.file>
</property>
```
