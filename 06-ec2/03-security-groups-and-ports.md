# Security Groups e portas clássicas

## O que é um Security Group?

Um **Security Group** é um firewall virtual associado a recursos de rede, como instâncias EC2. Ele controla o tráfego permitido de entrada e saída.

- **Inbound rules:** tráfego que pode entrar.
- **Outbound rules:** tráfego que pode sair.
- As regras especificam protocolos, portas e origens/destinos autorizados.
- As regras são permissivas: o tráfego não autorizado não é permitido por uma regra implícita de bloqueio.
- Security Groups são stateful: respostas ao tráfego permitido são tratadas automaticamente.

Um Security Group pode ser associado a mais de uma instância, e uma instância pode ter mais de um Security Group. Eles são vinculados à VPC e têm escopo regional.

### Comportamento padrão de um Security Group novo

| Direção | Padrão |
|---|---|
| **Inbound** | Tudo **bloqueado** (nenhuma regra) |
| **Outbound** | Tudo **liberado** (`0.0.0.0/0`, todos os protocolos) |

Security Groups **só têm regras de permissão (allow)**; não é possível criar uma regra de negação (deny). Para bloquear um IP específico, usa-se outro controle, como **Network ACLs** ou **AWS WAF**.

### Anatomia de uma regra

| Campo | Exemplo |
|---|---|
| Type | SSH |
| Protocol | TCP |
| Port range | 22 |
| Source | `203.0.113.10/32` (um único IP) |
| Description | "SSH do meu notebook" |

Sobre a origem (CIDR):

- `0.0.0.0/0` = qualquer endereço IPv4 da Internet.
- `::/0` = qualquer endereço IPv6.
- `/32` = exatamente um endereço IPv4.
- Também é possível usar **outro Security Group** como origem (ver abaixo).

### Referenciando outro Security Group

Em vez de IPs, uma regra pode dizer "permita tráfego vindo de qualquer instância que tenha o Security Group X". Isso é muito usado em arquiteturas em camadas:

```text
Internet ──443──> [SG-ALB] Load Balancer ──80──> [SG-WEB] EC2 ──3306──> [SG-DB] Banco
```

- `SG-WEB` aceita porta 80 **apenas** de `SG-ALB`.
- `SG-DB` aceita porta 3306 **apenas** de `SG-WEB`.

Assim, as regras continuam válidas mesmo quando instâncias são criadas ou substituídas e os IPs mudam.

## Diagnóstico rápido de conexão

| Sintoma | Causa mais provável |
|---|---|
| **Timeout** (a conexão fica "pendurada") | Security Group (ou rede) bloqueando |
| **Connection refused** | O tráfego chegou, mas a aplicação não está rodando/escutando na porta |
| **Permission denied (publickey)** | Rede OK; problema de chave ou usuário SSH |

## Fluxo simplificado

```text
Usuário / Internet
       |
       | tráfego de entrada (inbound)
       v
Security Group
       |
       v
Instância EC2
       |
       | tráfego de saída (outbound)
       v
Outro serviço / Internet
```

O Security Group não fica dentro do sistema operacional da instância; é um controle de rede da AWS.

## Portas importantes para o exame

| Porta | Protocolo/serviço | Uso típico |
|---|---|---|
| 22 | SSH | Acesso remoto a Linux/Unix |
| 21 | FTP | Transferência de arquivos por FTP tradicional |
| 22 | SFTP | Transferência de arquivos sobre SSH |
| 80 | HTTP | Tráfego web sem criptografia TLS |
| 443 | HTTPS | Tráfego web protegido por TLS |
| 3389 | RDP | Acesso remoto a Windows |

SFTP não usa a porta 21 por padrão; normalmente usa SSH na porta 22.

### Outras portas úteis no dia a dia

| Porta | Serviço |
|---|---|
| 53 | DNS (UDP e TCP) |
| 2049 | NFS — usado pelo **Amazon EFS** |
| 3306 | MySQL / MariaDB / Aurora MySQL |
| 5432 | PostgreSQL / Aurora PostgreSQL |
| 1433 | Microsoft SQL Server |
| 1521 | Oracle |
| 6379 | Redis |

## TCP

**TCP (Transmission Control Protocol)** é um protocolo de transporte confiável, que estabelece uma conexão e controla a entrega ordenada dos dados.

**UDP (User Datagram Protocol)** não estabelece conexão nem garante entrega/ordem, mas tem menos overhead. É usado em DNS, streaming, jogos e VoIP.

**ICMP** é usado por ferramentas como `ping`. Se o `ping` não responder, verifique se há uma regra de ICMP — muitas instâncias funcionam normalmente sem ela.

## Security Group x Network ACL

| | Security Group | Network ACL (NACL) |
|---|---|---|
| Nível | Instância (interface de rede) | Subnet |
| Estado | **Stateful** (resposta liberada automaticamente) | **Stateless** (resposta precisa de regra) |
| Regras | Apenas **allow** | **Allow** e **deny** |
| Avaliação | Todas as regras | Em ordem numérica, a primeira que corresponde |

## Boas práticas

- Não abra SSH (`22`) ou RDP (`3389`) para `0.0.0.0/0` sem necessidade.
- Restrinja a origem ao seu IP ou a uma rede confiável.
- Abra somente as portas necessárias.
- Separe regras por função quando isso facilitar a administração.
- Lembre-se de que Security Groups controlam tráfego de rede; autenticação e permissões da aplicação continuam sendo necessárias.

## Resumo

Security Group = firewall virtual da AWS. Inbound controla o que entra; outbound, o que sai. Portas comuns: 22 SSH, 80 HTTP, 443 HTTPS e 3389 RDP.
