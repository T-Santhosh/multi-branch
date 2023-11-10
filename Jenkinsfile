pipeline {
    agent any
    stages {
        stage ('git checkout'){
            steps{
                script{
                }
            }
        }
        stage ('Build') {
            steps {
                script{
                     sh 'npm install'
                }
            }
        }
        // stage ('SonarQube Analysis') {
        //     steps {
        //         script { 
        //             def scannerHome = tool 'sonarscanner4'
        //             withSonarQubeEnv('sonar-pro') {
        //                 sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=node.js-app"
        //             }
        //         }
        //     }
        // }
        stage('Docker Build Images') {
            steps {
                script {
                    sh 'docker build -t santhosh2969/multi:v2 .'
                    sh 'docker images'
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
                        sh "docker login -u santhosh2969 -p ${dockerPassword}"
                        sh 'docker push santhosh2969/multi:v2'
                        sh 'docker rmi santhosh2969/multi:v2'
                    }
                }
            }
        }
        stage('Deploy on k8s') {
            steps {
                script {
                    withKubeCredentials(kubectlCredentials: [[ credentialsId: 'kubernetes', namespace: 'ms' ]]) {
                        sh 'kubectl apply -f kube.yaml'
                        sh 'kubectl get pods -o wide'
                    }
                }
            }
        }
    }
}
