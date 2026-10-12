# Escalabilidade e alta disponibilidade

## Escalabilidade

Escalabilidade é a capacidade de adaptar recursos para atender à demanda.

### Escalabilidade vertical

Aumentar ou reduzir os recursos de uma máquina, como CPU e memória. É conhecida como **scale up** ou **scale down**.

Analogia: ampliar um prédio existente.

- Na AWS: trocar o tipo da instância, por exemplo de `t2.micro` para `t2.large` (exige parar e iniciar a instância).
- Comum em sistemas **não distribuídos**, como bancos de dados (ex.: RDS, ElastiCache).
- Tem um **limite**: o tamanho máximo de hardware disponível.

### Escalabilidade horizontal

Adicionar ou remover máquinas/instâncias. É conhecida como **scale out** ou **scale in**.

Analogia: construir mais prédios para distribuir as pessoas.

- Na AWS: **Auto Scaling Group** + **Load Balancer**.
- Exige que a aplicação seja **distribuída** (sem guardar estado em uma única instância); é o padrão das aplicações web modernas.
- Praticamente sem limite e mais fácil na nuvem, pois instâncias são criadas em minutos.

### Analogia do call center

| Conceito | No call center |
|---|---|
| Vertical (scale up) | Trocar um atendente júnior por um sênior, que atende mais rápido |
| Horizontal (scale out) | Contratar mais atendentes quando o volume de ligações aumenta |
| Alta disponibilidade | Ter um segundo call center em outra cidade; se um ficar sem energia, o outro continua atendendo |

```text
Vertical:     [ EC2 pequena ]  ──>  [   EC2 GRANDE   ]

Horizontal:   [ EC2 ]  ──>  [ EC2 ] [ EC2 ] [ EC2 ]
```

## Elasticidade

Elasticidade é a capacidade de ajustar recursos à demanda, frequentemente de forma automática. Está relacionada à escalabilidade, mas enfatiza o ajuste conforme a necessidade.

## Alta disponibilidade

Alta disponibilidade busca manter o serviço funcionando mesmo quando há falhas. Uma arquitetura pode distribuir recursos por duas ou mais Availability Zones para reduzir o impacto de uma falha isolada.

Escalabilidade e alta disponibilidade são conceitos diferentes: adicionar instâncias ajuda a lidar com carga, mas não garante, por si só, tolerância a falhas.

Alta disponibilidade normalmente anda junto com escalabilidade horizontal, e o objetivo é **sobreviver à perda de um data center / AZ**.

- **Ativa-passiva:** uma instância recebe tráfego e outra fica de reserva (ex.: RDS Multi-AZ).
- **Ativa-ativa:** várias instâncias em AZs diferentes atendem ao mesmo tempo (ex.: ASG multi-AZ atrás de um Load Balancer).

```text
                 Load Balancer (multi-AZ)
                 /                      \
        AZ A: EC2  EC2            AZ B: EC2  EC2
```

Se a AZ A falhar, a AZ B continua atendendo.

## Conceitos relacionados

| Termo | Ideia |
|---|---|
| **Escalabilidade** | O sistema **consegue** lidar com mais carga, adaptando hardware (vertical) ou quantidade de nós (horizontal) |
| **Elasticidade** | Depois de escalável, ajustar **automaticamente** a capacidade à carga, pagando pelo uso e otimizando custo |
| **Agilidade** | Recursos de TI disponíveis em minutos, não semanas (não é sobre escalabilidade) |
| **Alta disponibilidade** | Continuar funcionando com a falha de componentes/AZs |
| **Tolerância a falhas** | Continuar funcionando **sem nenhuma interrupção** perceptível, mesmo com falha — exigência mais forte que alta disponibilidade |

## Como cada conceito aparece na AWS

| Conceito | Recurso AWS |
|---|---|
| Escalabilidade vertical | Mudar o tipo de instância |
| Escalabilidade horizontal | Auto Scaling Group + Load Balancer |
| Elasticidade | Políticas de scaling do ASG |
| Alta disponibilidade | ASG e Load Balancer em **várias AZs** |

## Exemplo

Uma aplicação recebe muito tráfego à tarde. Um Auto Scaling Group pode adicionar instâncias e um Load Balancer pode distribuir solicitações entre as instâncias saudáveis.

## Resumo

- Vertical: aumentar recursos da máquina.
- Horizontal: aumentar a quantidade de máquinas.
- Elasticidade: ajustar capacidade à demanda.
- Alta disponibilidade: reduzir interrupções por falhas.
- Agilidade: obter recursos rapidamente (não confundir com escalabilidade).
- **Para a prova:** "sobreviver à falha de uma AZ" → recursos em **múltiplas AZs**.
