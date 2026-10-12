# Amazon EC2 Basics

## O que é EC2?

**Amazon EC2 (Elastic Compute Cloud)** é um serviço de computação da AWS que permite alugar capacidade de computação em forma de instâncias virtuais. É semelhante, em conceito, a uma VPS.

O EC2 é um exemplo de **IaaS (Infrastructure as a Service)**: você escolhe a configuração e administra partes do sistema, enquanto a AWS mantém a infraestrutura física.

## Para que serve?

- Hospedar sites e APIs.
- Executar aplicações e automações.
- Rodar serviços de backend.
- Processar dados.
- Criar ambientes de desenvolvimento e teste.

## O que escolher ao criar uma instância?

- **AMI (Amazon Machine Image):** imagem-base com sistema operacional e configuração inicial.
- **Instance type:** define recursos como CPU, memória, rede e, conforme o tipo, armazenamento local.
- **Key pair:** pode ser usado para autenticação SSH em sistemas compatíveis.
- **Network/VPC/subnet:** define onde a instância fica conectada.
- **Security Group:** firewall virtual para tráfego de entrada e saída.
- **Storage:** volumes EBS ou armazenamento local disponível no tipo de instância.
- **User Data:** script de inicialização opcional.

## Serviços que trabalham junto com EC2

- **EBS:** armazenamento em bloco persistente, geralmente conectado a uma instância.
- **EFS:** sistema de arquivos compartilhado via rede para múltiplas instâncias compatíveis.
- **ELB:** distribui tráfego entre destinos, como instâncias.
- **Auto Scaling Group (ASG):** mantém e ajusta a quantidade de instâncias conforme políticas.

## EC2 User Data / Bootstrap

O User Data permite fornecer comandos ou scripts para automatizar tarefas de inicialização, como instalar pacotes e preparar uma aplicação. Em configurações Linux comuns, o script é executado com privilégios elevados na primeira inicialização; o comportamento depende do sistema e da configuração.

Exemplo didático para uma AMI Linux compatível:

```bash
#!/bin/bash
dnf update -y
```

Não execute scripts de origem desconhecida. User Data pode conter dados visíveis a entidades com acesso à configuração da instância; não coloque segredos nele.

Por padrão, o User Data roda **apenas no primeiro boot** e como usuário **root**. O log da execução em Linux fica em `/var/log/cloud-init-output.log`, útil para depurar.

## Ciclo de vida da instância

```text
pending ──> running ──> stopping ──> stopped ──> (start) ──> pending
               │                         │
               ├──> rebooting ──> running │
               │                         │
               └──> shutting-down ──> terminated <──┘
```

| Ação | O que acontece | Cobrança de computação | Dados no EBS raiz |
|---|---|---|---|
| **Reboot** | Reinicia o SO; continua no mesmo host | Continua | Mantidos |
| **Stop** | Desliga a instância; pode voltar em outro host | Para (EBS continua cobrado) | Mantidos |
| **Hibernate** | Salva a RAM no EBS raiz e desliga | Para (EBS continua cobrado) | Mantidos, incluindo a RAM |
| **Terminate** | Exclui a instância definitivamente | Para | Excluídos se *Delete on Termination* estiver ativo (padrão para o volume raiz) |

Ao parar e iniciar uma instância, o **IP público muda** (o IP privado é mantido). Para um IP público fixo, use um **Elastic IP** — mas, em geral, prefira DNS ou Load Balancer.

> **Custo:** endereços IPv4 públicos (incluindo Elastic IPs) são cobrados por hora, mesmo quando associados a instâncias. Confira os preços atuais.

## Cobrança do EC2 On-Demand

Instâncias Linux e Windows On-Demand são cobradas **por segundo**, com mínimo de 60 segundos. Outros sistemas operacionais podem ser cobrados por hora. Não há cobrança de computação no estado `stopped`, mas volumes EBS e IPs continuam gerando custo.

## Instance Metadata (IMDS)

De dentro da instância, é possível consultar informações sobre ela mesma (ID, tipo, IP, credenciais da role) no endereço especial `169.254.169.254`. A AWS recomenda o **IMDSv2**, que exige um token de sessão:

```bash
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -s -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id
```

## EC2 e o Modelo de Responsabilidade Compartilhada

| AWS | Cliente |
|---|---|
| Infraestrutura física, hosts e rede global | Regras de Security Group |
| Isolamento entre instâncias no mesmo host | Patches e atualizações do sistema operacional |
| Substituição de hardware com falha | Software e utilitários instalados |
| Conformidade da infraestrutura | IAM roles e permissões; segurança dos dados |

## Resumo

EC2 é a máquina virtual; AMI é a imagem-base; instance type define os recursos; Security Group controla o tráfego; User Data automatiza tarefas de inicialização.

- **Stop** para de cobrar computação, mas não o armazenamento.
- **Terminate** é definitivo.
- **IP público** muda ao parar/iniciar; Elastic IP é fixo (e cobrado).
- **Patches do SO** são responsabilidade do cliente.
