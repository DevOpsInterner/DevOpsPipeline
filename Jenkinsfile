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
          sh 'mvn clean package'
       }
    }

    stage("Artifact Upload"){
     // echo "Add steps"  
        nexusPublisher nexusInstanceId: 'yacine', nexusRepositoryId: 'devopsdemo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar']], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'jar', version: '2.4']]]
    }

   stage ('Deploy Using Ansible') {
      steps {
        sh 'cp target/*.jar $ANSIBLE_DIRECTORY/dist/'
        sh "cd $ANSIBLE_DIRECTORY"
        sshagent(['610d3050-5b62-4edc-8395-acddb916ec5c']) {
          //sh 'ansible -v webserver -m copy -a "src=dist/ dest=/var/www/html" -i inventory -b'
          sh 'anisble-playbook -i inventory deploywebapp.yaml'
        }
       }
    }

  }

}
