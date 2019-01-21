# java-common
Reusable build configuration for for BorderTech open source projects.

## Status
[![Build Status](https://travis-ci.com/BorderTech/java-common.svg?branch=master)](https://travis-ci.com/BorderTech/java-common)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/c7a2226acd574943af9ae966c54b05e6)](https://app.codacy.com/app/BorderTech/java-common?utm_source=github.com&utm_medium=referral&utm_content=BorderTech/java-common&utm_campaign=Badge_Grade_Dashboard)

## qa-parent
BorderTech java projects should generally use this as their parent POM.

It runs quality assurance checks on your java code using tools such as checkstyle, pmd and findbugs.

By default qa checks do not run, you must enable them on a per-module basis in the pom.xml like so:

```xml
<properties>
	<!--
		Set bt.qa.skip to false to run QA checks.
	-->
	<bt.qa.skip>false</bt.qa.skip>
</properties>
```

The qa-parent inherits all of the release functionality from bordertech-parent, discussed below.

## bordertech-parent
This is the top-level pom.xml file.
It configures the maven release plugin for open source BorderTech projects to release to Maven Central.

_Note that java projects should generally not consume this directly but instead should use qa-parent as a parent POM instead._

Projects using this must ensure the necessary POM sections are overriden - these are marked in the bordertech-parent pom, for example:

```xml
<!--
	Descendants SHOULD override the url.
-->
<url>https://github.com/bordertech/java-common/</url>
```

Once you have configured your project and environment you can release to Maven Central. It may look a little something like the examples below.

### Releasing
The golden rule is ALWAYS do the release on a separate branch (it makes [backing out](https://github.com/BorderTech/java-common/wiki/Releasing#dealing-with-failure) much easier when problems arise).

Full documentation is available in the wiki under [Releasing](https://github.com/BorderTech/java-common/wiki/Releasing).

It is recommended projects use [gitflow with pull requests](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) model (ie feature, develop, release, master, hotfix branch paradigm).

More details on the pull request pattern are available [here](https://blog.axosoft.com/pull-requests-gitflow/).

## build-tools
This is primarily a shared resources module used by qa-parent and potentially other BorderTech maven modules.
