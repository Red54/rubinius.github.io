---
layout: doc_es
title: Gu�as - Migrando de MRI a Rubinius
previous: Gu�as
previous_url: guides
next: C�mo
next_url: how-to
---

Esta gu�a te acompa�ar� en el proceso de migrar tu aplicaci�n Ruby on Rails
de MRI (Matz's Ruby Implementation) a Rubinius. Esta gu�a asume que est�s
familiarizado con Ruby o Ruby on Rails, RubyGems, y Bundler. Tambi�n asume que
sabes utilizar el shell de la l�nea de comandos.

## 1. Instalar Rubinius

El primer paso en la migraci�n a Rubinius es instalar Rubinius. Ya que
probablemente necesitaras alternar entre MRI y Rubinius durante la migraci�n,
la soluci�n m�s pr�ctica es utilizar una utilidad para cambiar de Ruby.

Te recomendamos usar [chruby](https://github.com/postmodern/chruby) como
herramienta para la l�nea de comandos que permite cambiar entre las
implementaciones de Ruby. Todos los ejemplos de esta gu�a usaran `chruby`.

### Instalar chruby y ruby-install

Si est�s usando OS X, la forma m�s f�cil de instalar `chruby` y
`ruby-install` es usar el administrador de paquetes [Homebrew](https://github.com/Homebrew/homebrew)
como se describe a continuaci�n:

    $ brew update
    $ brew install chruby ruby-install

Si no usas Homebrew o usas un sistema operativo diferente a OS X, por favor
refi�rete a la siguiente documentaci�n:

1. [Instrucciones para instalar chruby](https://github.com/postmodern/chruby#install)
2. [Instrucciones instalar ruby-install](https://github.com/postmodern/ruby-install#install)

### Habilitar chruby

Para habilitar `chruby`, agrega lo siguiente a tu archivo ~/.bashrc o ~/.zshrc:

    source /usr/local/opt/chruby/share/chruby/chruby.sh

Refi�rete a la documentaci�n para encontrar m�s [instrucciones de configuraci�n](https://github.com/postmodern/chruby#configuration).

### Usar ruby-install

Una vez instales la utilidad `ruby-install`, instalar Rubinius es bastante
sencillo:

    $ ruby-install rbx 2.2.6

Para instrucciones sobre como instalar otras implementaciones de Ruby, por
favor refi�rete al [resumen de instalaci�n](https://github.com/postmodern/ruby-install#synopsis)
de `ruby-install`.

### Usar chruby

Ahora que Rubinius est� instalado, puedes activarlo con el siguiente comando:

    $ chruby rbx

## 2. Gemas

La mayor�a de las gemas que corren sobre MRI deber�an correr sobre Rubinius,
con excepci�n de las descritas m�s adelante. Adicionalmente, Rubinius pone
muchos componentes del sistema disponibles como gemas. Estos incluyen
herramientas de an�lisis y compilaci�n de c�digo Ruby, as� como el 
*debugger* y el *profiler*. Todas esas gemas son pre-instaladas cuando instalas
Rubinius.

### Gemas con C-extensions (extensiones de C)

Muchas gemas que usan extensiones de C corren bien sobre Rubinius. La 
excepci�n son las gemas que dependen de las estructuras de datos internas de
MRI. Estas gemas no pueden ser soportadas en Rubinius, entre las que se 
incluyen gemas como `ruby-debug` y `ruby-prof`. Rubinius provee sus propias
herramientas, tales como como su propio *debugger* y *profiler*. Para conocer
m�s detalles refi�rete a la [documentaci�n de las herramientas](http://rubini.us/doc/en/tools/).

## 3. Gemfiles

Tu Gemfile deber�a funcionar bien con Rubinius pero deber�as ejecutar 
`bundle update` para forzar que se vuelvan a calcular las dependencias de las 
gemas para Rubinius. Adicionalmente si est�s usando gemas que son
incompatibles con Rubinius, puedes ponerlas en un bloque llamado `platforms`
hasta que completes la migraci�n a Rubinius como se muestra a continuaci�n:

    # Ejemplo de un bloque con gemas espec�ficas a l plataforma MRI
    platforms :mri do
      gem 'ruby-prof'
      gem 'ruby-debug'
    end

## 4. Problemas de Compatibilidad

Rubinius cre� el proyecto [RubySpec](http://rubyspec.org), el cual mejora
continuamente, con el objetivo de describir el comportamiento de Ruby y 
monitorear la compatibilidad con MRI.

Fuera de unas pocas excepciones se espera que Rubinius sea compatible con la
versi�n 2.1 de MRI. Algunas caracter�sticas como los argumentos keyword, no han
sido implementados a�n. Puede que otras caracter�sticas no hayan sido
implementadas debido que son desconocidas y no existes RubySpecs para ellas 
a�n. Varios componentes de la librer�a est�ndar, incluyendo Continuation, 
Ripper, TracePoint, y Tracer, no han sido implementados a�n pero pueden ser
implementados en el futuro.

Si encuentras alg�n comportamiento incompatible en Rubinius en comparaci�n 
con MRI, probablemente sea un *bug*. Por favor [crea un issue](https://github.com/rubinius/rubinius/issues)
para reportarlo.
