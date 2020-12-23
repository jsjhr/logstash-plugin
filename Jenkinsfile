pipeline {
    agent {
    docker {
            image '${DOCKER_COMPILER_IMAGE}'
        }
    }
    
    parameters {
        string(name: 'GIT_PROJECT', defaultValue: 'http://192.168.0.13/x750618/jsf2-hello-world.git', description: 'GIT PROJECT')
        string(name: 'DOCKER_COMPILER_IMAGE', defaultValue: 'openjdk:8-jdk-alpine-jav-maven', description: 'docker image for build')
    }
    
    stages
    {
        stage('git') {
            steps {
                git '${GIT_PROJECT}'
            }
        }
    
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('run') { 
            steps { 
                echo "Sin implemantar aun en Jenkins"
            }    
            
        }
    }
}
