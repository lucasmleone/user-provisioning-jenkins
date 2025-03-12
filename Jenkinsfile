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
                sh "sudo useradd -m -s /bin/bash ${usuario}"
                sh """sudo usermod -aG \$(echo ${params.DEPARTAMENTO} | tr "[:upper:]" "[:lower:]" | tr "í" "i") ${usuario}"""
                sh "sudo usermod -aG sudo ${usuario}" // Agrega el usuario al grupo sudo
            }
        }
        stage('Password Create') {
            steps {
                script {
                    password = sh(script: 'openssl rand -base64 12', returnStdout: true).trim()
                    sh "echo '${usuario}:${password}' | sudo chpasswd"
                    echo "Contraseña generada para ${usuario}: ${password}"
                }
            }
        }
    }
}
