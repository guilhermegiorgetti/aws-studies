# Amazon EFS Overview

## O que é EFS?

**Amazon EFS (Elastic File System)** é um sistema de arquivos gerenciado que pode ser montado por várias instâncias EC2 compatíveis, permitindo compartilhar arquivos por rede.

Ele é frequentemente usado com Linux e com o protocolo NFS. O acesso depende da configuração de rede, dos mount targets, das regras de Security Group e das permissões do sistema de arquivos.

## Características

- sistema de arquivos elástico, que cresce e diminui conforme os dados;
- compartilhamento de arquivos entre várias instâncias;
- integração com várias Availability Zones dentro de uma região;
- classes de armazenamento e gerenciamento de ciclo de vida para dados acessados com menor frequência.

Outros pontos importantes:

- Funciona com **Linux** (sistema de arquivos compatível com POSIX) via **NFSv4**. Para Windows, a opção é o **FSx for Windows File Server**.
- **Não é preciso provisionar capacidade:** paga-se pelo **armazenamento efetivamente usado** (GB/mês), diferente do EBS.
- Pode ser montado por **centenas/milhares** de instâncias ao mesmo tempo, em **várias AZs**.
- Também pode ser acessado a partir de servidores on-premises (via VPN ou Direct Connect), contêineres (ECS/EKS) e funções Lambda.
- Criptografia em repouso (KMS) e em trânsito (TLS).

## Diagrama conceitual

```text
Região sa-east-1
│
├── AZ A:  EC2 Linux 1, EC2 Linux 2 ──> mount target A ──┐
├── AZ B:  EC2 Linux 3, EC2 Linux 4 ──> mount target B ──┼──> [ EFS: mesmo sistema de arquivos ]
└── AZ C:  EC2 Linux 5              ──> mount target C ──┘
```

Cada AZ tem um **mount target** (um endereço de rede na subnet) por onde as instâncias acessam o mesmo sistema de arquivos.

As instâncias precisam de conectividade e permissões adequadas para montar e acessar o sistema de arquivos. O Security Group do EFS normalmente precisa permitir o tráfego NFS necessário a partir das instâncias autorizadas — **porta TCP 2049**, de preferência tendo como origem o Security Group das instâncias.

## Classes de armazenamento

| Classe | Uso |
|---|---|
| **EFS Standard** | Arquivos acessados com frequência; dados replicados em várias AZs |
| **EFS Infrequent Access (IA)** | Arquivos acessados poucas vezes; armazenamento mais barato, com cobrança por acesso |
| **EFS Archive** | Arquivos raramente acessados (algumas vezes por ano); armazenamento ainda mais barato |
| **One Zone / One Zone-IA** | Dados em **uma única AZ**: mais barato, mas sem a resiliência multi-AZ (bom para dev/test ou dados recriáveis) |

## EFS Infrequent Access e Lifecycle Management

As classes de acesso infrequente podem reduzir custos de armazenamento para arquivos menos acessados. As políticas de ciclo de vida podem mover arquivos entre classes conforme as opções configuradas.

Exemplo de política: "mover para **IA** arquivos não acessados há **30 dias**; mover para **Archive** após **90 dias**; voltar para **Standard** no primeiro acesso". A movimentação é **transparente** para a aplicação: o caminho do arquivo não muda.

Os preços relativos dependem da região, classe e padrão de uso; não memorize uma proporção fixa de custo em relação ao EBS.

## Modos de desempenho e throughput (visão geral)

- **Throughput Elastic** (recomendado na maioria dos casos): escala automaticamente conforme a demanda.
- **Provisioned:** você define um throughput fixo, independentemente do tamanho.
- **Bursting:** o throughput cresce com a quantidade de dados armazenados.

## EBS x EFS x Instance Store

| | EBS | EFS | Instance Store |
|---|---|---|---|
| Tipo | Bloco | Arquivos (NFS) | Bloco (local) |
| Acesso | 1 instância (Multi-Attach em io1/io2) | Muitas instâncias, várias AZs | 1 instância |
| Escopo | 1 AZ | Região (ou 1 AZ no One Zone) | Host físico |
| SO | Linux e Windows | Linux | Linux e Windows |
| Capacidade | Provisionada | Elástica, paga pelo uso | Fixa pelo tipo de instância |
| Persistência | Sim | Sim | Efêmera |

## Resumo

EFS = sistema de arquivos compartilhado pela rede, útil quando várias instâncias precisam acessar os mesmos arquivos.

- **Linux + NFS (porta 2049)**, multi-AZ, paga-se pelo uso.
- Lifecycle move arquivos para **IA/Archive** e economiza custo.
- **Para a prova:** "várias instâncias Linux em AZs diferentes compartilhando arquivos" → **EFS**.
