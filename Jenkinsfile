pipeline {
    agent {
    docker {
            image '${DOCKER_COMPILER_IMAGE}'
        }
    }
    
    parameters {
        string(name: 'GIT_PROJECT', defaultValue: 'https://github.com/jsjhr/logstash-plugin.git', description: 'GIT PROJECT')
        string(name: 'DOCKER_COMPILER_IMAGE', defaultValue: 'openjdk:8-jdk-alpine-jav-maven', description: 'docker image for build')
        string(name: 'MAVEN_TARGETS', defaultValue: 'clean package -DskipTests', description: 'maven targets')
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
                sh 'mvn ${MAVEN_TARGETS}'
            }
        }
        
        stage('run') { 
            steps { 
                echo "Sin implemantar aun en Jenkins"
            }    
            
        }
    }
}
