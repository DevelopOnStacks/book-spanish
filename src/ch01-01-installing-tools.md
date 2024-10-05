## Instalación de Clarinet

### ¿Qué es Clarinet?
Clarinet es un entorno de ejecución de Clarity empaquetado como una herramienta de línea de comandos, diseñado para facilitar la comprensión, el desarrollo, la prueba y la implementación de contratos inteligentes. Clarinet consta de un REPL de Clarity y un arnés de prueba que, cuando se usan juntos, le permiten desarrollar y probar rápidamente un contrato inteligente de Clarity, sin la necesidad de implementar el contrato en una red de desarrollo o de prueba local.

Clarity es un lenguaje de contrato inteligente decidible que optimiza la previsibilidad y la seguridad, diseñado para la cadena de bloques Stacks. Los contratos inteligentes permiten a los desarrolladores codificar la lógica empresarial esencial en una cadena de bloques.

### Instalación en macOS (Homebrew)
Este proceso se basa en el administrador de paquetes de macOS llamado [Homebrew](https://brew.sh/). Con el comando `brew` puede agregar fácilmente una funcionalidad poderosa a su Mac, pero primero debemos instalarlo.

Para comenzar, inicie la aplicación [Terminal](https://support.apple.com/es-es/guide/terminal/welcome/mac) (/Aplicaciones/Utilidades/Terminal). Terminal es un sistema de línea de comandos versátil que viene con todas las computadoras Mac.

Si aún no tienes instalado [XCode](https://developer.apple.com/xcode/), lo mejor es instalar primero las herramientas de línea de comandos, ya que estas serán utilizadas por Homebrew:
```bash
xcode-select --install
```

Cuando XCode esté completo, procede con la instalación de Homebrew:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Ahora puedes probar tu instalación para asegurarte de que hayas instalado Brew correctamente, simplemente escribe:
```bash
% brew --version
Homebrew 3.6.3
```

Finalmente, ahora puedes instalar Clarinet en tu Mac:
```bash
brew install clarinet
```
Solo sigue las indicaciones de la terminal si es necesario.

### Instalar en Windows

La forma más sencilla de instalar Clarinet en Windows es usar el instalador MSI, que se puede descargar desde la [página de versiones](https://github.com/hirosystems/clarinet/releases).

Clarinet también está disponible en Winget, el administrador de paquetes que Microsoft comenzó a incluir en las últimas actualizaciones de Windows:

```powershell
winget install clarinet
```

### Instalar desde un binario precompilado

Para instalar Clarinet desde binarios precompilados, descargue la última versión desde la [página de versiones](https://github.com/hirosystems/clarinet/releases).
Descomprima el binario y luego cópielo en una ubicación que ya esté en su ruta, como `/usr/local/bin`.

```sh
# nota: puede cambiar la v0.27.0 con la versión que está disponible en la página de versiones.
wget -nv https://github.com/hirosystems/clarinet/releases/download/v0.27.0/clarinet-linux-x64-glibc.tar.gz -O clarinet-linux-x64.tar.gz
tar -xf clarinet-linux-x64.tar.gz
chmod +x ./clarinet
mv ./clarinet /usr/local/bin
```

En MacOS, puede recibir errores de seguridad al intentar ejecutar el binario precompilado. Puede resolver la advertencia de seguridad
con este comando:

```sh
xattr -d com.apple.quarantine /path/to/downloaded/clarinet/binary
```

### Instalar desde la fuente usando Cargo

#### Requisitos previos

[Instalar Rust](https://www.rust-lang.org/tools/install) para acceder a `cargo`, el administrador de paquetes de Rust.

En distribuciones basadas en Debian y Ubuntu, instale los siguientes paquetes antes de compilar Clarinet.

```bash
sudo apt install build-essential pkg-config libssl-dev
```

#### Construir Clarinet

Puedes construir Clarinet desde el código fuente usando Cargo con los siguientes comandos:

```bash
git clone https://github.com/hirosystems/clarinet.git --recursive
cd clarinet
cargo clarinet-install
```

De manera predeterminada, estarás en nuestra rama de desarrollo, `develop`, con código que aún no se ha publicado. Si planeas enviar algún cambio al código, esta es la rama adecuada para ti. Si solo quieres la última versión estable, cambia a la rama principal:

```bash
git checkout main
```

Si ya has verificado el código fuente, asegúrate de tener el código más reciente (incluidos los submódulos) antes de compilar usando:

```bash
git pull
git submodule update --recursive
```

### Verifica la instalación de Clarinet

Puedes verificar que Clarinet esté instalado correctamente ejecutando `clarinet --version` en
tu emulador de terminal favorito.

```bash
% clarinet --version
clarinet-cli 1.0.2
```

Más información sobre Clarinet: [https://github.com/hirosystems/clarinet/blob/develop/README.md](https://github.com/hirosystems/clarinet/blob/develop/README.md)
