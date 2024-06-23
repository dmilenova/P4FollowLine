# P4FollowLine
Explicación de ejecución y métodos utilizados:

Para la implementación de esta práctica hemos dividido nuestro código en dos partes, uno que comunica el Arduino con el ESP-32, y otro que se comunica de forma inversa. Además de comunicarse entre ellos, el código que se carga en el Arduino lee los valores del sensor de distancia y del infrarrojo para detectar tanto línea como obstáculos, y el del ESP-32 se utiliza para enviar los mensajes MQTT al Arduino.

A continuación se detalla una breve explicación de cada uno de ellos:

Comunicación Arduino-ESP32: 

Primero, configuramos los pines del infrarrojos y del sensor de distancia, al igual que los pines asignados a los motores (puestos en HIGH para que vayan hacia adelante). 

Después creamos un bucle while en el que leemos el puerto serie y esperamos a que nos llegue desde el ESP-32 un "mensaje de confirmación" que nosotros hemos llamado "--------", para que comience a ejecutar el resto del programa (este mensaje "-------" solo llegará una vez el ESP-32 se haya conectado tanto a la Wi-Fi como al servidor MQTT). 


[!["Video de demostración"](blob:https://urjc-my.sharepoint.com/529f144a-2bd8-4e6f-9d58-ea01bc0a8a79)](https://urjc-my.sharepoint.com/:v:/g/personal/d_milenova_2019_alumnos_urjc_es/EZ7R67EjpsNIgJ6sOvRk3rABhWEhnUhP-ohHuZIk0_vjDQ?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=AldwIv)


