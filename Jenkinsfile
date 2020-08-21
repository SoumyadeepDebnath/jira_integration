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
         
    stage('Initialize'){
      steps{
        echo "We are doing some test"
        echo "PATH = ${PATH}"
      }
    }
         
    stage('Build'){
      steps{
        sh "mvn clean install"
      }     
    }
         
    stage('Checkout SCM') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: 'master']],
          userRemoteConfigs: [[
            url: 'https://github.com/SoumyadeepDebnath/jira_integration.git',     
            credentialsId: 'cb799438-4019-41b1-826d-5ce2c4f53f10',
          ]]
        ])
      }
    }     
    
  }

  post {
    always {
      success 
      {
        create_jira_issue_failure()
      }
      failure
      {
        create_jira_issue_success()
      }
    }
  }
}

void create_jira_issue_failure() {
  node {
    stage('JIRA Issue - Failure') {
      def NewJiraIssue = [fields: [project: [key: 'JIRA'],
                                   summary: 'Build Failed',
                                   description: 'Issue Occured in Building Maven Code',
                                   issuetype: [name:'Task']]
                         ]
      response = jiraNewIssue issue: NewJiraIssue, site:'SamJIRA'
      echo response.successful.toString()
      echo response.data.toString()
    }
  }
}

void create_jira_issue_success() {
  node {
    stage('JIRA Issue - Success') {
      def NewJiraIssue = [fields: [project: [key: 'JIRA'],
                                   summary: 'Build Success',
                                   description: 'Task Successfull in Building Maven Code',
                                   issuetype: [name:'Task']]
                         ]
      response = jiraNewIssue issue: NewJiraIssue, site:'SamJIRA'
      echo response.successful.toString()
      echo response.data.toString()
    }
  }
}
