pipeline {
    agent {
    docker {
            image '${DOCKER_COMPILER_IMAGE}'
        }
    }
    
    parameters {
        string(name: 'GIT_PROJECT', defaultValue: 'https://github.com/jsjhr/logstash-plugin.git', description: 'GIT PROJECT')
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
