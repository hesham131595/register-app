pipeline {
    agent {
       docker {
        //    image 'softinstigate/maven-aws'
            image 'maven:3.8.4-openjdk-11'
            args '--user root -v /var/lib/jenkins/workspace/register-app:/workspace -w /workspace'
        }
    

    //   args '-v /root/.m2:/root/.m2' // mount Docker socket to access the host's Docker daemon
        
    }
    // environment {
	//     APP_NAME = "register-app-pipeline"
    //         RELEASE = "1.0.0"
    //         DOCKER_USER = "ashfaque9x"
    //         DOCKER_PASS = 'dockerhub'
    //         IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    //         IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	//     JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    // }
    stages {

        // stage("Cleanup Workspace"){
        //         steps {
        //         cleanWs()
        //         }
        // }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main',  url: 'https://github.com/hesham131595/register-app'
                }
        }

         stage('Check Docker Workspace') {
            steps {
                sh 'ls -la'
                sh 'pwd'
                sh 'whoami'
            }
        }

        stage("Build Application"){
            steps {
                sh 'mvn clean package'
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
        stage("Build & Push Docker Image") {
            // agent {
            //     docker {
            //        image 'amazon/aws-cli'
            //     }
            // }
            steps {
                script {
                    sh "apt update &&  apt upgrade -y"
                    sh "apt install awscli -y"
                    sh "apt install docker.io -y"
                    sh   "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/y8h8o1j3"
                    sh   "docker build -t hesham-repo ."
                    sh   "docker tag hesham-repo:latest public.ecr.aws/y8h8o1j3/hesham-repo:latest"
                    sh   "docker push public.ecr.aws/y8h8o1j3/hesham-repo:latest"
                    }
            }
        }
    }

}
//        stage("SonarQube Analysis"){
//            steps {
// 	           script {
// 		        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
//                         sh "mvn sonar:sonar"
// 		        }
// 	           }
//            }
//        }

//        stage("Quality Gate"){
//            steps {
//                script {
//                     waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
//                 }
//             }

//         }

     


                    // def ecrRepository = 'public.ecr.aws/y8h8o1j3'
                    // def repoName = 'hesham-repo'
                    // def imageName = 'maven-H'
                    // def imageTag = 'latest'
                    // def ecrregister = 'public.ecr.aws/y8h8o1j3'
                    // def dockerImageName = "${ecrRepository}/maven-H"
                    // def dockerImageTag = 'latest' 
                    // sh "docker build -t ${imageName}:${imageTag} ."
                    // sh "docker tag ${ImageName}:${ImageTag} ${ecrRepository}/${dockerImageName}:${dockerImageTag}"
                    // sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ecrregister}'
                    //  sh 'docker push ${ecrRepository}/${dockerImageName}:${dockerImageTag}'
                    // sh "docker push ${dockerImageName}:${dockerImageTag}"
                    // docker.withRegistry('',DOCKER_PASS) {
                    //     docker_image = docker.build "${IMAGE_NAME}"
                    // }

                    // docker.withRegistry('',DOCKER_PASS) {
                    //     docker_image.push("${IMAGE_TAG}")
                    //     docker_image.push('latest')
    

//        stage("Trivy Scan") {
//            steps {
//                script {
// 	            sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ashfaque9x/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
//                }
//            }
//        }

//        stage ('Cleanup Artifacts') {
//            steps {
//                script {
//                     sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
//                     sh "docker rmi ${IMAGE_NAME}:latest"
//                }
//           }
//        }

//        stage("Trigger CD Pipeline") {
//             steps {
//                 script {
//                     sh "curl -v -k --user clouduser:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-13-232-128-192.ap-south-1.compute.amazonaws.com:8080/job/gitops-register-app-cd/buildWithParameters?token=gitops-token'"
//                 }
//             }
//        }
//     }

//     post {
//        failure {
//              emailext body: '''${SCRIPT, template="groovy-html.template"}''',
//                       subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed",
//                       mimeType: 'text/html',to: "ashfaque.s510@gmail.com"
//       }
//       success {
//             emailext body: '''${SCRIPT, template="groovy-html.template"}''',
//                      subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful",
//                      mimeType: 'text/html',to: "ashfaque.s510@gmail.com"
//       }
//    }
// }
