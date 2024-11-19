![image](https://github.com/user-attachments/assets/ccc22468-2b5c-4664-85c0-b16f0082d429)# Mackenzie - Objetos Inteligentes Conectados - Sensor de estoque 

- O objetivo do projeto é o desenvolvimento de um protótipo em um controlador cm a IDE Arduino 2.3.3, que deve automatizar o controle de estoque, assegurando que todos os itens estejam disponíveis.

- Quando um item está em falta, o sensor calcula a distância até o produto e devido a variável proposta para distância mínima até o produto, é possível reconhecer se o produto existe ou não, de forma que o controlador junto a conexão MQTT envia uma mensagem para o dispositivo móvel do individual, garantindo que a informação chegue rapidamente aos responsáveis pela reposição.
  
**Os materiais e métodos escolhidos:

•	Controlador: O modelo ESP32 é altamente recomendado, pois já possui 
conectividade Wi-Fi integrada.

•	Sensor: O sensor de distância será o HCSR04 para detectar padrões e movimentações de retirada de itens.

•	Broker MQTT: Um servidor MQTT para comunicação, que pode ser configurado localmente ou usar um serviço online. Nesse caso será utilizado o Broker EMQX.

•	Biblioteca PubSubClient: Para a comunicação via MQTT no controlador, Biblioteca Wifi para conexão com a rede, Biblioteca do ESP32.

•	Led Branco para identificar a mudança e gerar um alerta visual quando é necessário repor o estoque.
Para o projeto, será utilizado o app MQTT Dashboard, que é uma ferramenta para testar e monitorar dispositivos IoT que utilizam do protocolo. Dessa forma, ele possibilita a visualização e publicação dos dados enviados e interage com o modelo do controlador escolhido (ESP32) diretamente do celular.
Para a programação, usaremos a IDE 2.3.3 do controlador juntamente com a linguagem de programação C++. Dessa forma, acessando o site oficial do mesmo, fazendo a
instalação e configurando a ESP32 na IDE 2.3.3, podemos iniciar o processo de programação do prototipo.


**Construção:

O sensor utilizado foi o HCSR04 que é conectado ao microcontrolador ESP32, que irá processar os dados do estoque. Os cabos de alimentação do sensor são conectados às entradas de 5V e GND do ESP32, enquanto o sinal de saída do sensor é conectado ao pino digital 14 e 25 (Trigger e Echo). Este sistema é alimentado por uma fonte de 5V, adequada para a operação dos componentes eletrônicos envolvidos. Também há a possibilidade de alimentar todo o circuito através de um cabo USB direto de um computador, já que a ESP32 possui entrada para tal. Também foi inserido o led e as conexões com o jumper.

![WhatsApp Image 2024-11-18 at 7 42 08 PM](https://github.com/user-attachments/assets/b15e495c-731d-48e3-a4ca-2828edd62ab9)


É possível fazer o download da IDE diretamente no site (https://www.arduino.cc/en/software) e a partir deste, devemos fazer algumas configurações:
O primeiro passo é quando já montado o prototipo, conectar o USB do controlador ao computador, se necessário, deverá ser baixado uma atualização de driver para compatibilidade com a porta USB.
Dentro da IDE, será necessário definir qual a board está usando e qual a porta para conexão. Além, também será necessário baixar as bibliotecas do ESP32 e do MQTT.

 
Depois de configurado tais cenários, será preciso criar a conexão com o protocolo MQTT via Broker, e também configurar o Widget (Mensagem de Texto) que será recebido.
Para tal, é necessário baixar e instalar o app do MQTT Dashboard Client (Android)
 
Figura 1 – Tela de início do App. Fonte: Imagem própria.
Logo, será necessário inserir os dados do broker, como:

![Imagem3](https://github.com/user-attachments/assets/923f9689-e8da-4a51-a5b3-97139c4ee886)

 
Figura 2 – Tela de configuração. Fonte: Imagem própria.

![Imagem4](https://github.com/user-attachments/assets/8076b56d-21cc-43c3-8719-f88f85a7b537)

•	Nome do servidor.
•	Id do client (já configurado para o dispositivo).
•	URL (que será o endereço ip ou URL do broker MQTT).
•	Porta 1883 (já configurada para o dispositivo).

Após a configuração, será necessário adicionar os tópicos para monitoramento e envio dos dados coletados, como por exemplo a notificação de movimentação de estoque. 
É possivel também adicionar widgets no app para melhor visualização, sendo possivel os seguintes formatos:

 ![Imagem5](https://github.com/user-attachments/assets/b0e77c63-ca2d-4828-8868-1b596e1b1340)

•	Text View: Para visualizar mensagens publicadas em um tópico específico (por exemplo, atualizações sobre o status do estoque).

Feito todos esses processos, será necessário fazer a integração com o controlador por via de código para que ele receba corretamente as mensagens quando o sensor HCSR04 detectar a alteração.

Primeiramente devem ser incluidos as bibliotecas de Wifi e PubSubClient, e configurado a rede wifi e a senha. Deve ser feito a configuração com o servidor MQTT público utilizado 	(broker.emqx.io) e também o nome do tópico (configurado como widget no aplicativo), onde as mensagens serão publicadas.
A configuração dos pinos dos sensores dependem da conexão feita no controlador/sensor, podendo ser variável. Neste caso foi utilizado os pinos 14 e 25.
Há então a tentative de conexão via rede Wifi e também do servidor MQTT, que se bem sucedido alerta a mensagem de "Conectado ao Wi-Fi!" e “Conectado ao broker MQTT!”. Caso dê qualquer tipo de falha, será indicado o tipo de erro também e será feito a tentativa de reconexão.
Já com as redes configuradas, então o sensor envia o pulso de emissão e echo e calcula a distância mensurada através do tempo que a informação do pulso é emitida e recebida. Com tal informação, podemos atribuir a variável de distância em cm.
Personalizando o código, para o projeto, foi utilizado uma distância de 7.5 centímetros para detectar se o produto se encontra no estoque ou não. Caso a medição dê 	maior ou igual a 7.5 centímetros, o retorno será em uma string de "Produto não encontrado. Repor estoque", caso seja menor que 7.5, retornará "Produto disponível.".
Após isso, a mensagem deve ser enviada no tópico MQTT do Broker criado (via widget de texto). O resultado é a mensagem via aplicativo para informar o indivíduo.

![Imagem2](https://github.com/user-attachments/assets/f5043937-0e24-465e-86c1-be19169d0f44)


**Resultados e fotos:

![image](https://github.com/user-attachments/assets/c6b0a5ee-ae52-4756-864c-25215cb5e177)
![image](https://github.com/user-attachments/assets/cf440b28-cbe3-45bb-8b92-d9fbb868cf1d)
![image](https://github.com/user-attachments/assets/5c84e996-c701-4291-ac92-94d3a0487cd8)
![image](https://github.com/user-attachments/assets/2e60bbda-525a-490b-b7ac-66f02be6a441)


#Apresentação Prática: 

Link do Youtube: 




