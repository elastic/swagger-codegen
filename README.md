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
  - CI-Staging [Jenkins job](https://ci-staging.found.no/job/cloud-swagger-codegen-fork/) is no longer working way to publish Maven Artifacts. Instead, when a PR is merged to master, artifacts need to be packaged locally:
    - Run `mvn clean package` target from the master branch.
    - Then, there are two possible ways:
        1) Run  `mvn -Darguments=-DskipTests clean deploy` target to deploy to Artifactory. For that you need to have local `~/.m2/settings.xml` configured.
        2) Deploy using [Artifactory](https://artifactory.elastic.dev/ui/repos/tree/General/cloud-release) UI `Deploy` button

#### A quick guide on how to configure `settings.xml` file:
[Artifactory](https://artifactory.elastic.dev/) provides a nice UI to help to setup settings file:
- Go to the `Artifacts` section (left column)
- Click the `Set Me Up` button on the top right
- Choose `cloud-release-local` repository
- Click `Generate Maven Settings` and copy generated file
- To deploy, you'll need to replace username with your `@elastic.co` email and password with the `API Key` generated in the `Edit Profile` section:
  ```
  <username>${security.getCurrentUsername()}</username>
  <password>${security.getEscapedEncryptedPassword()!"*** Insert encrypted password here ***"</password>
  ```
