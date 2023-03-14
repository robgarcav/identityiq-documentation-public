# Herramienta de Documentación de IdentityIQ

Esta herramienta permite a implementadores generar de forma automática documentación para su configuración de IdentityIQ, basandose en ficheros de configuracion XML. Estos ficheros deben haber sido fusionados en un unico documento XML.

## Ficheros del Proyecto

La herramienta se distribuye como un fichero XIP llamado `identityiq-documentation-generator-<version>-bin.zip`. En este fichero zip, hay tres carpetas principales: - `doc`: contiene este fichero README.md. - `lib`: contiene el fichero jar principal. - `xslt`: contiene todos los ficheros de estilo XSLT.

El fichero jar se llamará `IdentityIQDocumentationGenerator.jar`. Este fichero contiene todas las dependencias necesarias.

El fichero de estilo principal es `IdentityIQ-Documenter.xsl`, pero el fichero `IdentityIQ-Documenter-Config.xsl` puede ser utilizado para configurar qué cosas deben ser documentadas y cómo de detallada debe ser la documentación.

## Colección de ficheros

Hay dos formas de obtener los ficheros para la documentación:

1. Exportado por consola.
2. Recoger los ficheros de la construcción por SSB.

### Exportado de Consola

Los ficheros puede ser exportados desde la consola de IdentityIQ con la siguiente instrucción indicando los tipos de objeto a exportar:

```
> export -clean /home/jdoe/export.xml Application Rule Bundle ObjectConfig
```

Esto producirá un solo fichero con todos los objetos de las clases especificadas. Esto incluirá también todos los objetos que son de caja.

### Recoger ficheros desde SSB

En este escenario, solo los ficheros modificados/actualizados serán incluidos. Primero, el comando del SSB para construir la configuración y los fuentes que se debe ejecutar para generar todos los ficheros de configuración con las opciones parametrizadas en los ficheros. Por ejemplo:

```
./build.sh -Dtarget=production clean war
```

Una vez ese comando se completa correctamente, los ficheros de configuración se encontrarán bajo la ruta `build/extract/WEB-INF/config/custom`. Estos ficheros pueden ser combinados en un único fichero usando la herramienta XML Merger.

Después de que ese comando se complete correctamente, los archivos de configuración se encontrarán en `build/extract/WEB-INF/config/custom`. Estos archivos se pueden combinar en un solo archivo utilizando la herramienta [SailPointXMLMerger](https://github.com/menno-pieters-sp/sailpoint-xml-merger).

Suponiendo que esta herramienta se encuentra en `/home/jdoe/lib` como `SailPointXMLMerger.jar` y la salida debe ser archivada en `/home/jdoe/Documents`, el comando podría verse así:

```
java -jar /home/jdoe/lib/SailPointXMLMerger.jar build/extract/WEB-INF/config/custom > /home/jdoe/Documents/identityiq-production.xml
```

## Uso

Suponiendo que tiene un archivo `/home/jdoe/Documents/identityiq-production.xml`, también debe colocar el archivo DTD para la versión de IdentityIQ en la misma carpeta con el nombre `sailpoint.dtd`. También suponiendo que la herramienta de documentación está instalada en /home/jdoe/lib como `IdentityIQDocumentationGenerator.jar` y los archivos XSLT están en `/home/jdoe/xslt`, puede llamar a la herramienta de documentación de la siguiente manera:

```
java -jar /home/jdoe/lib/IdentityIQDocumentationGenerator.jar -IN /home/jdoe/Documents/identityiq-production.xml -XSL /home/jdoe/xslt/IdentityIQ-Documenter.xsl -HTML > /home/jdoe/Documents/identityiq-production.html
```

El fichero `/home/jdoe/Documents//home/jdoe/Documents/identityiq-production.html` puede visualizarse con un navegador WEB.
