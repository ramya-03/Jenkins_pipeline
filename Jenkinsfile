pipeline{
    agent any

    tools{
        maven 'Maven_3.9.0'
    }

    stages {
        stage('SCM Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ramya-03/java-maven-app.git']])
            }

        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'

            }
        }
        stage('Sonar Scan'){
            steps{
                withSonarQubeEnv("SonarQube_Assessment"){
                    sh "${tool("Sonar_4.8")}/bin/sonar-scanner \
                    -Dsonar.host.url=http://ec2-3-26-44-146.ap-southeast-2.compute.amazonaws.com:9000/ \
                    -Dsonar.login=sqp_da8fd33bdba1c3b89c25ac5c170a534647580cad \
                    -Dsonar.projectKey=java-maven-app \
                    -Dsonar.java.binaries=target "

                }
            }
        }
        stage('Nexus Upload'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }
}
