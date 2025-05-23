# Lei de Ohm

### Fórmula:

$$
V = I \times R
$$

Onde:

* **V** = Tensão (Volts, **V**)
* **I** = Corrente elétrica (Ampères, **A**)
* **R** = Resistência elétrica (Ohms, **Ω**)

---

### O que ela significa?

A **Lei de Ohm** descreve a **relação entre tensão, corrente e resistência** em um circuito elétrico.

Ela afirma que:

> A **tensão (V)** em um circuito é igual à **corrente (I)** que circula por ele **multiplicada pela resistência (R)** do circuito.

---

### Interpretação prática

* Se você **aumenta a tensão** e a resistência permanece constante, a corrente **aumenta**.
* Se você **aumenta a resistência** e a tensão permanece constante, a corrente **diminui**.

---

### Analogia com água:

Pense em:

* **Tensão** como a **pressão da água** em um cano.
* **Corrente** como o **fluxo de água**.
* **Resistência** como a **largura do cano** (ou um obstáculo que restringe o fluxo).

Quanto mais estreito o cano (maior a resistência), menos água passa (menor corrente), mesmo que a pressão seja a mesma.

---

### Exemplo 1: Calculando a corrente

Você liga um resistor de **220Ω** em uma fonte de **5V**:

$$
I = \frac{V}{R} = \frac{5}{220} \approx 0{,}0227 \text{ A} = 22{,}7 \text{ mA}
$$

Esse é o valor típico para acender um LED com segurança.

---

### Exemplo 2: Calculando o resistor ideal para um LED

* Fonte: 3.3V (do ESP32)
* LED consome 2V e 20mA
* A queda de tensão no resistor será:
  $3.3V - 2V = 1.3V$

$$
R = \frac{V}{I} = \frac{1.3}{0.020} = 65 \text{ Ω}
$$

Você usaria o valor comercial mais próximo, como **68Ω ou 100Ω**, para proteger o LED.

---

### Conclusão:

A **Lei de Ohm** é essencial para:

* Calcular o valor de resistores.
* Projetar circuitos seguros.
* Entender o comportamento dos componentes elétricos.




[Retornar](./GPIO.md)