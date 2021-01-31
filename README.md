# OpenDevStack Best practices

TOC:
1) **What is OpenDevStack**
3) **Anatomy of a Quickstarter Jenkinsfile**
4) **Groovy coding conventions**
5) **How to declare and use env vars during Quickstarter deploy and build processes**
6) **Predeterminated methods of quickstarter Jenkinsfile**
7) **How to create my custom methods in a quickstarter Jenkinsfile**

Content:
1) **What is OpenDevStack and OpenDevStack Quickstarters:**
OpenDevStack is a framework that let us to create ready to use software projects using Openshift container platform
and Atlassian Stack, OpenDevStack is composed mainly by a set of Jenkins pipelines and a graphical web application called 
Provisioning application that help us to create Software projects that contains openshift namespaces and associated
confluence project, bitbucket project and jira project. Also allow us to create easily ready to use sofware apps called 
quickstarters that are deployed to openshift and synchronyzed with Atlassian stack (automatic build and deploy after every git push, jira integration), OpenDevStack supports a long list of
containerized apps that can be found here: https://github.com/opendevstack/ods-quickstarters

2) **Anatomy of a Quickstarter Jenkinsfile:**
Every quickstarter have a file called Jenkinsfile, it's a groovy file that basically define all the instrucctions
to use during quickstarter build, test and deploy phases, it can be used for invoke typical ODS methods like sonarqube analyisis of our application (odsComponentStageScanWithSonar).


Here We can see a minimal Jenkinsfile used for simply build and deploy a minimal Docker application
```groovy
// See https://www.opendevstack.org/ods-documentation/ for usage and customization.
// trigger commit
@Library('ods-jenkins-shared-library@3.x') _

odsComponentPipeline(
  imageStreamTag: 'myodsproject/jenkins-agent-base:3.x',
  branchToEnvironmentMapping: [
    'master': 'dev',
    // 'release/': 'test'
  ]
) { context ->
  odsComponentStageImportOpenShiftImageOrElse(context) {
    stageBuild(context)
    stageUnitTest(context)
    /*
    * if you want to introduce scanning, uncomment the below line
    * and change the type in metadata.yml to 'ods'
    *
    * odsComponentStageScanWithSonar(context)
    */
    odsComponentStageBuildOpenShiftImage(context)
  }
  odsComponentStageRolloutOpenShiftDeployment(context)
}

def stageBuild(def context) {
  stage('Build') {
    // copy any other artifacts, if needed
    // sh "cp -r build docker/dist"
    // the docker context passed in /docker
  }
}

def stageUnitTest(def context) {
  stage('Unit Test') {
    // add your unit tests here, if needed
    
  }
}

```

Let's analyze all the code step by step.

First of all we've the import of OpenDevStack basic libraries at ```groovy @Library('ods-jenkins-shared-library@3.x') _ ```
```groovy odsComponentPipeline``` is the function that declares which ODS stages will be executed agains our application (build, test, deploy, etc).
and also declares basic metadata used by ODS for build and deploy our app.

**odsComponentPipeline basic metadada:**
* imageStreamTag: The jenkins-slave docker image that will be used for building our app, aka jenkins agent. All ready to use images can be found here: https://github.com/opendevstack/ods-quickstarters/tree/master/common/jenkins-agents, also we can create our own custom jenkins agents directly in Openshift and use them 
with ```imageStreamTag``` property
* branchToEnvironmentMapping:
Example:
```groovy
  branchToEnvironmentMapping: [
    'master': 'dev',
    // 'release/': 'test'
  ]
```
This list is used for defining in which openshift namespace will be deployed the code tracked by a Git branch,
the content of this list should follow this format: ``` 'branch_name':'openshift_namespace' ``` possible values for openshift_namespace: 'dev' or 'test' 
dev namespace aka "myproject-dev" is used as development environment, and test namespace aka "myproject-test" is used as QA environment 

**odsComponentPipeline basic stages**


