# Erros Gerais

> As respostas de erro tem a seguinte estrutura em JSON

```json
{
  "response": {
    "message": "Error message"
  }
}
```

Código | Descrição
------ | ---------
401 | Unauthorized -- Não foi possível validar as credenciais
405 | Method Not Allowed -- Método não implementado
429 | Too Many Requests -- Rate limit aplicado
500 | Internal Server Error -- Há um problema com nossos servidores, tente novamente mais tarde.
503 | Service Unavailable -- Servidor em manunteção. Tente novamente mais tarde.
