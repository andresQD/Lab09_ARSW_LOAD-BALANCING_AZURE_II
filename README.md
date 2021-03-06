### Escuela Colombiana de Ingeniería
### Arquitecturas de Software - ARSW

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/en-us/free/search/?&ef_id=Cj0KCQiA2ITuBRDkARIsAMK9Q7MuvuTqIfK15LWfaM7bLL_QsBbC5XhJJezUbcfx-qAnfPjH568chTMaAkAsEALw_wcB:G:s&OCID=AID2000068_SEM_alOkB9ZE&MarinID=alOkB9ZE_368060503322_%2Bazure_b_c__79187603991_kwd-23159435208&lnkd=Google_Azure_Brand&dclid=CjgKEAiA2ITuBRDchty8lqPlzS4SJAC3x4k1mAxU7XNhWdOSESfffUnMNjLWcAIuikQnj3C4U8xRG_D_BwE). Al hacerlo usted contará con $200 USD para gastar durante 1 mes.

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Escalabilidad Serverless (Functions)

1. Cree una Function App tal cual como se muestra en las  imagenes.

![](images/part3/part3-function-config.png)

![](images/part3/part3-function-configii.png)

2. Instale la extensión de **Azure Functions** para Visual Studio Code.

![](images/part3/part3-install-extension.png)

3. Despliegue la Function de Fibonacci a Azure usando Visual Studio Code. La primera vez que lo haga se le va a pedir autenticarse, siga las instrucciones.

![](images/part3/part3-deploy-function-1.png)

![](images/part3/part3-deploy-function-2.png)

4. Dirijase al portal de Azure y pruebe la function.

![](images/part3/part3-test-function.png)

5. Modifique la coleción de POSTMAN con NEWMAN de tal forma que pueda enviar 10 peticiones concurrentes. Verifique los resultados y presente un informe.

Con 10 peticiones concurrentes: 

![Screenshot_1](https://user-images.githubusercontent.com/48091585/80042055-a3cf9180-84c3-11ea-84fe-8b6f6ffac2a4.png)

![Screenshot_2](https://user-images.githubusercontent.com/48091585/80042056-a4682800-84c3-11ea-9316-c659997ab86b.png)

![Screenshot_3](https://user-images.githubusercontent.com/48091585/80042058-a500be80-84c3-11ea-9e6f-d82a6e079f7c.png)

![Screenshot_4](https://user-images.githubusercontent.com/48091585/80042059-a500be80-84c3-11ea-8e7b-aaa3c51e9624.png)

![Screenshot_5](https://user-images.githubusercontent.com/48091585/80042061-a500be80-84c3-11ea-930b-2fd69f518e1e.png)

![Screenshot_6](https://user-images.githubusercontent.com/48091585/80042063-a5995500-84c3-11ea-9fd1-6b2e0120d309.png)

![Screenshot_7](https://user-images.githubusercontent.com/48091585/80042064-a5995500-84c3-11ea-9cfb-bd118cc232ec.png)

![Screenshot_8](https://user-images.githubusercontent.com/48091585/80042065-a631eb80-84c3-11ea-9ee8-f22bcd09112d.png)

![Screenshot_9](https://user-images.githubusercontent.com/48091585/80042066-a631eb80-84c3-11ea-84d8-82ffc9e53715.png)

![Screenshot_10](https://user-images.githubusercontent.com/48091585/80042067-a631eb80-84c3-11ea-819a-0ca2cf5904f7.png)

El promedio de tiempo de las peticiones fue de 2min 23segundos.

6. Cree una nueva Function que resuleva el problema de Fibonacci pero esta vez utilice un enfoque recursivo con memoization. Pruebe la función varias veces, después no haga nada por al menos 5 minutos. Pruebe la función de nuevo con los valores anteriores. ¿Cuál es el comportamiento?.

![Screenshot_12](https://user-images.githubusercontent.com/48091585/80045596-8b647480-84cd-11ea-8839-643aac739765.png)

**Preguntas**

* ¿Qué es un Azure Function?

  es un servicio de computación sin servidor que permite ejecutar código activado por eventos sin tener que aprovisionar o administrar explícitamente la infraestructura.
  
* ¿Qué es serverless?

    La computación sin servidor es un modelo de ejecución de computación en la nube en el que el proveedor de la nube ejecuta el servidor y administra dinámicamente la asignación de recursos de la máquina. El precio se basa en la cantidad real de recursos consumidos por una aplicación, en lugar de en unidades de capacidad compradas previamente. Puede ser una forma de utilidad informática.
    
* ¿Qué es el runtime y que implica seleccionarlo al momento de crear el Function App?

  es el tiempo de ejecución del trabajo del lenguaje que se cargará en la aplicación de función. Se corresponderá con el lenguaje usado en la aplicación (por ejemplo, "dotnet"). El cual se da mediante el FUNCTIONS_WORKER_RUNTIME presente en a configuracion inicial de la aplicación. Este se agregó con el fin de mejorar la huella y el tiempo de inicio de la aplicacion.
  
* ¿Por qué es necesario crear un Storage Account de la mano de un Function App?

    Es necesario debido a que contiene contiene todos sus objetos de datos de Azure Storage: blobs, archivos, colas, tablas y discos. La cuenta de almacenamiento proporciona un espacio de nombres único para sus datos de Azure Storage al que se puede acceder desde cualquier lugar del mundo a través de HTTP o HTTPS. Los datos en su cuenta de almacenamiento de Azure son duraderos y altamente disponibles, seguros y escalables de forma masiva.
    
* ¿Cuáles son los tipos de planes para un Function App?, ¿En qué se diferencias?, mencione ventajas y desventajas de cada uno de ellos.
    
    Plan de App Service: se ejecutan las funciones igual que aplicaciones web. Si ya usa App Service para las otras aplicaciones, las funciones pueden ejecutarse en el mismo plan sin costo adicional
    
  Plan de consumo: Azure proporciona todos los recursos de cálculo necesarios. No tiene que preocuparse de la administración de recursos y solo paga por el tiempo que haya empleado en la ejecución del código.

  Plan Premium: especifique un número de instancias activadas previamente que siempre están en línea y preparadas para responder de inmediato. Cuando se ejecuta la función, Azure proporciona todos los recursos informáticos adicionales que sean necesarios. Se paga tanto por las instancias activadas previamente que se ejecutan de forma continua como por todas las instancias adicionales que se usen cuando Azure reduce y escala horizontalmente la aplicación.

* ¿Por qué la memoization falla o no funciona de forma correcta?

  Esto no funciona correccta debido a que al ejecutar la aplicación se excede el límite de recursión, normalmente suela suceder cuando la operacion se realiza con números muy grandes como lo es en este caso 1000000.
  
* ¿Cómo funciona el sistema de facturación de las Function App?

  La facturacion se basa en dos componentes. El primero es mediante el numero total de ejecuciones solicitadas al mes, y el segundo es mediante el consumo de recurso que se observa, medido en gigabytes por segundo.
  
* Informe

Podemos evidenciar que al implementar memoization, se reduce el tiempo en un minuto, podemos concluir que es una mejor forma de implementar esta solución

![Screenshot_11](https://user-images.githubusercontent.com/48091585/80045415-ef3a6d80-84cc-11ea-9bbc-387fc3bddb0c.png)

