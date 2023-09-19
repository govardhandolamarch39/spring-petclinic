pipeline {
    agent { label 'JDK_17' }
    triggers { pollSCM ('21 * * * 7') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/khajadevopsmarch23/spring-petclinic.git',
                    branch: 'pt'
            }
        }
        stage ('Artifactory configuration') {
            steps {
        stage('package') {
            tools {
                jdk 'JDK_17'
            }
            steps {
            }  
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}