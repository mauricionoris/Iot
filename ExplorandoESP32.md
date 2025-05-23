
# Portas Multiuso no ESP32: Explorando os GPIOs


## 1. O que são GPIOs?

**GPIO (General Purpose Input/Output)** significa "Entrada/Saída de Propósito Geral". São pinos digitais que podem ser programados via software para:

* Ler sinais digitais ou analógicos (entrada)
* Enviar sinais digitais ou analógicos (saída)
* Atuar como interfaces de comunicação (UART, SPI, I2C)
* Gerar sinais PWM (modulação por largura de pulso)
* E mais...

No ESP32, a maioria dos pinos físicos pode ser configurada dinamicamente para diferentes funções — por isso, são chamadas de **portas multiuso**.

---

## 2. Visão Geral das GPIOs no ESP32

###  Quantidade e Numeração

* O ESP32 possui até **34 GPIOs**, numeradas de **GPIO0 a GPIO39**.
* Nem todas as GPIOs estão disponíveis nos modelos de placas. Por exemplo, a placa **ESP32 DevKit v1** geralmente disponibiliza de 25 a 30 GPIOs utilizáveis.

###  Restrições e cuidados

| GPIO                    | Restrição / Observação                         |
| ----------------------- | ---------------------------------------------- |
| GPIO0, GPIO2, GPIO15    | Usados no boot (modo de inicialização)         |
| GPIO6–11                | Usados internamente para flash SPI, evite usar |
| GPIO34–GPIO39           | Apenas entrada, sem pull-up/down internos      |
| GPIO1 (TX) e GPIO3 (RX) | Usados como interface serial padrão (UART)     |

---

##  3. Tipos de Funções Possíveis em uma GPIO

###  3.1 Entrada Digital

* Detecta níveis lógicos: **HIGH (1)** ou **LOW (0)**.
* Pode usar resistores **pull-up** ou **pull-down** internos.

```cpp
pinMode(4, INPUT_PULLUP); // Habilita pull-up interno no GPIO4
int estado = digitalRead(4);
```

###  3.2 Saída Digital

* Controla dispositivos como LEDs, relés, buzzers.

```cpp
pinMode(2, OUTPUT);
digitalWrite(2, HIGH); // Liga LED
```

###  3.3 PWM (Pulse Width Modulation)

* Controla intensidade de brilho ou velocidade (ex: motores).
* Usado com `ledcWrite()` no ESP32.

```cpp
ledcAttachPin(2, 0); // GPIO2, canal 0
ledcSetup(0, 5000, 8); // freq. 5kHz, resolução 8 bits
ledcWrite(0, 128); // 50% duty cycle
```

###  3.4 Entrada Analógica (ADC)

* Permite leitura de tensões variáveis (ex: sensores, potenciômetro).
* Resolução padrão: 12 bits (0 a 4095)

```cpp
int valor = analogRead(34); // GPIO34 como entrada analógica
```

>  Faixa de leitura: 0–3.3V (ou menos, dependendo da configuração de atenuação).

### 3.5 Saída Analógica (DAC)

* Apenas em GPIO25 e GPIO26
* Gera sinal analógico real (via conversor DAC)

```cpp
dacWrite(25, 128); // Gera ~1.65V (metade de 3.3V)
```

###  3.6 Comunicação Serial

* GPIOs podem funcionar como portas de comunicação:

  * **UART** (Serial): GPIO1 (TX), GPIO3 (RX)
  * **I2C**: geralmente GPIO21 (SDA) e GPIO22 (SCL)
  * **SPI**: GPIO18 (CLK), GPIO19 (MISO), GPIO23 (MOSI), GPIO5 (CS)

---

##  4. Exemplo Prático: Controle de LED com Botão

###  Circuito:

* Botão entre GPIO4 e GND
* LED entre GPIO2 e resistor de 220Ω, ligado ao GND

###  Código:

```cpp
int botao = 4;
int led = 2;

void setup() {
  pinMode(botao, INPUT_PULLUP);
  pinMode(led, OUTPUT);
}

void loop() {
  if (digitalRead(botao) == LOW) { // Botão pressionado
    digitalWrite(led, HIGH);
  } else {
    digitalWrite(led, LOW);
  }
}
```

---

##  5. Tabela Resumo de Funções GPIO (ESP32 DevKit)

| Função                   | GPIOs Comuns                           |
| ------------------------ | -------------------------------------- |
| Entrada Digital          | 0–39 (exceto GPIO6–11)                 |
| Saída Digital            | 0–33                                   |
| Entrada Analógica (ADC1) | 32–39                                  |
| Entrada Analógica (ADC2) | 0, 2, 4, 12–15, 25–27                  |
| Saída Analógica (DAC)    | 25, 26                                 |
| PWM                      | Qualquer GPIO com função de saída      |
| UART                     | 1, 3 (default), mas pode ser remapeado |
| I2C                      | 21 (SDA), 22 (SCL) — configurável      |
| SPI                      | 5, 18, 19, 23 (exemplo) — configurável |

---

##  6. Conclusão

* GPIOs são o coração da interação do ESP32 com o mundo real.
* O uso correto exige atenção ao **datasheet**, **restrições de boot**, e **funções múltiplas por software**.
* Uma boa prática é **documentar o uso de cada pino** no início do seu projeto.

---

##  Atividade Proposta

Monte um circuito com:

* Um botão no GPIO4
* Um LED no GPIO2
* Um sensor LDR no GPIO34

Desenvolva o código para:

* Acender o LED ao pressionar o botão **ou**
* Acender o LED automaticamente se a luminosidade cair abaixo de um nível mínimo.


[Retornar](./GPIO.md)

## Material Adicional
- [ESP32 Series Datasheet (Versão 4.9)](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)
- [PinModes - PullUP](./PULLUP.md)
