pipeline{
	environment {

		registryCredential = 'dockerhub'
		dockerimage = 'siyad/petclinic'
		sonarhome = tool 'sonar'
	}

	agent any

		stages{

			// stage('SonarQube Code Analysis'){
            //           steps{
            //             	 withSonarQubeEnv('sonarqube'){
			// 					 sh './mvnw sonar:sonar'
            //                  }
            //                    timeout(time: 30, unit: 'MINUTES'){
            //                       waitForQualityGate abortPipeline: false
            //                  }
            //           }
            //         }

			stage('Building Docker image'){
				steps{
					
					sh'./mvnw clean install -P buildDocker'

				}
			}

			// stage('publishing docker image to Docker hub'){
			// 	steps{
			// 		script {
			// 			withDockerRegistry(credentialsId: registryCredential, url: ''){
			// 				docker.image(dockerimage).push("${BUILD_NUMBER}")
			// 				docker.image(dockerimage).push("latest")

			// 		     }
			// 		}
			// 	}


			// stage('Removing Docker image'){
			// 	steps{
					
			// 		sh'docker rmi ${dockerimage}:${BUILD_NUMBER}'

			// 	}
			// }

	// 		stage('SonarQube Code Analysis'){
    //                   steps{
    //                     dir("${env.WORKSPACE}"){
    //                     	 withSonarQubeEnv('ParkJockey SonarQube'){
    //                             gradlew('sonarqube', '--info')
    //                          }
    //                            timeout(time: 30, unit: 'MINUTES'){
    //                               waitForQualityGate abortPipeline: false
    //                          }
    //                     }
    //                   }
    //                 }


	// 		stage('Building Docker image'){
	// 			steps{
	// 				sh 'ls'
	// 					dir("${env.WORKSPACE}"){
	// 						gradlew('jibDockerBuild')

	// 					}

	// 			}
	// 		}

	// 		stage('publishing docker image to ecr'){
	// 			steps{
	// 				script {
	// 					withDockerRegistry(credentialsId: 'pet', url: 's'){
	// 						docker.image("siyad/petclinic").push("${env.BUILD_ID}")
	// 				     }
	// 				}
	// 			}
	// 		}

	// 	   

	// stage('Integration Test'){
	// 			steps{


	// 				script {

	// 					try {
                            
	// 						dir("${env.WORKSPACE}/integro-test"){

	// 							git([url: 'pett.git', branch: 'master', credentialsId: 'pet'])


	// 							gradlew('cucumber -Denv=localAccount -Dcucumber.options="-t @accountService"')

	// 						}
	// 					}
	// 					catch (Exception e) {
	// 						echo "Integration failed , but we continue"
	// 					}
	// 				}
	// 			}
	// 		}
	// 		stage('reports') {
	// 			steps {
	// 				dir("integro-test"){
	// 					script {
	// 						allure([
	// 							includeProperties: false,
	// 							jdk: '',
	// 							properties: [],
	// 							reportBuildPolicy: 'ALWAYS',
	// 							results: [[path: 'build/allure-results']]
	// 						])
	// 					}
	// 				}
	// 			}
	// 		}

	// 		  stage('Terraform- updating swagger file'){

    //         				steps {
    //         					script {

    //                     dir("${env.WORKSPACE}/infra-as-code"){
    //         						git([url: 'petclinic.git', branch: 'master', credentialsId: 'pet'])
    //         						sh "cd terraform-aws-apigateway && terraform init"
    //         						sh "cd terraform-aws-apigateway && bash run.sh ${env.WORKSPACE}/cicd/swagger.yaml"



    //         					  }
    //         					}
    //         				}
    //         			}
              }
	post {
		always{
			echo "success"
			//junit 'build/test-results/**/*.xml'
			//cleanWs()
		}
		failure{ //Send an email to the person that broke the build
              step([$class            : 'Mailer',
              notifyEveryUnstableBuild: true,
              recipients              : [emailextrecipients([[$class: 'CulpritsRecipientProvider'], [$class: 'RequesterRecipientProvider']])].join(' ')])
            }
          }
        }


