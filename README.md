# README - Sistema de Monitoramento de Umidade para Salas de Comando
## Visão Geral
Este projeto foi desenvolvido para proteger a sala de comando de uma usina hidrelétrica contra danos causados por infiltração de água. A sala de comando é um componente essencial para o controle e operação da geração de energia, e sua segurança é crucial. O sistema utiliza um Arduino integrado a sensores de umidade, LEDs e um buzzer para detectar condições de risco, emitindo alertas e prevenindo danos aos equipamentos críticos.
## Funcionalidades
Monitora continuamente o nível de umidade na sala de comando.
Aciona um LED vermelho e um buzzer ao detectar níveis de umidade acima de 5%.
Envia um alerta para a central de comunicação.
O sistema só desativa o alerta quando a umidade retorna a níveis seguros (abaixo de 5%).
## Hardware Necessário
1x Arduino Uno 
1x Sensor de Umidade (DHT22 ou similar)
1x Buzzer
1x LED vermelho
1x Resistor (220Ω)
Jumpers e cabos de conexão
Protoboard
Fonte de alimentação para o Arduino
## Software Necessário
IDE do Arduino (Download Aqui)
Biblioteca DHT para leitura do sensor de umidade.

## Montagem do Circuito
1. Conecte o DHT22 ao Arduino:
 - VCC -> 5V do Arduino
 - GND -> GND do Arduino
 - Sinal -> Pino digital 5 (ou outro configurado no código)

2. Conecte o LED:
  - Anodo -> Pino digital 9 (resistor de 220Ω)
  - Catodo -> GND

3. Conecte o Buzzer:
  - Pino positivo -> Pino digital 10
  - Pino negativo -> GND

## Código do Projeto
Abaixo está o código-fonte básico para o funcionamento do sistema:

```cpp

Para avisar a central de comando quando o alarme de umidade for acionado, você pode implementar a comunicação com a central usando um dos seguintes métodos, dependendo da infraestrutura disponível na usina e das suas necessidades:

Comunicação Serial: Enviar uma mensagem simples via serial, que pode ser monitorada por um sistema na central.
Comunicação sem fio: Usar um módulo como ESP8266 ou ESP32 para enviar uma notificação por HTTP (como uma mensagem em um servidor ou API).
SMS/Email: Usar um módulo GSM (como o SIM800L) ou uma plataforma de email/SMS para enviar notificações.
Sistema de IoT: Usar uma plataforma como Blynk, ThingSpeak, ou MQTT para enviar notificações em tempo real para a central.
Vou apresentar um exemplo para cada um desses métodos.

1. Usando Comunicação Serial (Método simples)
Aqui, a central pode monitorar a saída serial do Arduino. Toda vez que a umidade ultrapassar 5%, o Arduino envia uma mensagem para a central.

Exemplo de Código com Comunicação Serial:
cpp
Copiar código
#include <DHT.h>

#define DHTPIN 5       // Pino onde o DHT22 está conectado
#define DHTTYPE DHT22  // Modelo do sensor de umidade
#define BUZZER 10      // Pino do buzzer
#define LED 9          // Pino do LED

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  pinMode(BUZZER, OUTPUT);
  pinMode(LED, OUTPUT);
  dht.begin();
  Serial.begin(9600);  // Inicializa a comunicação serial
}

void loop() {
  float umidade = dht.readHumidity();

  // Verifica se o sensor está funcionando corretamente
  if (isnan(umidade)) {
    Serial.println("Erro ao ler o sensor de umidade!");
    return;
  }

  Serial.print("Umidade: ");
  Serial.print(umidade);
  Serial.println("%");

  // Aciona alertas se a umidade estiver acima de 5%
  if (umidade > 5) {
    digitalWrite(LED, HIGH);
    digitalWrite(BUZZER, HIGH);
    // Envia um alerta para a central de comando via serial
    Serial.println("ALERTA: Umidade acima de 5%! A sala de comando pode estar em risco.");
  } else {  // Desativa alertas quando a umidade estiver abaixo de 5%
    digitalWrite(LED, LOW);
    digitalWrite(BUZZER, LOW);
  }

  delay(2000);  // Aguarda 2 segundos antes da próxima leitura
}
```
## Resultados Esperados
Quando a umidade atingir mais de 5%, o sistema:
- Acionará o LED vermelho.
- Emitirá um som contínuo no buzzer.
- Enviará alertas à central de comunicação.
- Quando a umidade retornar a menos de 5%, os alertas serão desativados automaticamente.

## Link do Simulador


