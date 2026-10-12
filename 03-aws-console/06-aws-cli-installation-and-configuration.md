# Instalação e configuração do AWS CLI no Windows

## 1. Instalar

1. Abra a documentação oficial de instalação do AWS CLI versão 2 para Windows: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
2. Baixe o instalador oficial de 64 bits (`AWSCLIV2.msi`), se o seu Windows for 64 bits.
3. Execute o instalador e conclua o assistente.
4. Abra um novo CMD ou PowerShell.

## 2. Verificar a instalação

```powershell
aws --version
```

A resposta deve começar com algo parecido com `aws-cli/2...`.

Se aparecer `'aws' não é reconhecido como um comando interno`, feche e abra o terminal novamente. Se persistir, confira se a pasta de instalação (normalmente `C:\Program Files\Amazon\AWSCLIV2\`) está no `PATH`.

## 3. Criar uma Access Key (pré-requisito)

O `aws configure` pede uma Access Key. Para criá-la para um usuário IAM (não para o root):

1. Abra **IAM → Users** e selecione o usuário.
2. Aba **Security credentials** → **Create access key**.
3. Escolha o caso de uso **Command Line Interface (CLI)** e leia as alternativas recomendadas.
4. Copie o **Access Key ID** e o **Secret Access Key** ou baixe o `.csv`.

> O **Secret Access Key só é exibido uma vez**. Se perder, desative/exclua a chave e crie outra. Cada usuário IAM pode ter no máximo **2 Access Keys**, o que permite fazer a rotação sem interrupção.

## 4. Configurar

```powershell
aws configure
```

Informe os dados solicitados. Para a região de São Paulo, use:

```text
sa-east-1
```

O formato de saída `json` é uma escolha comum para estudar:

```text
json
```

### Onde a configuração fica salva?

O `aws configure` grava dois arquivos de texto na pasta do usuário:

| Arquivo | Conteúdo |
|---|---|
| `%USERPROFILE%\.aws\credentials` | Access Key ID e Secret Access Key |
| `%USERPROFILE%\.aws\config` | Região e formato de saída |

No Linux/macOS/CloudShell, a pasta é `~/.aws/`. Essa pasta **nunca** deve entrar em um repositório Git.

### Perfis (profiles)

É possível guardar mais de um conjunto de credenciais, por exemplo para contas diferentes:

```powershell
aws configure --profile estudos
aws s3 ls --profile estudos
```

Para usar um perfil em todos os comandos da sessão do PowerShell:

```powershell
$env:AWS_PROFILE = "estudos"
```

Para ver qual configuração está ativa:

```powershell
aws configure list
aws configure list-profiles
```

### Alternativa recomendada: IAM Identity Center (SSO)

Em vez de Access Keys de longo prazo, a AWS recomenda, quando possível, usar o **IAM Identity Center** com credenciais temporárias:

```powershell
aws configure sso
aws sso login --profile meu-perfil-sso
```

## 5. Testar

```powershell
aws sts get-caller-identity
```

O comando mostra a identidade da conta usada pela CLI. Se aparecer erro de credencial, confira se os dados foram configurados corretamente e se ainda são válidos.

Exemplo de resposta (valores fictícios):

```json
{
    "UserId": "AIDAEXEMPLO123",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/guilherme"
}
```

## Erros comuns

| Erro | Causa provável |
|---|---|
| `Unable to locate credentials` | `aws configure` não foi executado ou o perfil está errado |
| `InvalidClientTokenId` | Access Key ID incorreto, desativado ou excluído |
| `SignatureDoesNotMatch` | Secret Access Key incorreto ou relógio do computador errado |
| `AccessDenied` / `UnauthorizedOperation` | Credenciais válidas, mas falta permissão IAM |
| `Could not connect to the endpoint URL` | Região digitada errada (ex.: `sa-east1`) ou sem rede |

## Atenção às credenciais

- Não cole chaves em prints, mensagens ou arquivos públicos.
- Não envie arquivos de credenciais para o GitHub.
- Evite usar a conta root para tarefas cotidianas.
- Em uma instância EC2, prefira associar uma IAM role com permissões mínimas em vez de executar `aws configure` com chaves pessoais.
- O `aws configure` cria configuração local; ele não concede permissões à conta. As permissões vêm do IAM.
