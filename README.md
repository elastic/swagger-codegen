Fork of [swagger-codegen](https://github.com/swagger-api/swagger-codegen), primarily maintained by the Elastic Cloud team.

### Why fork?

The Cloud team relies on the Scala and Java clients, generated from our [OpenAPI specification](https://github.com/elastic/cloud/blob/master/scala-services/adminconsole/src/main/resources/apidocs-full.json), for building our integration tests. The Scala client in particular has proven to be buggy. Despite our efforts to contribute patches upstream, each new version of the client seems to bring on newer problems :smile:. We've decided it's easier to just maintain our own fork for the sake of having a stable and reliable client for our tests.

### Publishing a new version to our artifactory

  - First, join the `#cloud-api` channel on Slack and give the Cloud API team a heads up :smile:
  - Open a PR bumping the version in:
    - [parent pom.xml](https://github.com/elastic/swagger-codegen/blob/master/pom.xml)
    - [swagger-codegen pom.xml](https://github.com/elastic/swagger-codegen/blob/master/modules/swagger-codegen/pom.xml)
    - [swagger-codegen-cli pom.xml](https://github.com/elastic/swagger-codegen/blob/master/modules/swagger-codegen-cli/pom.xml)
    - [swagger-codegen-maven-plugin pom.xml](https://github.com/elastic/swagger-codegen/blob/master/modules/swagger-codegen-maven-plugin/pom.xml)
    - [swagger-generator pom.xml](https://github.com/elastic/swagger-codegen/blob/master/modules/swagger-generator/pom.xml)
  - Run [this Jenkins job](https://ci-staging.found.no/job/cloud-swagger-codegen-fork/) to publish the Maven artifacts:
    - First, run the `clean package` goal (`master` branch by default)
    - Then, run the `deploy` goal  (`master` branch by default)
