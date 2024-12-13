node{
    
    stage('checkout'){
        git 'https://github.com/shubhamkushwah123/insurance-project-demo.git'
    }
    
    stage('maven build'){
        sh '/opt/homebrew/bin/mvn clean package'
    }
    
    stage('containerize'){
        sh '/usr/local/bin/docker build -t saran7de/insure-me:1.0 .'
    }
    
    stage('Release'){
        withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHubPwd')]) {
        sh "/usr/local/bin/docker login -u saran7de -p ${dockerHubPwd}"
        sh '/usr/local/bin/docker push saran7de/insure-me:1.0'
        }
    }
    
    environment {
        // Add your ansible-playbook path here
        PATH = "/usr/local/bin:/opt/homebrew/bin:${env.PATH}"  // Adjust paths accordingly
    }

    stage('Deploy to Test'){
     //ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
     sh 'TMPDIR=/tmp /opt/homebrew/bin/ansible-playbook configure-test-server.yml -i /etc/ansible/hosts -b --become-user root'
    }
    
    stage('checkout regression test source code'){
        git 'https://github.com/saran7de/my-selenium-test-app.git'
    }
    
    stage('build test scripts'){
        sh '/opt/homebrew/bin/mvn clean package assembly:single'
    }
    
    stage('execute selenium test script'){
        sh 'java -jar target/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }

    stage('checkout'){
        git 'https://github.com/saran7de/insurance-project-demo.git'
    }
    
     stage('Deploy to Test'){
     ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
    
    
    
}
