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
        location = 'us-central1-c'
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

        // stage("Nuget Restore"){
        //     steps{
        //         bat "dotnet restore ${workspace}\\WebApplication2\\WebApplication2.csproj"
        //     }
        // }

        // stage('Start SonarQube Analysis'){
        //     when { 
        //         branch 'master';
        //     }
        //     steps{
        //         withSonarQubeEnv(installationName: 'Test_Sonar')  
        //          {
        //             //Scan Modules  
        //             echo 'Sonarqube scanning started'                               
        //             bat  """ ${SonarQubeScanner} -Dsonar.projectKey=using_jenkins -Dsonar.projectname=using_jenkins -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=${workspace}\\WebApplication2 -Dsonar.cs.nunit.reportsPaths=${workspace}\\NUnitResults.xml -Dsonar.verbose=true """
        //          }
        //     }
        // }

        // stage('Code Build'){
        //     steps{
        //         echo "clean starts here"
        //         bat "dotnet clean ${workspace}\\WebApplication2\\WebApplication2.csproj"
        //         echo "Build starts here"
        //         bat "dotnet build ${workspace}\\WebApplication2.sln"
        //     }
        // }

        // stage('Stop SonarQube Analysis'){
        //     when { 
        //         branch 'master';
        //     }
        //     steps{
        //         withSonarQubeEnv(installationName: 'Test_Sonar')  
        //          {
        //             //Scan Modules  
        //             echo 'Sonarqube scanning Stopped'                               
        //             //bat  """ ${SonarQubeScanner} -Dsonar.projectKey=using_jenkins -Dsonar.projectname=using_jenkins -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=${workspace}\\WebApplication2 -Dsonar.cs.nunit.reportsPaths=${workspace}\\NUnitResults.xml -Dsonar.verbose=true """
        //          }
        //     }
        // }

        // stage('Release Artifact'){
        //     when { 
        //         branch 'develop';
        //     }
        //     steps{
        //             echo 'Release Artifact comes here'                               
        //     }
        // }

        // // stage('Unit Test'){
        // //     steps{
        // //         script{
        // //             def nunit = "${workspace}\\NUnit\\extension\\netcoreapp3.1\\nunit3-console.exe"
        // //             echo "Unit Test Starts Here"
        // //             bat "\"${nunit}\" --result=NUnitResults.xml ${workspace}\\TestProject1\\bin\\Debug\\netcoreapp3.1\\TestProject1.dll"
        // //         }
        // //     }
        // // }


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

        // stage('Containers'){
        //     steps{
        //         parallel {
        //             stage('PreContainerCheck') {
        //                 steps {
        //                     CONTAINER_ID = (docker ps -a | findstr 7100)
        //                     if [ $CONTAINER_ID ]
        //                     then
        //                        echo CONTAINER_ID
        //                 }
        //             }
        //             stage('PushtoDockerHub') {
        //                 steps {
        //                     script {
        //                         def branchName = env.BRANCH_NAME
        //                         def buildNumber = env.BUILD_NUMBER
        //                         def imgName = "i-${userName}-${branchName}:${buildNumber}"
        //                         withDockerRegistry(credentialsId: 'dockercredentials', url: 'https://registry.hub.docker.com'){
        //                             echo "tag docker image"
        //                             bat "docker tag ${imgName} ${dockerHubUsername}/${imgName}"
        //                             echo "push docker image"
        //                             bat "docker push ${dockerHubUsername}/${imgName}:latest"
        //                             echo "delete tagged docker image from local"
        //                             bat "docker rmi ${dockerHubUsername}/${imgName}"
        //                         }
        //                     }
        //                 }
        //             }
        //         }
        //     }
        // }

        // stage("Docker Deployment develop"){
        //     when { 
        //         branch 'develop';
        //     }
        //     steps{
        //         script {
        //             bat "docker run -d --name ${contName} -p 7300:7300 ${imgName}";
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

        stage("Kubernetes Deployment"){
            when { 
                branch 'develop';
            }
            steps{
                step ([$class: 'KubernetesEngineBuilder', projectId: env.project_id, clusterName: env.cluster_name, location: env.location, manifestPattern: 'deployment.yaml', credentialsId: env.credentials_id, verifyDeployments: true])
            }
        }
    }
}