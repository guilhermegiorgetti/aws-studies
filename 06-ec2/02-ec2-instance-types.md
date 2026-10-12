# EC2 Instance Types

## Como ler o nome de um tipo

Exemplo: `m5.2xlarge`

- `m`: família/classe da instância.
- `5`: geração.
- `2xlarge`: tamanho, que influencia recursos como CPU e memória.

O significado exato dos recursos depende da família e do tamanho escolhidos.

### Sufixos que aparecem depois da geração

Exemplo: `m7g.large`, `c6i.xlarge`, `r6a.2xlarge`, `m5d.large`.

| Sufixo | Significado |
|---|---|
| `g` | Processador **AWS Graviton** (ARM) — geralmente melhor custo-benefício |
| `i` | Processador Intel |
| `a` | Processador AMD |
| `d` | Inclui disco local NVMe (**Instance Store**) |
| `n` | Rede com maior desempenho |
| `e` | Memória ou armazenamento extra |

### Tamanhos

Dentro de uma família, cada tamanho costuma **dobrar** os recursos do anterior:

```text
nano < micro < small < medium < large < xlarge < 2xlarge < 4xlarge < ... < metal
```

`metal` dá acesso direto ao servidor físico (bare metal).

Exemplo de comparação (valores aproximados; confira a documentação):

| Tipo | vCPU | Memória (GiB) |
|---|---|---|
| `t2.micro` | 1 | 1 |
| `t3.medium` | 2 | 4 |
| `m5.large` | 2 | 8 |
| `m5.2xlarge` | 8 | 32 |
| `c5.2xlarge` | 8 | 16 |
| `r5.2xlarge` | 8 | 64 |

Repare como, com a mesma quantidade de vCPU, a família **C** tem menos memória e a **R** tem mais.

## Famílias principais

### 1. General purpose

Equilibra CPU, memória e rede. Pode servir para servidores web, ambientes de desenvolvimento e aplicações de uso geral.

Exemplo de família: `T` e `M`. Instâncias T, como `t2.micro` ou gerações posteriores, são conhecidas por oferecer desempenho com créditos de CPU; disponibilidade e elegibilidade de nível gratuito dependem das condições atuais da AWS.

### 2. Compute optimized

Voltada a tarefas que exigem mais processamento, como processamento em lote, transcodificação de mídia, aplicações de alto desempenho e certas cargas de machine learning.

Exemplo de família: `C`.

### 3. Memory optimized

Indicada para aplicações que precisam de muita memória RAM, como bancos de dados em memória, caches e análise de conjuntos grandes de dados.

Exemplo de famílias: `R`, `X` e outras famílias especializadas, conforme a geração.

### 4. Storage optimized

Voltada a cargas com altas necessidades de acesso a dados e armazenamento local, como certos bancos de dados e processamento de grandes volumes de dados.

Exemplo de família: `I` e outras famílias específicas.

### 5. Accelerated computing

Usa aceleradores de hardware, como **GPUs** e chips dedicados, para machine learning, computação gráfica e processamento numérico.

Exemplos de famílias: `P` e `G` (GPUs), `Inf` (AWS Inferentia, inferência de ML) e `Trn` (AWS Trainium, treinamento de ML).

### 6. HPC optimized

Para computação de alto desempenho em larga escala (simulações, modelagem). Exemplo de família: `Hpc`.

A AWS possui mais famílias e variantes do que estas categorias. O catálogo muda ao longo do tempo; use a documentação atual para escolher uma instância real.

## Instâncias burstable (família T)

As instâncias T têm um **desempenho base** de CPU e acumulam **créditos de CPU** quando ficam abaixo desse nível. Quando a carga aumenta, gastam os créditos para "estourar" (burst) acima da base.

- Boas para cargas com uso baixo na maior parte do tempo e picos ocasionais (sites pequenos, ambientes de dev).
- Se os créditos acabarem, o desempenho cai para a base — ou, no modo **unlimited**, a instância continua acima da base com cobrança adicional.
- Para uso de CPU alto e constante, uma família M ou C costuma ser mais adequada.

## Dica para memorizar

| Família | Lembre como |
|---|---|
| **T** | *Turbo* / burstable |
| **M** | *Main* / uso geral equilibrado |
| **C** | *Compute* |
| **R** | *RAM* |
| **X** | *eXtra* memória |
| **I** | *I/O* (armazenamento local) |
| **P / G** | *GPU* / gráficos |

## Ferramentas para comparar

- **EC2 → Instance Types** no Console: filtra por vCPU, memória, arquitetura etc.
- Página **Amazon EC2 Instance Types** e a de **preços On-Demand** da AWS.
- **AWS Compute Optimizer:** recomenda tipos de instância com base no uso real (rightsizing).

## Outros fatores de configuração

Além da CPU e RAM, observe:
- capacidade e tipo de armazenamento;
- desempenho de rede;
- suporte a EBS;
- arquitetura do processador;
- sistema operacional e compatibilidade da aplicação;
- custo por hora e região.

## Resumo

Escolha a família conforme o gargalo da aplicação: equilíbrio geral, CPU, memória ou armazenamento. Depois escolha o tamanho conforme a carga e o orçamento.
