# Proyecto_IoT
# Sistema IoT de Monitoreo Ambiental en Tiempo Real

Este proyecto consiste en un sistema IoT full-stack desarrollado para el monitoreo automatizado de temperatura y humedad ambiental. El ecosistema integra la adquisición de datos por hardware, el almacenamiento en la nube en tiempo real y el despliegue de una interfaz web (Dashboard) interactiva para el análisis y exportación de datos históricos.

## 🚀 Arquitectura del Sistema

El sistema sigue una arquitectura IoT de tres capas:
1. **Capa de Percepción (Hardware):** Microcontrolador ESP8266 (ESP-12E) acoplado a un sensor digital DHT11.
2. **Capa de Red y Almacenamiento (Cloud):** Protocolo HTTP/Secure hacia Firebase Realtime Database.
3. **Capa de Aplicación (Software/Web):** Servidor backend en Python (Flask) y frontend dinámico con JavaScript (Chart.js y Firebase SDK).

---

## 🛠️ Tecnologías y Requisitos

### Hardware
* **Microcontrolador:** NodeMCU 1.0 (ESP-12E Module)
* **Sensor:** DHT11 (Sensor de humedad y temperatura)
* **Conectividad:** Antena integrada Wi-Fi de 2.4 GHz

### Software & Librerías
* **Firmware:** Arduino IDE (C++)
  * `ESP8266WiFi.h`
  * `FirebaseESP8266.h` (por Mobizt)
  * `DHT.h` (por Adafruit)
* **Backend:** Python 3.x
  * `Flask`
  * `firebase-admin`
* **Frontend:** HTML5, CSS3 (Estructura Cyberpunk/Modern), JavaScript (ES6)
  * `Chart.js` (Librería de gráficos vectoriales)

---

## 🔌 Diagrama de Conexión (Hardware)

El mapeo de pines entre el sensor DHT11 (Módulo de 3 pines) y la placa ESP-12E se realizó de la siguiente manera:

| Patita DHT11 | Función | Pin NodeMCU ESP-12E |
| :--- | :--- | :--- |
| **`+` / VCC** | Alimentación Eléctrica | **3V3** (Línea de 3.3V) |
| **`-` / GND** | Tierra Común | **GND** |
| **`S` / OUT** | Señal de Datos Digitales | **D2** (GPIO4) |

---

## ⚙️ Implementación del Código

### 1. Inicialización y Sincronización del Servidor (Firmware)
Para optimizar el rendimiento y la precisión temporal de los datos, el firmware del ESP-12E se configuró para inyectar la directiva nativa del servidor de Firebase para marcas de tiempo (`timestamp/ .sv`), evitando el desfase horario local del microcontrolador.

### 2. Backend Flask (`app.py`)
El servidor Python actúa como puente para la renderización de la interfaz y la compilación dinámica de reportes históricos en formatos planos de ingeniería:

* **`/download/csv`**: Consulta la base de datos, parsea los timestamps de Unix (milisegundos) a objetos `datetime` de Python y descarga un archivo `.csv` compatible con hojas de cálculo estructuradas.
* **`/download/txt`**: Genera un archivo `.txt` legible con formato de logs estructurados para auditorías manuales.

### 3. Frontend Interactivo (`dashboard.js`)
Implementa WebSockets implícitos mediante el método `.on('value')` del SDK de Firebase. Escucha asíncronamente los últimos 10 registros históricos, formatea el eje X utilizando `Date.prototype.toLocaleTimeString()` en JavaScript y renderiza gráficos lineales dinámicos.

---

## 📊 Resultados Obtenidos

El proyecto cumplió satisfactoriamente con todos los requerimientos técnicos y funcionales planteados:

1. **Conectividad Estable:** El microcontrolador demostró resiliencia en la red local (`WANTELCO9143`) en la banda de 2.4 GHz, implementando un sistema automático de reconexión de sockets en caso de parpadeo del router.
2. **Persistencia Exacta:** Se eliminaron los problemas de ordenamiento temporal ("Invalid Date") mediante la migración exitosa a marcas de tiempo Unix por Unix-Epoch.
3. **Visualización en Tiempo Real:** La interfaz web procesa las fluctuaciones térmicas en menos de 1.2 segundos tras el envío del hardware.
4. **Exportación de Datos:** Los módulos de descarga generan reportes limpios, permitiendo a los usuarios finales realizar análisis estadísticos en software externo con marcas temporales explícitas en formato `DD/MM/AAAA HH:MM:SS`.
