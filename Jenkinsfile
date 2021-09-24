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
        stage("Image Build, Publish and Run"){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''docker stop $(docker ps -a -q);docker rm -f $(docker ps -a -q);docker rmi -f $(docker images -a -q);cd /opt/docker;docker build -t app_on_tomcat .''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }        
    }
}
