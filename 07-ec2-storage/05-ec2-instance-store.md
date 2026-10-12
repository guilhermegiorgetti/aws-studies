# EC2 Instance Store

## O que é?

O **Instance Store** oferece armazenamento em discos físicos conectados ao host da instância. Ele só está disponível em determinados tipos de instância.

## Características

- Pode oferecer desempenho elevado e baixa latência.
- É adequado para dados temporários, cache e buffers que podem ser recriados.
- Os dados são efêmeros: podem ser perdidos quando a instância é parada, terminada ou sofre determinados eventos do ciclo de vida, conforme o tipo de instância e a situação.
- Não deve ser usado como única cópia de dados importantes.

## O que acontece com os dados?

| Evento | Dados do Instance Store |
|---|---|
| **Reboot** (reinício) | **Mantidos** |
| **Stop** (parar) | **Perdidos** |
| **Hibernate** | **Perdidos** |
| **Terminate** | **Perdidos** |
| Falha do hardware/host | **Perdidos** |

Por isso o curso chama o Instance Store de **armazenamento efêmero** (ephemeral storage).

## Outros pontos importantes

- O tamanho e a quantidade de discos são **definidos pelo tipo de instância** (ex.: `m5d`, `c6gd`, `i4i`; repare no `d` ou na família `I`). Não é possível aumentar o tamanho nem adicioná-lo depois do lançamento.
- Pode atingir **milhões de IOPS** em leitura aleatória — muito acima de um volume EBS típico.
- **Backup e replicação são responsabilidade do cliente** (Modelo de Responsabilidade Compartilhada).
- O custo está **incluído** no preço da instância.

## EBS versus Instance Store

| Característica | EBS | Instance Store |
|---|---|---|
| Tipo | Volume em bloco via infraestrutura de armazenamento da AWS | Disco local ligado ao host |
| Persistência | Normalmente persiste após parar/reiniciar a instância | Efêmero; não conte com persistência |
| Uso típico | Sistema, dados e arquivos persistentes | Cache, scratch, temporários |
| Disponibilidade | Conforme tipo de volume e instância | Apenas em tipos específicos |
| Conexão | Pela rede | Física, no próprio host |
| Desanexar/mover | Sim (mesma AZ) | Não |
| Snapshots | Sim | Não (backup manual) |
| Desempenho máximo | Alto | Muito alto (latência mínima) |
| Cobrança | Separada (GiB provisionado) | Incluída na instância |

> **Para a prova:** "precisa do **maior desempenho de I/O** possível em disco e os dados são temporários/replicados" → **Instance Store**. "Os dados precisam sobreviver ao stop/terminate" → **EBS**.

## IOPS

**IOPS (Input/Output Operations Per Second)** mede quantas operações de entrada/saída de armazenamento podem ocorrer por segundo. O desempenho real depende do tipo de armazenamento, tamanho, configuração, padrão de acesso e limites do serviço.

## Resumo

Instance Store = armazenamento local rápido para dados temporários. EBS = volume persistente para dados que precisam sobreviver ao ciclo de vida da instância.
