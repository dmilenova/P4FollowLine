P4FollowLIne:

Como funciona:

Para la practica tenemos que comunicar entre si la ESP32 y el arduino. Para ello lo que hemos hecho es un Serial en ESP32 para que se comunique con el arduino.

  // Serial port to communicate with Arduino UNO
  Serial2.begin(9600, SERIAL_8N1, RXD2, TXD2);

Este serial será el que estara leyendo los mensajes que el arduino va mandando, tales como, que empieza vuelta, pierde la linea...

Para leer los mensajes que esta mandando el arduino creamos un buffer vacio, basicamente un string vacio. Comprobamos si el Serial2 esta disponible y leemos el char del serial y 
lo guardamos en el buffer. Como los mensajes que manda nuestro arduino son ya mensaje completo, comprobamos todo el rato si el caracter leido es "}", en caso de que sea ese significa
que hemos llegado al final del mensaje y debemos enviarlo. Para ello entramos en un if que al principio elimina espacios en blanco tanto al principio como al final del buffer. Luego 
hacemos otra comprobacion para ver si la longitud del buffer es mayor que 2, pues este debe empezar por "{" y acabar en "}". Si no es mayor que 2 el mensaje es descartado. En caso de que
sea un mensaje correcto el que se ha recibido, lo publicamos por el MQTT.

El MQTT Esta configurado de la siguiente manera:

Adafruit_MQTT_Client mqtt(&espClient, SERVER, SERVERPORT);
Adafruit_MQTT_Publish test = Adafruit_MQTT_Publish(&mqtt, "/SETR/2023/16/");

Donde server espClient es un WifiClient, Server:193.147.53.2 y SERVERPORT:21883.

Respecto a Arduino.

Como funciona es muy sencillo:

En el bucle loop para empezar tenemos una cuenta atras para que de tiempo al ESP32 a conectarse a la red WiFI, una vez que este delay termina, empezamos a cronometrar el tiempo de la vueltay
y mandamos el mensaje de que empieza vuelta. Luego empezamos a leer de los sensores para comprobar si esta o no en la linea, además de comprobar si hay un obstaculo cerca. En caso de que 
el sensor derecho o izq, dejen de detectar la linea veran cual es la ultima lectura que hicieron, si fue izquierda, iran a la derecha y viceversa. Además mandaran mensajes de linea perdida
o linea encontrada en cada caso. 

Para el ping es un counter de tiempo que cada 4 segundos manda el mensaje.

Para finalizar la vuelta es necesario detectar el obstaculo. Para ello vamos leyendo en cada iteracion el sensor de distancia y comprobando a cuanto esta. Cuando este cerca este se parara
y enviara un mensaje diciendo la distancia a la que esta el obstaculo. Ademñas enviara un mensaje de que ha acabado la vuelta y el tiempo que ha tardado.



Observaciones.

En la entrega extraordinaria nos fallo que los mensajes se mandaban en una cadena con muchos mensajes y no uno por uno. Esto se debia a que estabamos leyendo el string completo del serial 
y este a veces era un conjunto de muchos mensajes. Para solucionarlo hemos implementado el buffer que va guardando los caracteres y al encontrar el final del mensaje, lo envia.
Además tambien nos fallo que cuando lo solucionamos en el examen enviaba caracteres nulos, esto fue porque no esperabamos a que hubiera datos en el serial ni esperabamos a que estuviera
disponible, por lo cual enviaba "basura". Con los ultimos retoques hemnos conseguido que envie los mensajes correctamente.

https://github.com/dmilenova/P4FollowLine/assets/73531592/3b097510-2268-460d-be57-1f8f8d8906a2




