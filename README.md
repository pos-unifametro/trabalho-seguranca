# Desafio: Auditoria de Segurança em APIs

## Contexto

Sua equipe foi contratada para realizar uma auditoria de segurança em sistemas que estão prestes a entrar em produção.

Para cada caso, identifique:

1. Vulnerabilidades
2. Possíveis ataques
3. Impactos para o negócio
4. Estratégias de mitigação
5. Prioridade das correções

---

## Entregavel hoje 20/06/2026
- 1 relatório em PDF (ou markdown) com as propostas de melhoria
- 1 apresentação de até 20min explicando:
  - Contexto da aplicação
  - Problemas encontrados
  - Propostas de solução
  - Prazo para implementação da solução

--- 

# Caso 1 — Sistema Escolar

## Cenário

O sistema possui login com e-mail e senha.

Características identificadas:

* Senhas armazenadas utilizando MD5
* Sem limite de tentativas de login
* JWT sem expiração
* Endpoint para consulta de notas:

```http
GET /notas/{id}
```

## Desafio

Analise os riscos existentes e proponha melhorias para autenticação, armazenamento de credenciais e proteção dos dados dos alunos.

---

# Caso 2 — E-commerce

## Cenário

O sistema possui o seguinte endpoint:

```http
POST /usuarios
```

Payload recebido:

```json
{
    "nome": "João",
    "email": "joao@email.com",
    "is_admin": true
}
```

Implementação atual:

```php
User::create(
    $request->all()
);
```

## Desafio

Avalie os riscos relacionados ao cadastro de usuários e proponha uma estratégia segura para persistência dos dados recebidos.

---

# Caso 3 — Sistema Financeiro

## Cenário

O sistema possui o endpoint:

```http
GET /extratos/123
```

O usuário está autenticado com JWT válido.

Ao alterar a URL para:

```http
GET /extratos/124
```

o sistema retorna normalmente o extrato de outro cliente.

## Desafio

Identifique o problema de segurança existente e proponha mecanismos para garantir que um usuário só consiga acessar seus próprios dados.

---

# Caso 4 — Marketplace

## Cenário

Existem três perfis:

* ADMIN
* VENDEDOR
* CLIENTE

Os vendedores podem editar produtos através do endpoint:

```http
PUT /produtos/{id}
```

Porém o sistema não verifica se o produto pertence ao vendedor autenticado.

## Desafio

Avalie os impactos dessa falha e proponha uma estratégia de autorização adequada para proteger os recursos.

---

# Caso 5 — Startup em Crescimento

## Cenário

Uma startup lançou uma API pública.

Atualmente não existem:

* Rate Limiting
* Logs
* Monitoramento
* Alertas
* Auditoria de acessos

## Desafio

Liste os principais riscos operacionais e de segurança e proponha um plano de implementação priorizado para tornar a API mais resiliente.

---

# Caso 6 — Aplicativo de Saúde

## Cenário

A API retorna a seguinte resposta:

```json
{
    "id": 15,
    "nome": "João",
    "email": "joao@email.com",
    "senha": "$2y$10...",
    "token": "eyJhbGc...",
    "telefone": "(85) 99999-9999"
}
```

## Desafio

Avalie quais informações não deveriam ser expostas ao cliente e proponha uma resposta mais adequada para a API.

---

# Entrega

Cada equipe deverá apresentar:

## 1. Vulnerabilidades encontradas

Quais problemas foram identificados?

---

## 2. Possíveis ataques

Como um atacante poderia explorar as falhas?

---

## 3. Impactos

Quais seriam os impactos para:

* Usuários
* Empresa
* Operação
* Reputação

---

## 4. Estratégias de correção

O que deveria ser implementado para mitigar os riscos?

---

## 5. Priorização

Classifique as correções em:

### Alta Prioridade

Correções que precisam acontecer imediatamente.

### Média Prioridade

Importantes, mas não bloqueiam a operação.

### Baixa Prioridade

Melhorias futuras.

---

# Critérios de Avaliação

| Critério                 | Pontos |
| ------------------------ | ------ |
| Identificação dos riscos | 3      |
| Qualidade das soluções   | 3      |
| Argumentação técnica     | 2      |
| Priorização das ações    | 2      |
| **Total**                | **10** |

---

# Pergunta Final

Se vocês fossem os responsáveis por colocar esses sistemas em produção amanhã, quais correções fariam primeiro e por quê?
