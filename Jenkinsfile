pipeline {
    agent any

    environment {
        SonarQubeScanner = tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
        def app = ''
        def CONTAINER_ID = ''
        def userName = "kamal02"
        def masterContName = "c-kamal02-master"
        def developContName = "c-kamal02-develop"
        def dockerHubUsername = "kamalmittal2020"
        cluster_name = 'app-kamal02'
        location = 'us-central1'
        credentials_id = 'TestJenkinsApi'
        project_id = 'nagp2021'

    }

    stages {
        stage('Checkout Source Code') {
            steps {
                echo "Clean old checkout from workspace"
                cleanWs()
                echo "Source Code Checkout start"
                checkout scm
                echo "Source Code Checkout stop"
            }
        }

        stage("Nuget Restore"){
            steps{
                bat "dotnet restore ${workspace}\\WebApplication2\\WebApplication2.csproj"
            }
        }

        // stage('Start SonarScanner (Unit Test)'){
        //     when { 
        //         branch 'master';
        //     }
        //     steps{
        //         script{
        //             def nunit = "${workspace}\\NUnit\\extension\\netcoreapp3.1\\nunit3-console.exe"
        //             echo "Unit test report for Sonar Scanner"
        //             bat "\"${nunit}\" --result=NUnitResults.xml ${workspace}\\TestProject1\\bin\\Debug\\netcoreapp3.1\\TestProject1.dll"
        //         }
        //     }
        // }


        stage('Code Build'){
            steps{
                echo "clean starts here"
                bat "dotnet clean ${workspace}\\WebApplication2\\WebApplication2.csproj"
                echo "Build starts here"
                bat "dotnet build ${workspace}\\WebApplication2.sln"
            }
        }

        // stage('Stop SonarQube Analysis(Completion)'){
        //     when { 
        //         branch 'master';
        //     }
        //     steps{
        //         withSonarQubeEnv(installationName: 'Test_Sonar')  
        //          {
        //             //Scan Modules  
                                                  
        //             bat  """ ${SonarQubeScanner} -Dsonar.projectKey=sonar-kamal02 -Dsonar.projectname=sonar-kamal02 -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=${workspace}\\WebApplication2 -Dsonar.cs.nunit.reportsPaths=${workspace}\\NUnitResults.xml -Dsonar.verbose=true """
        //             echo 'Sonarqube scanning Stopped' 
        //          }
        //     }
        // }

        stage('Release Artifact'){
            when { 
                branch 'develop';
            }
            steps{
                    bat "dotnet publish ${workspace}\\WebApplication2.sln -c Release"                         
            }
        }


        // stage('Docker Image'){
        //     steps{
        //         script{
        //             def branchName = env.BRANCH_NAME
        //             def buildNumber = env.BUILD_NUMBER
        //             def imgName = "i-${userName}-${branchName}:${buildNumber}"
        //             echo "create docker image"
        //             docker.build("${imgName}","-f ${workspace}\\WebApplication2\\Dockerfile .")
        //         }
        //     }
        // }

        stage('Containers'){
            parallel {
                stage('Pre Container Check') {
                    steps {
                        script {
                            bat """ docker ps -a | findstr 7100 > dev_port_check.txt
                                    set /p container=<dev_port_check.txt
                                    docker rm -f %container:~0,4%
                                """
                        }
                    }
                }
                stage('Push to DockerHub') {
                    steps {
                        echo "second command"
                        // script {
                            // def branchName = env.BRANCH_NAME
                            // def buildNumber = env.BUILD_NUMBER
                            // def imgName = "i-${userName}-${branchName}:${buildNumber}"
                            // withDockerRegistry(credentialsId: 'dockercredentials', url: 'https://registry.hub.docker.com'){
                            //     echo "tag docker image"
                            //     bat "docker tag ${imgName} ${dockerHubUsername}/${imgName}"
                            //     echo "push docker image"
                            //     bat "docker push ${dockerHubUsername}/${imgName}:latest"
                            //     echo "delete tagged docker image from local"
                            //     bat "docker rmi ${dockerHubUsername}/${imgName}"
                            // }
                        // }
                    }
                }
            }
        }

        // stage("Docker Deployment develop"){
        //     when { 
        //         branch 'develop';
        //     }
        //     steps{
        //         script {
        //             bat "docker run -d --name ${developContName} -p 7300:7300 ${imgName}";
        //         }
        //     }
        // }

        // stage("Docker Deployment master"){
        //     when { 
        //         branch 'master';
        //     }
        //     steps{
        //         script {
        //             bat "docker run -d --name ${masterContName} -p 7200:7200 ${imgName}";
        //         }
        //     }
        // }

        // stage("Kubernetes Deployment"){
        //     steps{
        //         echo 'Kubernetes Deployment started ...'
        //         step ([$class: 'KubernetesEngineBuilder', projectId: env.project_id, clusterName: env.cluster_name, location: env.location, manifestPattern: 'deployment.yaml', credentialsId: env.credentials_id, verifyDeployments: false])

        //         step ([$class: 'KubernetesEngineBuilder', projectId: env.project_id, clusterName: env.cluster_name, location: env.location, manifestPattern: 'service.yaml', credentialsId: env.credentials_id, verifyDeployments: false])
        //         echo 'Kubernetes Deployment Finished ...'
        //     }
        // }
    }
}