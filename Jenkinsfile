pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/YugeshKumar01/TESTING-PURPOSE.git'
        GIT_USER_NAME = 'YugeshKumar01'
        GIT_USER_EMAIL = 'yugeshkumar6202@gmail.com'
        BRANCH_NAME = 'main' // Change if using a different branch
        GIT_CREDENTIALS_ID = 'github' // The ID of the Jenkins credential you created
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir() // Clean the workspace directory
            }
        }

        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}", branch: "${BRANCH_NAME}", credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }

        stage('Update File') {
            steps {
                script {
                    def BUILD_NUMBER = env.BUILD_NUMBER ?: '1'
                    
                    sh """
                        echo "Replacing 'replaceImageTag' with ${BUILD_NUMBER} in deployment.yml"
                        sed -i 's/replaceImageTag/${BUILD_NUMBER}/g' deployment.yml
                        
                        # Show updated file content
                        cat deployment.yml
                    """
                }
            }
        }

        stage('Commit and Push') {
            steps {
                script {
                    sh """
                        # Configure git
                        git config user.name "${GIT_USER_NAME}"
                        git config user.email "${GIT_USER_EMAIL}"
                        
                        # Add changes to the git index
                        git add deployment.yml
                        
                        # Commit changes
                        git commit -m "Update deployment image to version ${BUILD_NUMBER}" || echo "No changes to commit"
                        
                        # Push changes with authentication
                        git push https://${GIT_USER_NAME}:${GIT_PASSWORD}@github.com/${GIT_USER_NAME}/TESTING-PURPOSE.git ${BRANCH_NAME}
                    """
                }
            }
        }
    }
}
