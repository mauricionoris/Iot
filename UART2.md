
## Comunicação Serial USART no ESP32

### O que é USART?

**USART** (Universal Synchronous/Asynchronous Receiver/Transmitter) é um periférico usado para comunicação serial. Ele pode operar de forma:

* **Assíncrona**: modo UART tradicional (mais comum no ESP32)
* **Síncrona**: menos utilizado no ESP32

No caso do ESP32, usamos geralmente a interface UART (modo assíncrono), que faz parte do USART.

---

### UART no ESP32

O ESP32 possui três interfaces UART:

* `UART0`: normalmente usada para comunicação com o computador via USB.
* `UART1` e `UART2`: disponíveis para uso geral com pinos configuráveis via software.

---

### Alternativas ao USART/UART

O ESP32 suporta vários métodos de comunicação, além do UART:

| Protocolo          | Tipo    | Vantagens                              | Aplicações comuns           |
| ------------------ | ------- | -------------------------------------- | --------------------------- |
| **I2C**            | Serial  | Comunicação com múltiplos dispositivos | Sensores, EEPROMs           |
| **SPI**            | Serial  | Alta velocidade, full duplex           | Memórias flash, displays    |
| **CAN**            | Serial  | Robusto em ambientes industriais       | Redes automotivas           |
| **Bluetooth**      | Sem fio | Sem necessidade de cabos               | Comunicação com smartphones |
| **Wi-Fi (TCP/IP)** | Sem fio | Comunicação via rede                   | IoT, servidores Web         |

---

### Exemplo de Comunicação Serial com UART (USART) no ESP32

Vamos usar a interface `UART1` com os seguintes pinos:

* **TX = GPIO17**
* **RX = GPIO16**

#### Conexões

* TX do ESP32 (GPIO17) → RX do outro dispositivo
* RX do ESP32 (GPIO16) → TX do outro dispositivo
* GND → GND

#### Código de exemplo (ESP-IDF)

```c
#include "driver/uart.h"
#include "esp_log.h"

#define UART_PORT_NUM      UART_NUM_1
#define UART_TX_PIN        17
#define UART_RX_PIN        16
#define UART_BAUD_RATE     115200
#define BUF_SIZE           1024

void app_main(void) {
    // Configuração UART
    uart_config_t uart_config = {
        .baud_rate = UART_BAUD_RATE,
        .data_bits = UART_DATA_8_BITS,
        .parity    = UART_PARITY_DISABLE,
        .stop_bits = UART_STOP_BITS_1,
        .flow_ctrl = UART_HW_FLOWCTRL_DISABLE
    };

    uart_param_config(UART_PORT_NUM, &uart_config);
    uart_set_pin(UART_PORT_NUM, UART_TX_PIN, UART_RX_PIN, UART_PIN_NO_CHANGE, UART_PIN_NO_CHANGE);
    uart_driver_install(UART_PORT_NUM, BUF_SIZE * 2, 0, 0, NULL, 0);

    const char* test_str = "Olá do ESP32 via UART!\r\n";
    uart_write_bytes(UART_PORT_NUM, test_str, strlen(test_str));

    uint8_t data[BUF_SIZE];
    while (1) {
        int len = uart_read_bytes(UART_PORT_NUM, data, BUF_SIZE, 100 / portTICK_PERIOD_MS);
        if (len > 0) {
            uart_write_bytes(UART_PORT_NUM, (const char *) data, len); // eco
        }
    }
}
```

---

### Dicas

* Se estiver usando dispositivos de 5V (como Arduino), verifique o nível lógico para evitar danificar o ESP32 (3.3V).
* É possível mapear os pinos UART livremente, exceto no UART0 (que normalmente está ligado ao USB).
* UART é excelente para debug, mas para transferências maiores ou sem fio, considere usar SPI ou Wi-Fi.

---

### Quando Usar UART2?

| Situação                                    | UART2 é recomendável?   |
| ------------------------------------------- | ---------------------------- |
| Comunicação simples com sensores seriais    | Sim                          |
| Comunicação com outro microcontrolador      | Sim                          |
| Transmissão de grandes volumes de dados     | Não (use SPI ou Wi-Fi)       |
| Comunicação sem fio com dispositivos móveis | Não (use Bluetooth ou Wi-Fi) |

# Para saber mais

- [Exemplo de comunicação UART0 para UART1](https://wokwi.com/projects/420603062355148801)
- [UART para computador Terminal](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/establish-serial-connection.html)
- [UART Communication with Other Devices](https://randomnerdtutorials.com/esp32-uart-communication-serial-arduino/)
