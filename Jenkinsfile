pipeline {
    agent {label "Linux"}
    parameters {
	choice(choices: 'build\ndev\nqa',name:'Stage')  
	}
	stages {
        stage('scm-checkout') {
                steps {
                        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/deepakbhattarai/maven-project.git']]])
                  }
        }
        stage('build'){
				steps {
					script {
						if (Stage in ["build", "dev"]){		                
							sh 'mvn clean install'
						}
					}
				}
			}
        stage('archive'){
                steps {
                        archiveArtifacts '**/target/*.war'
            }
        }
        stage('upload to artifactory'){
                steps {
                        sh 'curl -X PUT -u admin:APTMavrHJnqV2XY -T ${WORKSPACE}/webapp/target/webapp.war "http://192.168.50.8:8090/artifactory/libs-release-local/com/mkyong/CounterWebApp/webapp-1.0.war"'
            }
        }
        stage('deploy to tomcat from ansible playbook'){
                steps {
                    sh 'ansible-playbook deploy-tomcat-ansible.yml'
					echo 'deploying war file'
                }
        }
	}
}