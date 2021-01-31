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
quickstarters that are deployed to openshift and synchronyzed, OpenDevStack quickstarters supports a long list of
containerized apps that can be found here: https://github.com/opendevstack/ods-quickstarters

2) **What is an OpenDevStack Quickstarter**
