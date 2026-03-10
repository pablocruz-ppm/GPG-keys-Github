# GPG-keys-Github

## Paso 1 (Instalar Herramienta GPG MacOs)

```
brew install gnupg
```

## Paso 2 (Verificar Tener las Herramientas Instaladas)

```
git --version
gpg --version
```

De esta manera confirmamos que tenemos todo lo necesario para poder generar nuestras llaves.

## Paso 3 (Generar nuestras llaves GPG)

### 1.- Empezamos con la generación

```
gpg --full-generate-key
```

### 2.- Seleccionamos el Tipo

 * Opción 9 para seleccionar ECC. Rápida y más segura que RSA  
 * Presionar Enter

### 3.- Seleccionar la Curva

 * Opción 1 para seleccionar Curve25519  
 * Presionar Enter

### 4.- Establecer expiración

 * Seleccionar 1y para un año o seleccionar 0 para que no expire
 * Presionar Enter

### 5.- Agregar Detalles del usuario

 * Nombre
 * Email
 * Comentario (opcional)

### 6.- Escoger un Passphase

 * Agregar una clave fuerte para proteger la llave privada.
 * Confirmar la clave.

## Paso 4 (Encontrar el ID de la Llave GPG)

### 1.- Listar las llaves

```
gpg --list-secret-keys --keyid-format=long
```

### 2.- Se puede visualizar una respuesta parecida

```
---------
sec   ed25519/0F685EDC2C61527E 2026-03-09 [SC]
      4D3AC0ED4B17CEA2F1AA88290F685EDC2C61527E
uid              [  absoluta ] pablo cruz <pcruz@ppm.com.ec>
ssb   cv25519/743A30403F4067FF 2026-03-09 [E]

PPM-PCRUZ:github-actions pcruz$ gpg --armor --export 0F685EDC2C61527E
-----BEGIN PGP PUBLIC KEY BLOCK-----
```
*El id de la llave es 0F685EDC2C61527E (luego de ed25519/)*

## Paso 5 (Exportar la Llave Pública)

### 1.- Exportar en formato ASCII

```
gpg --armor --export 0F685EDC2C61527E
```
### 2.- Copiar el contenido de la llave

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
mQENBF...
-----END PGP PUBLIC KEY BLOCK-----
```
### 3.- (Opcional) Guardar en un archivo

```
gpg --armor --export 0F685EDC2C61527E > pcruz.asc
```
## Paso 6 (Agregar Llave Pública a Github)

### 1 Dentro de Github

* Visitar  [Settings > SSH and GPG Keys](https://github.com/settings/keys).
* Click en **New GPG key or Add GPG key**.

### Pegar la Llave:

* La llave pública generada anteriormente se la pega dentro de la caja de texto.

### Guardar:

* Guardar la llave.

## Paso 7 (Configurar Git para usar la llave GPG)

#### 1.- Enlazar la llave GPG con Git

```
git config --global user.signingkey 0F685EDC2C61527E
```

#### 2.- Habilitar Autofirmado

```
git config --global commit.gpgsign true
```
#### 3.- Verificar la configuración

```
git config --global --list
```
*Se debe obtener una respuesta parecida a la siguiente:*
```
user.signingkey=0F685EDC2C61527E
commit.gpgsign=true
```
## Paso 8 (Verificar el firmado)
Una vez realizado un commit de manera local. Procedemos a verificar
```
git log --show-signature -1
```
*Se debe obtener una respuesta parecida a la siguiente:*
```
commit d19c093ba0e3ba1d2098290414469ff64bcb4dab (HEAD -> main, origin/main)
gpg: Firmado el Tue Mar 10 09:53:05 2026 -05
gpg:                usando EDDSA clave 4D3AC0ED4B17CEA2F1AA88290F685EDC2C61527E
gpg: Firma correcta de "pablo cruz <pcruz@ppm.com.ec>" [absoluta]
Author: Pablo Cruz <pcruz@ppm.com.ec>
Date:   Tue Mar 10 09:53:05 2026 -0500

    first commit
```
En caso de subir el commit a Github:
```
git push -u origin remote-branch
```
Dentro de la plataforma web de Github, podemos verificar que el commit se ha firmado correctamente.

## Pasos Adicionales(En caso de errores)

Agregar de manera permanente dentro de nuestra configuración. Ya sea en el *~/.bashrc o ~/.zshrc*

```
export GPG_TTY=$(tty)
```
Reiniciamos la configuración
```
source ~/.bashrc
```
Reiniciamos el agente GPG

```
gpgconf --kill gpg-agent
```

