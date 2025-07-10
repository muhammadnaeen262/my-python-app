// pipeline {
//     agent any

//     environment {
//         VENV_DIR = ".venv"
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Setup Python') {
//             steps {
//                 sh 'python3 -m venv $VENV_DIR'
//                 sh './$VENV_DIR/bin/pip install -r requirements.txt'
//             }
//         }

//         stage('Run Tests') {
//             steps {
//                 sh './$VENV_DIR/bin/pytest'
//             }
//         }
//         // stage('new Test') {
//         //     steps {
//         //         sh './$VENV_DIR/bin/pytest tests/test_new.py'
//         //     }
//         // }
//     }

//     post {
//         always {
//             echo "Pipeline completed."
//         }
//     }
// }

pipeline {
    agent any

    environment {
        IMAGE_NAME = "mnaiem262/my-python-app"
        DOCKER_CREDENTIALS_ID = "dockerhub"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    COMMIT_SHA = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    IMAGE_TAG = "${IMAGE_NAME}:${COMMIT_SHA}"
                    env.IMAGE_TAG = IMAGE_TAG
                }
                sh 'docker build -t $IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${docker_Credential}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push $IMAGE_TAG
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Done: $IMAGE_TAG"
        }
    }
}
