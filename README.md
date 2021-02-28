# cei-bot 💸

Bot para ler dados do Canal Eletrônico do Investidor, a principal funcionalidade desse projeto é obter dados do CEI - Canal Eletrônico do Investidor e tranformá-lo em informações que possam ser consumidos por outras aplicações.

## Dependências

O `cei-bot` utiliza a seguinte dependência:

- [puppeteer](https://github.com/puppeteer/puppeteer) API de alto nível para controlar o Chrome ou Chromium

## Instalação

Basta instalar utilizando o NPM:

```
npm i cei-bot
ou
yarn add cei-bot
```

## Utilização

1. **Crie uma instância do `CeiService` passando com os seguintes parametros:**

- `login`: Usuário de entrada no CEI que pode ser o CPF ou CNPJ;
- `password`: Senha de acesso ao CEI;
- `settings`: Configurações para controle da interação com a página do CEI;

### a - Typescript

```javascript
import { CeiService } from 'cei-bot';

const ceiService = new CeiService({
  login: 'username',
  password: 'password',
});

(async () => {
  const data = await ceiService.getIncomeAsync();
  console.log(JSON.stringify(data, null, 2));
})();
```

### b - Javascript

```javascript
const CeiService = require('cei-bot').CeiService;

const ceiService = new CeiService({
  login: 'username',
  password: 'password',
});

ceiService.getIncomeAsync().then(data => {
  console.log(JSON.stringify(data, null, 2));
});
```

### c - Settings

```javascript
import { CeiService } from 'cei-bot';

const ceiService = new CeiService(
  {
    login: 'username',
    password: 'password',
  },
  {
    headless: false,
    delay: 200,
    timeout: 400,
  },
);

(async () => {
  const data = await ceiService.getIncomeAsync();
  console.log(JSON.stringify(data, null, 2));
})();
```

2. **Obtenção dos Rendimentos**

Lembrando que o processo de obtenção e assíncrona.

a - Javascript

```javascript
const CeiService = require('cei-bot').CeiService;

const ceiService = new CeiService({
  login: 'username',
  password: 'password',
});

ceiService.getIncomeAsync().then(data => {
  console.log(JSON.stringify(data, null, 2));
});
```

b - TypeScript

```javascript
import { CeiService } from 'cei-bot';

const ceiService = new CeiService({
  login: 'username',
  password: 'password',
});

const data = await ceiService.getIncomeAsync();
console.log(JSON.stringify(data, null, 2));
```

3. **Obtenção carteira de ativos**

Lembrando que o processo de obtenção e assíncrona.

a - Javascript

```javascript
const CeiService = require('cei-bot').CeiService;

const ceiService = new CeiService({
  login: 'username',
  password: 'password',
});

ceiService.getPortfolioAsync().then(data => {
  console.log(JSON.stringify(data, null, 2));
});
```

b - TypeScript

```javascript
import { CeiService } from 'cei-bot';

const ceiService = new CeiService({
  login: 'username',
  password: 'password',
});

const data = await ceiService.getPortfolioAsync();
console.log(JSON.stringify(data, null, 2));
```

## Settings

| Propriedade  | Tipo      | Default | Descrição                                                                                                                                 |
| ------------ | --------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **headless** | _Boolean_ | _true_  | Se `true`, o navegador interno do Puppeteer será executado pode ser observado as acões.                                                   |
| **delay**    | _Number_  | 300     | Tempo, em ms, usado para adicionar valores nos inputs para evitar problemas de digitação muito rápida.                                    |
| **timeout**  | _Number_  | 300     | Tempo, em ms, de resposta de uma ação, para evitar obtenção de informações nulas ou vazias espera um tempo para a respsosta do navegador. |

### Métodos disponíveis

#### GetIncomeAsync()

Retorna os dados os rendiementos do mês no CEI.

```javascript
const resuls = await ceiService.getIncomeAsync();
```

Resultado:

```javascript
[
  {
    brokerName: '100 - Corretora 100',
    brokerCode: '100',
    status: 'S',
    errors: [],
    incomeList: [
      {
        active: 'Ação 01',
        specification: 'ON NM',
        code: 'ACAO01',
        paymentDate: '01/01/2020',
        eventType: 'DIVIDENDO',
        quantity: 100,
        quotationFactor: 1,
        grossValue: 50.06,
        netValue: 50,
      },
      {
        active: 'Ação 02',
        specification: 'ON NM',
        code: 'ACAO02',
        paymentDate: '05/01/2020',
        eventType: 'DIVIDENDO',
        quantity: 118,
        quotationFactor: 1,
        grossValue: 80.01,
        netValue: 80,
      },
      {
        active: 'Ação 03',
        specification: 'ON NM',
        code: 'ACAO03',
        paymentDate: '10/01/2020',
        eventType: 'JUROS SOBRE CAPITAL PRÓPRIO',
        quantity: 118,
        quotationFactor: 1,
        grossValue: 25.04,
        netValue: 21.13,
      },
      {
        active: 'Ação 04',
        specification: 'CI',
        code: 'ACAO04',
        paymentDate: '15/01/2020',
        eventType: 'RENDIMENTO',
        quantity: 29,
        quotationFactor: 1,
        grossValue: 34.87,
        netValue: 34.06,
      },
    ],
  },
  {
    brokerName: '200 - Corretora 200',
    brokerCode: '200',
    status: 'N',
    errors: [],
    incomeList: [],
  },
  {
    brokerName: '300 - Corretora 300',
    brokerCode: '300',
    status: 'N',
    errors: [],
    incomeList: [],
  },
];
```

- brokerName = Nome da corretora;

- brokerCode = Código da corretora;

- status = Situação do retorno que pode ser S para Sucesso, E paea Erro e N para resposta não encontrada. Quando S representa que obteve dados do CEI se N significa não existem informações para a corretora;

- errors = Lista de erros em formato de String;
- incomeList = Lista com todos os rendimentos encontrado no CEI;

## Income List

| Propriedade         | Tipo     | Descrição                                                                              |
| ------------------- | -------- | -------------------------------------------------------------------------------------- |
| **active**          | _String_ | Nome completo da ação.                                                                 |
| **specification**   | _String_ | Especificação da ação se é Ações ordinárias (ON) ou preferenciais (PN).                |
| **code**            | _String_ | Código de negociação do ativo.                                                         |
| **paymentDate**     | _String_ | Data de pagamento ou previsão de pagamento do ativo.                                   |
| **eventType**       | _String_ | Tipo de evento do ativo que pode ser: Dividendo, Juros sobre capital, Rendimento, etc. |
| **quantity**        | _Number_ | Quantidade do ativo na carteira.                                                       |
| **quotationFactor** | _Number_ | Fator da cotação.                                                                      |
| **grossValue**      | _Number_ | Valor Bruto (R$) do rendimento do ativo.                                               |
| **netValue**        | _Number_ | Valor líquido (R$) do rendimento do ativo.                                             |

#### GetPortfolioAsync()

Retorna os dados da carteira de ativos do mês no CEI.

```javascript
const resuls = await ceiService.getPortfolioAsync();
```

Resultado:

```javascript
[
  {
    status: 'S',
    errors: [],
    brokerName: "100 - Corretora 100'",
    brokerCode: '100',
    portfolioList: [
      {
        company: 'Empresa 01',
        specification: 'CI',
        code: 'ABC01',
        codISIN: 'BRBABC01',
        price: '110,10',
        quantity: '60',
        quotationFactor: 1,
        grossValue: '2.100,10',
      },
      {
        company: 'Empresa 02',
        specification: 'ON NM',
        code: 'CCC02',
        codISIN: 'BRECCC02',
        price: '40,60',
        quantity: '200',
        quotationFactor: 1,
        grossValue: '5.000,70',
      },
      {
        company: 'Empresa 03',
        specification: 'ON NM',
        code: 'CCC03',
        codISIN: 'BRECCC03',
        price: '60,60',
        quantity: '700',
        quotationFactor: 1,
        grossValue: '1.000,10',
      },
      {
        company: 'Empresa 04',
        specification: 'ON NM',
        code: 'CCC04',
        codISIN: 'BRECCC04',
        price: '15,60',
        quantity: '600',
        quotationFactor: 1,
        grossValue: '10.000,70',
      },
    ],
  },
  {
    status: 'N',
    errors: [],
    brokerName: 'Empresa 02',
    brokerCode: '200',
    portfolioList: [],
  },
  {
    status: 'N',
    errors: [],
    brokerName: 'Empresa 03',
    brokerCode: '300',
    portfolioList: [],
  },
];
```

- brokerName = Nome da corretora;

- brokerCode = Código da corretora;

- status = Situação do retorno que pode ser S para Sucesso, E paea Erro e N para resposta não encontrada. Quando S representa que obteve dados do CEI se N significa não existem informações para a corretora;

- errors = Lista de erros em formato de String;
- portfolioList = Lista com todos os ativos da sua carteira encontrado no CEI;

## Portfolio List

| Propriedade         | Tipo     | Descrição                                                               |
| ------------------- | -------- | ----------------------------------------------------------------------- |
| **company**         | _String_ | Nome da empresa .                                                       |
| **specification**   | _String_ | Especificação da ação se é Ações ordinárias (ON) ou preferenciais (PN). |
| **code**            | _String_ | Código de negociação do ativo.                                          |
| **codISIN**         | _String_ | International Securities Identification Number.                         |
| **price**           | _Number_ | Preço do ativo.                                                         |
| **quantity**        | _Number_ | Quantidade do ativo na carteira.                                        |
| **quotationFactor** | _Number_ | Fator da cotação.                                                       |
| **grossValue**      | _Number_ | Valor Bruto (R$) do rendimento do ativo.                                |

## Features

- [x] Rendiemntos
- [x] Carteira de ações

## Licença

MIT
