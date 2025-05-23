
## Aula 3 – Eletrônica Básica e GPIOs com ESP32 (Wowki)

### Objetivos da Aula

* Compreender os conceitos fundamentais de corrente, tensão e resistência.
* Identificar componentes básicos como resistores e LEDs.
* Aprender a montar circuitos simples na protoboard.
* Controlar entradas e saídas digitais com a placa ESP32 Wowki.
* Realizar testes práticos com LEDs e botões.

---

### Estrutura da Aula

#### **1. Introdução à Eletrônica**

* **Conceitos-chave**:

  * Tensão (Voltagem): diferença de potencial elétrico (V)
  * Corrente: fluxo de elétrons (A)
  * Resistência: oposição ao fluxo de corrente (Ω)
* **Detalhes importantes**:

  * [Lei de Ohm](./Lei_ohm.md): `V = I × R`
  * [Eletrônica Básica](./EletronicaBasica.md)

    

* **Componentes**:

  * Resistores: código de cores
  * LEDs: polaridade (ânodo/cátodo)
  * Protoboard: linhas de alimentação e trilhas

---

#### **2. Conhecendo o ESP32 Wowki**

* Visão geral da placa:

  * Portas GPIO
  * Fonte de alimentação (USB, 3.3V, GND)
  * Comunicação serial
* Ambiente de programação:

  * Arduino IDE (ou alternativa como PlatformIO)
  * Código-fonte básico: configuração de pino como `OUTPUT` e `INPUT`

---

### *[Detalhes do microcontrolador ESP32](./ExplorandoESP32.md)*

---

#### **3. Saídas Digitais: Piscar LED**

* **Circuito**:

  * ESP32 GPIO → resistor (220Ω) → LED → GND
* **Código Arduino**:

  ```cpp
  void setup() {
    pinMode(2, OUTPUT); // GPIO2
  }

  void loop() {
    digitalWrite(2, HIGH);
    delay(500);
    digitalWrite(2, LOW);
    delay(500);
  }
  ```

> *Atividade prática*: Cada aluno montar e testar o circuito do LED piscante.

---

#### **4. Entradas Digitais: Leitura de Botão**

* **Circuito**:

  * Botão entre GPIO e GND
  * `INPUT_PULLUP` no código
* **Código Arduino**:

  ```cpp
  void setup() {
    pinMode(4, INPUT_PULLUP); // GPIO4 com resistor interno
    pinMode(2, OUTPUT);       // LED no GPIO2
  }

  void loop() {
    if (digitalRead(4) == LOW) {
      digitalWrite(2, HIGH);
    } else {
      digitalWrite(2, LOW);
    }
  }
  ```




# Para saber mais

- [Getting Started with PlatformIO](https://www.youtube.com/watch?v=JmvMvIphMnY)
- [Blink LED Circuit on Wokwi](https://wokwi.com/projects/431766921937642497)
  