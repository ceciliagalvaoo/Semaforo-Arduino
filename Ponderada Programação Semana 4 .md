# Relato de Montagem

&nbsp;&nbsp;&nbsp;&nbsp;Este projeto de controle de semáforo foi construído com um Arduino, três LEDs (vermelho, amarelo e verde) que representam as fases de sinalização, um botão e resistores de 330 ohms para proteger os LEDs. Os LEDs foram conectados aos pinos digitais 8, 10 e 13, enquanto o botão foi ligado ao pino 2 e configurado com um resistor pull-up interno, simplificando o circuito e garantindo uma leitura correta do estado do botão.

&nbsp;&nbsp;&nbsp;&nbsp;A lógica de funcionamento permite que, ao pressionar o botão, o semáforo seja ligado e inicie seu ciclo de operação: acende o LED vermelho por 6 segundos, depois o amarelo por 2 segundos e, em seguida, o verde, simulando as condições de um semáforo real. O ciclo inclui uma fase adicional para pedestres durante o verde, garantindo uma sequência precisa. Além disso, caso o botão seja pressionado novamente enquanto o semáforo está ligado, o sistema entra no modo de alerta, no qual o LED amarelo pisca continuamente para sinalizar atenção.

&nbsp;&nbsp;&nbsp;&nbsp;Logo, este projeto foi implementado e testado no Arduino IDE, com o código estruturado para alternar automaticamente entre os modos de operação e alerta, conforme o botão é pressionado. A simplicidade e a funcionalidade do design garantem que o semáforo simulado seja fácil de controlar e altamente realista, além de ser seguro para o hardware, devido ao uso apropriado de resistores.

# Código

```cpp
// Código C++ para controle de semáforo com botão para ligar e ativar modo alerta (LED amarelo piscando)

const int pinVermelho = 8;     // Pino para o LED vermelho
const int pinAmarelo = 10;     // Pino para o LED amarelo
const int pinVerde = 13;       // Pino para o LED verde
const int botaoPin = 2;        // Pino para o botão de liga/alerta
bool semaforoLigado = false;   // Estado inicial do semáforo desligado
bool modoAlerta = false;       // Estado inicial do modo alerta desligado

void setup() {
  pinMode(pinVermelho, OUTPUT); 
  pinMode(pinAmarelo, OUTPUT);  
  pinMode(pinVerde, OUTPUT);    
  pinMode(botaoPin, INPUT_PULLUP); // Configura o pino do botão com resistor pull-up interno
}

void loop() {
  // Verifica se o botão foi pressionado
  if (digitalRead(botaoPin) == LOW) {
    delay(200); // Pausa para debounce
    
    if (!semaforoLigado) {
      // Liga o semáforo se ele estiver desligado
      semaforoLigado = true;
    } else {
      // Alterna o modo alerta se o semáforo já estiver ligado
      modoAlerta = !modoAlerta;
    }

    // Aguarda até o botão ser solto antes de continuar
    while (digitalRead(botaoPin) == LOW);
  }

  if (semaforoLigado) {
    if (modoAlerta) {
      // Modo Alerta: Pisca o LED amarelo continuamente
      digitalWrite(pinVermelho, LOW);
      digitalWrite(pinVerde, LOW);
      digitalWrite(pinAmarelo, HIGH);
      delay(500);
      digitalWrite(pinAmarelo, LOW);
      delay(500);
    } else {
      // Ciclo de semáforo normal
      // Fase Vermelho (6 segundos)
      digitalWrite(pinVermelho, HIGH);  // Acende o vermelho
      digitalWrite(pinAmarelo, LOW);    // Apaga o amarelo
      digitalWrite(pinVerde, LOW);      // Apaga o verde
      delay(6000);                      // Espera 6 segundos

      // Fase Amarelo (2 segundos)
      digitalWrite(pinVermelho, LOW);   // Apaga o vermelho
      digitalWrite(pinAmarelo, HIGH);   // Acende o amarelo
      digitalWrite(pinVerde, LOW);      // Apaga o verde
      delay(2000);                      // Espera 2 segundos

      // Fase Verde (2 segundos)
      digitalWrite(pinVermelho, LOW);   // Apaga o vermelho
      digitalWrite(pinAmarelo, LOW);    // Apaga o amarelo
      digitalWrite(pinVerde, HIGH);     // Acende o verde
      delay(2000);                      // Espera 2 segundos

      // Tempo adicional para pedestres (2 segundos)
      delay(2000);                      // Mantém o verde aceso por mais 2 segundos

      // Fase Amarelo (2 segundos)
      digitalWrite(pinVermelho, LOW);   // Apaga o vermelho
      digitalWrite(pinAmarelo, HIGH);   // Acende o amarelo
      digitalWrite(pinVerde, LOW);      // Apaga o verde
      delay(2000);                      // Espera 2 segundos
    }
  } else {
    // Semáforo desligado
    digitalWrite(pinVermelho, LOW);
    digitalWrite(pinAmarelo, LOW);
    digitalWrite(pinVerde, LOW);
  }
}

```

# Tabela de Componentes

| Componente           | Especificação                           | Quantidade | Função no Projeto                                           |
|----------------------|-----------------------------------------|------------|-------------------------------------------------------------|
| Arduino              | Microcontrolador compatível com IDE Arduino | 1          | Controla o ciclo de sinalização e o modo de alerta          |
| LED Vermelho         | LED de 5 mm, cor vermelha               | 1          | Representa a fase de parada do semáforo                     |
| LED Amarelo          | LED de 5 mm, cor amarela               | 1          | Indica a fase de atenção e o modo de alerta                 |
| LED Verde            | LED de 5 mm, cor verde                 | 1          | Indica a fase de trânsito livre no semáforo                 |
| Resistores           | 330 ohms                               | 3          | Limita a corrente para proteger os LEDs                     |
| Botão                | Push button                            | 1          | Controla o liga/desliga e ativa o modo alerta               |
| Jumpers              | Cabos de conexão                       | 11         | Conecta os componentes ao Arduino                           |

# Avaliação

### Avaliador: Nataly de Souza Cunha

| Critério                                                                                                 | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador                             |
|---------------------------------------------------------------------------------------------------------|--------------------|----------------------------------|--------------------------|-------------------------------------------------------|
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                | Até 3              | Até 1,5                          | 0                        | Excelente montagem física, cores e componentes bem organizados.   |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                  | Até 3              | Até 1,5                          | 0                        | Temporização precisa e bem executada.               |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) | Até 3              | Até 1,5                          | 0                        | Código estruturado com clareza e bons comentários. |
| Extra: Implementou um componente de liga/desliga no semáforo e/ou usou ponteiros no código              | Até 1              | Até 0,5                          | 0                        | Componente bem implementado.       |
|                                                                                                         |              |                                  |                          | **Pontuação Total: 10**                                 |

### Avaliador: Mariella Sayumi Mercado Kamezawa

| Critério                                                                                                 | Contempla (Pontos) | Contempla Parcialmente (Pontos) | Não Contempla (Pontos) | Observações do Avaliador                             |
|---------------------------------------------------------------------------------------------------------|--------------------|----------------------------------|--------------------------|-------------------------------------------------------|
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                | Até 3              | Até 1,5                          | 0                        | Montagem bem organizada, com uso apropriado das cores e resistores.   |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                  | Até 3              | Até 1,5                          | 0                        | Temporização estável e de acordo com os parâmetros exigidos.               |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) | Até 3              | Até 1,5                          | 0                        | Código bem estruturado, com boas práticas e comentários claros. |
| Extra: Implementou um componente de liga/desliga no semáforo e/ou usou ponteiros no código              | Até 1              | Até 0,5                          | 0                        | Componente útil agregando valor ao projeto.       |
|                                                                                                         |             |                                  |                          | **Pontuação Total: 10**                                 |

