pipeline{

    agent any

    stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'main', url: 'https://github.com/thisismanikandan/devops1'
                }
            }
        }
        stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){

            steps{

                script{

                    sh 'mvn clean install'
                }
            }
        }
        stage('Sonar Cube Analysis'){

            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonarapikey') {

                        sh 'mvn clean package sonar:sonar'
                    }
                   }

                }
            }
            stage('Quality Gate Status'){

                steps{

                    script{

                        waitForQualityGate abortPipeline: false, credentialsId: 'sonarapikey'
                    }
                }
            }
        stage('Uploading war file to Nexus'){
            steps{
                script{
                    def pom = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-auth', groupId: 'com.example', nexusUrl: '192.168.10.10:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'project1-release', version: "${pom.version}"
                }
            }
        }
        stage('Docker image build'){
            steps{
                script{
                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image -t $JOB_NAME:v1.$BUILD_ID manidevopsengineer/project1/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image -t $JOB_NAME:v1.$BUILD_ID manidevopsengineer/project1/$JOB_NAME:latest'
                }
            }
        }
        }

}
