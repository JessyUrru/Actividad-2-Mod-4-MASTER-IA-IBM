# Parte II: puesta en producción (30%)

Tareas propias de la automatización del ciclo de vida y puesta en producción del modelo/s (guiadas con ejemplo práctico):

1. Integración de pre-procesado de datos y entrenamiento del modelo en una pipeline automatizada.
1. Gestión de entornos virtuales, versionado de código, dependencias, configuración, etc.
1. Creación y conexión a repositorios para versionado de modelos, métricas y artefactos.
1. Creación de aplicación en la nube para automatizar entrenamiento.
1. Apificación del modelo en la nube (inferencia).

La nota final de la Parte II dependerá de los siguientes parámetros:

1. Haber completado las tareas establecidas
1. La calidad del código
1. Cualquier elemento adicional considerado por el alumno (incluir test unitarios, visualización de métricas, etc.).
1. Claridad y calidad del contenido y razonamientos en el documento descriptivo.


* Basado en: git@github.com:tenoglez/Proyecto_Modulo4_UEM_2021_TRAIN.git

## Pasos (Mac):

1. Instalar pip3:

```pip3 install virtualenv ```

1. Crear y activar el entorno de Python:

```virtualenv .env```
```source .env/bin/activate```

1. Instalar dependencias
```pip3 install -r apps/001-train/src/requirements.txt```

1. Ejecutar la aplicación en local:

```cd apps/001-train/src/ && python3 run.py```

1. Chequear que la app Flask está ok:

```open http://0.0.0.0:8000/```

1. Apagar la app con ctrl + c

1. Desactivar el entorno virtual

```deactivate```

1. Copiar el fichero apps/001-train/src/manifest.yml-dist en apps/001-train/src/manifest.yml, cambiar el nombre de la aplicación, pues debe ser único.

1. Instalar IBM Cloud CLI

```curl -sL https://raw.githubusercontent.com/IBM-Cloud/ibm-cloud-developer-tools/master/linux-installer/idt-installer | bash```

1. Verificar que ibm cloub cli está instalado ok:

```ibmcloud dev help```

1. Hacer login en ibm cloud cli, solicitará las credenciales de IBM Cloud:

```ibmcloud login```

1. Instalar el cli de Cloud foundry:

```ibmcloud cf install```

1. (Opcional) Cambiar el grupo de recursos a usar, pues por defecto pone el "Default" - en IBM CLoud web se puede crear un grupo de recursos.

```ibmcloud target -g "el-grupo-de-recursos-que-queremos-usar"```

1. Activar el target de CloudFoundry 

```ibmcloud target --cf```

1. Cambiar el directorio actual donde esté la app a desplegar:

```cd apps/001-train/src```

1. Empujar a IBM Cloud Foundry la aplicación

```ibmcloud cf push```

1. Ver los logs (El nombre de la app es el que pone en manifest.yml):

```ibmcloud cf logs modulo-4-actividad-1-parte-II-train --recent```

1. Obtener el endpoint de la app (Ver donde pone "url" y abrirlo en el navegador):

```ibmcloud cf apps```

1. Configurar el storage - para subir los artefactos generados en el cloud storage previamente, activando el Cloud Storage en el grupo de recursos y luego creando un bucket, Al bucker llamarle igual a como se indica en el argumento bucket_name de train_model.py

1. En la sección de Cloud Storage > Service Credentials crear una credencial, de tipo writer. Una vez creada la credencial copiar el fichero vcap-local.json-dist a vcap-local.json y actualizar solo la sección cloud-object-storage > credentials, exceptuando endpoints que debe ser https://s3.eu-gb.cloud-object-storage.appdomain.cloud    ; con los datos de la credencial recién creada.

1. Empujar los cambios realizados (Estando en apps/001-train/src):

```ibmcloud cf push```

1. Autorizar la conexión desde CF a COS, en COS > Connections > Create Connections y luego darle a "Restage".

1. Activar Cloudant para almacenar las metricas. Esto se hará en IBM Cloud y crear una nueva credencial de acceso al servicio con rol manager, actualizar el fichero vcap-local.json en la sección "services"> "cloudantNoSQLDB"> "credentials": { y empujar nuevamente la app Estando en apps/001-train/src):

```ibmcloud cf push```

1. Autorizar la conexión desde CF a Cloudant creando una connexión en Cloudant > Connections > Create Connections y luego darle a "Restage".

1. Crear una BD no-particionada en Cloudant mediante el dashboard de Cloudant. Llamarle igual a como se indica en el argumento model_info_db_name de train_model.py para no tener que re-desplegar la app. (vcap-local.json)

1. Crear una entrada en la BD, ya que load_model_config le necesita. Esto es en el dashboar de cloudant,, darle al "Create Document" y escribir:

{
  "_id": "model_config",
  "model_config": {
      "model_name": "RandomForest",
      "n_estimators": 100,
      "max_features": 5,
      "target": "Survived",
      "cols_to_remove": [
          "PassengerId","Name","Ticket","Cabin"
      ]
  }
}


1. Ejecutar la aplicación de entrenamiento, primero obtener la URL de la app con el comando que se indica y luego abrir dicha url con el path/train-model en el navegador:

```ibmcloud cf apps```

```curl "AQUI-LA-URL-DEL-TRAIN/train-model"```

1. Comprobar que en la BD existe la entrada del entrenamiento.

1. Comprobar que en el cloud storage existe el modelo en fichero.

## Predicción:

1. Copiar el fichero apps/002-predict/src/manifest.yml-dist en apps/002-predict/src/manifest.yml, cambiar el nombre de la aplicación, pues debe ser único.

1. Copiar el fichero apps/001-train/src/vcap-local.json en apps/002-predict/src/vcap-local.json

1. Cambiarse a la carpeta apps/002-predict/src

```cd apps/002-predict/src```

1. Empujar la app:

```ibmcloud cf push```

1. Repetir el paso de autorizar la conexión desde CloudAnt y ClooudStorage - darle a re-stage en ambas y esperar el re-despliegue.

1. Obtener el endpoint de la app de predict (Ver donde pone "url")

1. Ejecutar las predicciones mediante POST request (Via Curl) 

```curl -X POST -H "Content-Type: application/json" --data '[[10,2,"Nasser, Mrs. Nichols","female","12,1,0,"237736",30.0708, null, "C"]]'  "AQUI-LA-URL-DEL-PREDICT/predict"```

* Nota: Esto debería llevar content-type application/json pero no está así el código.





