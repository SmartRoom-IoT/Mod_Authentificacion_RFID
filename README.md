# Mod-_Authentificacion_RFID

# 📄 Sistema de Control de Acceso RFID con ESP32

Este es un sistema de control de acceso basado en la lectura de tarjetas RFID (MFRC522), implementado en una placa **ESP32**, con conectividad **MQTT** para reportar eventos, y un servomotor que simula la apertura de una puerta. El sistema también utiliza LEDs para indicar el estado del acceso (permitido/denegado/espera) y registra fecha y hora de cada evento mediante sincronización NTP.

---

## 🧰 Requisitos de Hardware

- Placa **ESP32 Dev Module**
- Módulo lector **RFID MFRC522**
- Servomotor (por ejemplo: SG90)
- 3 LEDs (pueden ser: amarillo, rojo y blanco)
- Resistencias (220Ω - 330Ω)
- Protoboard y cables de conexión
- Fuente de alimentación adecuada
- Red WiFi disponible

---

## 📚 Librerías Necesarias

Antes de cargar este sketch en tu ESP32, asegúrate de tener instaladas las siguientes librerías:

1. **ESP32Servo**  
   - Herramientas > Administrador de bibliotecas > Buscar `ESP32Servo`
   - Alternativa: [https://github.com/madhephaestus/ESP32Servo](https://github.com/madhephaestus/ESP32Servo) 

2. **MFRC522**  
   - Herramientas > Administrador de bibliotecas > Buscar `MFRC522`
   - Alternativa: [https://github.com/miguelbalboa/rfid](https://github.com/miguelbalboa/rfid) 

3. **PubSubClient**  
   - Herramientas > Administrador de bibliotecas > Buscar `PubSubClient`
   - Alternativa: [https://github.com/knolleary/pubsubclient](https://github.com/knolleary/pubsubclient) 

4. **WiFi.h / SPI.h / time.h**  
   - Estas vienen preinstaladas con el soporte de ESP32

> 💡 **Importante:** Asegúrate de tener instalado el paquete de placas ESP32 en Arduino IDE:
>
> Archivo > Preferencias > Additional Boards Manager URLs:  
> `https://dl.espressif.com/dl/package_esp32_index.json`   
> Luego, Herramientas > Placa > Board Manager > Instalar "ESP32 by Espressif Systems"

---

## ⚙️ Conexiones del Hardware

### MFRC522 a ESP32

| MFRC522 | ESP32         |
|--------|---------------|
| SDA    | GPIO D5 (SS)   |
| SCK    | GPIO D18       |
| MOSI   | GPIO D23       |
| MISO   | GPIO D19       |
| RST    | GPIO D27       |
| GND    | GND           |
| 3.3V   | 3.3V          |

### Servomotor

- Señal: GPIO 22  
- VCC: 5V (o según especificaciones del servo)  
- GND: GND  

### LEDs

- LED Amarillo (Acceso permitido): GPIO 2  
- LED Rojo (Acceso denegado): GPIO 25  
- LED Blanco/España (Espera): GPIO 15  

---

## 🛠️ Configuración Inicial

Edite los siguientes valores en el código antes de cargarlo:

```cpp
// WiFi
const char* ssid = "Tu_red";
const char* password = "Tu_Contraseña";

// MQTT
//Localmente la dirección ip de tu equipo o un servidor publico
//Ejemplo: const char* mqtt_server = "192.168.75.107"
// o const char* mqtt_server = "test.mosquitto.org"
const char* mqtt_server = "Tu_servidor";
const int mqtt_port = 1883;
const char* mqtt_topic = "casa/acceso/rfid";     // Tópico de acceso
const char* mqtt_status = "casa/acceso/estado";  // Tópico de estado

// Zona horaria
const long gmtOffset_sec = -18000; // UTC-5 (ajusta si es necesario)
