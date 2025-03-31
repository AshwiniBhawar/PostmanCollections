pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                echo "Building the war"
            }
        }

        stage("Deploy to QA") {
            steps {
                echo "Deploying to QA"
            }
        }
		
        stage('Checkout') {
            steps {
                git url: 'https://github.com/AshwiniBhawar/GoRestAPICiCD'
            }
        }

        stage('Pull Docker Images') {
			parallel{
				stage('Pull GoRest Image') {
                    steps {
                        bat 'docker pull ashwinibhawar2892/gorestapicicd:1.0'
                    }
				}	
				stage('Pull ContactDD Image') {
					steps {
                        bat 'docker pull ashwinibhawar2892/api_contactdd:1.0'
                    }
				}	
			}
        }

        stage('Run API Test Cases in parallel') {
			parallel{
				stage('Run GoRest Cases') {
                    steps {
                        bat 'docker run -v "%cd%\\newman:/app/newman" ashwinibhawar2892/gorestapicicd:1.0'
                    }
				}
				stage('Run ContactDD Cases') {
					steps {
                        bat 'docker run -v "%cd%\\newman:/app/newman" ashwinibhawar2892/api_contactdd:1.0'
            		}
				}
			}
        }

        stage('Publish HTML Extra Reports') {
			parallel{
				stage('Publish GoRest Report') {
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: '*.html',
                            reportName: 'GoRestAPIReport',
                            reportTitles: ''
                        ])               
					}
				}
				stage('Publish ContactDD Report') {
					steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: '*.html',
                            reportName: 'ContactDDReport',
                            reportTitles: ''
                        ])               
					}
				}
			}
        }

        stage("Deploy to PROD") {
            steps {
                echo "Deploying to PROD"
            }
        }
    }
}