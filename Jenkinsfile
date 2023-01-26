pipeline {
    agent any
    parameters { 
        choice(name: "file_name", choices: ["all", "c", "java", "python"], description: "Select the file you want to execute")
        string(name: "con_name", defaultValue: "project", description: "Name of the container")
        string(name: "port", defaultValue: "8998", description: "Port to map the container")
        string(name: "email_address", defaultValue: "yossi1999azu@gmail.com", description: "enter your email adddress")
    }
    stages {
        stage('GITHUB') {
            steps {
                git url: "https://github.com/yossiazu/jenkins_docker_project.git", branch: "main"
                sh 'pwd'
                sh 'ls -la'
            }
        }
        stage('look fo the file') {
            steps{ 
                sh'''
                if [ $file_name = "all" ]; then
                    cp all index.html
                elif [ $file_name = "java" ]; then
                    cp java index.html
                elif [ $file_name = "python" ]; then
                    cp python index.html
                elif [ $file_name = "c" ]; then
                    cp c index.html
                else
                    echo "Invalid file selected"
                    exit 1
                fi
                docker build -t my-apache2 .
                docker run -dit --name $con_name -p $port:80 my-apache2
                '''
            }
        }
    }
    post{
        success {
            mail to: "${email_address}",
                subject: "the build was successful",
                body: "the selected language is: ${file_name}"
        }
        failure {
            mail to: "${email_address}",
                subject: "the build failed",
                body: "The pipeline ${currentBuild.fullDisplayName} has failed. Check the logs for more information."      
        }
    }
}
