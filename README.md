# Desafio Observabilidade e Open Telemetry

Este repositório se trata do desafio *Observabilidade e Open Telemetry**, da Pós Graduação **Go Expert / FullCycle**.

## Escopo do Desafio

Objetivo: Desenvolver um sistema em Go que receba um CEP, identifica a cidade e retorna o clima atual (temperatura em graus celsius, fahrenheit e kelvin) juntamente com a cidade. Esse sistema deverá implementar OTEL(Open Telemetry) e Zipkin.

Basedo no cenário conhecido "Sistema de temperatura por CEP" denominado Serviço B, será incluso um novo projeto, denominado Serviço A.

 

Requisitos - Serviço A (responsável pelo input):

1. O sistema deve receber um input de 8 dígitos via POST, através do schema:  { "cep": "29902555" }
2. O sistema deve validar se o input é valido (contem 8 dígitos) e é uma STRING
2.1. Caso seja válido, será encaminhado para o Serviço B via HTTP
2.2. Caso não seja válido, deve retornar:
2.2.1. Código HTTP: 422
2.2.2. Mensagem: invalid zipcode

Requisitos - Serviço B (responsável pela orquestração):

1. O sistema deve receber um CEP válido de 8 digitos
2. O sistema deve realizar a pesquisa do CEP e encontrar o nome da localização, a partir disso, deverá retornar as temperaturas e formata-lás em: Celsius, Fahrenheit, Kelvin juntamente com o nome da localização.
3. O sistema deve responder adequadamente nos seguintes cenários:
3.1. Em caso de sucesso:
3.1.1. Código HTTP: 200
3.1.2. Response Body: { "city: "São Paulo", "temp_C": 28.5, "temp_F": 28.5, "temp_K": 28.5 }
3.2. Em caso de falha, caso o CEP não seja válido (com formato correto):
3.2.1. Código HTTP: 422
3.2.2. Mensagem: invalid zipcode
​​​3.3. Em caso de falha, caso o CEP não seja encontrado:
3.3.1. Código HTTP: 404
3.3.2. Mensagem: can not find zipcode

Após a implementação dos serviços, adicione a implementação do OTEL + Zipkin:

- Implementar tracing distribuído entre Serviço A - Serviço B
- Utilizar span para medir o tempo de resposta do serviço de busca de CEP e busca de temperatura

Dicas:

- Utilize a API viaCEP (ou similar) para encontrar a localização que deseja consultar a temperatura: https://viacep.com.br/
- Utilize a API WeatherAPI (ou similar) para consultar as temperaturas desejadas: https://www.weatherapi.com/
- Para realizar a conversão de Celsius para Fahrenheit, utilize a seguinte fórmula: F = C * 1,8 + 32
- Para realizar a conversão de Celsius para Kelvin, utilize a seguinte fórmula: K = C + 273
Sendo F = Fahrenheit
Sendo C = Celsius
Sendo K = Kelvin
- Para dúvidas da implementação do OTEL, você pode clicar aqui
- Para implementação de spans, você pode clicar aqui
- Você precisará utilizar um serviço de collector do OTEL
- Para mais informações sobre Zipkin, você pode clicar aqui

Entrega:

- O código-fonte completo da implementação.
- Documentação explicando como rodar o projeto em ambiente dev.
- Utilize docker/docker-compose para que possamos realizar os testes de sua aplicação.

## Passos para executar o projeto

1. Construa e execute os contêineres:
`docker-compose up --build`

2. Teste o serviço:
Envie uma requisição POST para `http://localhost:8080/cep` com o seguinte corpo:
`{ "cep": "29902555" }`

3. Acesse o Zipkin:
Abra o navegador e acesse http://localhost:9411 para visualizar os traces gerados pelos serviços.
