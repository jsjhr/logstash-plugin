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
        string(name: 'NEXUS_VERSION', defaultValue: 'nexus3', description: '')
        string(name: 'NEXUS_PROTOCOL', defaultValue: 'http', description: '')
        string(name: 'NEXUS_URL', defaultValue: '10.128.0.3:8081', description: '')
        string(name: 'NEXUS_REPOSITORY', defaultValue: 'repository-example', description: '')
        string(name: 'NEXUS_CREDENTIAL_ID', defaultValue: 'jenkinsnexus', description: '')
      
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
        
        stage("publish to nexus") {
            steps {
                script {
                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
                    pom = readMavenPom file: "pom.xml";
                    // Find built artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    // Print some info from the artifact found
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    // Extract the path from the File found
                    artifactPath = filesByGlob[0].path;
                    // Assign to a boolean response verifying If the artifact name exists
                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );

                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                   }

                }
            }
        }
        
        stage('run') { 
            steps { 
                echo "Sin implemantar aun en Jenkins"
            }    
            
        }
    }
}
