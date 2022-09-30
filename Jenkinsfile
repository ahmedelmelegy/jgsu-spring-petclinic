pipeline{
    agent any
    triggers { pollSCM '* * * * *' }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ahmedelmelegy/jgsu-spring-petclinic.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package'
                //sh 'false' // true
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                // }
                //   changed {
                        emailext attachLog: true, to: "test@jenkins.com",
                        body: 'Please go to ${BUILD_URL} and verify the build',
                        compressLog: true, recipientProviders: [upstreamDevelopers(), requestor()],
                        subject: 'job \'$(JOB_NAME)\' ({$BUILD_NUMBER}) is waiting for input'
                    } 
            }
        }
    }
}
