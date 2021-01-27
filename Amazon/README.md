# Amazon Web Services

## AWS Global Infra

* Hay 64 zonas en 22 regiones del mundo.
* **Availability Zones**: Uno o mas Data Centers.
* **Region**: Lugar en el mundo con muchos Availability Zones
* **Edge Location**: Datacenter cuyo dueño no es AWS.

### Regiones

* Lugar en el mundo con muchos DataCenters
* Cada region tiene minimo 2 AZ
* El mayor es US EAST Virginia
* No todos los servicios estan en todas las regiones
* USEAST1 tiene toda la info de Pricing $$
* USEAST tiene todos los servicios

### Availability Zones

* Es un datacenter de AWS donde corren los servicios
* Son region code y un numero (US-EAST-2A)
* Hay muchos AZ ya que si uno cae se puede pasar a otro.
* Su ubicacion esta calculada para que haya poca latencia.

### Edge Locations

Es un datacenter manejado no por AWS pero tiene una correccion directa a AWS

* Cloudfront y Route S3 son servicios que se manejan en estos locations
* S3 y Api Gateway tambien hacen eso
* Son un endpoint especial para que la data vaya mas rapido ya que hay baja latencia

### Regiones GovCloud

* Es una region para informacion sensible
* Solo para ciudadanos del US y entidades especiales

## IAM

* Politicas de Password (min caracteres, etc)
* Cambiar URL ruta del dashboard
* Establecer grupos
* Activar MFA (multifactor Auth) con autenticator app
* Crear usuario IAM para no usar siempre el root
* Creamos el usuario y podemos darle un acceso admin
* **Poweruser**: Tiene mucho acceso pero con limitaciones como el agregar usuarios
* Si quiero acceder como root es con Email y MFA

### Org y Cuentas

* Root User: Todos los account tienen su root user y todo el org.
* Organizacion: Control centralizad de todos
* Org Units: Grupos de cuentas de AWS que dentro pueden contener otros org units.

## Elastic Cloud Compute EC2

* Lanzas servidores/Instancias
* Elegir servidor, especs del servidor, roles (hay que crear uno), espacio que tendrá, crear keypair para launch (puedo sin el keypair pero no es seguro) y correr el servidor
* Apagar y stopear la instancia

### Sessions Manager

* Recomendado por AWS
* Se puede acceder a la consola del servidor (linux) desde la pantalla de EC2
* Con SM puedo logearme a la consola como root
* Ver historial de sesiones

## Amazon Machine Image AMI

* Copia de todo el servidor
* Hago copias del servidor creado para que, a partir de una imagen, pueda hacer copias con mejor procesador, espacio, etc..

## AutoScaling Groups

* Muchos servers corren, que al menos siempre este corriendo uno (o un minimo)
* Si tengo una app con mucho trafico, se determinan cuantas instancias estaran levantadas y viceversa
* EC2 -> Auto Scaling -> Auto Scaling Groups
* Si termino todas las instancias, el auto scaling group pondra a correr nuevas instancias de vuelta.

## Elastic Load Balancer

* EC2/Load Balancers
* Distribuye el trafico en muchas instancias para que no colapse
* Si una instancia se llena, se pasa todo a otra instancia automaticamente para que no haya un no servicio
* Hay 3 tipos de Load Balancers, App y Classic
* Minimo deben tener 2 AZ
* Target group -> Referencia a las instancias

## Simple Storage Service S3

* Es global, no requiere un AZ particular
* Lugar de los archivos, debo crear un bucket para esto

## CloudFront

* Es un distribuidor de contenido, por ejemplo, si quiero distribuit una imagen uso este servicio, copia ese elemento en muchas partes del mundo y asi va a cargar rapido en cualquier lugar del mundo
* Debo seleccionar un Bucket y va a teer un DNS en si mismo
* Tarda mucho en crear

## Relational Database Service RDS

### Amazon Aurora

* Es el motor de base de datos relacional mas caro de todos
* Maneja varios templates
  * Production, 600 dolares por mes, para grandes empresas
  * Dev/Test Para empresas medianas, 262 dolares
  * Freetier es prueba, gratis, son 720 horas en la herramienta, y despues 15 dolares al mes.
* Existe la opcion de autoscaling para el limite de la base de datos
* Para acceder tengo que abrir la instancia en PC tipo session para usarlo, no desde AWS.

### Servicios de BD

* **Dynamo DB**: NoSQL, key/value, como cassandra
* **Document DB**: NoSQL, Document, compatible con mongoDB
* **RDS**: Relacional, soporta varios motores, es el mas popular
* **Aurora**: MySQL y PSQL (Posgres) mas rapido
* **Aurora Serverless:** Corre solo cuando lo necesitas, como lambda
* **Neptune**: GraphDB
* **Redshift**: ColumnarDB, en vez de rows son columnas
* **Elasticashe**: Redis o memcachedDB, para catching

## EC2 Pricing Models

### On Demand

* LowCost, flexible, pagás por hora, no sabes cuanto vas a usar, no es long term, no se interrumpe como el spot pero es mas caro.
* La instancia es terminada por AWS cuando sea necesario, es para rellenar lugares disponibles

### Spot

* Para Jobs que no son criticos, es muy barato (90% off)

### Reserved Instances

* **Scheduled**: Se reserva por periodos de tiempo determinados
* **Standar**: No puedo cambiar ningun atributo
* **Convertible**: Puedo cambiar algunos atributos pero no ahorro tanto
* Es bueno para el largo plazo, ahorras hasta un 75% cuando se sabe de que tamaño va a ser la app. Mas long term, mas ahorras
* **Dedicated**: Cumple con las regulaciones de seguridad, no comparte hardware. Es para empresas, es el mas caro.
* Hay servicios gratuitos, pero algunos pueden provisionar servicios que si salen dinero.

## Storage Services

* **S3** : Objetos, espacio ilimitado
* **S3 Glacier**: De bajo costo para backups longterm
* **Storage Gateway**: Se puede usar como backup. Es una solucion hibrida.
* **EBS**: Un hard drive que se attachea a las instancias EC2 (SSD, HDD, etc)
* **EFS**: Lo mismo al anterior pero a varios EC2 a la vez
* **Snowball**: Mover mucha data de un lado a otro de forma rapida, esta Edge que tiene mas espacio y es una version mejor, y Snowmobile, es un datacenter que va en camion.

## API Gateway

Crear, publicar, mantener, monitorear y asegurar APIs de cualquier tamaño. Se hace una API para reutilizar codigo para web o mobile.
Tambien sirve para mantener las versiones de la API para que funcione bien en todos los clientes.

## Lambda Foundations

### Serverless Computing

* La infraestructura es abstracta
* Te concentras en el codigo de la app
* El codigo solo corre cuando se necesita, de lo contrario no se cobra nada

### On Premise

* Sistema Operativo
* Actualizaciones
* Mantenimiento
* Monitoreo de Instancias

## Lambda

* Puedo elegir en que lenguaje va a ser hecha la lambda
* Con el permiso basico puede escribir logs en Cloudwatch
* Son funciones que corren por poco tiempo, maximo 15 minutos
* Puedo hacer triggers, por ejemplo, si algo se inserta en la DynamoDB se corre la lambda
* Permite correr el codigo sin servidores, se corre con triggers definidos por el usuario, y es autoescalable dependiendo de la demanda
* IAM Policy -> Lambda puede poner un item en Dynamo
* Trust Policy  -> Lambda puede asumir un rol tuyo e invocar a la funcion con tus permisos
* Tambien incluye logs para Cloudwatch
* Un uso comun seria: API Call -> API Gateway -> Lambda
* Lambda corre codigo que puede escribir en una tabla Dynamo y trabajar con otros servicios de AWS.
* Las lambdas usan IAM (herramienta de permisos) para controlar el acceso a las funciones
* **IAM Resource Policy / Function Policy:**
  Permisos para invocar a la funcion (Cuando hay un trigger, puedo invocar a esta lambda?)
* **IAM Excecution Role**: Qué puede hacer la funcion de Lambda

### Como funciona una Lambda?



