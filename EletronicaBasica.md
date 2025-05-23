# Conceitos-Chave de Eletrônica: Tensão, Corrente e Resistência

### 1. **Tensão (Voltagem) – Unidade: Volt (V)**

#### O que é?

A **tensão elétrica** é a diferença de potencial elétrico entre dois pontos. Ela é a "força" que empurra os elétrons através de um circuito. Sem tensão, não há movimento de carga elétrica — ou seja, não há corrente.

#### Analogia com hidráulica:

Pense em um sistema de encanamento: a **tensão** é como a **pressão da água**. Quanto maior a pressão, mais força para mover a água pelo cano. Na eletricidade, quanto maior a tensão, mais força para empurrar os elétrons pelos fios.

#### Fórmula:

$$
V = \frac{W}{Q}
$$

* $V$: Tensão (Volts)
* $W$: Trabalho (Joules)
* $Q$: Carga elétrica (Coulombs)

#### Exemplo:

* Uma pilha comum tem 1,5V — isso significa que ela pode fornecer uma diferença de potencial de 1,5 volts entre seus polos positivo e negativo.

---

### 2. **Corrente – Unidade: Ampère (A)**

#### O que é?

A **corrente elétrica** é o fluxo de elétrons através de um condutor. Representa **quantos elétrons** passam por um ponto do circuito em determinado tempo.

#### Analogia com hidráulica:

Se a tensão é a pressão da água, a **corrente** é o **fluxo da água**: quantos litros passam por segundo. Na eletricidade, é o número de elétrons por segundo.

#### Fórmula:

$$
I = \frac{Q}{t}
$$

* $I$: Corrente (Ampères)
* $Q$: Carga elétrica (Coulombs)
* $t$: Tempo (segundos)

#### Exemplo:

* Um LED pode funcionar com correntes muito baixas, como 0,02 A (20 mA).

---

### 3. **Resistência – Unidade: Ohm (Ω)**

#### O que é?

A **resistência elétrica** é a **oposição à passagem da corrente** em um material. Componentes como **resistores** são usados intencionalmente para limitar a corrente em um circuito.

#### Analogia com hidráulica:

Imagine um cano estreito: ele dificulta a passagem da água. A resistência faz isso com os elétrons — quanto maior a resistência, mais difícil para a corrente passar.

#### Lei de Ohm:

$$
V = I \times R
$$

* $V$: Tensão (Volts)
* $I$: Corrente (Ampères)
* $R$: Resistência (Ohms)

#### Exemplo:

* Um resistor de 220Ω limita a corrente para que um LED não queime quando alimentado com 3.3V.

---

### Resumo Visual:

| Conceito    | Símbolo | Unidade | Definição simples               | Analogia com água          |
| ----------- | ------- | ------- | ------------------------------- | -------------------------- |
| Tensão      | V       | Volt    | Diferença de potencial elétrico | Pressão da água            |
| Corrente    | I       | Ampère  | Fluxo de elétrons               | Vazão (litros por segundo) |
| Resistência | R       | Ohm     | Oposição à passagem da corrente | Cano estreito ou entupido  |

---


# 1. **Exemplo de Circuito em Série**

### Componentes:

* 1 fonte de 5V (pode ser o ESP32 ou uma fonte externa)
* 2 LEDs
* 2 resistores de 220Ω
* Fios e protoboard

###  Montagem:

* A corrente passa por **um LED**, depois pelo **outro LED**, **em sequência**.
* Os dois resistores também estão **em série com os LEDs**, cada um protegendo um LED.

```
(+5V) ──► [Resistor 220Ω] ──► [LED1] ──► [Resistor 220Ω] ──► [LED2] ──► GND
```

### Comportamento:

* A **mesma corrente** passa por todos os componentes.
* A **tensão se divide** entre os LEDs.
* Se um LED for removido ou queimar, **o circuito inteiro para de funcionar**.

---

## 2. **Exemplo de Circuito em Paralelo**

### Componentes:

* 1 fonte de 5V
* 2 LEDs
* 2 resistores de 220Ω
* Fios e protoboard

### Montagem:

* Os dois LEDs estão **conectados em paralelo**, **cada um com seu próprio resistor**.
* Ambos os caminhos estão ligados entre o mesmo ponto de **+5V e GND**.

```
(+5V)
   ├─► [Resistor 220Ω] ─► [LED1] ─► GND
   └─► [Resistor 220Ω] ─► [LED2] ─► GND
```

### Comportamento:

* A **tensão é igual** em ambos os ramos (cada LED recebe 5V).
* A **corrente se divide** entre os dois caminhos.
* Se um LED for removido ou queimar, **o outro continua funcionando normalmente**.

[Retornar](./GPIO.md)