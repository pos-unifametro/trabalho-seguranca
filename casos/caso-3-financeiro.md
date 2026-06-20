# Caso 3 — Sistema Financeiro

## Contexto do sistema

O sistema **“FinanceHub”** é uma plataforma financeira que permite:

- Consulta de extratos bancários
- Transferências entre contas
- Emissão de relatórios financeiros
- Histórico de transações

Todos os usuários acessam o sistema após autenticação via JWT.

---

## endpoint crítico

```
GET /extratos/{id}
Authorization: Bearer <jwt_token>
```

---

## comportamento observado

Um usuário autenticado realizou a seguinte sequência de requisições:

```
2026-07-19 09:10:01 GET /extratos/123 user_id=10
2026-07-19 09:10:05 GET /extratos/124 user_id=10
2026-07-19 09:10:08 GET /extratos/125 user_id=10
2026-07-19 09:10:12 GET /extratos/126 user_id=10
```

Mesmo sem alterar o token JWT, o sistema retornou diferentes extratos pertencentes a outros clientes.

---

## observação importante

O JWT utilizado na requisição contém apenas:

```
{
  "user_id": 10,
  "role": "cliente",
  "iat": 1752571930
}
```

Não há validação adicional do vínculo entre o usuário autenticado e o recurso acessado.

---

## problema identificado

O sistema permite acesso direto a recursos através de IDs sequenciais na URL, sem verificar propriedade.

---

## desafio

Identifique o problema de segurança existente e proponha mecanismos para garantir que:

- Um usuário só consiga acessar seus próprios extratos
- Não seja possível enumerar contas ou dados de outros usuários
- O backend valide corretamente a propriedade do recurso
- A autenticação e autorização sejam aplicadas de forma consistente

---

## perguntas para análise

- Qual é o tipo de vulnerabilidade presente nesse cenário?
- Onde a lógica de autorização deveria ser aplicada?
- Como impedir acesso via alteração de IDs na URL?
