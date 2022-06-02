pipeline{
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
    stage('Build'){
      steps{
        sh 'mvn -B -DskipTests clean package'
      }
     }
    stage('Test'){
      steps{
        sh 'mvn test'
      }
      }
      stage('collect artifact'){
        steps{
           archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
         }
         }
     stage('deploy to artifactory')
     {
     steps{
      rtUpload (
         serverId: 'ARTIFACTORY_SERVER',
            spec: '''{
                 "files": [
            {
              "pattern": "target/*.jar",
              "target": "art-doc-dev-loc"
            }
         ]
    }''',
    )
}}
   stage('download to artifactory')
   {
     steps {
       rtDownload (
                         serverId: 'ARTIFACTORY_SERVER',
                     spec: '''{
                             "files": [
                                      {
                                      "pattern": "art-doc-dev-loc/my-app-1.0-SNAPSHOT.jar",
                                      "target": "bazinga/"
                                    }
                                ]
                            }'''
                        )
                        }}
  }
}
