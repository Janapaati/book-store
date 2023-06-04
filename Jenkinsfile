pipeline {
    agent any
    
    environment {
        msname = "${params.microservice}"
    }
    stages {
        stage('clean WS') {
            steps {
                cleanWs()
            }
        }
        stage('scm checkout') {
            steps {
               git 'https://github.com/Janapaati/book-store.git' 
            }
        }
        stage('build-ms'){
            steps {
                sh '''
                    cd ${msname}
                    mvn clean install package
                '''
            }
        }
        stage('docker build & push'){
            steps{
                script{
                    if(Docker_build_and_push_required.toBoolean()){
                        stage('executing docker build & push'){
                              sh '''
                                cd ${msname}
                                pwd
                                ls -latr
                                docker build -t $msname:1.0 .
                                docker login -u=662906 -p=Naga@1995
                                docker tag $msname:1.0 662906/$msname:1.0
                                docker push 662906/$msname:1.0
                            '''
                        }
                    }
                    else{
                        echo "skipped docker build & push"
                    }
                }
            }    
        }
        stage('deploy in ec2'){
            steps{
                script{
                    if(Deploy_ec2.toBoolean()){
                        build job: 'book-store-deploy-ec2', parameters: [
                            string(name: 'microservice', value: "${msname}")
                        ]
                    }
                    else{
                        echo "deployment in EC2 skipped"
                    }
                }
            }
        }
        stage('deploy in k8s'){
            steps{
                script{
                    if(Deploy_k8s.toBoolean()){
                        build job: 'book-store-deploy-k8s', parameters: [
                            string(name: 'microservice', value: "${msname}")
                        ]
                    }
                    else{
                        echo "deployment in k8s skipped"
                    }
                }
            }
        }
    }
}
