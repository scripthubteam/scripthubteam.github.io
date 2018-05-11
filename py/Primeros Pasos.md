# Índice

1. [Introducción](##introducción) - Explicación breve de lo que es `discord.py`.
2. [Instalación](##instalación) - Cómo instalar `discord.py` en nuestro Sistema.
3. [Requisitos Obligatorios](##dependencias) - Lista de librerías que son necesarios para la API.
4. [Ejemplos de Uso](##ejemplos-de-uso) - Serie de Ejemplos sencillos sobre el uso del Bot.

## Introducción

[Discord.py](https://github.com/Rapptz/discord.py "Discord.py GitHub") es una API Wrapper de [Discord API](https://discordapp.com/developers/docs/intro "Discord API Documentation"), algo así como un simplicador de las herramientas que suministra Discord.

`Discord.py` fue desarrollado por [Rapptz](https://github.com/Rapptz) y es una herramienta muy útil a la hora de crear, manejar o modificar Bots de Discord escritos en Python.

## Instalación

[![Python](https://img.shields.io/badge/Python-3.4.2+-green.svg?style=flat-square)](https://www.python.org/downloads/ "Download Python")
[![PyPI](https://img.shields.io/badge/PyPI-Lastest-blue.svg?style=flat-square)](https://pypi.org/project/pip/ "Download pip")

- [Requisitos](###requisitos)
- [Instalar discord.py](###instalar-discordpy)

---

### Requisitos

Para instalar `discord.py` se necesitará **Python 3.4.2** o superior y **PyPI** (cualquier versión de PyPI que incluya `discord.py`).

Podemos comprobar si **Python** está instalado en nuestro Sistema escribiendo `python --version` o `python3 --version` en la consola (los sistemas Linux suelen incluir tanto **Python 2.X+** como **Python 3.X+** en las librerías).

De igual manera podemos comprobar si **PyPI** está instalado escribien `pip --version` o `pip3 --version` en la consola.

Si ya tenemos **PyPI** instalado pero queremos actualizarlo a su última versión podemos recurrir a los comandos de consola:

#### Linux y MacOS

`pip install -U pip` o `pip install --upgrade pip`

#### Windows

`python -m pip install -U pip` o `python -m pip install --upgrade pip`

### Links de Descarga

- [Python](https://www.python.org/downloads/ "Download Python")
- [PyPI](https://pypi.org/project/pip/ "Download PyPI")

---

### Instalar discord.py

Una vez tengamos todos los requisitos necesarios bastará instalar `discord.py` en función de nuestras necesidades, para ello a continuación se muestran los comandos de consola necesarios para dicha instalación.

*(Se mostrarán dos líneas de comandos, simplemente usa la que funcione en tu sistema operativo).*

**Funciones de Texto exclusivamente:**

```Bash
python3 -m pip install -U discord.py
```

```Bash
python -m pip install -U discord.py
```

**Funciones de Texto y Funciones de Voz:**

*(Para esta versión se requieren las siguientes librerías):*

- libffi-dev (Puede ser que lo encuentres como `libffi-devel` en algunos Sistemas Operativos)
- python-dev (Varía en función de la versión de Python. **Ejemplo:** `python3.5-dev` para **Python3.5**)

```Bash
python3 -m pip install -U discord.py[voice]
```

```Bash
python -m pip install -U discord.py[voice]
```

**Versión de Desarrollo (Inestable):**

*(De igual manera que la anterior, esta versión requiere librerías):*

- libffi-dev (También llamado `libffi-devel`)
- python-dev (**Ejemplo:** `python3.5-dev` para **Python3.5**)

```Bash
python3 -m pip install -U https://github.com/Rapptz/discord.py/archive/master.zip#egg=discord.py[voice]
```

```Bash
python -m pip install -U https://github.com/Rapptz/discord.py/archive/master.zip#egg=discord.py[voice]
```

## Dependencias

- Python3.4.2 o superior.
- Librerías:
  - `aiohttp`
  - `websockets`
  - `PyNaCl` (Opcional)
    - *Esta librería solo es necesaria si has optado por la [versión de discord.py](##instalar-discordpy) con soporte para Voz o por la de Desarrollo.*

## Ejemplos de Uso

### Ejemplo 1 - Usando `discord.Client()`

```Python
import discord  # Importando el API de Discord para Python
import asyncio  # Python3.5+ debería traer la dependencia async integrada

prefix = "_"  # Variable opcional para poner un prefijo

client = discord.Client()  # Creamos el cliente de Discord
bot_user = client.user  # Obtenemos el objeto User del cliente

@client.event
async def on_ready():
    print("Conectando Bot...")
    print("Nombre: ", bot_user.name)
    print("ID: ", bot_user.id)
    print("¡Bot Conectado!")
    print("---------------")

@client.event
async def on_message(message):  # Este método recibirá como parámetro un objeto Message
    if message.content.startswith(prefix+"ping"):
        # El método send_message() requiere el canal y el mensaje en sus parámetros
        client.send_message(message.channel, "Pong!")

client.run("your-token")  # El token del Bot
```