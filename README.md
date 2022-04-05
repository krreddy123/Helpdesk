CICD Pipeline(GITHUB-JENKINS-JFROG-AZUREDEVOPS):
===============================================
1. Store our code into GitHub Repository.
2. https://github.com/krreddy123/Helpdesk.git

3. Then create the CI Pipeline.
==============================
Helpdesk job:
============-
4. ![Uploading Jenkins main page.PNG…]()

5. https://github.com/krreddy123/Helpdesk.git
6. Poll SCM * * * * *
7. C:/Users/azuredevops/.jenkins/workspace/Helpdesk/nuget.exe restore C:/Users/azuredevops/.jenkins/workspace/Helpdesk/HelpdeskMVC.sln
8. Build a Visual Studio project or solution using MSBuild
9. MSBuild Version: MSBUILD Default
10. MSBuild Build File : C:\Users\azuredevops\.jenkins\workspace\Helpdesk\HelpdeskMVC.sln
11. Command Line Arguments : /p:WarningLevel=0 /p:DeployOnBuild=true
12. Post-build Actions:
13. Save
Upload Job:
===========
1. Select another freestyle job:
2. Advanced Project Options
3. Pipeline
4. Pipeline script
5. ===============
6. pipeline {
    agent {
        label ''
    }
    triggers {
        pollSCM ''
    }
    stages {
        stage ('Deploy to Artifactory') {
            steps {
                rtServer (
                    id: "ramankonatham123456",
                    url: 'https://ramankonatham123456.jfrog.io/artifactory',
                    credentialsId: 'bb6526cd-1d88-4c35-b6c8-6db1dbd1d215',
                    bypassProxy: false,
                    timeout: 300
                )
                
                rtUpload (
                    serverId: 'ramankonatham123456',
                    spec: '''{
                        "files": [
                        {
                            "pattern": "C:\\Users\\azuredevops\\.jenkins\\workspace\\Helpdesk\\HelpdeskMVC\\obj\\Debug\\Package\\HelpdeskMVC.zip",
                            "target": "azuredevop-123"
                        }
                        ]
                    }''',
                    buildName: 'Helpdesk'
                )
                rtPublishBuildInfo (
                    serverId: 'ramankonatham123456',
                    buildName: 'Helpdesk'
                )
            }
        }
    }
}

