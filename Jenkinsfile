pipeline { 
    agent { 
        docker { 
            // Image containing Maven and Git 
            image 'my-maven-git:latest' 
            // To reuse local Maven cache between builds 
            args '-v $HOME/.m2:/root/.m2' 
        } 
    } 
 
    stages { 
        stage('Checkout') { 
            steps { 
                echo '========================================='
                echo 'Stage: Checkout - Cloning Git repository'
                echo '========================================='
                
                // Clean the directory 
                sh "rm -rf *" 
                
                // Checkout the Git repository 
                sh "git clone https://github.com/simoks/java-maven.git" 
                
                echo 'Checkout completed successfully!'
            } 
        } 
        
        stage('Build') { 
            steps {
                echo '========================================='
                echo 'Stage: Build - Compilation and Tests'
                echo '========================================='
                
                script { 
                    def currentDir = pwd() 
                    echo "Current directory: ${currentDir}" 
                     
                    // Navigate to the directory containing the Maven project 
                    dir('java-maven/maven') { 
                        echo 'Running Maven clean test package...'
                        
                        // Run Maven commands 
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
                    dir('java-maven/maven') {
                        echo 'Launching Java application...'
                        
                        // Run the JAR file
                        sh "java -jar target/maven-0.0.1-SNAPSHOT.jar"
                        
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
