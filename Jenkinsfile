node{
    stage('SCM Checkout'){
        git branch: 'main', url: 'https://github.com/T-Santhosh/multi-branch.git'

    }

    stage('Compile Package'){
        def mvnHome =  tool name: 'maven3', type: 'maven'
        sh "${mvnHome}/bin/mvn clean package"
        sh 'mv target/onlinebookstore.war target/newapp.war'
    }
    stage('Build Docker Image'){
        sh 'docker build -t myimg/latest .'
    }
}
