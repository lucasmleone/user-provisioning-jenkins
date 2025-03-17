# User Provisioning Jenkins Pipeline

Este proyecto contiene un pipeline de Jenkins para la provisión de usuarios en un sistema Linux. El pipeline permite crear usuarios, asignarlos a grupos específicos y generar contraseñas temporales.

## Requerimientos

Antes de ejecutar este pipeline, asegúrate de cumplir con los siguientes requerimientos:

1. **Jenkins**: Asegúrate de tener Jenkins instalado y configurado.
2. **Permisos de sudoers**: El usuario de Jenkins debe tener permisos de sudo sin contraseña para ejecutar los siguientes comandos:
    ```plaintext
    jenkins ALL=(ALL) NOPASSWD: /usr/sbin/useradd, /usr/sbin/usermod, /usr/sbin/chpasswd, /usr/bin/passwd --expire *
    ```
    Para agregar estos permisos, edita el archivo sudoers con el comando `sudo visudo` y agrega la línea anterior.
    ![alt text](image.png)

## Parámetros del Pipeline

| Parámetro    | Tipo    | Descripción |
|-------------|--------|-------------|
| `NOMBRE`    | String | Nombre del usuario a crear.|
| `APELLIDO`  | String | Apellido del usuario a crear.|
| `DEPARTAMENTO` | Choice | Departamento del usuario. Opciones disponibles: `Contabilidad`, `Finanzas`, `Tecnologia`. |

## Uso

Sigue estos pasos para usar el pipeline:

1. **Configura el pipeline en Jenkins**:
    - Crea un nuevo pipeline en Jenkins.
    - Configura el pipeline para que apunte a este repositorio (público): `https://github.com/lucasmleone/user-provisioning-jenkins.git`.
    - Configura el pipeline para que use la rama `main`.

2. **Ejecuta el pipeline**:
    - Inicia una nueva ejecución del pipeline.
    - Proporciona los valores para los parámetros `NOMBRE`, `APELLIDO` y `DEPARTAMENTO`.

3. **Verifica la creación del usuario**:
    - El pipeline creará el usuario, lo agregará al grupo correspondiente y generará una contraseña temporal.
    - La contraseña temporal se mostrará en la salida del pipeline. Asegúrate de que el usuario cambie su contraseña en su primer inicio de sesión.

## Ejemplo de Ejecución

A continuación se muestra un ejemplo de cómo ejecutar el pipeline:

1. **Parámetros de entrada**:
    - `NOMBRE`: `Juan`
    - `APELLIDO`: `Perez`
    - `DEPARTAMENTO`: `Tecnologia`

2. **Salida esperada**:
    ```plaintext
    Usuario generado: juan-perez
    El usuario juan-perez ya existe
    Contraseña TEMPORAL generada para juan-perez: <contraseña_generada>
    Por favor, cambie su contraseña en su primer inicio de sesión.
    ```

## Notas

- Asegúrate de que los nombres y apellidos proporcionados no contengan caracteres especiales. El pipeline eliminará cualquier carácter especial durante la normalización de los inputs.
- El pipeline verifica si el grupo del departamento existe y lo crea si no existe.
- La contraseña generada es temporal y se fuerza al usuario a cambiarla en su primer inicio de sesión.


