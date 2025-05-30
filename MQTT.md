# **MQTT com ESP32 — Guia Completo**

## O que é MQTT?

MQTT (Message Queuing Telemetry Transport) é um protocolo de mensagens leve ideal para aplicações de Internet das Coisas (IoT). Ele utiliza um modelo de comunicação do tipo **publicador/assinante (pub/sub)**, sendo ideal para dispositivos com recursos limitados, como o **ESP32**.

### Conceitos-chave

* **Broker**: servidor intermediário que recebe e distribui mensagens.
* **Tópicos**: canais nomeados para envio e recebimento de mensagens.
* **Publicador (Publisher)**: envia dados para um tópico.
* **Assinante (Subscriber)**: escuta mensagens de um tópico.

---

## Pré-requisitos

### Hardware:

* ESP32
* Sensor (opcional, como DHT11)
* LED (opcional)

### Software:

* Arduino IDE com suporte ao ESP32
* Biblioteca **PubSubClient**
* Conexão Wi-Fi

---

## Instalação das bibliotecas

1. Instale o suporte ao ESP32 na Arduino IDE.
2. Vá em **Sketch > Include Library > Manage Libraries...**
3. Pesquise e instale:

   * `PubSubClient` (por Nick O'Leary)
   * `WiFi` (já incluída para ESP32)

---

## Configuração de um Broker MQTT

Você pode usar um broker local ou um serviço online. Algumas opções:

* **Mosquitto** (instalável localmente)
* **HiveMQ** (serviço gratuito online)
* **broker.hivemq.com** (teste público)

---

## Exemplo 1 — Publicador simples com ESP32

O ESP32 envia uma mensagem ao broker a cada 5 segundos.

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";
const char* mqttServer = "broker.hivemq.com";
const int mqttPort = 1883;

WiFiClient espClient;
PubSubClient client(espClient);

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Conectando ao WiFi...");
  }

  Serial.println("Conectado!");

  client.setServer(mqttServer, mqttPort);

  while (!client.connected()) {
    if (client.connect("ESP32Client")) {
      Serial.println("Conectado ao MQTT Broker!");
    } else {
      Serial.print("Falha, rc=");
      Serial.print(client.state());
      delay(2000);
    }
  }
}

void loop() {
  client.loop();
  client.publish("esp32/teste", "Ola do ESP32!");
  delay(5000);
}
```

---

## Exemplo 2 — Assinando um tópico e controlando um LED

O ESP32 escuta mensagens e liga/desliga um LED.

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";
const char* mqttServer = "broker.hivemq.com";
const int mqttPort = 1883;
const int ledPin = 2;

WiFiClient espClient;
PubSubClient client(espClient);

void callback(char* topic, byte* payload, unsigned int length) {
  String message;
  for (int i = 0; i < length; i++) {
    message += (char)payload[i];
  }

  if (message == "ligar") {
    digitalWrite(ledPin, HIGH);
  } else if (message == "desligar") {
    digitalWrite(ledPin, LOW);
  }
}

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Conectando ao WiFi...");
  }

  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);

  while (!client.connected()) {
    if (client.connect("ESP32LEDClient")) {
      client.subscribe("esp32/led");
    } else {
      delay(2000);
    }
  }
}

void loop() {
  client.loop();
}
```

---

## Dicas de Teste com MQTT

Você pode testar os tópicos com ferramentas como:

* **MQTT.fx** (aplicativo desktop)
* **MQTT Explorer**
* **Console Web da HiveMQ**: [https://www.hivemq.com/demos/websocket-client/](https://www.hivemq.com/demos/websocket-client/)


# **MQTT com ESP32 — Funcionamento, Topologia e Limitações**

---

## 1. **Como Funciona o MQTT**

O MQTT é baseado no modelo **pub/sub (publicador/assinante)**. Diferente do modelo tradicional cliente-servidor, os dispositivos não se comunicam diretamente entre si, mas através de um intermediário: o **broker**.

### 1.1. Fluxo básico:

1. O **dispositivo publicador** envia uma mensagem para um **tópico**.
2. O **broker MQTT** recebe a mensagem e a encaminha para todos os **assinantes** daquele tópico.
3. Os **assinantes** processam a mensagem recebida.

### 1.2. Exemplo:

* O ESP32 publica `"25.4"` no tópico `"sensor/temperatura"`.
* Todos os dispositivos assinando `"sensor/temperatura"` recebem esse valor automaticamente.

---

## 2. **Topologias de Uso**

### 2.1. Topologia Centralizada

A mais comum. Todos os dispositivos se conectam a um único broker (ponto central de comunicação).

**Vantagens:**

* Simples de implementar.
* Ideal para pequenos sistemas e testes.

**Desvantagens:**

* Ponto único de falha.
* Limitações de escalabilidade em sistemas maiores.

### 2.2. Topologia em Nuvem

Utiliza brokers em serviços na nuvem (como AWS IoT, Azure IoT Hub, HiveMQ Cloud, Mosquitto remoto).

**Vantagens:**

* Alta disponibilidade.
* Pode ser acessado de qualquer lugar.

**Desvantagens:**

* Depende de conexão constante à Internet.
* Latência maior do que uma solução local.

### 2.3. Topologia Híbrida

Dispositivos locais conectam-se a um broker local que sincroniza dados com um broker na nuvem.

**Vantagens:**

* Alta resiliência.
* Operação offline parcial.

---

## 3. **QoS (Quality of Service)**

Define o nível de garantia de entrega das mensagens:

| Nível | Nome          | Garantia de Entrega                           | Uso típico                     |
| ----- | ------------- | --------------------------------------------- | ------------------------------ |
| 0     | At most once  | A mensagem é enviada uma vez sem confirmação. | Telemetria sem criticidade     |
| 1     | At least once | A mensagem é reenviada até ser confirmada.    | Mensagens importantes          |
| 2     | Exactly once  | A mensagem é entregue **uma única vez**.      | Controle preciso (menos comum) |

---

## 4. **Tópicos e Hierarquias**

Os tópicos são **strings separadas por barra ("/")**, e funcionam como caminhos:

Exemplo:

```
casa/sala/temperatura
casa/cozinha/umidade
```

### Curingas:

* `+`: um nível (ex: `casa/+/temperatura`)
* `#`: todos os níveis seguintes (ex: `casa/#`)

---

## 5. **Limitações do MQTT com ESP32**

### 5.1. Recursos do dispositivo

* O ESP32 possui RAM limitada (cerca de 520KB). O uso de muitos tópicos, QoS alto, e buffers grandes pode causar travamentos.
* É recomendável manter as mensagens pequenas (<256 bytes).

### 5.2. Confiabilidade da conexão Wi-Fi

* Uma queda de conexão Wi-Fi interrompe a comunicação MQTT.
* Recomenda-se implementar **reconexão automática** ao Wi-Fi e ao broker.

### 5.3. Segurança

* O protocolo MQTT não é seguro por padrão.
* Para proteger a comunicação:

  * Use **TLS/SSL** (com brokers que suportam MQTTs).
  * Adote **autenticação com usuário/senha**.
  * Utilize autenticação baseada em certificados para alta segurança (com ESP32-WROOM/ESP32-S3, por exemplo).

---

## 6. **Limites técnicos do protocolo MQTT**

| Recurso                  | Valor Típico                            |
| ------------------------ | --------------------------------------- |
| Tamanho da mensagem      | Até 256MB (prática: <1KB com ESP32)     |
| N° de conexões no broker | Depende do broker (mosquitto: milhares) |
| Níveis de tópico         | Ilimitados, mas recomendado < 5         |
| Retenção de mensagens    | Sim (usando `retain = true`)            |
| Persistência             | Broker pode armazenar mensagens offline |

---

## 7. **Casos de Uso Comuns com ESP32**

* Sensores (temperatura, umidade, gás)
* Automação residencial (luzes, portas, alarmes)
* Monitoramento de dispositivos industriais
* Controle de irrigação
* Alarmes remotos

---

## 8. **Dicas Avançadas para Projetos MQTT com ESP32**

* Use `client.loop()` frequentemente ou crie uma **task FreeRTOS** para manter a conexão.
* Use o tópico `$SYS/` para monitorar o estado do broker (conexões ativas, uptime etc).
* Evite reconectar com muita frequência — implemente **retentativas com atraso exponencial**.
* Combine com **OTA (Over The Air)** para atualizar seu firmware via MQTT.




## Próximos passos

* Adicionar sensores e publicar dados (ex: temperatura).
* Usar autenticação com usuário e senha no broker.
* Criar uma dashboard com Node-RED ou plataformas como ThingsBoard.


# **Exemplo: Publicar Temperatura Simulada no Wokwi via MQTT**

### O que será feito:

* ESP32 simulado envia dados de temperatura (gerados aleatoriamente).
* Usa MQTT para publicar os dados no tópico `wokwi/temperatura`.
* Broker: `test.mosquitto.org` (público).

---

### 1. Código para o Wokwi

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";

const char* mqtt_server = "test.mosquitto.org";

WiFiClient espClient;
PubSubClient client(espClient);

void setup_wifi() {
  delay(10);
  Serial.println("Conectando ao WiFi...");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("WiFi conectado");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Tentando conexão MQTT...");
    if (client.connect("ESP32-WokwiClient")) {
      Serial.println("conectado!");
    } else {
      Serial.print("falhou, rc=");
      Serial.print(client.state());
      delay(2000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  float temperatura = random(200, 300) / 10.0; // 20.0 a 30.0 °C
  char payload[10];
  dtostrf(temperatura, 4, 1, payload);

  client.publish("wokwi/temperatura", payload);
  Serial.print("Publicado: ");
  Serial.println(payload);

  delay(5000);
}
```

---

### 2. Como testar no Wokwi

1. Acesse: [https://wokwi.com](https://wokwi.com)
2. Clique em **New Project** > **ESP32 Project**.
3. Cole o código acima no `sketch.ino`.
4. Clique em **Start Simulation**.
5. Abra o monitor serial para ver a publicação.
6. Você pode usar o MQTT Explorer ou [HiveMQ Web Client](https://www.hivemq.com/demos/websocket-client/) e se conectar ao broker `test.mosquitto.org` para verificar o tópico `wokwi/temperatura`.

---

### 3. Comparativo MQTT vs HTTP vs CoAP

| Protocolo | Modelo     | Overhead    | Confiabilidade | Recomendado para ESP32? | Característica principal            |
| --------- | ---------- | ----------- | -------------- | ----------------------- | ----------------------------------- |
| MQTT      | Pub/Sub    | Baixo       | Alta (QoS 0–2) | Sim                     | Ideal para IoT, leve e eficiente    |
| HTTP      | Requisição | Alto        | Alta           | Parcialmente            | Comunicação direta, mais pesado     |
| CoAP      | Requisição | Muito baixo | Média          | Sim (com bibliotecas)   | Muito leve, ideal para redes locais |

---

### 4. Próximo passo

* Adicionar sensores reais (DHT22, LDR etc).
* Testar QoS 1 e 2.
* Simular comandos recebidos no ESP32 (assinante).

