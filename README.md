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

		Note: it's called wc.qa.skip because this functionality originated in the WComponents project.
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

### Example Releasing
The golden rule is ALWAYS do this on a separate branch (it makes [backing out](https://github.com/BorderTech/java-common/wiki/Releasing#dealing-with-failure) much easier when problems arise).

```bash
# make sure you are on the main/default branch
git checkout master # or "git checkout bobbie" etc

# fetch latest changes from main repository
git fetch origin # or "git fetch upstream" if you are on a fork

# ensure local branch is up-to-date
git merge origin/master # or "git merge upstream/master" or "git merge upstream/bobbie" etc

# create a new release branch (you could skip this step if it already exists which it probably shouldn't)
git branch release-xyz # "git branch release-123" etc

# switch to release branch
git checkout release-xyz # "git checkout !$" is easier :)

# perform the release
mvn release:clean release:prepare release:perform -Psonatype-oss-release
```

or to skip tests while releasing add `-Darguments="-DskipTests"`:

```
mvn release:clean release:prepare release:perform -Psonatype-oss-release -Darguments="-DskipTests"
```


Full documentation is available in the wiki under [Releasing](https://github.com/BorderTech/java-common/wiki/Releasing)

## build-tools
This is primarily a shared resources module used by qa-parent and potentially other BorderTech maven modules.

