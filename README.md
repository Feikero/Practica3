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
