pipeline {
    agent any
    
     parameters{
        string( name: 'KEY_WORD', defaultValue: 'logstash', description: 'Key Word') 
        string( name: 'CODE', defaultValue: '200', description: 'Code') 
    }

    stages {
        stage('get apache logs') {
            steps {
                sh 'cd /tmp/ejercicio1 && curl -O  https://raw.githubusercontent.com/elastic/examples/master/Common%20Data%20Formats/apache_logs/apache_logs'
            }
        }
        stage('filter apache logs') {
            steps {
                sh """cd /tmp/ejercicio1 && grep '${KEY_WORD}' apache_logs | grep "${CODE}" > apache_logs_filtered"""
            }
        }
        stage('count lines') {
            steps {
                sh 'cd /tmp/ejercicio1 && wc -l apache_logs_filtered'
            }
        }
    }
}
