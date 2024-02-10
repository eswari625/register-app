pipeline{
    tools{
        jdk 'jdk17'
        maven 'Maven3'
    }
    environment{
        sonar_scanner=tool 'sonar-scanner'
    }
    stages{
        stage("Clear Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Git checkout"){
            steps{
                git branch:'main', url: 'https://github.com/eswari625/register-app.git'
            }
        }
        stage("Package"){
            steps{
                sh 'mvn clean package'
            }
        }
        stage("test"){
            steps{
                sh 'mvn test'
            }
        }
        stage("Sonarqube analysis"){
            steps{
                
                    withSonarqubeEnv('sonar-server') {
                        sh ''' $sonar_scanner/bin/sonar-scanner sonar.projectName=Register-app \
                        sonar.projectKey=Register-app '''
                    }
   
                }
            }
        stage("Quality gate"){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
            }
            
        }
        stage("install dependencies"){
            steps{
                sh 'mvn install'
            }
        }
        }
    }
