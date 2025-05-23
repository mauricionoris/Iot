A diretiva `INPUT_PULLUP` é uma **configuração especial de entrada digital** usada com a função `pinMode()` no Arduino/ESP32. Ela ativa um **resistor de pull-up interno** conectado ao pino de entrada. Vamos entender isso em partes:

---

## O que é `INPUT_PULLUP`?

```cpp
pinMode(pino, INPUT_PULLUP);
```

* **INPUT**: configura o pino como entrada simples.
* **INPUT\_PULLUP**: configura o pino como entrada **e ativa um resistor de pull-up interno**.

---

## O que é um resistor pull-up?

É um resistor que conecta o pino de entrada ao nível lógico **HIGH (3.3V ou 5V)**. Ele serve para manter o pino em um estado definido (não flutuante) **quando nada está conectado a ele**.

* Sem pull-up: o pino "flutua", captando ruídos elétricos, o que causa leitura instável.
* Com pull-up: o pino permanece em HIGH até que seja **forçado a LOW** (ex: botão pressionado que conecta o pino ao GND).

---

## Exemplo Prático: Botão com INPUT\_PULLUP

### Esquema:

* Um botão entre **GPIO4 e GND**
* Sem resistor externo — usamos o pull-up interno

### Código:

```cpp
const int botao = 4;

void setup() {
  pinMode(botao, INPUT_PULLUP); // Entrada com resistor pull-up interno
  Serial.begin(115200);
}

void loop() {
  if (digitalRead(botao) == LOW) {
    Serial.println("Botão pressionado");
  } else {
    Serial.println("Botão solto");
  }
  delay(500);
}
```

### Funcionamento:

* Sem pressionar o botão: pino está em **HIGH** (graças ao pull-up)
* Ao pressionar o botão: pino vai para **LOW** (curto para GND)

---

## Vantagens do `INPUT_PULLUP`

* **Economiza componentes**: não precisa de resistor externo
* **Facilita a prototipagem**
* Funciona bem com **botões, chaves** e sensores digitais com saída open-drain


## Observação

 O `pinMode()` no Arduino (e plataformas compatíveis como ESP32) serve para configurar o **modo de operação** de um pino digital. Abaixo estão **todos os principais modos disponíveis**, incluindo os específicos do **ESP32**, que é mais avançado que o Arduino UNO padrão.



## Modos do `pinMode()` – Arduino/ESP32

| Modo                | Descrição                                                                |
| ------------------- | ------------------------------------------------------------------------ |
| `INPUT`             | Configura o pino como **entrada digital**. Espera receber HIGH ou LOW.   |
| `INPUT_PULLUP`      | Entrada digital com **resistor de pull-up interno** ativado.             |
| `OUTPUT`            | Configura o pino como **saída digital**. Pode escrever HIGH ou LOW.      |
| `INPUT_PULLDOWN`    | *(ESP32 somente)* Entrada digital com **resistor de pull-down interno**. |
| `OUTPUT_OPEN_DRAIN` | *(ESP32 somente)* Saída tipo open-drain (sem HIGH interno).              |



### `INPUT`

* Espera receber sinais digitais (0V ou 3.3V no ESP32)
* Não tem resistor interno → o pino **pode flutuar** se estiver desconectado

---

### `INPUT_PULLUP`

* Mesmo que `INPUT`, **mas ativa um resistor de pull-up interno** (\~45kΩ a 100kΩ)
* Mantém o pino em **nível HIGH até que algo o puxe para LOW**

---

### `INPUT_PULLDOWN` *(somente no ESP32)*

* Como o `INPUT_PULLUP`, mas o resistor conecta o pino ao **GND**
* Mantém o pino em LOW até que um sinal externo o leve a HIGH
* Útil quando o botão está conectado entre **3.3V e o pino** (o contrário do habitual)

```cpp
pinMode(4, INPUT_PULLDOWN); // Somente no ESP32
```

---

### `OUTPUT`

* Permite enviar sinais digitais: `HIGH` (3.3V) ou `LOW` (0V)
* Usado para acionar LEDs, relés, buzzers, etc.

---

### `OUTPUT_OPEN_DRAIN` *(somente no ESP32)*

* O pino **nunca gera HIGH internamente**
* Só pode:

  * Conectar ao GND (LOW)
  * **Ficar flutuando (alta impedância)** — para gerar HIGH, precisa de pull-up externo

Usos típicos:

* I2C em alguns modos
* Barramentos compartilhados
* Comunicação entre dispositivos com diferentes tensões (com pull-up externo)

```cpp
pinMode(5, OUTPUT_OPEN_DRAIN);
digitalWrite(5, LOW);  // Conecta ao GND
digitalWrite(5, HIGH); // Fica flutuando (espera pull-up externo gerar HIGH)
```

---

## Comparação Visual

| Situação          | INPUT | INPUT\_PULLUP | INPUT\_PULLDOWN | OUTPUT | OPEN\_DRAIN        |
| ----------------- | ----- | ------------- | --------------- | ------ | ------------------ |
| Pino flutua?      | Sim   | Não           | Não             | Não    | Depende (sem HIGH) |
| Resistor interno? | Não   | Pull-up       | Pull-down       | —      | Não                |
| Usa no botão?     | Sim   | Sim ✔️        | Sim (ESP32) ✔️  | —      | —                  |



[Retornar](./ExplorandoESP32.md)