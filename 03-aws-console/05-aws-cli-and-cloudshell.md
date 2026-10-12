# AWS CLI e AWS CloudShell

## 1. AWS CLI

**AWS CLI (Command Line Interface)** é uma ferramenta para gerenciar serviços da AWS usando comandos no terminal, em vez de fazer todas as operações pelo Console AWS.

Com ela, é possível consultar recursos, automatizar tarefas e executar operações em serviços AWS, desde que as credenciais e permissões permitam.

## 2. AWS CLI v2

A versão recomendada para novos estudos e instalações é a **AWS CLI versão 2**. No Windows, a instalação comum é feita pelo instalador oficial `.msi`.

Depois de instalar, abra um novo CMD ou PowerShell e verifique:

```powershell
aws --version
```

Para configurar um perfil inicial:

```powershell
aws configure
```

O comando solicita:
- `AWS Access Key ID`
- `AWS Secret Access Key`
- `Default region name`
- `Default output format`

Para a região de São Paulo, use `sa-east-1`. O formato de saída pode ser `json`, `yaml`, `text` ou deixado em branco.

> **Segurança:** nunca compartilhe chaves de acesso nem as coloque em repositórios Git. Para uso real, prefira credenciais temporárias e mecanismos como IAM roles sempre que possível.

## 3. AWS CloudShell

O **AWS CloudShell** é um terminal acessado pelo navegador, dentro do Console AWS. Ele já inclui ferramentas como o AWS CLI e autentica as chamadas usando o contexto da sessão do Console.

É diferente do CMD local: o CloudShell roda em um ambiente Linux gerenciado pela AWS, enquanto o CMD/PowerShell roda no Windows do computador.

### Características importantes do CloudShell

- **Sem custo adicional** pelo uso do shell (os recursos AWS criados pelos comandos são cobrados normalmente).
- **Não precisa de `aws configure`:** usa as permissões da identidade logada no Console.
- **Região:** os comandos usam, por padrão, a região selecionada no Console. Use `--region` para trabalhar em outra.
- **Armazenamento persistente:** cerca de **1 GB por região** no diretório `$HOME`; arquivos fora dele são perdidos ao encerrar a sessão. Dados podem ser apagados após um longo período sem uso na região.
- **Upload / download:** o menu **Actions** permite enviar e baixar arquivos.
- **Ferramentas pré-instaladas:** AWS CLI, Python, Node.js, git, entre outras.
- **Disponibilidade:** não está disponível em todas as regiões; confira a lista na documentação.
- A sessão é encerrada após um período de inatividade.

## CLI local x CloudShell

| | AWS CLI local | AWS CloudShell |
|---|---|---|
| Onde roda | Seu computador | Navegador (ambiente Linux da AWS) |
| Instalação | Necessária | Já vem pronta |
| Credenciais | `aws configure` (Access Keys) ou SSO | Sessão do Console |
| Arquivos | Disco local | `$HOME` persistente limitado |
| Bom para | Uso diário, scripts, automação | Testes rápidos sem instalar nada |

## 4. Comandos básicos

| Objetivo | Linux / CloudShell | Windows CMD |
|---|---|---|
| Mostrar diretório atual | `pwd` | `cd` ou `echo %cd%` |
| Listar arquivos | `ls` | `dir` |
| Mostrar arquivo | `cat arquivo.txt` | `type arquivo.txt` |
| Criar pasta | `mkdir teste` | `mkdir teste` |
| Entrar na pasta | `cd teste` | `cd teste` |
| Voltar uma pasta | `cd ..` | `cd ..` |

Criar um arquivo com conteúdo no CloudShell:

```bash
echo "teste" > demo.txt
```

O `>` cria o arquivo se ele não existir e substitui seu conteúdo se já existir. Para acrescentar conteúdo sem apagar o anterior, use `>>`.

## 5. Exemplos de comandos AWS CLI

```bash
aws sts get-caller-identity
aws s3 ls
aws ec2 describe-instances --region sa-east-1
```

Esses comandos consultam a identidade usada, listam buckets S3 e consultam instâncias EC2, respectivamente. O resultado depende das permissões e dos recursos existentes.

### Estrutura de um comando

```text
aws <serviço> <operação> [parâmetros] [opções globais]
aws  ec2      describe-instances  --region sa-east-1 --output table
```

### Opções globais úteis

| Opção | Para que serve |
|---|---|
| `--region` | Escolhe a região só para aquele comando |
| `--output` | Formato da saída: `json`, `yaml`, `text`, `table` |
| `--profile` | Usa um perfil de credenciais específico |
| `--query` | Filtra a saída (sintaxe JMESPath) |
| `help` | Mostra a ajuda: `aws ec2 help`, `aws ec2 describe-instances help` |

Exemplo com `--query`, listando apenas IDs e estados das instâncias:

```bash
aws ec2 describe-instances \
  --query "Reservations[].Instances[].[InstanceId,State.Name]" \
  --output table
```

### Mais comandos para praticar

```bash
aws iam list-users                 # usuários IAM (serviço global)
aws ec2 describe-regions --output table
aws s3 mb s3://nome-unico-do-bucket   # cria um bucket
aws s3 cp demo.txt s3://nome-unico-do-bucket/
```

> Se um comando retornar `AccessDenied`/`UnauthorizedOperation`, a CLI está funcionando: o problema é de **permissão IAM**, não de instalação.

## 6. SDKs e Boto3

Um **SDK (Software Development Kit)** permite chamar serviços AWS a partir de código de uma linguagem de programação. A AWS oferece SDKs para várias linguagens e plataformas, incluindo JavaScript, Python, Java, .NET, Go e aplicações móveis.

**Boto3** é o SDK da AWS para Python. Ele permite, por exemplo, listar objetos de um bucket S3 ou consultar recursos AWS usando código Python, sem precisar montar manualmente cada requisição HTTP.

## Resumo

- **Console AWS:** interface gráfica no navegador.
- **AWS CLI:** comandos no terminal.
- **CloudShell:** terminal Linux no navegador, integrado ao Console AWS.
- **SDK:** biblioteca para usar serviços AWS dentro de um programa.
- **Boto3:** SDK AWS para Python.
