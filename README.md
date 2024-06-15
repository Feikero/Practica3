# Practica 3
## Part A
### Codi en Línea 
```cpp
#include <WiFi.h>
#include <WebServer.h>
#include <Arduino.h>

// SSID & Password 
const char* ssid = "Redmi_Note_11S";  // Enter your SSID here 
const char* password = "12345678";  //Enter your Password here 
WebServer server(80);  // Object of WebServer(HTTP port, 80 is defult) 
void setup() { 
Serial.begin(115200); 
Serial.println("Try Connecting to "); 
Serial.println(ssid); 
// Connect to your wi-fi modem 
WiFi.begin(ssid, password); 
// Check wi-fi is connected to wi-fi network 
while (WiFi.status() != WL_CONNECTED) { 
delay(1000); 
Serial.print("."); 
} 
Serial.println(""); 
Serial.println("WiFi connected successfully"); 
Serial.print("Got IP: "); 
Serial.println(WiFi.localIP());  //Show ESP32 IP on serial 
server.on("/", handle_root); 
server.begin(); 
Serial.println("HTTP server started"); 
delay(100);  
} 
void loop() { 
server.handleClient(); 
} 
// HTML & CSS contents which display on web server 
String HTML = "<!DOCTYPE html>\ 
<html>\ 
<body>\ 
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\ 
</body>\ 
</html>"; 
// Handle root url (/) 
void handle_root() { 
server.send(200, "text/html", HTML); 
} 
```

### Explicació del codi
`1.Inclusió de llibreries`
```cpp
#include <WiFi.h>
#include <WebServer.h>
#include <Arduino.h>
```
Aquestes llibreries serveixen per poder utilitzar la conectivitat Wi-Fi, el servidor web i les funcions bàsiques d'Arduino.

`2.Declaració de variables i objectes`
```cpp
// SSID & Password 
const char* ssid = "Redmi_Note_11S";  // Enter your SSID here 
const char* password = "12345678";  // Enter your Password here 
WebServer server(80);  // Object of WebServer(HTTP port, 80 is default) 
```
- **'const char * ssid:'** Guarda el nom de la xarxa Wi-Fi.
- **'const char * password:'** Guarda la contrasenya de la xarxa Wi-Fi.
- **'WebServer server(80);:'** Crea un objecte server per controlar un servidor web al port 80 (el port HTTP predeterminat).

`3.Setup`
```cpp
void setup() { 
  Serial.begin(115200); 
  Serial.println("Try Connecting to "); 
  Serial.println(ssid); 
  // Connect to your wi-fi modem 
  WiFi.begin(ssid, password); 
  // Check wi-fi is connected to wi-fi network 
  while (WiFi.status() != WL_CONNECTED) { 
    delay(1000); 
    Serial.print("."); 
  } 
  Serial.println(""); 
  Serial.println("WiFi connected successfully"); 
  Serial.print("Got IP: "); 
  Serial.println(WiFi.localIP());  // Show ESP32 IP on serial 
  server.on("/", handle_root); 
  server.begin(); 
  Serial.println("HTTP server started"); 
  delay(100);  
} 
```
- **'Serial.begin(115200);:'** Inicia la comunicació serial a 115200 bauds.
- **'WiFi.begin(ssid, password);:'** Inicia la conexió a la xarxa Wi-Fi amb el SSID y la contrasenya guardats.
- **'while (WiFi.status() != WL_CONNECTED):'** Espera a que la conexió Wi-Fi se estableixi, mostrant com a sortida un punt cada segon.
- **'Serial.println("WiFi connected successfully");:'** Mostra com a sortida un missatge indicant que la conexió Wi-Fi s'ha establert.
- **'Serial.print("Got IP: "); Serial.println(WiFi.localIP());:'** Mostra com a sortida la direcció IP assignada al ESP32.
- **'server.on("/", handle_root);:'** Configura el servidor web per tractar solicituts en l'arrel ("/") cridant a la funció 'handle_root'.
- **'server.begin();:'** Inicia el servidor web.
- **'Serial.println("HTTP server started");:'** Mostra per pantalla un missatge indicant que el servidor HTTP se ha iniciat.

`4.Loop`
```cpp
void loop() { 
  server.handleClient(); 
} 
```
**'server.handleClient();:'** Tracta les solicituts dels clients conectats al servidor web. Aquesta funció deu ser cridada repetidament al bucle principal.

`5.HTML`
```cpp
String HTML = "<!DOCTYPE html>\ 
<html>\ 
<body>\ 
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\ 
</body>\ 
</html>";
```
Aquí es defineix una cadena que conté el codi HTML que s'enviará als clients quan visitin la página arrel del servidor.

`6.handle_root`
```cpp
void handle_root() { 
  server.send(200, "text/html", HTML); 
}
```
Aquí s'envia una resposta HTTP amb el codi d'estat 200 (OK), el tipús de contingut "text/html" i el contingut HTML definit anteriorment.

## Part B
### Codi en Línea 
```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"
 
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED) 
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it 
#endif 
BluetoothSerial SerialBT; 
void setup() { 
Serial.begin(115200); 
SerialBT.begin("IKer"); //Bluetooth device name 
Serial.println("The device started, now you can pair it with bluetooth!"); 
} 
void loop() { 
if (Serial.available()) { 
SerialBT.write(Serial.read()); 
} 
if (SerialBT.available()) { 
Serial.write(SerialBT.read()); 
} 
delay(20); 
} 
```

### Explicació del codi
`1.Inclusió de llibreries`
```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"
```
Aquí s'inclou la llibreria de 'Arduino.h' la qual ens permet utilitzar les funcions bàsiques del microcontrolador i la de 'BluetoothSerial.h' per poder controlar funcions bluetooth.

`2.Verificació de configuració Bluetooth`
```cpp
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
```
Aquesta secció es per verificar si Bluetooth y Bluedroid estan habilitats. En cas negatiu, es produeix un error de compilació amb el missatge indicat. Això és útil per assegurar-se de que el entorn de compilació está configurat correctament per soportar Bluetooth.

`3.Declaració d'objecte Bluetooth`
```cpp
BluetoothSerial SerialBT;
```
Es crea un objecte **'SerialBT'** de classe 'BluetoothSerial' que s'haurà de fer servir per controlar la comunicació bluetooth.

`4.Setup`
```cpp
void setup() { 
  Serial.begin(115200); 
  SerialBT.begin("IKer"); // Bluetooth device name 
  Serial.println("The device started, now you can pair it with bluetooth!"); 
}
```
**'Serial.begin(115200);:'** Inicia la comunicació serial a 115200 bauds.
**'SerialBT.begin("IKer");:'** Inicia la comunicació Bluetooth amb el nom del dispositiu "IKer".
**'Serial.println("The device started, now you can pair it with bluetooth!");:'** Mostra per pantalla un missatge en el monitor serial indicant que el dispositiu Bluetooth està llest per aparellar-se.

`5.Loop`
```cpp
void loop() { 
  if (Serial.available()) { 
    SerialBT.write(Serial.read()); 
  } 
  if (SerialBT.available()) { 
    Serial.write(SerialBT.read()); 
  } 
  delay(20); 
}
```
- **'if (Serial.available()) { SerialBT.write(Serial.read()); }:'** Si hi ha dades disponibles en el port serial (per exemple, des de la computadora), llegeix un byte de 'Serial' y ho envía per medi de Bluetooth utilitzant 'SerialBT.write()'.
- **'if (SerialBT.available()) { Serial.write(SerialBT.read()); }:'** Si hi ha dades disponibles en el port Bluetooth, llegeix un byte de 'SerialBT' y ho envía per medi del port serial (a la computadora) utilitzant 'Serial.write()'.
- **'delay(20);:'** Introdueix una petita pausa de 20 milisegons per evitar sobrecarregar la CPU.
