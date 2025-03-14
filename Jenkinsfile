pipeline {
    agent any

    parameters {
        string(name: 'NOMBRE', description: 'INGRESA TU NOMBRE')
        string(name: 'APELLIDO', description: 'INGRESA TU APELLIDO')
        choice(name: 'DEPARTAMENTO', choices: ['Contabilidad', 'Finanzas', 'Tecnologia'], description: 'Selecciona el departamento')
    }

    stages {
        stage('Validate Input and groups') {
            steps {
                script {
                    if (!params.NOMBRE?.trim() || !params.APELLIDO?.trim()) {
                        error("❌ ERROR: Debes ingresar NOMBRE y APELLIDO.")
                    }
                    if (!params.NOMBRE?.trim() || !params.APELLIDO?.trim()) {
                        error("❌ ERROR: Debes ingresar NOMBRE y APELLIDO.")
                    }
                }
                sh "getent group $(echo ${params.DEPARTAMENTO} | tr '[:upper:]' '[:lower:]') > /dev/null || (echo 'El grupo no existe procedere a crearlo' && groupadd $(echo ${params.DEPARTAMENTO} | tr '[:upper:]' '[:lower:]'))"
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
                sh "sudo useradd -m -s /bin/bash ${usuario}" //falta controlar si el usuario ya existe
                sh """sudo usermod -aG \$(echo ${params.DEPARTAMENTO} | tr "[:upper:]" "[:lower:]") ${usuario}"""
                sh "sudo usermod -aG sudo ${usuario}" 
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
