XBuilder requiere que tu configures algunos valores como la moneda de emisión, el valor del IGV, etc. Para mayor información lee la documentación oficial [crear-un-config](https://project-openubl.github.io/docs/xbuilder/create-xml#crear-un-config)

El presente ejemplo tiene variables como el IGV escrito en el código; sin embargo, en tu software tu puedes decidir cargar esos valores desde tu base de datos, variables de entorno, archivos, etc.

## Crea `ConfigDefaults.java`

Aquí tienes una clase Java llamada `ConfigDefaults.java` que contiene todo lo requerido por XBuilder.

Has click en el siguiente comando para que pueda ser ejecutado:

````shell
cat << EOF > xbuilder-quickstart/src/main/java/org/openubl/xbuilder/ConfigDefaults.java
package org.openubl.xbuilder;

import io.github.project.openubl.xmlbuilderlib.config.Config;
import io.github.project.openubl.xmlbuilderlib.models.catalogs.Catalog10;
import io.github.project.openubl.xmlbuilderlib.models.catalogs.Catalog7;
import io.github.project.openubl.xmlbuilderlib.models.catalogs.Catalog9;

import java.math.BigDecimal;

public class ConfigDefaults implements Config {
    private BigDecimal igv = new BigDecimal("0.18");
    private BigDecimal ivap = new BigDecimal("0.04");
    private String defaultMoneda = "PEN";
    private String defaultUnidadMedida = "NIU";
    private Catalog9 defaultTipoNotaCredito = Catalog9.ANULACION_DE_LA_OPERACION;
    private Catalog10 defaultTipoNotaDebito = Catalog10.AUMENTO_EN_EL_VALOR;
    private BigDecimal defaultIcb = new BigDecimal("0.2");
    private Catalog7 defaultTipoIgv = Catalog7.GRAVADO_OPERACION_ONEROSA;

    @Override
    public BigDecimal getIgv() {
        return igv;
    }

    @Override
    public BigDecimal getIvap() {
        return ivap;
    }

    @Override
    public String getDefaultMoneda() {
        return defaultMoneda;
    }

    @Override
    public String getDefaultUnidadMedida() {
        return defaultUnidadMedida;
    }

    @Override
    public Catalog9 getDefaultTipoNotaCredito() {
        return defaultTipoNotaCredito;
    }

    @Override
    public Catalog10 getDefaultTipoNotaDebito() {
        return defaultTipoNotaDebito;
    }

    @Override
    public BigDecimal getDefaultIcb() {
        return defaultIcb;
    }

    @Override
    public Catalog7 getDefaultTipoIgv() {
        return defaultTipoIgv;
    }
}
EOF
```{{execute}}

## Observa el contenido de `ConfigDefaults.java`

Abre el archivo `xbuilder-quickstart/src/main/java/org/openubl/xbuilder/ConfigDefaults.java`{{open}} y analiza el contenido.



## Crea `DefaultSystemClock.java`

Aquí tienes una clase Java llamada `DefaultSystemClock.java` que contiene todo lo requerido por XBuilder.

Has click en el siguiente comando para que pueda ser ejecutado:

```shell
cat << EOF > xbuilder-quickstart/src/main/java/org/openubl/xbuilder/DefaultSystemClock.java
package org.openubl.xbuilder;

import io.github.project.openubl.xmlbuilderlib.clock.SystemClock;

import java.util.Calendar;
import java.util.TimeZone;

public class DefaultSystemClock implements SystemClock {

    @Override
    public TimeZone getTimeZone() {
        return TimeZone.getTimeZone("America/Lima");
    }

    @Override
    public Calendar getCalendarInstance() {
        return Calendar.getInstance();
    }

}
EOF
```{{execute}}

## Observa el contenido de `DefaultSystemClock.java`

Abre el archivo `xbuilder-quickstart/src/main/java/org/openubl/xbuilder/DefaultSystemClock.java`{{open}} y analiza el contenido.

Tenemos todo configurado y estamos listos para ver el XML generado por nuestro proyecto Java.

## Ejecuta el proyecto Maven

Ejecuta el proyecto usando el comando:

`mvn -f xbuilder-quickstart compile exec:java -Dexec.mainClass="org.openubl.xbuilder.Main"`{{execute T1}}


### Visualiza el XML con una herramienta

- Copia el XML resultado de ejecutar el comando del primer paso.
- Abre la página https://jsonformatter.org/xml-pretty-print y pega el XML.
