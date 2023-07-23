pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven_3.9.3"
    }

    stages {
        stage('Git Checkout🍴') {

            steps {
                
                //Checking whether the repository exists or not
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nafiursan/aws_pipeline.git']])

            }
        }
        
        
        stage('Modify application.properties') {
    steps {
        // Run the sed command to replace the URL in the application.properties file
        //sh "sed -i \"s|^spring\\\\.datasource\\\\.url =.*|spring.datasource.url = jdbc:mysql://clouddevdb.cjldgoxvvwoc.us-west-2.rds.amazonaws.com:3306/sparklmsdb?useSSL=true|\" /var/lib/jenkins/workspace/AWS-Deployment/src/main/resources/application.properties"
    sh 'echo haha'
        
    }
}

        
        
        
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }
        //ssh
                stage('send jar file to ec2') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'aws-ec2', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'java -jar spark-lms-0.0.1-SNAPSHOT.jar &> /dev/null &', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: 'target/*jar')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
        
        //ssh ends
    
   
    }
    
  /*  
   
   post {
        always {
            // Clean up any temporary files or resources here
            deleteDir()
        }

        success {
            // Perform actions if the build succeeds
            echo 'Build succeeded!'
        }

        failure {
            // Perform actions if the build fails
            echo 'Build failed!'
        }
    }
  */ 
   
}
