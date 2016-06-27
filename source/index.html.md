---
title: API Reference

language_tabs:
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Loja Integrada API V2 - Beta

API experimental utilizada para gerenciar e administrar os recursos disponíveis na Loja Integrada.

# Autenticação - integrando.se

```python
import json

jwt_example_token = '''eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.\
eyJzdG9yZSI6eyJwaG9uZSI6Iis1NSgxMSk5MTM4Mi0xMjkzIiwiZG9tYWluIjoiMjRob3Vycy5jb20iLCJ2ZXJpZmllZCI6dHJ1ZSwiaXNfY29tcGFueSI6IlBKIiwiYWRkcmVzcyI6eyJjb21wbGVtZW50IjoiRG8gbGFkbyBkbyBGQkkiLCJ6aXBjb2RlIjoiMDM1ODYtMDAwIiwibnVtYmVyIjoiNzU2In0sImRvY3VtZW50IjoiMzM1LjAxMC43NzgtODIiLCJpZCI6MTAxMjMsInRlbXBfZG9tYWluIjoidGVtcG9yYXJpby0yNGhvdXJzLmxvamFpbnRlZ3JhZGEuY29tLmJyIn0sInVzZXIiOnsibmFtZSI6IkphY2sgQmF1ZXIiLCJlbWFpbCI6ImphY2suYmF1ZXJAMjRob3Vycy5jb20ifSwiZXhwIjoxNDY3MDM0Nzk2fQ.\
OH8mfcYEk5TABUwzCq3Gj8FVhfc4KatPsHIAsX-eEzs'''
header, payload, verify = jwt_example_token.split('.')
# Necessário para conseguir decodificar a string em base64
payload += "=" * ((4 - len(data) % 4) % 4)
payload_dict = json.loads(payload.decode('base64'))
print payload_dict['store']['domain'] # '24 hours'
```

> Ao decodificar o token a seguinte estrutura em JSON é retornada

```json
{
  "store": {
    "phone": "+55(11)91382-1293",
    "domain": "24hours.com",
    "verified": true,
    "is_company": "PJ",
    "address": {
      "complement": "Do lado do FBI",
      "zipcode": "03586-000",
      "number": "756"
    },
    "document": "335.010.778-82",
    "id": 10123,
    "temp_domain": "temporario-24hours.lojaintegrada.com.br"
  },
  "user": {
    "name": "Jack Bauer",
    "email": "jack.bauer@24hours.com"
  },
  "exp": 1467034796
}
```

O processo de autenticação consiste em obter um token de autenticação válido por 6 horas, para obtê-lo o usuário precisa ser redirecionado para o painel da Loja Integrada, após a validação das credenciais o usuário será encaminhado para um endereço pré configurado. O token pode ser obtido através do cookies. Abaixo segue uma imagem exemplificando o fluxo de autenticação:

![](images/auth-flow-integrandose.png)

O fluxo pode ser descrito nos seguintes passos:

- Redirecionar o usuário para `https://app.lojaintegrada.com.br?integrandose=true`;
- O usuário fornece as credenciais no painel da Loja Integrada e as confirma;
- Um novo token é gerado e a página é redirecionada para um endereço pré configurado;
- A partir desse ponto é possível obter os `AUTH_TOKEN` a partir dos cookies.



# Temas

## Instala/Ativa novo tema

```python
import requests, json, base64

api_url = 'https://api.awsli.com.br/v2/themes'
auth_token = 'MY_AUTH_TOKEN'
headers = {
  'Content-Type': 'application/json',
  'Authorization: Bearer %s' % auth_token
}
data_params = {
  'name': 'clean-store-black-edition',
  'powered_by': 'Integrandose',
  'temp_domain_base': 'temporario-24hours.lojaintegrada.com.br',
  'bundle': None
}

# Encoda arquivo .zip em base64
with open('clean-store-black-edition.zip', 'rb') as f:
  data_params['bundle'] = base64.b64encode(f.read())

# Instala e ativa o novo tema
response = requests.post(api_url, data=json.dumps(data_params), headers=headers)
if response.status_code == 201:
  print 'Tema criado e ativado com sucesso'

print json.dumps(response.json(), indent=2)
```

> A resposta irá retornar a seguinte estrutura em JSON

```json
{
  "response": {
      "theme_id": 42
  }
}
```

Esse endpoint instala e ativa um novo tema para o lojista

### HTTP Request

`POST http://api.awsli.com.br/v2/themes/`

### Data Params

Parâmetro | Tipo | Requerido | Descrição
--------- | ---- | --------- | ---------
name | string(unique) | Sim | O nome do tema
powered_by | string | Não | O autor do tema
temp_domain_base | string | Sim | O endereço da loja onde o tema base esta instalado
bundle | base64 | Sim | O arquivo zip contendo a estrutura dos temas

O nome do tema deve ser único, caso já exista um tema com esse nome um erro irá ocorrer informando a inconsistência.

### Status Code Responses

Código | Descrição
------ | ---------
201 | Created -- Tema foi criado e ativado com sucesso
400 | Bad Request -- Ocorreu um erro ao instalar o tema
409 | Conflict -- O tema já existe

## Atualiza tema

Esse endpoint atualiza os atributos do tema especificado

```python
import requests
import json

api_url = 'https://api.awsli.com.br/v2/themes/42'
auth_token = 'JWT_AUTH_TOKEN'
headers = {
  'Content-Type': 'application/json',
  'Authorization: Bearer %s' % auth_token
}
data_params = {}

# Encoda arquivo .zip em base64
with open('clean-store-black-edition-v2.zip', 'rb') as f:
  dadata_params['bundle'] = base64.b64encode(f.read())

# Atualiza o bundle do tema
response = requests.put(api_url, data=json.dumps(data_params), headers=headers)
if response.status_code == 204:
  print 'Tema atualizado com sucesso'

# Atualiza o autor do tema
response = requests.put(api_ul, data=json.dumps({'powered_by' : 'Integrandose-v2'}))
if response.status_code == 204:
  print 'Tema atualizado com sucesso'
```

### HTTP Request

`PUT http://api.awsli.com.br/v2/themes/:ID`

### Query Params - Store

Parâmetro | Tipo | Requerido | Descrição
--------- | ---- | --------- | ---------
ID | integer | Sim | O ID da loja

### Data Params

Parâmetro | Tipo | Requerido | Descrição
--------- | ---- | --------- | ---------
powered_by | string | Não | O autor do tema
bundle | base64 | Sim | O arquivo zip contendo a estrutura dos temas

### Status Code Responses

Código | Descrição
------ | ---------
204 | No Content -- Tema atualizado com sucesso
400 | Bad Request -- Erro ao atualizar tema
404 | Not Found -- Tema não encontrado/instalado

## Ativa tema

```python
import requests, json

api_url = 'https://api.awsli.com.br/v2/themes/42/state?activate=true'
auth_token = 'JWT_AUTH_TOKEN'
headers = {
  'Content-Type': 'application/json',
  'Authorization: Bearer %s' % auth_token
}

# Ativa o tema na loja
response = requests.put(api_url, data={}, headers=headers)
if response.status_code == 204:
  print 'Tema atualizado com sucesso'
```

Ativa o tema na loja

### HTTP Request

`PUT http://api.awsli.com.br/v2/themes/:ID/state?activate=true`

### Query Params

Parâmetro | Tipo | Requerido | Descrição
--------- | ---- | --------- | ---------
ID | integer | Sim | ID da loja
activate | boolean | Sim | Ativa o tema na loja

### Status Code Responses

Código | Descrição
------ | ---------
204 | No Content -- Tema ativado com sucesso
400 | Bad Request -- Erro ao ativar tema
404 | Not Found -- Tema não encontrado/instalado

<aside class="notice">O ID da loja é obtido através do payload `AUTH_TOKEN`.</aside>
