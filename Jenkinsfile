pipeline{
    agent{
        label "rhel"
    }
    triggers {
        pollSCM '* * * * *'
    }
    stages{
        stage("Requirement Gathering - Cloning Repo"){
            steps{
                git 'https://github.com/krushnabhanage10/sysfoo.git'
            }
        }
        stage("Build"){
            steps{
                step def mvnHome= tool name: 'maven', type: 'maven'
                step def mvnCMD= "${mvnHome}/bin/mvn"
                sh "${mvnCMD} clean install package"
            }
        }

    }
}
