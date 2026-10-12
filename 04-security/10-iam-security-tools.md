# IAM Security Tools

A AWS oferece ferramentas para ajudar a auditar credenciais, revisar permissões e identificar acessos desnecessários.

## 1. IAM Credentials Report

Gera um relatório em formato CSV com informações sobre as credenciais dos usuários IAM da conta, como estado e datas relacionadas à senha e às Access Keys.

- **Escopo:** visão da conta, incluindo os usuários IAM abrangidos pelo relatório.
- **Uso:** identificar credenciais antigas, inativas ou que precisam de revisão.
- **Atenção:** o relatório não é uma lista de todas as permissões concedidas; ele trata do estado das credenciais.
- **Frequência:** um novo relatório pode ser gerado no máximo a cada **4 horas**; antes disso, a AWS devolve o último gerado.
- **Inclui o root:** a primeira linha do CSV representa a conta root (`<root_account>`).

Campos úteis do CSV:

| Campo | O que indica |
|---|---|
| `password_enabled` / `password_last_used` | Se o usuário tem senha e quando a usou |
| `mfa_active` | Se o MFA está ativo |
| `access_key_1_active` / `access_key_1_last_rotated` | Se a chave está ativa e quando foi rotacionada |
| `access_key_1_last_used_date` | Último uso da chave |

Pela CLI:

```bash
aws iam generate-credential-report
aws iam get-credential-report --query Content --output text | base64 -d > report.csv
```

## 2. IAM Access Advisor / Last accessed

Mostra informações sobre os serviços AWS aos quais uma identidade tem permissões e quando esses serviços foram acessados pela última vez.

- **Escopo:** análise de uma identidade, como um usuário ou role.
- **Uso:** identificar permissões potencialmente desnecessárias e apoiar o princípio do menor privilégio.
- **Atenção:** ausência de acesso recente não prova, sozinha, que uma permissão nunca será necessária. Avalie o uso esperado antes de removê-la.

No Console, a apresentação pode aparecer como informações de **Last accessed** (último acesso), dependendo da tela e da versão da interface.

- Também está disponível para **grupos**, **roles** e **policies**.
- Os dados de último acesso têm um período de rastreamento limitado e podem levar algumas horas para aparecer; confira a documentação atual.
- Para alguns serviços, mostra até o nível de **ação** (ex.: quais ações do S3 foram usadas), não só o serviço.

## Nível de conta x nível de usuário (cai na prova)

```text
IAM Credentials Report  ──>  nível da CONTA    ──>  todos os usuários e suas credenciais
IAM Access Advisor      ──>  nível do USUÁRIO  ──>  serviços permitidos e último acesso
```

## 3. IAM Policy Simulator

Permite testar como as policies IAM avaliariam determinadas solicitações, sem precisar executar a ação real no serviço.

É útil para entender por que uma ação seria permitida ou negada. O simulador não substitui testes completos em um ambiente real e pode haver diferenças em cenários específicos.

## 4. IAM Access Analyzer

Ajuda a identificar recursos compartilhados com entidades externas à zona de confiança definida, além de oferecer recursos de análise de policies. Os recursos disponíveis dependem da configuração e do tipo de análise.

Principais recursos:

- **External access findings:** aponta recursos (buckets S3, roles, chaves KMS etc.) acessíveis por outras contas ou publicamente.
- **Unused access findings:** aponta roles, Access Keys, senhas e permissões não usadas (recurso pago).
- **Policy validation:** verifica erros de sintaxe e boas práticas ao escrever policies.
- **Policy generation:** gera uma policy de menor privilégio com base na atividade registrada no CloudTrail.

## Outros serviços relacionados

- **AWS CloudTrail:** registra as chamadas de API da conta — responde "**quem** fez **o quê**, **quando** e **de onde**".
- **AWS Trusted Advisor:** inclui verificações de segurança, como MFA no root e chaves expostas.
- **AWS Security Hub:** consolida achados de segurança de vários serviços.

## Comparação rápida

| Ferramenta | Para que serve |
|---|---|
| Credentials Report | Revisar o estado das credenciais dos usuários IAM |
| Access Advisor / Last accessed | Ver quando serviços foram acessados por uma identidade |
| Policy Simulator | Simular decisões de autorização de policies |
| Access Analyzer | Identificar acessos externos e analisar policies |

## Resumo

Essas ferramentas ajudam a **auditar credenciais, analisar o uso de acessos, testar policies e identificar configurações de permissão que merecem revisão**.
