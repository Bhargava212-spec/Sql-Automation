pipeline {
    agent any
    environment {
        LIQUIBASE_HOME = "C:/Program Files/liquibase/liquibase.bat"
        GIT_REPO = "https://github.com/Bhargava212-spec/Sql-Automation.git"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${env.GIT_REPO}"
            }
        }
        stage('Configure Properties') {
            steps {
                script {
                    def author = "${env.BUILD_USER}" ?: 'default_user'
                    def uuid = UUID.randomUUID().toString()
                    sh "sed -i 's/\${author_name}/${author}/g' db.changelog-master.yml"
                    sh "sed -i 's/\${random_uuid}/${uuid}/g' db.changelog-master.yml"
                }
            }
        }
        stage('Run Liquibase') {
            steps {
                script {
                    // Run liquibase update with the configured properties
                    sh "${env.LIQUIBASE_HOME}/liquibase --changeLogFile=Sql-Automation/liquibase.properties update"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
