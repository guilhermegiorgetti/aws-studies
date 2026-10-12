# Hands-on: IAM Role para um serviço AWS

## Objetivo

Entender como uma instância EC2 recebe permissões para acessar outro serviço AWS sem guardar chaves permanentes no código.

## Exemplo proposto

Uma aplicação em EC2 precisa ler objetos de um bucket S3.

## Passos conceituais

1. Abra **IAM → Roles → Create role**.
2. Escolha o serviço confiável **EC2** como entidade que pode assumir a role.
3. Associe uma policy com o menor conjunto de permissões S3 necessário.
4. Dê um nome claro à role, por exemplo `EC2ReadReportsFromS3`.
5. Abra **EC2 → Instances**, selecione a instância e procure a opção de modificar a IAM role.
6. Associe a role à instância.
7. Teste o acesso pela aplicação ou por um comando AWS CLI, se o CLI estiver instalado e configurado para usar as credenciais da role.

## Exemplo de policy de menor privilégio

Em vez de `AmazonS3FullAccess`, permita só leitura de um bucket específico (substitua o nome do bucket):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::meu-bucket-de-relatorios"
    },
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::meu-bucket-de-relatorios/*"
    }
  ]
}
```

Repare que `ListBucket` atua sobre o **bucket**, enquanto `GetObject` atua sobre os **objetos** (`/*`).

## Testando de dentro da instância

Em uma instância Amazon Linux (o AWS CLI já vem instalado), conecte-se e execute:

```bash
# Antes de associar a role: deve falhar com "Unable to locate credentials"
aws s3 ls s3://meu-bucket-de-relatorios

# Depois de associar a role:
aws sts get-caller-identity
aws s3 ls s3://meu-bucket-de-relatorios
```

O `get-caller-identity` mostrará um ARN do tipo `arn:aws:sts::123456789012:assumed-role/EC2ReadReportsFromS3/i-0abc...`, comprovando que a instância está usando a **role**, e não um usuário.

Um teste negativo também é útil: tente uma ação não permitida (ex.: `aws s3 rm ...` ou `aws iam list-users`) e confirme que recebe `AccessDenied`. A associação ou a remoção da role pode levar alguns segundos para refletir.

## Limpeza

1. Desassocie a role da instância (**Actions → Security → Modify IAM role**).
2. Termine a instância de laboratório.
3. Exclua a role e as policies personalizadas criadas para o teste.

## Segurança

- Use um bucket de teste e dados não sensíveis.
- Não adicione `AdministratorAccess` apenas para fazer o teste funcionar.
- Não configure Access Keys pessoais na instância com `aws configure`.
- Se o acesso for negado, revise a policy, a role associada, a policy do bucket e outras condições de autorização.

## Conceito importante

A role controla as chamadas às APIs AWS feitas pelo software na instância. Ela não abre a porta SSH e não substitui as regras de rede ou a autenticação do sistema operacional.
