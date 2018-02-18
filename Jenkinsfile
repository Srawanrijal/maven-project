pipeline {
    agent {label "master"}
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
                        sh 'curl -X PUT -u admin:APBRGkS37r4D8ndDPnqbAb8yUUe -T ${WORKSPACE}/webapp/target/webapp.war "http://http://192.168.0.184:8081/artifactory/libs-release-local/com/mkyong/CounterWebApp/webapp-1.0.war"'
            }
        }
        stage('deploy to tomcat from ansible playbook'){
                steps {
                     ansiblePlaybook credentialsId: 'dbab669b-5a6a-47ff-87fc-76a8ca81d442', inventory: '/etc/ansible/hosts', playbook: 'deploy-tomcat-ansible.yml', sudoUser: null
					echo 'deploying war file'
                }
        }
	}
}
