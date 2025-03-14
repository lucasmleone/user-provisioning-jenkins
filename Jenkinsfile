pipeline {
    agent any

    parameters {
        // Parámetro para ingresar el nombre
        string(name: 'NOMBRE', description: 'INGRESA TU NOMBRE')
        // Parámetro para ingresar el apellido
        string(name: 'APELLIDO', description: 'INGRESA TU APELLIDO')
        // Parámetro para seleccionar el departamento (Contabilidad, Finanzas, Tecnologia)
        choice(name: 'DEPARTAMENTO', choices: ['Contabilidad', 'Finanzas', 'Tecnologia'], description: 'Selecciona el departamento')
    }

    stages {
        stage('Validate Input and groups') {
            steps {
                script {
                    // Verifica que tanto NOMBRE como APELLIDO no estén vacíos
                    if (!params.NOMBRE?.trim() || !params.APELLIDO?.trim()) {
                        error("❌ ERROR: Debes ingresar NOMBRE y APELLIDO.")
                    }
                }
                // Verifica si el grupo del departamento existe y, de no existir, lo crea
                sh "getent group $(echo ${params.DEPARTAMENTO} | tr '[:upper:]' '[:lower:]') > /dev/null || (echo 'El grupo no existe procedere a crearlo' && groupadd $(echo ${params.DEPARTAMENTO} | tr '[:upper:]' '[:lower:]'))"
            }
        }
        stage('Normalize inputs') {
            steps {
                script {
                    // Normaliza los inputs combinando nombre y apellido, eliminando caracteres no alfabéticos y pasando a minúsculas
                    usuario = sh(script: """echo "${params.NOMBRE}-${params.APELLIDO}" | tr -cd "[:alpha:]-" | tr "[:upper:]" "[:lower:]" """, returnStdout: true).trim()
                    echo "Usuario generado: ${usuario}"
                }
            }
        }
        stage('User Create') {
            steps {
                // Crea el usuario; se debe agregar control para evitar duplicados si existiera
                sh "sudo useradd -m -s /bin/bash ${usuario}" //falta controlar si el usuario ya existe
                // Agrega el usuario al grupo correspondiente del departamento
                sh """sudo usermod -aG \$(echo ${params.DEPARTAMENTO} | tr "[:upper:]" "[:lower:]") ${usuario}"""
                // Agrega el usuario al grupo sudo para permisos administrativos
                sh "sudo usermod -aG sudo ${usuario}" 
            }
        }
        stage('Password Create') {
            steps {
                script {
                    // Genera una contraseña aleatoria y la asigna al usuario creado
                    password = sh(script: 'openssl rand -base64 12', returnStdout: true).trim()
                    sh "echo '${usuario}:${password}' | sudo chpasswd"
                    echo "Contraseña generada para ${usuario}: ${password}"
                }
            }
        }
    }
}
