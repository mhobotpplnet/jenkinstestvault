pipeline {
  agent any
stages {  
  stage('Integration Tests') {
      steps {
      sh 'curl -s -o vault.zip https://releases.hashicorp.com/vault/0.9.1/vault_0.9.1_linux_amd64.zip ; yes | unzip vault.zip '
        withCredentials([string(credentialsId: 'JAVA_EXAMPLE_ROLE_ID', variable: 'ROLE_ID'),string(credentialsId: 'VAULT_TOKEN', variable: 'JENKINS_VAULT_TOKEN')]) {
        sh '''
  set +x
  export VAULT_ADDR=http://vault:8200
  export VAULT_SKIP_VERIFY=true
  export SECRET_ID=$(./vault write -field=secret_id -f auth/approle/role/java-example/secret-id)
  export JOB_VAULT_TOKEN=$(./vault write -field=token auth/approle/login role_id=${ROLE_ID}   secret_id=${SECRET_ID})
./vault read -address=http://vault:8200 secret/hello 
        '''
    }
   }
  }
 }
}
