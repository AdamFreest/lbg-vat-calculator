// This adds install and test stages before static code analysis
pipeline
{
 environment 
 {
 registry = "adamfreest/vatcal"
        registryCredentials = "dockerhub_id"
        dockerImage = ""
  }
    agent any
        stages 
        {
            stage ('Build Docker Image')
            {
                steps
                {
                    script 
                    {
                        dockerImage = docker.build(registry)
                    }
                }
            }
            //TODO: Add comment
            stage ("Push to Docker Hub")
            {
                steps 
                {
                    script 
                    {
                        docker.withRegistry('', registryCredentials) 
                        {
                            dockerImage.push("${env.BUILD_NUMBER}")
                            dockerImage.push("latest")
                        }
                    }
                }
            }
            //clean and force the clean for thise >48 hours old
            stage ("Clean up")
            {
                steps 
                {
                    script 
                    {
                        sh 'docker image prune --all --force --filter "until=48h"'
                    }
                }
            }
        }
}