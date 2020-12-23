pipeline {
    agent {
    docker {
            image '${DOCKER_COMPILER_IMAGE}'
        }
    }
    
    parameters {
        string(name: 'GIT_PROJECT', defaultValue: 'http://192.168.0.13/x750618/jsf2-hello-world.git', description: 'GIT PROJECT')
        string(name: 'DOCKER_COMPILER_IMAGE', defaultValue: 'openjdk:8-jdk-alpine-jav-maven', description: 'docker image for build')
        string(name: 'NEXUS_VERSION', defaultValue: 'nexus3', description: '')
        string(name: 'NEXUS_PROTOCOL', defaultValue: 'http', description: '')
        string(name: 'NEXUS_URL', defaultValue: '192.168.0.13:8081', description: '')
        string(name: 'NEXUS_REPOSITORY', defaultValue: 'repository-example', description: '')
        string(name: 'NEXUS_CREDENTIAL_ID', defaultValue: 'jenkinsnexus', description: '')
        string(name: 'SONAR_PROJECT', defaultValue: 'my:project', description: '')
        string(name: 'SONAR_URL', defaultValue: 'http://192.168.0.13:9000', description: '')
        string(name: 'SONAR_LOGIN', defaultValue: '6216dadeedca1e1e0ccedbb64fc2d3933d6c346d', description: '')
    }
    
    stages
    {
        stage('git') {
            steps {
                git '${GIT_PROJECT}'
            }
        }
    
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner';
                    withSonarQubeEnv("sonarqube") {
                        sh "${tool("SonarQubeScanner")}/bin/sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT} \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=. \
                        -Dsonar.host.url=${SONAR_URL} \
                        -Dsonar.login=${SONAR_LOGIN}"
                    }
                }
            }
        }

        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Promote to DEV Env') {
            steps {
                echo "Promote to dev"
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
                echo "Sin implemantar aun en servidor de aplicaciones"
            }    
            
        }
    }
}
