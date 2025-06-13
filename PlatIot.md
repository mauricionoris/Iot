## **Atividade: Enviando dados simulados de sensor para uma plataforma de IoT**

### **Objetivo**

Criar uma conta em uma plataforma de Internet das Coisas (IoT), simular o envio de dados de um sensor (como temperatura, umidade ou luminosidade), e visualizar essas informações por meio de gráficos em tempo real.

---

### **Instruções Gerais**

1. Escolha **uma** das plataformas sugeridas abaixo.
2. Crie uma conta gratuita na plataforma.
3. Configure um projeto ou dispositivo virtual.
4. Simule o envio de dados de um sensor.
5. Crie e visualize um gráfico com os dados recebidos.
6. Capture uma imagem da visualização final (ou compartilhe o link, se a plataforma permitir).

---

### **Plataformas sugeridas**

#### **1. ThingSpeak (da MathWorks)**

**Passo a passo:**

1. Acesse [https://thingspeak.com](https://thingspeak.com)
2. Clique em "Sign Up" e crie uma conta gratuita (ou use uma conta Google).
3. Após o login, vá em "Channels" e clique em "New Channel".
4. Dê um nome ao seu canal e ative os campos que desejar (Field1, Field2...).
5. Clique em "Save Channel".
6. Na aba "API Keys", anote a **Write API Key**.
7. Para simular dados, use o seguinte link no navegador, substituindo a sua chave:

```
https://api.thingspeak.com/update?api_key=SUA_CHAVE&field1=25
```

8. Acesse a aba "Private View" ou "Public View" e veja o gráfico gerado com os dados simulados.

---

#### **2. Ubidots**

**Passo a passo:**

1. Acesse [https://ubidots.com](https://ubidots.com)
2. Clique em "Start for free" e registre-se com uma conta gratuita.
3. Após o login, vá até o menu "Devices" > "Devices" > "Add Device".
4. Escolha "Blank Device", dê um nome e clique em "Create".
5. Dentro do dispositivo, clique em "Add Variable" e escolha o tipo (por exemplo, temperatura).
6. Para simular dados, acesse a aba "API Credentials" e copie o **Token**.
7. Use o seguinte comando em seu navegador ou com uma ferramenta como Postman:

```
POST https://industrial.api.ubidots.com/api/v1.6/devices/NOME_DO_DISPOSITIVO/
Headers: X-Auth-Token: SEU_TOKEN
Body (JSON): {"temperatura": 25}
```

8. Vá até "Data" > "Dashboards", crie um novo painel e adicione um gráfico com os dados.

---

#### **3. TagoIO**

**Passo a passo:**

1. Acesse [https://tago.io](https://tago.io)
2. Clique em "Create free account" e preencha os dados.
3. No painel, vá em "Devices" > "Add Device" > escolha "Blank Device".
4. Dê um nome ao dispositivo e clique em "Create Device".
5. Clique no dispositivo criado e vá em "Token" > "Generate Token" e copie o token.
6. Para simular dados, envie uma requisição POST (usando Postman ou cURL):

```
POST https://api.tago.io/data
Headers: Device-Token: SEU_TOKEN
Body (JSON): 
[
  {
    "variable": "temperatura",
    "value": 26
  }
]
```

7. Vá em "Dashboards" > "New Dashboard", e adicione um widget de gráfico para visualizar os dados.

---

### **Entrega da Atividade**

* Capture uma imagem do gráfico gerado com os dados simulados.
* Informe qual plataforma utilizou.
* Opcional: compartilhe o link público do painel, se disponível.
* Envie a imagem e as informações via classroom utilizada pela turma.

