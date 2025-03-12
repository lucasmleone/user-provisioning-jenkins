pipeline {
    agent any

    parameters {
        string(name: 'NOMBRE', description: 'INGRESA TU NOMBRE')
        string(name: 'APELLIDO', description: 'INGRESA TU APELLIDO')
        choice(name: 'DEPARTAMENTO', choices: ['Contabilidad', 'Finanzas', 'Tecnología'], description: 'Selecciona el departamento')
    }

    stages {
        stage('Validate Input') {
            steps {
                script {
                    if (!params.NOMBRE?.trim() || !params.APELLIDO?.trim()) {
                        error("❌ ERROR: Debes ingresar NOMBRE y APELLIDO.")
                    }
                }
            }
        }
        stage('Normalize inputs') {
            steps {
                sh """usuario=$(echo "${NOMBRE} - ${APELLIDO}" | tr -cd "[:alpha:]- | tr [:upper:] [:lower:])"""
            }
        }
        stage('User Create') {
            steps {
                sh """cd /tmp/ejercicio1 && grep '${KEY_WORD}' apache_logs | grep "${CODE}" > apache_logs_filtered"""
            }
        }
        stage('Password Create') {
            steps {
                sh 'cd /tmp/ejercicio1 && wc -l apache_logs_filtered'
            }
        }
    }
}
