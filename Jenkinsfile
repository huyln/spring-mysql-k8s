pipeline{
    agent none
    // agent {
    //     docker {
    //         image 'huyfinn98/maven-tool'
    //     }
    // }
    stages {
        stage('Clone Repo') {
            agent {
                docker {
                    image 'huyfinn98/maven-tool'
                }
            }
            steps {
                // Get some code from a GitHub repository
                // git 'https://github.com/huynq22/devopsJenkinDemo.git'
                git 'https://github.com/huynq22/spring-mysql-k8s.git'
                
            }
        }    
        // stage('Build Project') {
        //     agent {
        //         docker {
        //             image 'huyfinn98/maven-tool'
        //         }
        //     }
        //     steps{
        //         // build project via maven
        //         sh 'mvn -version'
        //         sh "mvn clean install"
        //         // script{
        //         //     try{
        //         //         sh "mvn clean install"
        //         //     }
        //         //     catch (err){
        //         //         currentBuild.result = "ABORTED"
        //         //     }
                    
        //         // }
        //     }
        // }
        // stage('Build Docker Image') {
        //     agent {
        //         docker {
        //             image 'huyfinn98/maven-tool'
        //         }
        //     }
        //     steps{
        //         script{
        //             // build docker image
        //             docker.build("devopsexample:${env.BUILD_NUMBER}")
        //         }
        //     }
        // }
        stage('Deploy To Kubernetes'){
            agent{
                label 'kubepod'
            }
            steps{
                script{
                    // kubernetesDeploy(configs: "k8s-deploys/mysql-pv.yaml", kubeconfigId: "mykubeconfig")
                    withKubeConfig([credentialsId: 'jenkins-kind', serverUrl: 'https://35.186.156.110', namespace: 'jenkins']) {
                        sh 'kubectl apply -f spring-config.yaml'
                    }
//                     withCredentials([string(credentialsId: 'jenkins-kind', variable: 'FILE')]) {
//                         sh 'kubectl apply -f spring-config.yaml -n jenkins'
//                     }          
                }
            }
        }
        // stage('Push To DockerHub'){
        //     steps{
        //         // login to docker hub and push Image
        //         withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'jenkins-dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
        //             sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
        //             sh "docker tag devopsexample:${env.BUILD_NUMBER} huyfinn98/devopsexample:${env.BUILD_NUMBER}"
        //             sh "docker push huyfinn98/devopsexample:${env.BUILD_NUMBER}"
        //         }
        //     }
        // }
        // stage('Pull Image From Dockerhub'){
        //     steps {
        //         // get image from dockerhub
        //         sh "docker pull huyfinn98/devopsexample:${env.BUILD_NUMBER}"
        //         sh "docker logout"
        //     }
        // }
        
    }
}
