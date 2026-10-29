# API de Gerenciamento de Pedidos

Projeto avaliativo construído seguindo o estilo do código de referência `API-AULA`: rotas, controllers e services separados, `console.log` para acompanhar o fluxo da requisição e funções exportadas com `module.exports`.

## Instalação

Requer Node.js 18 ou superior.

```bash
npm install
npm start
```

O servidor inicia na porta 3000. Para executar os testes:

```bash
npm test
```

## Estrutura

- `index.js`: configura Express e inicia o servidor.
- `routes/pedidoRoutes.js`: define os endpoints.
- `controllers/pedidoController.js`: recebe a requisição, chama o service e envia a resposta.
- `services/pedidoService.js`: aplica regras de negócio e mantém os pedidos em memória.
- `validators/pedidoValidator.js`: valida os dados e as transições de status.
- `middlewares/errorHandler.js`: padroniza erros.
- `utils/AppError.js`: representa erros esperados da aplicação.
- `tests/pedidos.test.js`: testes automatizados com Jest e Supertest.

## Endpoints

| Método | Rota | Resposta esperada |
|---|---|---|
| POST | `/pedidos` | 201 ao criar |
| GET | `/pedidos` | 200 com lista |
| GET | `/pedidos/:id` | 200 se existir; 404 se não existir |
| PATCH | `/pedidos/:id/status` | 200 ao atualizar; 400 para dados/transição inválidos; 404 se não existir |

### Exemplo de cadastro

`POST http://localhost:3000/pedidos`

Body > raw > JSON:

```json
{
  "cliente": "Carlos Silva",
  "tipo": "entrega",
  "valor": 350,
  "prioridade": "normal"
}
```

O retorno esperado é HTTP 201:

```json
{
  "id": 1,
  "cliente": "Carlos Silva",
  "tipo": "entrega",
  "valor": 350,
  "prioridade": "normal",
  "status": "recebido"
}
```

### Exemplo de atualização

`PATCH http://localhost:3000/pedidos/1/status`

```json
{ "status": "em_preparacao" }
```

## Valores e regras

- Tipo: `entrega` ou `retirada`.
- Prioridade: `baixa`, `normal` ou `alta`.
- Status: `recebido`, `em_preparacao`, `enviado`, `entregue` ou `cancelado`.
- Transições permitidas: `recebido` → `em_preparacao` ou `cancelado`; `em_preparacao` → `enviado` ou `cancelado`; `enviado` → `entregue` ou `cancelado`.
- `entregue` e `cancelado` são estados finais.

O armazenamento é em memória, permitido pelo enunciado. Os pedidos são apagados quando o servidor reinicia.

## Evidências Postman

As capturas dos testes realizados no Postman estão salvas na pasta `evidencias/`.

1. `01_Criacao.png` — criação de pedido válido (POST, HTTP 201).
2. `02_Validacao.png` — validação de dados inválidos (POST, HTTP 400).
3. `03_Listagem.png` — listagem de pedidos (GET, HTTP 200).
4. `04_Consulta_Inexistente.png` — consulta de pedido inexistente (GET, HTTP 404).
5. `05_Atualizacao.png` — atualização de status válida (PATCH, HTTP 200).

Para acompanhar o caminho dos dados, deixe o terminal visível: o controller registra a entrada, o service registra os dados recebidos e o pedido criado, seguindo a ideia dos logs do projeto de referência.

## GitHub e commits

Faça os commits conforme for desenvolvendo e testando; não crie histórico fictício. Sugestões:

1. `chore: configurar projeto Express`
2. `feat: criar rotas e controller de pedidos`
3. `feat: implementar regras no service`
4. `feat: adicionar validacoes e tratamento de erros`
5. `test: adicionar testes automatizados`
6. `docs: documentar endpoints e evidencias`

Depois de criar um repositório vazio no GitHub:

```bash
git init
git add .
git commit -m "chore: configurar projeto Express"
git branch -M main
git remote add origin URL_DO_SEU_REPOSITORIO
git push -u origin main
```

Não envie `node_modules`, tokens, senhas ou arquivos `.env` com credenciais.

## Uso de IA

A IA foi utilizada como apoio para organizar os arquivos, elaborar a lógica inicial dos endpoints, sugerir validações e preparar testes e documentação. Uma sugestão adotada foi acompanhar o fluxo com mensagens `console.log` no controller e no service, seguindo o padrão do código de referência; também foi adotado um middleware central para padronizar erros. O aluno deve executar, testar e compreender o código antes de entregá-lo.

## Verificação dos testes

A API foi testada utilizando o Postman para verificar
a criação, listagem, consulta e atualização de pedidos,
incluindo o tratamento de dados inválidos e pedidos inexistentes.