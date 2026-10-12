# Hands-on: Credentials Report e Last accessed

## Parte A — IAM Credentials Report

1. Entre no Console AWS.
2. Abra **IAM**.
3. Procure **Credential report** no painel de IAM.
4. Gere o relatório e baixe o CSV quando estiver disponível.
5. Abra o arquivo em uma planilha e observe os campos de senha e Access Keys.

O relatório é uma visão das credenciais de usuários IAM da conta. A possibilidade de gerar ou consultar o relatório depende das permissões IAM da identidade utilizada; não é obrigatório usar root se a identidade tiver as permissões apropriadas.

### Checklist de análise do CSV

Para cada linha (usuário), verifique:

- [ ] `mfa_active` = `false` em usuário com senha? → ativar MFA.
- [ ] `password_last_used` muito antigo ou `no_information`? → usuário pode estar abandonado.
- [ ] `access_key_1_active` = `true` com `access_key_1_last_used_date` = `N/A`? → chave criada e nunca usada.
- [ ] `access_key_1_last_rotated` com mais de 90 dias? → considerar rotação.
- [ ] Linha `<root_account>` com `access_key_1_active` = `true`? → o root **não** deve ter Access Keys.

O relatório só pode ser regenerado a cada 4 horas; se você acabou de fazer uma mudança, ela pode ainda não aparecer.

## Parte B — Last accessed / Access Advisor

1. Abra **IAM → Users**.
2. Selecione um usuário.
3. Procure a área de permissões e as informações de acesso aos serviços. A localização e o rótulo podem variar na interface.
4. Observe quais serviços aparecem e quando foram acessados pela última vez.
5. Repita para um **grupo** e uma **role** (ex.: a role do hands-on anterior) e compare.

### Exercício: aplicando o menor privilégio

1. Escolha um usuário de laboratório com uma policy ampla (ex.: `AmazonEC2FullAccess` + `AmazonS3FullAccess`).
2. Use-o por alguns dias apenas para o EC2.
3. Volte ao Last accessed: o S3 deve aparecer como **Not accessed in the tracking period**.
4. Avalie e, se fizer sentido, remova a permissão do S3 do grupo/usuário.

Pela CLI, o mesmo dado pode ser obtido com:

```bash
aws iam generate-service-last-accessed-details --arn arn:aws:iam::123456789012:user/nome
aws iam get-service-last-accessed-details --job-id <JobId-retornado>
```

## O que comparar?

- O Credentials Report responde: **qual é o estado das credenciais?**
- Last accessed responde: **quando os serviços foram acessados por essa identidade?**

## Cuidados

Não remova uma permissão apenas porque não houve acesso recente. Confirme se ela ainda é necessária para tarefas futuras, emergências ou processos periódicos.
