# Amazon EBS Overview

## O que é EBS?

**Amazon EBS (Elastic Block Store)** oferece volumes de armazenamento em bloco para uso com EC2. Pense nele como um disco virtual conectado à instância.

Os dados em um volume EBS persistem quando a instância é reiniciada ou parada, ao contrário do armazenamento efêmero de algumas opções de Instance Store. A retenção do volume ao terminar a instância depende das configurações e políticas de exclusão.

## Para que serve?

- sistema operacional e arquivos;
- dados da aplicação;
- logs e configurações;
- bancos de dados que usam armazenamento em bloco.

## Características importantes

- Os volumes EBS são recursos de uma Availability Zone.
- Um volume normalmente é anexado a uma instância de cada vez, mas há exceções: o recurso Multi-Attach é suportado apenas por determinados tipos de volume e exige configuração compatível.
- Volumes e snapshots podem gerar custos mesmo quando não estão anexados a uma instância.
- É possível criar snapshots para backup e recuperação.

## Analogia

EBS é como um disco virtual separado da máquina. A instância pode ser substituída e o volume pode, conforme a configuração e as regras do serviço, ser anexado a outra instância compatível na mesma Availability Zone.

## Diagrama conceitual

```text
Availability Zone A
  EC2 A  <---->  EBS Volume A (10 GiB)
  EC2 B  <---->  EBS Volume B (100 GiB)

Availability Zone B
  EC2 C  <---->  EBS Volume C (50 GiB)
```

Volumes EBS não são anexados diretamente entre Availability Zones. Para levar dados a outra zona, uma abordagem comum é criar um snapshot e restaurar um volume na zona de destino.

## EBS é um disco de rede

O EBS **não** é um disco físico dentro do servidor: a comunicação passa pela rede da AWS. Por isso:

- pode haver um pouco mais de latência do que um disco local (Instance Store);
- o volume pode ser **desanexado** de uma instância e **anexado** a outra rapidamente (mesma AZ);
- uma instância pode ter **vários** volumes EBS.

Analogia do curso: é como um **"pendrive de rede"** que você pluga em uma instância de cada vez.

## Capacidade provisionada

Você escolhe o **tamanho (GiB)** e, em alguns tipos, o **desempenho (IOPS e throughput)** ao criar o volume. A cobrança é sobre o que foi **provisionado**, não sobre o que está usado. O tamanho pode ser aumentado depois (mas não reduzido).

## Delete on Termination

| Volume | Comportamento padrão ao terminar a instância |
|---|---|
| **Volume raiz** (root) | **Excluído** (`Delete on Termination` = ativado) |
| **Volumes adicionais** | **Mantidos** (`Delete on Termination` = desativado) |

O atributo pode ser alterado na criação da instância (ou depois, pela CLI). Desative-o no volume raiz quando quiser preservar os dados ao terminar a instância.

## Tipos de volume

| Tipo | Categoria | Uso típico | Pode ser volume de boot? |
|---|---|---|---|
| **gp3** | SSD de uso geral | Padrão atual; boot, apps, dev/test. IOPS e throughput configuráveis independentemente do tamanho | Sim |
| **gp2** | SSD de uso geral (geração anterior) | IOPS crescem com o tamanho do volume | Sim |
| **io2 Block Express / io1** | SSD de IOPS provisionadas | Bancos de dados críticos com IOPS altas e consistentes; suportam **Multi-Attach** | Sim |
| **st1** | HDD otimizado para throughput | Big data, data warehouse, logs (acesso sequencial) | **Não** |
| **sc1** | HDD "cold" | Dados pouco acessados, menor custo | **Não** |

> **Para a prova:** volumes de boot só podem ser **SSD** (gp2/gp3/io1/io2). HDD (st1/sc1) não serve como volume raiz.

### Multi-Attach

Volumes **io1/io2** podem ser anexados a **várias instâncias na mesma AZ** ao mesmo tempo (até 16 instâncias Nitro). A aplicação precisa saber lidar com gravações concorrentes (sistema de arquivos com suporte a cluster).

## Criptografia

Volumes EBS podem ser criptografados com chaves do **AWS KMS**, com impacto mínimo de desempenho. Quando um volume é criptografado:

- os dados em repouso, os dados em trânsito entre instância e volume, os snapshots e os volumes criados a partir deles ficam criptografados;
- é possível habilitar **criptografia por padrão** na região (configuração da conta no EC2).

## EBS e o Modelo de Responsabilidade Compartilhada

- **AWS:** replicação do volume dentro da AZ, substituição de hardware com falha.
- **Cliente:** fazer snapshots/backups, habilitar criptografia, controlar quem pode acessar os dados.

## Resumo

EBS = armazenamento em bloco persistente para EC2. A zona de disponibilidade importa, e snapshots ajudam no backup e na recuperação.

- Disco **de rede**, preso a **uma AZ**.
- Paga-se pela capacidade **provisionada**.
- Volume raiz é excluído ao terminar a instância (por padrão); volumes extras, não.
- Boot: só SSD (gp2/gp3/io1/io2). Multi-Attach: io1/io2.
