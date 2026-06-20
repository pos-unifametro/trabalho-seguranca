# Caso 1 — Sistema Escolar

## auth.log
2026-07-15 08:12:10 LOGIN_SUCCESS aluno@escola.com IP=187.54.22.10 DEVICE=mobile
2026-07-15 08:12:11 TOKEN_ISSUED user_id=1 role=aluno
2026-07-15 08:12:13 GET /notas/145 user_id=1
2026-07-15 08:12:13 GET /notas/146 user_id=1
2026-07-15 08:12:14 GET /notas/147 user_id=1
2026-07-15 08:12:14 GET /notas/148 user_id=1
2026-07-15 08:12:15 GET /notas/149 user_id=1
2026-07-15 08:12:15 GET /notas/150 user_id=1
2026-07-15 08:12:16 GET /notas/151 user_id=1
2026-07-15 08:12:17 GET /notas/152 user_id=1
2026-07-15 08:12:18 EXPORT_BOLETIM user_id=1 format=pdf
2026-07-15 08:12:19 LOGOUT user_id=1

2026-07-15 08:13:02 LOGIN_FAILED aluno@escola.com IP=45.88.91.12
2026-07-15 08:13:03 LOGIN_FAILED aluno@escola.com IP=45.88.91.12
2026-07-15 08:13:04 LOGIN_FAILED aluno@escola.com IP=45.88.91.12
2026-07-15 08:13:05 LOGIN_SUCCESS aluno@escola.com IP=45.88.91.12
2026-07-15 08:13:06 TOKEN_ISSUED user_id=1 role=aluno
2026-07-15 08:13:06 GET /admin/relatorios

## banco.sql
id | email              | senha (MD5)                           | role
1  | aluno@escola.com   | 482c811da5d5b4bc6d497ffa98491e38     | aluno
2  | maria@escola.com   | 827ccb0eea8a706c4c34a16891f84e7b     | aluno
3  | joao@escola.com    | e10adc3949ba59abbe56e057f20f883e     | aluno
4  | prof@escola.com    | 25d55ad283aa400af464c76d713c07ad     | professor
5  | coord@escola.com   | 5f4dcc3b5aa765d61d8327deb882cf99     | coordenador

## JWT extraido do sistema
header:
{
  "alg": "HS256",
  "typ": "JWT"
}

payload:
{
  "user_id": 1,
  "email": "aluno@escola.com",
  "role": "aluno",
  "iat": 1752571930
}

### Problemas adicionais observados
- Logs mostram múltiplos LOGIN_FAILED seguidos do mesmo IP
- Depois do login, há acesso a /admin/relatorios por usuário com role “aluno”
- Exportação de boletim sem validação adicional
- Tokens JWT permanecem válidos indefinidamente
- Senhas armazenadas em MD5 sem salt
- Não há indícios de rate limiting ou bloqueio progressivo
- IDs de notas são sequenciais e previsíveis


## Pergunta principal
- O que está acontecendo no sistema durante esse fluxo?

### Desafio
- Analise o cenário e proponha melhorias para:
  - Autenticação e controle de sessão
  - Armazenamento de credenciais
  - Autorização por perfil (RBAC)
  - Proteção de endpoints sensíveis
  - Segurança de tokens JWT
  - Mitigação de ataques de força bruta
  - Proteção contra enumeração de dados (IDs previsíveis)
