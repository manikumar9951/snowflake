pipeline {
    agent {label 'slave'}
    stages {
        stage('scm'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: 'dev']], 
                extensions: [], 
                userRemoteConfigs: [[credentialsId: 'faa7509c-4a0f-4b59-89f6-f50fe0cf9857',
                url: 'https://github.com/manikumar9951/snowflake.git']]])
            }
        }
        stage('Run schemachange') {
            steps {
                //sh "pip3 install schemachange --upgrade"
                //sh "pip3 install cryptography"
                sh "pip3 show schemachange "
                sh "schemachange -f migrations -a ${SF_ACCOUNT} -u ${SF_USERNAME} -r ${SF_ROLE} -w ${SF_WAREHOUSE} -d ${SF_DATABASE} -c ${SF_DATABASE}.SCHEMACHANGE.CHANGE_HISTORY --create-change-history-table"
            }
        }
    }
    post{
        always{
            build job: 'codeflow-from-dev-to-uat'
       }
    }
}  
