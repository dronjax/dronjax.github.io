---
layout: post
title:  "Java Jar - Publish to Open Source"
date: 2019-07-28
tags: [java, jar]
excerpt_separator: <!--more-->
---
Hello again, this time I am gonna tell you about the time I publish a open source [jar][jar-wiki]{:target="_blank"} to [maven central repository][maven-search]{:target="_blank"}.

This post is about publishing [jar file][jar-wiki]{:target="_blank"} which is an artifact that is generated using [java programming language][java-wiki]{:target="_blank"}.

If you managed to generate a library into [jar file][jar-wiki]{:target="_blank"}, you can publish it to [maven central repository][maven-search]{:target="_blank"}. The catch is that you gotta make the library as open source library.

You can follow the guide provided by [OSSRH (Open Source Software Repository Hosting)][ossrh-guide]{:target="_blank"}.

In summary, the steps are:

1. Create a jira account for the OSSRH jira via [this link][jira-signup]{:target="_blank"}.
<!--more-->
2. Create a new jira project via [this link][jira-create-project]{:target="_blank"}.
   Your new project will look like [this][jira-request-example]{:target="_blank"}.
   ![JIRA ticket example](/assets/images/oss-jira-ticket-example.png)
3. You might need to verify your domain ownership if you use personal domain.

   You can verify the domain by adding the TXT record that reference to the jira ticket.

   After adding the TXT record, you can check if it's propagated properly like [this][dns-check]{:target="_blank"}.

   ![TXT DNS check](/assets/images/oss-txt-dns-check.png)
4. Follow this [requirements][library-req]{:target="_blank"} of the library setup.
   The requirements require you to publish javadoc and sign your library with developer information and license.

   You can also refer to this [example][library-example]{:target="_blank"}.
5. Publish your library using OSSRH jira credentials (username and password) to staging.
6. Login to the [OSSRH Nexus][oss-nexus]{:target="_blank"} using OSSRH jira credentials.
7. You should be able to see your library under the [staging menu][oss-nexus-staging]{:target="_blank"}.
8. Promote your staging artifact using the menu in the nexus (can right click also).

   This step will trigger some checks on the library.

   If something fails, try to fix the error or retry.
9. After you promote your first artifact, you can comment on the jira ticket you created previously to enable the sync to maven central.
   The sync might takes sometime up to some hours, you can check if your artifact is available via [maven search][maven-search]{:target="_blank"}.

   If you got confused along the way, you can also refer to the [release guide][release-guide]{:target="_blank"}.

And, that's all folks.

Hope that this guide can help you contribute to the Open Source Software.

[maven-search]: https://search.maven.org/
[jar-wiki]: https://en.wikipedia.org/wiki/JAR_%28file_format%29
[java-wiki]: https://en.wikipedia.org/wiki/Java_%28programming_language%29
[ossrh-guide]: https://central.sonatype.org/pages/ossrh-guide.html
[jira-signup]: https://issues.sonatype.org/secure/Signup!default.jspa
[jira-create-project]: https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134
[jira-request-example]: https://issues.sonatype.org/browse/OSSRH-49542
[dns-check]: https://dnschecker.org/#TXT/dronjax.com
[library-req]: https://central.sonatype.org/pages/requirements.html
[library-example]: https://github.com/dronjax/java-object-shutdown-proxy/blob/master/build.gradle
[oss-nexus]: https://oss.sonatype.org
[oss-nexus-staging]: https://oss.sonatype.org/#stagingRepositories
[release-guide]: https://central.sonatype.org/pages/releasing-the-deployment.html