pipeline{
    agent {label "built-in"}
    stages{
        stage("Docker Login"){
            steps{
                echo "Logging to docker account"
                withCredentials([string(credentialsId: 'DOCKER_HUB_TOKEN', variable: 'DOCKER_HUB_TOKEN')]) {
                sh "echo $DOCKER_HUB_TOKEN | docker login -u pranaygupta1988 --password-stdin"
                }
            }
        }
        stage("Image Building"){
            steps{
                sh "docker image build -t pranaygupta1988/webapp1:$BUILD_NUMBER ."
            }
        }
        stage("Pushing Image"){
            steps{
                sh "docker image push pranaygupta1988/webapp1:$BUILD_NUMBER"
            }
        }
        stage("Update Deployment File"){
            steps{
                sh """
                sed -i "s/webapp1:.*/webapp1:${BUILD_NUMBER}/g" deploy1.yaml
                """
            }
        }
        stage("Update Git Repository"){
            steps{
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
                sh "git config --global user.email 'pranaygupta2022@gmail.com'"
                sh "git config --global user.name 'pranaygupta1988'"
                sh "git add deploy1.yaml"
                sh "git commit -m 'Deployment file update with version: $BUILD_NUMBER' || true" 
                sh "git push https://$GITHUB_TOKEN@github.com/pranaygupta1988/webapp1 HEAD:main"
                }
            }
        }
    }
}
