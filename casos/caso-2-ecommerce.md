# Caso 2 — Sistema de E-commerce

## Contexto do sistema

O sistema **“Loja Rápida”** é um e-commerce que permite:

- Cadastro de usuários
- Compra de produtos
- Painel administrativo para gestão de pedidos
- Integração com gateways de pagamento
- Controle de estoque

Recentemente, foi implementado um endpoint simplificado para cadastro de usuários com foco em agilidade no desenvolvimento.

---

## access.log

```
2026-07-18 10:01:02 POST /usuarios status=201 ip=191.34.22.10
body={"nome":"João","email":"joao@email.com","is_admin":false}

2026-07-18 10:02:11 POST /usuarios status=201 ip=191.34.22.11
body={"nome":"Maria","email":"maria@email.com","is_admin":false}

2026-07-18 10:03:45 POST /usuarios status=201 ip=191.34.22.12
body={"nome":"AdminTest","email":"admin@test.com","is_admin":true}

2026-07-18 10:03:46 POST /usuarios status=201 ip=191.34.22.12
body={"nome":"BackupAdmin","email":"backup@shop.com","is_admin":true}

2026-07-18 10:04:02 GET /admin/painel user_id=87 role=cliente ip=191.34.22.12
2026-07-18 10:04:03 GET /admin/pedidos user_id=87 role=cliente ip=191.34.22.12

2026-07-18 10:04:05 POST /pedidos/estorno user_id=87 role=cliente ip=191.34.22.12
body={"pedido_id":1045,"motivo":"teste"}

2026-07-18 10:04:10 GET /usuarios/87
2026-07-18 10:04:11 GET /usuarios/88
2026-07-18 10:04:12 GET /usuarios/89

2026-07-18 10:04:13 EXPORT_DADOS_CLIENTES format=csv user_id=87
```

---

## banco.sql

```
id | nome        | email              | senha_hash | is_admin | created_at
1  | João        | joao@email.com     | ***        | false    | 2026-07-18
2  | Maria       | maria@email.com    | ***        | false    | 2026-07-18
3  | AdminTest   | admin@test.com     | ***        | true     | 2026-07-18
4  | BackupAdmin | backup@shop.com    | ***        | true     | 2026-07-18
```

---

## endpoint crítico

```
POST /usuarios
Content-Type: application/json

{
  "nome": "João",
  "email": "joao@email.com",
  "is_admin": true
}
```

---

## implementação atual

```php
User::create(
    $request->all()
);
```

---

## problemas identificados

- Usuários comuns sendo criados como admin
- Acesso indevido a rotas administrativas
- Falta de validação de input
- Mass assignment
- Falta de controle de autorização
- Exposição de dados sensíveis

---

## desafio

Analise os riscos e proponha melhorias para:

- Prevenção de mass assignment
- Validação de input
- Controle de atributos sensíveis
- Separação entre request e domínio
- Segurança de criação de usuários
- Proteção contra escalonamento de privilégios
