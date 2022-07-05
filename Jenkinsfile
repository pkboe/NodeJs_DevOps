pipeline{
    agent{
        label "nodejs"
    }
    stages{
        stage("Prebuild_Notification"){
            steps{
                echo "========executing Prebuild_Notification========"
                emailext body: '''<html>
                             <body>
                                <h1>Prebuild Notification</h1>
                                <p><b>Build ID:</b> $BUILD_ID
                                <p href="$JOB_URL" ><b>Build URL:</b> $JOB_URL/$BUILD_ID
                                The Build has been initiated by $BUILD_USER_ID
                                </p>                        
                             '''
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Prebuild_Notification executed successfully========"
                }
                failure{
                    echo "========Prebuild_Notification execution failed========"
                }
            }
        }

        stage('Git_Checkout'){
            steps{
                echo "========executing Git_Checkout========"
                sh 'git clone $GIT_REPO_URL $GIT_REPO_NAME'
                sh 'cd $GIT_REPO_NAME'
                sh 'git checkout $GIT_BRANCH'
                sh 'git pull'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Git_Checkout executed successfully========"
                }
                failure{
                    echo "========Git_Checkout execution failed========"
                }
            }
        }
        stage('Build_Node_Project'){
            steps{
                echo "========executing Build_Node_Project========"
                sh 'npm install'
                sh 'npm run build'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Build_Node_Project executed successfully========"
                }
                failure{
                    echo "========Build_Node_Project execution failed========"
                }
            }
        }
        stage('Docker_Build'){
            steps{
                echo "========executing Docker_Build========"
                sh 'docker build -t $DOCKER_IMAGE_NAME .'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Docker_Build executed successfully========"
                }
                failure{
                    echo "========Docker_Build execution failed========"
                }
            }
        }
        stage('Docker_Export_Image'){
            steps{
                echo "========executing Docker_Export_Image========"
                sh 'docker save $DOCKER_IMAGE_NAME > $DOCKER_IMAGE_NAME.tar'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Docker_Export_Image executed successfully========"
                }
                failure{
                    echo "========Docker_Export_Image execution failed========"
                }
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}