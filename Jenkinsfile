pipeline{
    agent none
    stages {
        stage('Clone Repo') {
            agent {
                label 'kubepod'
            }
            steps {
                // Get some code from a GitHub repository
//                 git 'https://github.com/huynq22/devopsJenkinDemo.git'
                sh 'git --version'
                git 'https://github.com/huynq22/spring-mysql-k8s.git'               
            }
        }    
        stage('Build Project') {
//             agent {
//                 docker {
//                     image 'huyfinn98/maven-tool'
//                 }
//             }
            agent {
                kubernetes {
                  yaml '''
                    apiVersion: v1
                    kind: Pod
                    spec:
                      containers:
                      - name: maven
                        image: maven:alpine
                        command:
                        - cat
                        tty: true
                    '''
                }
            }
            steps{
                // build project via maven
                sh 'mvn -version'
                sh "mvn clean install"
            }
        }
        stage('Build Docker Image') {
            agent {
                docker {
                    image 'huyfinn98/maven-tool'
                }
            }
            steps{
                script{
                    // build docker image
                    docker.build("myspring:${env.BUILD_NUMBER}")
                }
            }
        }
//         stage('Credential For GCR'){
//             agent {
//                 label 'kubepod'
//             }
//             steps{
//                 script {
//                     withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-ui', vaultUrl: 'http://34.101.203.241:8200'], 
//                     vaultSecrets: [[engineVersion: 1, path: 'kv/gcloud/auth', secretValues: [[envVar: 'auth', vaultKey: 'config-file']]]]) {
//                         writeFile file: 'keyfile.json', text: "${auth}"
//                         sh 'gcloud auth activate-service-account 267009820763-compute@developer.gserviceaccount.com --key-file=keyfile.json'
//                     }
// //                     sh "docker tag myspring:${env.BUILD_NUMBER} asia.gcr.io/concise-orb-329505/myspring:${env.BUILD_NUMBER}"
// //                     sh "docker push asia.gcr.io/concise-orb-329505/myspring:${env.BUILD_NUMBER}"
//                 }
//             }
//         }
        stage('Push To GCR'){
            agent {
                docker {
                    image 'huyfinn98/maven-tool'
                }
            }
            steps{
                script {
                    withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-ui', vaultUrl: 'http://34.101.203.241:8200'], 
                    vaultSecrets: [[engineVersion: 1, path: 'kv/gcloud/auth', secretValues: [[envVar: 'auth', vaultKey: 'config-file']]]]) {
                        writeFile file: 'keyfile.json', text: "${auth}"
                        sh 'gcloud auth activate-service-account 267009820763-compute@developer.gserviceaccount.com --key-file=keyfile.json'
                    }
                    sh "docker tag myspring:${env.BUILD_NUMBER} asia.gcr.io/concise-orb-329505/myspring:${env.BUILD_NUMBER}"
                    sh "docker push asia.gcr.io/concise-orb-329505/myspring:${env.BUILD_NUMBER}"
                }
            }
        }
//         stage('Deploy To Kubernetes'){
//             agent{
//                 label 'kubepod'
//             }
//             steps{
//                 script{
//                     // kubernetesDeploy(configs: "k8s-deploys/mysql-pv.yaml", kubeconfigId: "mykubeconfig")
//                     withKubeConfig([credentialsId: 'jenkins-kind', serverUrl: 'https://35.186.156.110', namespace: 'jenkins']) {
//                         sh 'kubectl apply -f spring-config.yaml -n jenkins'
//                         sh 'kubectl apply -f k8s-deploys/mysql-pv.yaml -n jenkins'
//                         sh 'kubectl apply -f k8s-deploys/mysql-deployment.yaml -n jenkins'
//                         sh 'sed -i "s|asia.gcr.io/concise-orb-329505/myspring:v2|asia.gcr.io/concise-orb-329505/myspring:${env.BUILD_NUMBER}|g" k8s-deploys/spring.yaml'
//                         sh 'kubectl apply -f k8s-deploys/spring.yaml -n jenkins'
//                     }       
//                 }
//             }
//         }
//         stage('Push To DockerHub'){
//             steps{
//                 // login to docker hub and push Image
//                 withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'jenkins-dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
//                     sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
//                     sh "docker tag devopsexample:${env.BUILD_NUMBER} huyfinn98/devopsexample:${env.BUILD_NUMBER}"
//                     sh "docker push huyfinn98/devopsexample:${env.BUILD_NUMBER}"
//                 }
//             }
//         }
        // stage('Pull Image From Dockerhub'){
        //     steps {
        //         // get image from dockerhub
        //         sh "docker pull huyfinn98/devopsexample:${env.BUILD_NUMBER}"
        //         sh "docker logout"
        //     }
        // }
        
    }
}
