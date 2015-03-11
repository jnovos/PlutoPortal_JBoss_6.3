# Jboss y Apache Pluto Portal

Para integrar Pluto Portal 1.1.7 en Jboss EAP 6.3.0 GA tememos que seguir los siguientes pasos :

  * Descargarse JBoss Eap 6.3.0 GA desde [aquí](http://www.redhat.com/j/elqNow/elqRedir.htm?ref=https://www.jboss.org/download-manager/content/origin/files/sha256/7f/7f4e6d63196edc3cf15240b693391b9c0be474cda2194ba7575be31881f1a3d5/jboss-eap-6.3.0.zip).
  * Descargarse Apache Pluto Portal 1.1.7 desde [aquí](http://archive.apache.org/dist/portals/pluto/BINARIES/v1.1.7/pluto-1.1.7-bundle.zip).
  * Una vez descargado el Jboss lo instalamos en la carpeta donde queramos.
  * Acedemos a la carpeta /jboss-eap-6.3/bin y añadimos un nuevo usuario `administración` , por ejemplo `admin` con las contraseña `admin123` ;-), con el comando add-user.sh, si de da error la configuración de la contraseña puedes cambiar las preferencias en `add-user.properties` en la misma carpeta. Comentando `password.restriction.minAlpha`, `password.restriction.minSymbol` y cambianddo a `password.restriction.strength=VERY_WEAK`.
  * Añade un nuevo usuario del tipo `Aplicación` con nombre `pluto` la contraseña la que quieras.
* Decomprimimos pluto-1.1.7-bundle.zip, este zip contiene Pluto Portal sobre Tomcat, lo necesitamos para saber que librería compartidas tenemos que utilizar.
  * Vamos a la carpeta `jboss-eap-6.3/modules/system/layers/base/org/apache` y creamos una nueva carpeta `pluto/main` aquí metemos los jars de la carpeta `/pluto-1.1.7/shared/lib`.
  * Creamos en la carpeta `jboss-eap-6.3/modules/system/layers/base/org/apache/pluto/main` el fichero `module.xml` y descargamos `jstl-1.2.jar` de [aquí](http://download.java.net/maven/1/jstl/jars/jstl-1.2.jar) y lo guardamos en esta carpeta.
  *  Damos de alta el nuevo modulo en `jboss-eap-6.3/standalone/configuration/standalone.xml` en tag `subsystem xmlns="urn:jboss:domain:ee:1.2` como global
            <global-modules>
                <module name="org.apache.pluto" slot="main"/>
            </global-modules>
Esto también se puede hacer vía administración web.
  * En la carpeta `pluto-1.1.7/PlutoDomain` descomprimimos `pluto-portal-1.1.7.war` que es la aplicación del pluto portal. Acedemos a la carpeta descomprimida `/pluto-1.1.7/PlutoDomain/pluto-portal-1.1.7/WEB-INF/lib` y copiamos todos los jars a `/pluto-1.1.7/shared/lib`.
  * Eliminar del war `pluto-portal-1.1.7.war` los jars que hay en /WEB-INF/lib.
  * Modificar del war `pluto-portal-1.1.7.war` el fichero `MANIFEST.MF`del `/META-INF/` añadiendo `Dependency: org.apache.pluto`.
  * Eliminamos los portlets de prueba `plutotestsuite` del fichero `/WEB-INF/pluto-portal-driver-config.xml`.
  * Copiamos de la carpeta `/WEB-INF/classes/`el fichero `ToolTips.properties` y lo metemos dentro del jar con nombre `pluto-portal-driver-1.1.7.jar` en  `jboss-eap-6.3/modules/system/layers/base/org/apache/pluto/main`
  * Renombrar el war `pluto-portal-1.1.7.war` a `pluto.war`
  * Copiar `pluto.war` a `jboss-eap-6.3/standalone/deployments`.
  * Una vez creado ejecutamos JBoss o desde standalone.bat, si estas en windows o standalone.sh si estas en linux en la carpeta `bin`.
  * Puedes acceder a la web desde http://localhost:8080/pluto, te pedirá usuario y contraseña, el usuario es pluto.