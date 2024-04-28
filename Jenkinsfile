pipeline {
    agent any

    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}


    stages {

        stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}           
    //    stage('Test microservice catalog') {
    //      agent {
    //        docker {
    //          image 'golang:1.20.1'
    //          args '-u 0:0'
    //        }
    //       }
    //        steps {
    //            sh '''
    //        cd $WORKSPACE/REVIVE/src/catalog/
    //        go test
    //            '''
    //        }
    //    }           
    //   stage('Test maven-cart') {
	//    agent {
    //       docker {
    //         image 'maven:3.8.7-openjdk-18'
    //       }
    //      }
    //       steps {
    //           sh '''
    //       cd $WORKSPACE/REVIVE/src/cart/
    //       mvn  test  -Dmaven.test.skip=true --quiet
    //           '''
    //       }
    //   }
    //       
    //       
    //   
//
    //  stage('Test maven-orders') {
	//    agent {
    //      docker {
    //        image 'maven:3.8.7-openjdk-18'
    //      }
    //     }
    //      steps {
    //          sh '''
    //      cd $WORKSPACE/REVIVE/src/orders/
    //      mvn   test -Dmaven.test.skip=true --quiet 
    //          '''
    //      }
    //  }
    //  
    //  stage('Test maven-ui') {
	//    agent {
    //      docker {
    //        image 'maven:3.8.7-openjdk-18'
    //      }
    //     }
    //      steps {
    //          sh '''
    //      cd $WORKSPACE/REVIVE/src/ui/
    //      mvn  test -Dmaven.test.skip=true --quiet 
    //          '''
    //      }
//
    //  }
    //  
    //   stage('Test maven-node') {
	//     agent {
    //       docker {
    //         image 'node'
    //       }
    //      }
    //       steps {
    //           sh '''
    //       cd $WORKSPACE/REVIVE/src/checkout/
    //       npm install
    //      
    //           '''
    //       }
    //   }
//
    //    stage('SonarQube analysis') {
    //            agent {
    //                docker {
    //                  image 'fridade/sonar-scanner-revive:v1.0.0'
    //                }
    //               }
    //               environment {
    //        CI = 'true'
    //        scannerHome='/opt/sonar-scanner'
    //    }
    //            steps{
    //                withSonarQubeEnv('sonar') {
    //                    sh "${scannerHome}/bin/sonar-scanner"
    //                }
    //            }
    //        }
    //    stage("Quality Gate") {
    //            steps {
    //              timeout(time: 1, unit: 'HOURS') {
    //                waitForQualityGate abortPipeline: true
    //              }
    //            }
    //          } 
//

  //      stage('Build microservice catalog ') {//
	     agent {
  //         docker {
  //           image 'golang'
  //           args '-u 0:0'
  //         }
  //        }
  //         steps {
  //             sh '''
  //         cd $WORKSPACE/REVIVE/src/catalog/
  //         go build   -buildvcs=false
  //             '''
  //         }
  //      }
  //      stage('build maven-orders') {//
  //	      agent {
  //          docker {
  //            image 'maven:3.8.7-openjdk-18'
  //          }
  //         }
  //          steps {
  //              sh '''
  //          cd $WORKSPACE/REVIVE/src/orders/
  //          mvn  package -Dmaven.test.skip=true --quiet 
  //              '''
  //          }
  //      }
  //      stage('build  maven-carts') {//
  //	      agent {
  //          docker {
  //            image 'maven:3.8.7-openjdk-18'
  //          }
  //         }
  //          steps {
  //              sh '''
  //          cd $WORKSPACE/REVIVE/src/cart/
  //          mvn  package -Dmaven.test.skip=true --quiet 
  //              '''
  //          }
  //      }
  //      stage('build maven-ui') {//
  //	      agent {
  //          docker {
  //            image 'maven:3.8.7-openjdk-18'
  //          }
  //         }
  //          steps {
  //              sh '''
  //          cd $WORKSPACE/REVIVE/src/ui/
  //          mvn  package -Dmaven.test.skip=true --quiet 
  //              '''
  //          }
  //      }
  //   stage('Build checkout') {
  //      agent {
  //          docker {
  //              image 'node' 
  //              args '-u 0:0' 
  //          }
  //      }
  //      steps {
  //          dir("${WORKSPACE}/REVIVE/src/checkout/") {
  //      
  //          sh '''
  //          npm install
  //          npm run build
  //          '''
  //          }
  //      }
 //   

    stage('Build-images-ui') {
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/ui/
            docker build -t  fridade/ui:jenkins-$BUILD_NUMBER .
                '''
            }

    }
    

      stage('Build-images-orders') {
            steps {
                sh '''
           cd $WORKSPACE/REVIVE/src/orders/
           docker build  -t fridade/orders:jenkins-$BUILD_NUMBER .
           docker build -f Dockerfile-rabbitmq  -t fridade/orders-rabbitmq:jenkins-$BUILD_NUMBER .
           docker build -f Dockerfile-mysql  -t fridade/orders-mysql:jenkins-$BUILD_NUMBER .
                '''
            }
        }


      stage('Build-images-checkout') {
            steps {
                sh '''
          cd $WORKSPACE/REVIVE/src/checkout/
          docker build -t  fridade/checkout:jenkins-$BUILD_NUMBER .
          docker build -f Dockerfile-redis  -t fridade/checkout-redis:jenkins-$BUILD_NUMBER .
                '''
            }
        }


     stage('Build-images-cart') {
            steps {
                sh '''
         cd $WORKSPACE/REVIVE/src/cart/
         docker build -t  fridade/cart:jenkins-$BUILD_NUMBER .
         docker build -f Dockerfile-dynamodb -t fridade/cart-dynamodb:jenkins-$BUILD_NUMBER .
                '''
            }
        }


    stage('Build-images-catalog') {
            steps {
                sh '''
         cd $WORKSPACE/REVIVE/src/catalog/
         docker build -t  fridade/catalog:jenkins-$BUILD_NUMBER .
         docker build -f Dockerfile-mysql -t fridade/catalog-mysql:jenkins-$BUILD_NUMBER .
                '''
            }
        }

    stage('Build-images-assets') {
                steps {
                    sh '''
         cd $WORKSPACE/REVIVE/src/assets/
         docker build -t  fridade/assets:jenkins-$BUILD_NUMBER .
         cd $WORKSPACE
                    '''
                }
            }
    
    stage('Push-ui') {
          
            steps {
               sh 'docker push fridade/ui:jenkins-$BUILD_NUMBER'
            }
    }
            

    stage('Push-orders') {
        // Push-orders stage
        steps {
           
            sh '''
                docker push fridade/orders:jenkins-$BUILD_NUMBER 
                docker push fridade/orders-rabbitmq:jenkins-$BUILD_NUMBER 
                docker push fridade/orders-mysql:jenkins-$BUILD_NUMBER 
            '''
                }
            }
      
    

    stage('Push-checkout') {
        // Push-checkout stage
        steps {
            
          sh '''
              docker push fridade/checkout:jenkins-$BUILD_NUMBER 
              docker push fridade/checkout-redis:jenkins-$BUILD_NUMBER 
          '''
        }
           
    }
    

    stage('Push-cart') {
        // Push-cart stage
        steps {
           
            sh '''
                docker push fridade/cart:jenkins-$BUILD_NUMBER 
                docker push fridade/cart-dynamodb:jenkins-$BUILD_NUMBER
            '''
        }
            
        }
    

    stage('Push-catalog') {
        // Push-catalog stage
        steps {
            
          sh '''
              docker push fridade/catalog:jenkins-$BUILD_NUMBER 
              docker push fridade/catalog-mysql:jenkins-$BUILD_NUMBER 
          '''
        }
    }
        
    

    stage('Push-assets') {
        // Push-assets stage
        steps {
            
          sh 'docker push fridade/assets:jenkins-$BUILD_NUMBER'
        }
    }

    stage('helm-charts') {


	      steps {
	        script {
	          withCredentials([
	            string(credentialsId: 'github_token', variable: 'TOKEN')
	          ]) {

	            sh '''
                rm -rf revive-helm-chart || true 
                git clone https://github.com/fridade/revive-helm-chart.git
                cd revive-helm-chart
cat << EOF > dev-values.yaml
       repository:
         tag:   jenkins-$BUILD_NUMBER
         assets:
          image: fridade/assets
       
         carts:
          image: fridade/cart
       
         cart_dynamodb:
          image: fridade/cart-dynamodb
        
         catalog:
          image: fridade/catalog
       
        
         catalog_mysql:
          image: fridade/catalog-mysql
       
         checkout:
          image: fridade/checkout
       
         checkout_redis:
          image: fridade/checkout-redis
       
         orders:
          image: fridade/orders
       
         orders_mysql:
          image: fridade/orders-mysql
       
         orders_rabbitmq:
          image: fridade/orders-rabbitmq 
       
         ui:
          image: fridade/ui
EOF
git config --global user.name "fridade"
git config --global user.email "info@fridade.com"
   cat  dev-values.yaml
   
   git add -A 
    git commit -m "Change from JENKINS" 
    git push  https://fridade:$TOKEN@github.com/fridade/revive-helm-chart.git
	            '''
	          }

	        }

	      }

	    }


    }

   post {
       success {
           slackSend color: '#2EB67D',
           channel: 'terraform', 
           message: "*Alpha Project Build Status*" +
           "\n Project Name: Alpha" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : *SUCCESS*" +
           "\n Build url : ${env.BUILD_URL}"
       }
       failure {
           slackSend color: '#E01E5A',
           channel: 'terraform',  
           message: "*Alpha Project Build Status*" +
           "\n Project Name: Alpha" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : *FAILED*" +
           "\n Build User : *Tia*" +
           "\n Action : Please check the console output to fix this job IMMEDIATELY" +
           "\n Build url : ${env.BUILD_URL}"
       }
       unstable {
           slackSend color: '#ECB22E',
           channel: 'del-uk', 
           message: "*Alpha Project Build Status*" +
           "\n Project Name: Alpha" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : *UNSTABLE*" +
           "\n Action : Please check the console output to fix this job IMMEDIATELY" +
           "\n Build url : ${env.BUILD_URL}"
       }   
   
  }
}
}