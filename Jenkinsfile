properties([pipelineTriggers([githubPush()])])
pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  environment {
   registry = "sdebnath13/testing"
   registryCredential = "cb799438-4019-41b1-826d-5ce2c4f53f10"
  }
  stages {
    stage('Checkout SCM') {
  steps {
    checkout([
      $class: 'GitSCM',
      branches: [[name: 'master']],
      userRemoteConfigs: [[
        url: 'https://github.com/SoumyadeepDebnath/jira_integration.git',     
        credentialsId: '8acfc31c-d902-463d-ad29-afdc446892df',
      ]]
     ])
   }
}     
    stage('Build'){
           steps
           {
        sh "mvn clean install"
    }     
    }
        
  }
post {
       always {
            echo 'I will always say Hello again!'
     create_newjira_issue()
       }
    }

}
void create_newjira_issue() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'JIRA'],
            summary: 'Maven Build',
            description: 'Facing some issue in building Maven Code',
            issuetype: [name:'Task']]]


    response = jiraNewIssue issue: NewJiraIssue, site:'SamJIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}
