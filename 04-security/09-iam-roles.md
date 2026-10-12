# IAM Roles

## O que é uma IAM role?

Uma **IAM role** é uma identidade da AWS que possui permissões definidas por policies e pode ser assumida por uma entidade autorizada, como um serviço AWS, uma aplicação ou um usuário de outra conta.

No caso de serviços AWS, a role permite que eles obtenham **credenciais temporárias** para realizar somente as ações autorizadas.

## Por que isso é importante?

Sem uma role, alguém poderia tentar guardar Access Keys permanentes dentro do código ou na máquina. Isso aumenta o risco de vazamento e dificulta a rotação das chaves.

Com uma role:
- as credenciais são temporárias;
- as permissões são controladas por policies;
- não é necessário embutir uma chave permanente no código;
- o acesso pode ser limitado ao necessário.

## Exemplo prático: EC2 acessando S3

Imagine uma aplicação rodando em uma instância EC2 que precisa ler relatórios de um bucket S3.

1. Crie uma IAM role confiável para o serviço EC2.
2. Associe à role uma policy que permita apenas as ações S3 necessárias.
3. Associe a role à instância EC2.
4. A aplicação usa as credenciais temporárias fornecidas pelo ambiente AWS para acessar o S3.

A aplicação pode acessar o S3 conforme a policy permite, sem armazenar uma Access Key e uma Secret Access Key permanentes no código.

## Role, policy e permission

- **Policy:** documento que descreve as permissões, por exemplo, permitir `s3:GetObject`.
- **Permission (permissão):** ação que uma identidade pode ou não realizar.
- **Role:** identidade assumível à qual policies podem ser associadas.

Roles não são exclusivamente para serviços, e policies não são exclusivamente para usuários. Policies podem ser associadas a usuários, grupos e roles.

## As duas policies de uma role

Toda role tem dois lados:

| Policy | Pergunta que responde | Exemplo |
|---|---|---|
| **Trust policy** (política de confiança) | **Quem** pode assumir a role? | O serviço EC2 |
| **Permissions policy** (política de permissões) | **O que** a role pode fazer? | Ler objetos do S3 |

Exemplo de trust policy que permite ao serviço EC2 assumir a role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "ec2.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Quem fornece as credenciais temporárias é o **AWS STS (Security Token Service)**, pela ação `sts:AssumeRole`. As credenciais expiram e são renovadas automaticamente pelo serviço.

## Instance profile

No EC2, a role é entregue à instância por meio de um **instance profile** — um "contêiner" que carrega a role. Quando você cria uma role para EC2 pelo Console, o instance profile com o mesmo nome é criado automaticamente. Pela CLI ou por infraestrutura como código, pode ser necessário criá-lo à parte.

Uma instância tem **no máximo uma role** associada por vez, mas a role pode ser trocada sem parar a instância.

## Usos comuns de roles

- **EC2 instance role:** aplicação na instância acessa S3, DynamoDB etc.
- **Lambda execution role:** função Lambda grava logs no CloudWatch ou lê uma fila.
- **CloudFormation service role:** permite ao CloudFormation criar recursos.
- **Acesso entre contas (cross-account):** um usuário da conta A assume uma role na conta B.
- **Federação / SSO:** pessoas autenticadas fora do IAM (ex.: IAM Identity Center, Google, Active Directory) recebem acesso por meio de roles.

## User x Role

| | IAM User | IAM Role |
|---|---|---|
| Representa | Uma pessoa ou aplicação específica | Uma "função" que pode ser assumida |
| Credenciais | Senha e/ou Access Keys de longo prazo | Temporárias (STS) |
| Associado a | Uma única identidade | Quem estiver autorizado na trust policy |
| Analogia | Crachá pessoal | Colete de "visitante autorizado" que se veste e devolve |

## Importante: role EC2 não é a mesma coisa que SSH

Uma IAM role de instância controla o que o software da instância pode fazer **nas APIs da AWS**. Ela não configura, por si só, quem pode entrar por SSH no sistema operacional.

O acesso SSH depende, entre outros fatores, das regras de rede do Security Group, da chave/autenticação SSH e da configuração do sistema.

## Resumo para anotar

**IAM role:** identidade assumida por um serviço ou outra entidade, com permissões definidas por policies e, normalmente, credenciais temporárias. Evita guardar chaves permanentes no código e limita o acesso.

- **Trust policy:** quem pode assumir.
- **Permissions policy:** o que pode fazer.
- **STS:** emite as credenciais temporárias.
- **Para a prova:** "um serviço AWS precisa acessar outro serviço AWS" → **IAM role**.
