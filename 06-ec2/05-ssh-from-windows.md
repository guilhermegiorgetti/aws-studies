# Conectar a uma instância EC2 via SSH no Windows

## O que é SSH?

**SSH (Secure Shell)** é um protocolo para acessar remotamente um terminal de outro computador de forma criptografada. É comum para administrar instâncias Linux.

## Antes de conectar

Você precisa de:
- uma instância EC2 em execução;
- um endereço IP público ou caminho de rede até a instância;
- um Security Group que permita TCP/22 a partir da origem autorizada;
- a chave privada correta, se a instância usar autenticação por key pair;
- o nome de usuário adequado à AMI.

## Exemplo no PowerShell

```powershell
ssh -i .\EC2Tutorial.pem ec2-user@IP_PUBLICO_DA_INSTANCIA
```

Substitua o nome do arquivo, o usuário e o IP pelos valores reais. `ec2-user` é comum em algumas AMIs Amazon Linux; outras imagens usam outros nomes de usuário.

Se o arquivo estiver em outra pasta, informe o caminho correto. Não compartilhe a chave privada `.pem` e não a envie ao GitHub.

O cliente OpenSSH já vem instalado no **Windows 10 (versão 1809+) e no Windows 11**. Para conferir:

```powershell
ssh -V
```

Para sair da sessão remota, digite `exit`.

## Usuário padrão por AMI

| AMI | Usuário |
|---|---|
| Amazon Linux 2 / Amazon Linux 2023 | `ec2-user` |
| Ubuntu | `ubuntu` |
| Debian | `admin` |
| RHEL | `ec2-user` |
| SUSE | `ec2-user` |
| Fedora | `fedora` (ou `ec2-user`) |

Se a AMI for de terceiros (Marketplace), confira a documentação do fornecedor.

## Erro "UNPROTECTED PRIVATE KEY FILE"

O SSH recusa chaves que outros usuários do computador podem ler. No Linux/macOS o comando seria `chmod 400 chave.pem`. No Windows, ajuste as permissões com `icacls` (no PowerShell, na pasta da chave):

```powershell
icacls.exe .\EC2Tutorial.pem /reset
icacls.exe .\EC2Tutorial.pem /grant:r "$($env:USERNAME):(R)"
icacls.exe .\EC2Tutorial.pem /inheritance:r
```

Isso deixa apenas o seu usuário com permissão de leitura. Pela interface gráfica: **Propriedades → Segurança → Avançado → Desabilitar herança → remover todos os outros usuários**.

## Alternativa: PuTTY

O PuTTY é um cliente SSH gráfico. Ele usa o formato `.ppk`: ao criar o key pair, escolha `.ppk`, ou converta o `.pem` com o **PuTTYgen**. Em **Connection → SSH → Auth → Credentials**, aponte para o `.ppk` e conecte em `ec2-user@IP`.

## Arquivo `config` do SSH (atalho)

Para não digitar o comando completo toda vez, crie `%USERPROFILE%\.ssh\config`:

```text
Host ec2-estudos
    HostName IP_PUBLICO_DA_INSTANCIA
    User ec2-user
    IdentityFile C:\Users\SEU_USUARIO\.ssh\EC2Tutorial.pem
```

Depois, basta `ssh ec2-estudos`. Lembre-se de atualizar o IP se a instância for parada e iniciada.

## Erros comuns

- **Timeout:** confira IP, rota de rede, instância em execução e regra inbound do Security Group.
- **Permission denied:** confira o usuário, a chave e a configuração de autenticação.
- **Host key warning:** leia o aviso antes de aceitar; ele protege contra conexões ao servidor errado.
- **Sem IP público:** talvez seja necessário usar VPN, bastion host, EC2 Instance Connect ou AWS Systems Manager, conforme a arquitetura.
- **IP mudou:** depois de parar e iniciar, o IP público muda; copie o novo IP no Console.
- **Rede bloqueia a porta 22:** algumas redes corporativas ou universitárias bloqueiam SSH de saída; teste outra rede ou use EC2 Instance Connect / Session Manager.
- **"Your IP" desatualizado:** se a regra do Security Group usa o seu IP e o provedor mudou o seu IP, a conexão dará timeout.

## Resumo

SSH permite administrar o terminal de uma instância Linux. A role IAM da instância não substitui a chave SSH nem abre a porta 22.
