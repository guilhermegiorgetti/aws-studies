# EC2 Instance Connect e IAM Role

## EC2 Instance Connect

O EC2 Instance Connect é uma opção para conectar a instâncias EC2 compatíveis pelo Console AWS ou por ferramentas suportadas, sem precisar administrar manualmente uma chave SSH permanente para cada conexão, conforme o método utilizado.

O caminho comum no Console é:
1. Abra **EC2 → Instances**.
2. Selecione a instância.
3. Clique em **Connect**.
4. Escolha um método disponível, como EC2 Instance Connect, e siga as instruções.

A opção depende do sistema operacional, da rede, da configuração da instância e da região.

### Como funciona por trás

```text
Console (navegador) ──1. envia chave pública temporária──> Instance Connect API
                                                                  │
                                                                  v
                                                      instância (válida por ~60 s)
Console (navegador) ──2. conexão SSH na porta 22─────────────> instância
```

- O Instance Connect **ainda usa SSH**: o Security Group precisa permitir **TCP/22** a partir dos endereços do serviço EC2 Instance Connect na região (em laboratório, muitas vezes se usa `0.0.0.0/0`, o que é menos seguro).
- A chave pública temporária é válida por cerca de **60 segundos**, só para iniciar a conexão.
- O acesso é controlado por IAM (ação `ec2-instance-connect:SendSSHPublicKey`).
- Vem pré-instalado em **Amazon Linux 2 / 2023** e em versões recentes do **Ubuntu**.
- Para instâncias **sem IP público**, existe o **EC2 Instance Connect Endpoint**.

## Outras formas de conectar

| Método | Precisa de porta 22? | Precisa de key pair? | Observação |
|---|---|---|---|
| **SSH** (PowerShell, PuTTY) | Sim | Sim | Clássico |
| **EC2 Instance Connect** | Sim | Não (chave temporária) | Pelo navegador |
| **Session Manager** (Systems Manager) | **Não** | **Não** | Precisa do SSM Agent e de uma role com `AmazonSSMManagedInstanceCore`; sessões podem ser auditadas |
| **EC2 Serial Console** | Não | Não | Para recuperar instâncias com problema de boot/rede |

> O **Session Manager** é muito usado em produção porque dispensa abrir a porta 22 e dispensa gerenciar chaves.

## IAM Role da instância

Uma IAM role associada à instância permite que o software executado nela acesse APIs AWS com credenciais temporárias e permissões definidas.

Exemplo: permitir que uma aplicação na EC2 leia arquivos específicos do S3.

### Demonstração clássica

Dentro de uma instância Amazon Linux (o AWS CLI já vem instalado):

```bash
aws iam list-users
# Sem role: "Unable to locate credentials. You can configure credentials by running aws configure."
```

O caminho **errado** seria rodar `aws configure` e colar a sua Access Key na instância — qualquer pessoa com acesso à máquina poderia lê-la em `~/.aws/credentials`.

O caminho **certo**:

1. Crie uma role para EC2 com a policy `IAMReadOnlyAccess` (ex.: `DemoRoleForEC2`).
2. Em **EC2 → Instances → Actions → Security → Modify IAM role**, associe a role.
3. Execute novamente:

```bash
aws iam list-users
# Agora retorna a lista de usuários.
```

Se você remover a policy da role, o comando volta a falhar — agora com `AccessDenied`, porque há credenciais, mas falta permissão. Mudanças podem levar alguns segundos para refletir.

## Não confunda os controles

- **Security Group:** controla tráfego de rede, como permitir TCP/22.
- **SSH / autenticação do sistema:** controla a autenticação para entrar no sistema operacional.
- **IAM Role:** controla quais APIs e recursos AWS o software na instância pode acessar.
- **IAM User/Identity Center:** controla identidades humanas e seus acessos ao Console ou APIs.

Não execute `aws configure` com suas chaves pessoais em uma EC2 compartilhada ou de produção. Prefira uma IAM role com o menor privilégio necessário.

## Resumo

- **Instance Connect:** SSH pelo navegador com chave temporária; ainda exige porta 22.
- **Session Manager:** acesso sem porta 22 e sem chave; exige SSM Agent e role.
- **IAM role na EC2:** forma correta de dar permissões de API à instância — nunca Access Keys pessoais.
