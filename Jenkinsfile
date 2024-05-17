echo "Hello World"

pipeline {
    agent any
    stages{
        stage ("Build"){
            steps{
                sh "./mvnw package"
            }
        }
        stage("Parallel") {
            steps {
                parallel (
                    "firstTask": {
                        //do some stuff
                        sh "echo test setA"
                        sleep 2
                        sleep 3
        	        },
                    "secondTask": {
        	            sh "echo test setB"
        	            sleep 4
        	        },
                    "thirdTask": {
                        sleep 4
        	        }, failFast: true
                )
            }
        }
        stage ("Archive"){
            steps{
                // **/target/*.jar
                archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
            }
        }
        stage ("Test Result Capture"){
            steps{
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
        stage ("jacoco"){
            steps{
                jacoco()
            }
        }
    }

    post{
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", 
            recipientProviders: [developers()], 
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]", 
            to: 'testbuild@developer.com'
        }
    }

}
