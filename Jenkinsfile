pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven_3.9.3"
    }

    stages {
        stage('Git Checkout🍴') {
            steps {
                // Checking whether the repository exists or not
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nafiursan/aws_pipeline.git']])
            }
        }

        stage('Add Environment Variables🏷') {
            steps {
                // Inject database credentials as environment variables
                withCredentials([usernamePassword(credentialsId: 'RDS_credentials', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    // Run the sed commands to replace the URL, username, and password in the application.properties file
                    sh """
                        sed -i 's|^spring\\.datasource\\.url =.*|spring.datasource.url = jdbc:mysql://clouddevdb.cjldgoxvvwoc.us-west-2.rds.amazonaws.com:3306/sparklmsdb?useSSL=true|' /var/lib/jenkins/workspace/AWS-Deployment/src/main/resources/application.properties
                        sed -i 's|^spring\\.datasource\\.username =.*|spring.datasource.username = ${user}|' /var/lib/jenkins/workspace/AWS-Deployment/src/main/resources/application.properties
                        sed -i 's|^spring\\.datasource\\.password =.*|spring.datasource.password = ${pass}|' /var/lib/jenkins/workspace/AWS-Deployment/src/main/resources/application.properties
                    """
                }
            }
        }

        stage('Build with Maven 🔨') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        // SSH Deployment
        stage('Send JAR file to EC2🥫') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'aws-ec2', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: './start_spark_lms.sh', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: 'target/*jar')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }

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
}
