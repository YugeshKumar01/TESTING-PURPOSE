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

        # Check if deployment.yml exists
        if [ -f "deployment.yml" ]; then
            echo "deployment.yml found."
        else
            echo "deployment.yml not found!"
            exit 1
        fi

        # Show the content of deployment.yml before making changes
        echo "Content of deployment.yml before change:"
        cat deployment.yml

        # Update deployment.yml with the build number
        BUILD_NUMBER=${BUILD_NUMBER}
        echo "Replacing 'replaceImageTag' with ${BUILD_NUMBER} in deployment.yml"
        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" deployment.yml

        # Check if deployment.yml was modified
        echo "Content of deployment.yml after change:"
        cat deployment.yml

        # Check for any changes using git
        if git diff --quiet; then
            echo "No changes detected in deployment.yml"
            exit 0
        else
            echo "Changes detected in deployment.yml"
        fi

        # Show git status before committing
        echo "Git status before commit:"
        git status

        # Configure git and commit changes
        git config user.email "yugeshkumar6202@gmail.com"
        git config user.name "YugeshKumar01"
        git add deployment.yml
        git commit -m "Update deployment image to version ${BUILD_NUMBER}"

        # Show git status after committing
        echo "Git status after commit:"
        git status

        # Push changes
        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main

        # Show the latest git log
        echo "Latest git log:"
        git log -1
    '''
}

                    }
                }
            }
        }
    }
}
