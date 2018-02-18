pipeline {
    agent {label "linux"}
    parameters {
	choice(choices: 'build\ndev\nqa',name:'Stage')  
	}
	stages {
        stage('scm-checkout') {
                steps {
                        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Srawanrijal/maven-project.git']]])
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
                        sh 'curl -X PUT -u admin:APBRGkS37r4D8ndDPnqbAb8yUUe -T ${WORKSPACE}/webapp/target/webapp.war "http://192.168.50.226:8081/artifactory/libs-release-local/com/mkyong/CounterWebApp/webapp-1.0.war"'
            }
        }
        stage('deploy to tomcat from ansible playbook'){
                steps {
                    sh 'ansible-playbook /opt/deploy-tomcat-ansible.yml -i /etc/ansible -f 5 --private-key /tmp/ssh1105626470053572756.key -u root'
					echo 'deploying war file'
                }
        }
	}
}
