node {
    
    // Use the Maven tool configured in Jenkins (must exist)
    def mvnHome = tool 'maven-latest'

    // Docker image name & tag for push
    def imageName = "devopse-demo"
    def repo = "vickeyyvickey/${imageName}"
    def tag = "${env.BUILD_NUMBER}"
    def fullImageName = "${repo}:${tag}"

    stage('Clone Repo') {
        // Clone your repo
        git 'https://github.com/faiz1487/DevOps-ci-example.git'
        mvnHome = tool 'maven-latest'
    }

    stage('Build Project') {
        // Build using Maven (Skip tests for speed)
        sh "'${mvnHome}/bin/mvn' clean package -DskipTests"
    }

    stage('Build Docker Image') {
        // Build Docker Image using jar from target/
        sh "docker build -t ${fullImageName} ."
    }

    stage('Docker Login') {
        // Login using stored credentials (NEVER hardcode password)
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            sh "echo $PASS | docker login -u $USER --password-stdin"
        }
    }

    stage('Docker Push') {
        // Push to Docker Hub
        sh "docker push ${fullImageName}"
    }

    stage('Cleanup') {
        // Removes workspace files after build
        cleanWs()
    }
}
