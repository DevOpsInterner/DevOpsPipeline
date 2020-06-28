pipeline {
  agent any
  tools {
    maven 'maven'
  }

  stages {
    stage ('Initialize') {
      steps {
         sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
        echo "Starting Your Pipeline Build"
      }
    }

    stage ('Build') {
      steps {
         // sh 'mvn clean package'
        echo " Build always works"
       }
    }

    stage("Artifact Upload"){
     // echo "Add steps"  
      steps{
       // nexusPublisher nexusInstanceId: 'yacine', nexusRepositoryId: 'devopsdemo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar']], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'jar', version: '2.4']]]
      echo 'Artificat Uploading Is working properly'
      }
    }

   stage ('Deploy Using Ansible') {
      steps {
        sh 'cp target/*.jar $ANSIBLE_DIRECTORY/dist/'
       // withEnv(['ANSIBLE_DIRECTORY=$ANSIBLE_DIRECTORY/']) {
          sshagent(['610d3050-5b62-4edc-8395-acddb916ec5c']) {
            //sh 'ansible -v webserver -m copy -a "src=dist/ dest=/var/www/html" -i inventory -b'
            sh '''
              cd $ANSIBLE_DIRECTORY
              pwd
              ansible-playbook -i inventory deploywebapp.yaml
            '''
          }
    //     }
       }
    }

  }

}
