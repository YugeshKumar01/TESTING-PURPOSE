pipeline {
    agent any

    environment {
        GIT_REPO_NAME = "TESTING-PURPOSE" // Replace with your actual repo name
        GIT_USER_NAME = "YugeshKumar01" // Replace with your GitHub username
    }

    stages {
        stage('Clone Repository') {
            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                    script {
                        // Clone the repository
                        sh '''
                            if [ ! -d "TESTING-PURPOSE" ]; then
                                git clone https://github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}.git
                            else
                                echo "Repository directory already exists."
                            fi
                        '''
                    }
                }
            }
        }

        stage('Update Deployment File') {
            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                    dir('TESTING-PURPOSE') {
                        script {
                            sh '''
                                # Ensure we are in the correct directory
                                echo "Current directory: $(pwd)"
                                ls -la

                                # Update deployment.yml with the build number
                                BUILD_NUMBER=${BUILD_NUMBER}
                                sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" deployment.yml

                                # Configure git and push changes
                                git config user.email "yugeshkumar6202@gmail.com" // Replace with your email
                                git config user.name "${GIT_USER_NAME}"
                                git add deployment.yml
                                git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                                git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                            '''
                        }
                    }
                }
            }
        }
    }
}
