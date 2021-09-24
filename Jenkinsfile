pipeline{
    agent{
        label "rhel"
    }
    triggers {
        pollSCM '* * * * *'
    }
    tools {
        maven "maven"
    }
    stages{
        stage("Requirement Gathering - Cloning Repo"){
            steps{
                git 'https://github.com/krushnabhanage10/sysfoo.git'
            }
        }
        stage("Build Package"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Artifact Transfer"){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        stage("Image Build and Publish"){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''docker stop $(docker ps -a -q);docker rm -f $(docker ps -a -q);docker rmi -f $(docker images -a -q);cd /opt/docker;docker build -t app_on_tomcat .''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }        
        }
        stage("Pull Image and Run Container"){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''docker login -u "krushnabhanage10" -p "Admin@123";docker tag app_on_tomcat krushnabhanage10/tomcat-sysinfo:latest;docker push krushnabhanage10/tomcat-sysinfo:latest;docker stop $(docker ps -a -q);docker rm $(docker ps -a -q);docker rmi $(docker images -a);docker run -d -p 8090:8080 krushnabhanage10/tomcat-sysinfo:latest''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }        
        }
    }
        post{
        always{
            echo "The Pipeline execution is completed."
        }
        success{
            echo "Success.....!!!"
            mail bcc: '', body: 'The Pipeline is Executed: Successfully', cc: '', from: '', replyTo: '', subject: 'The Pipeline is Execution is Successful', to: 'Krushnabhanage10@gmail.com'
        }
        failure{
            echo "Failure,,, Kindly check the pipline file in repo"
            mail bcc: '', body: 'The Pipeline is Executed: Failed', cc: '', from: '', replyTo: '', subject: 'The Pipeline is Execution is Failed', to: 'Krushnabhanage10@gmail.com'
        }
        }
}
