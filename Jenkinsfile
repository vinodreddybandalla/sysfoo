pipeline{
    agent{
        label "rhel"
    }
    stages{
        stage("Requirement Gathering - Cloning Repo"){
            steps{
                git 'https://github.com/krushnabhanage10/sysfoo.git'
                
            }
        }
        stage("Build"){
            steps{
                sh "mvn clean install package"
                
            }
        }
    }
}
