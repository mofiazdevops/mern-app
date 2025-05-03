pipeline {
    agent any
    environment {
        SONAR_HOME= tool "Sonar"           
    }
    stages {
        stage("Code"){
            steps{
                echo "Cloning the code from GitHub repo"
                git url: "https://github.com/mofiazdevops/mern-app.git", branch: "main"
            }
        }
        stage("SonarQube Quality Analysis "){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=mern-app -Dsonar.projectKey=mern-app"
                }
            }
        }
        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Sonar Quality Gates"){
            steps{
                timeout(time: 2, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File system Scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("Build"){
            steps {
                echo "build the docker image "
            }
        }
        stage("Test"){
            steps{
                echo "Testing the code "
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the app with docker-compsose "
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
