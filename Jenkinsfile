pipeline {
    agent {
        label "maven"
    }

    triggers {
        pollSCM('H/30 * * * *')
    }

    stages {
        stage('Build') {
            when {
                changeRequest target: 'master'
            }
            steps {
                container('maven'){
                    sh 'mvn clean package'
                }
            }
        }

        stage('Release') {
            when {
                branch 'master'
            }
            steps {
                container('maven'){
                    sh 'mvn release:prepare -B'
                    sh 'mvn release:perform -Darguments="-DsignSkip=false -DdocSkip=false"'
                }
            }
        }
    }
}