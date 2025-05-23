---
description: >-
  Este proyecto lee los correos de una carpeta y genera una entrada en una base
  de datos
icon: envelope-open-text
coverY: 0
---

# Lectura de Correos

Flujo:

{% tabs %}
{% tab title="Diagrama" %}
<figure><img src="../../../.gitbook/assets/LecturaCorreosDiagrama.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Descripcion" %}
Paso 1: Se monitorea el correo a travez del Event Bridge programado.

Paso 2: Se dispara un lambda que toma los datos del correo y son enviados a un SQS a travez de un SNS.

Paso 3: Un lambda lee el SQS y genera una entrada a la base de datos
{% endtab %}

{% tab title="Actores por paso" %}
Paso 1:

* Even Bridge: [TriggerMailReader](https://us-east-1.console.aws.amazon.com/scheduler/home?region=us-east-1#/schedules/default/TriggerMailReader)

Paso 2:

* Lambda: [LambdaGmailListener](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions/LambdaGmailListener)
* SNS: [CorreoEventos](https://us-east-1.console.aws.amazon.com/sns/v3/home?region=us-east-1#/topic/arn:aws:sns:us-east-1:970547342167:CorreoEventos)
* SQS: [LecturaCorreos-Recibidos](https://us-east-1.console.aws.amazon.com/sqs/v3/home?region=us-east-1#/queues/https%3A%2F%2Fsqs.us-east-1.amazonaws.com%2F970547342167%2FLecturaCorreos-Recibidos)

Paso 3:

* Lambda: [LambdaCorreoProcessor](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions/LambdaCorreoProcessor)
* Base de datos: [db-lecturacorreosaws](https://us-east-1.console.aws.amazon.com/rds/home?region=us-east-1#database:id=db-lecturacorreosaws;is-cluster=false)
{% endtab %}
{% endtabs %}

Tecnologias generales:

* Codigo: C# 8.0
* Base de datos: MySQL 8.0.40

Repositorio:

* General: [https://github.com/aGonzalezHdez/LecturaCorreosSeparado-AWS](https://github.com/aGonzalezHdez/LecturaCorreosSeparado-AWS)
* Proyectos:

<table><thead><tr><th width="174.66668701171875">Proyecto</th><th>Descripcion</th></tr></thead><tbody><tr><td>DataBaseInitializer</td><td>Contiene la estructura de la Base de Datos utilizando EF</td></tr><tr><td>MailRead</td><td>Lectura de correos y envio de informacion al SNS</td></tr><tr><td>EmailProcessor</td><td>Genera la entrada a la base de datos.</td></tr></tbody></table>

Especificaciones:

* Se leen los correos cada 5 min.
* Correo:
  * El correo debe estar en la carpeta llamada Desarrollo.
  * Debe estar como no leído.
