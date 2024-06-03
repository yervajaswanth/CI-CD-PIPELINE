pipeline {
    agent any
   
    tools{
        jdk "jdk17"
        maven "maven3"
    }
   
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/yervajaswanth/CI-CD-PIPELINE.git'
            }
        }
        stage('compile') {
            steps {
                sh "mvn compile"
               
            }
        }
        stage('test') {
            steps {
                sh "mvn test"
            }
        }
        stage('file system scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs.html ."
            }
        }
        stage('sonarcube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                   
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BoardGame -Dsonar.projectkey=BoardGame\
                -Dsonar.java.binaries=. '''
               
              }
            }
        }
        stage('quality gate') {
            steps {
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('build') {
            steps {
                sh "mvn package"
            }
        }
        stage('publish to nexus') {
            steps {
               withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                    sh " mvn deploy "
}
            }
        }
        stage('build nd tag docker image') {
            steps {
                script{
                withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh " docker build -t boardgame ."
                        sh "docker tag boardgame boardgame:latest"
                }
            }
           
            }
        }
        stage('docker scan') {
            steps {
                sh "trivy image --format table -o image-report.html boardgame:latest"
            }
        }
        stage('push docker image') {
            steps {
                script{
                withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh " docker push boardgame:latest"
                }
            }
           
            }
        }
       
        stage('deploy to k8s') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: ' https://172.31.15.229:6443') {
                    sh "kubectl apply -f deployment-service.yaml"
}
            }
        }
        stage('verifying deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: ' https://172.31.15.229:6443') {
                    sh "kubectl  get pods"
                    sh "kubectl get svc"
}
            }
        }
       
       
    }
}
