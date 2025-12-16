pipeline { 
    agent { 
        docker { 
            // Image containing Maven and Git 
            image 'my-maven-git:latest' 
            // Reuse local Maven cache between builds 
            args '-v $HOME/.m2:/root/.m2' 
        } 
    } 
 
    stages { 
        stage('Checkout') { 
            steps { 
                echo '========================================='
                echo 'Stage: Checkout - Cloning repository'
                echo '========================================='
                
                // Clean workspace
                sh 'rm -rf *'

                // GitHub credentials (PAT)
                withCredentials([usernamePassword(
                    credentialsId: 'github-creds',
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_TOKEN'
                )]) {
                    sh '''
                        git clone https://${GIT_USER}:${GIT_TOKEN}@github.com/spring-guides/gs-maven.git
                    '''
                }

                echo 'Checkout completed successfully!'
            } 
        } 
        
        stage('Build') { 
            steps {
                echo '========================================='
                echo 'Stage: Build - Compilation and Tests'
                echo '========================================='
                
                script { 
                    echo "Current directory: ${pwd()}" 
                     
                    // Navigate to Maven project directory
                    dir('gs-maven/complete') { 
                        echo 'Running Maven clean test package...'
                        sh 'mvn clean test package'
                        echo 'Maven build completed successfully!'
                    } 
                }
            }
        }
        
        stage('Run Application') {
            steps {
                echo '========================================='
                echo 'Stage: Run - Running application'
                echo '========================================='
                
                script {
                    dir('gs-maven/complete') {
                        echo 'Launching Java application...'
                        sh 'java -jar target/gs-maven-0.1.0.jar'
                        echo 'Application executed successfully!'
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo '========================================='
            echo 'Pipeline executed with SUCCESS!'
            echo '========================================='
        }
        failure {
            echo '========================================='
            echo 'Pipeline FAILED!'
            echo '========================================='
        }
        always {
            echo '========================================='
            echo 'Cleanup and end of pipeline'
            echo '========================================='
        }
    }
}
