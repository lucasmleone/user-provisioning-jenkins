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
                script {
                    usuario = sh(script: """echo "${params.NOMBRE}-${params.APELLIDO}" | tr -cd "[:alpha:]-" | tr "[:upper:]" "[:lower:]" """, returnStdout: true).trim()
                    echo "Usuario generado: ${usuario}"
                }
            }
        }
        stage('User Create') {
            steps {
                sh "useradd -m -s /bin/bash ${usuario}"
                sh """usermod -aG \$(echo ${params.DEPARTAMENTO} | tr "[:upper:]" "[:lower:]" | tr "í" "i") ${usuario}"""
            }
        }
        stage('Password Create') {
            steps {
                sh """password=\$(openssl rand -base64 12) && echo "${usuario}:\${password}" | chpasswd"""
                sh """echo 'Contraseña generada para ${usuario}: \${password}'"""
            }
        }
    }
}
