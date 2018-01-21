// change the project version (don't rely on version from pom.xml)
env.BN = VersionNumber([
        versionNumberString : '${BUILD_MONTH}.${BUILDS_TODAY}.${BUILD_NUMBER}', 
        projectStartDate : '2017-02-09', 
        versionPrefix : 'v1.'
    ])
		
node ("master") {                  
    stage('Provision') {                
        echo 'PIPELINE STARTED'
        
        echo 'Checkout source code from GitHub ...'
        retry(5){
            git branch: 'master', credentialsId: '13870c4d-d3fa-477e-bfc1-be26c8eee6ef', url: 'https://github.com/devops98/SpringMVCDemo.git'
        }
        
        echo 'Change the project version ...'
        def W_M2_HOME = tool 'Maven_Home'
        bat "${W_M2_HOME}\\bin\\mvn versions:set -DnewVersion=$BN -DgenerateBackupPoms=false"                
        
        echo "Create a new branch with name release_${BN} ..."
        def W_GIT_HOME = tool 'Git'
        bat "${W_GIT_HOME} checkout -b release_${BN}"
        
        echo 'Stash the project source code ...'
        stash includes: '**', excludes: '**/TestPlan.jmx', name: 'SOURCE_CODE'
    }
}

