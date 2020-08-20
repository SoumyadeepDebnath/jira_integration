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
           steps
           {
        sh "mvn clean install"
    }     
    }
        
}

post {
     create_newjira_issue()
    }

}
void create_newjira_issue() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'TES'],
            summary: 'Maven Build',
            description: 'Facing some issue in building Maven Code',
            issuetype: [id: '3']]]


    response = jiraNewIssue issue: NewJiraIssue, site: 'http://51.105.154.96:8080/'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}
