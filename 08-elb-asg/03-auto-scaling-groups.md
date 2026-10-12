# Auto Scaling Groups (ASG)

## O que é um ASG?

Um **Auto Scaling Group** administra um conjunto de instâncias EC2 e ajuda a manter a capacidade desejada da aplicação.

O grupo pode definir:
- **Minimum capacity:** quantidade mínima;
- **Desired capacity:** quantidade desejada;
- **Maximum capacity:** quantidade máxima.

```text
 mínimo           desejado                    máximo
   │                 │                          │
   ▼                 ▼                          ▼
[EC2] [EC2]    [EC2] [EC2]    ...    [EC2] [EC2] [EC2] [EC2]
                     ◄── scale in ── │ ── scale out ──►
```

A regra é sempre: **mínimo ≤ desejado ≤ máximo**.

## Exemplo

Um ASG tem:
- mínimo: 1 instância;
- desejado: 2 instâncias;
- máximo: 4 instâncias.

Se uma das duas instâncias em execução for encerrada manualmente, o ASG normalmente tentará iniciar outra para voltar à capacidade desejada, desde que existam capacidade, permissões e configuração adequadas.

## Launch Template

Para criar novas instâncias, o ASG usa um **Launch Template**, que descreve como cada instância deve ser:

- AMI;
- tipo de instância;
- key pair;
- Security Groups;
- volumes EBS;
- IAM role (instance profile);
- User Data;
- configurações de rede.

Além do template, o ASG define as **subnets/AZs** onde as instâncias serão criadas. Escolher subnets em **várias AZs** dá alta disponibilidade.

> Os antigos *Launch Configurations* são legados; a AWS recomenda Launch Templates.

## Por que usar?

- substituir instâncias que deixam de funcionar;
- ajustar capacidade à demanda;
- manter um número mínimo de instâncias;
- trabalhar com Load Balancers para distribuir tráfego.

Funções do ASG, resumidas:

- **Scale out:** adicionar instâncias quando a carga aumenta.
- **Scale in:** remover instâncias quando a carga diminui.
- **Garantir** um número mínimo e máximo de instâncias em execução.
- **Registrar automaticamente** as novas instâncias no Load Balancer.
- **Substituir** instâncias com problema (self-healing).
- **Economizar custos**: só roda o necessário.

## Health checks no ASG

| Tipo | O que verifica |
|---|---|
| **EC2** (padrão) | O status da instância no EC2 (hardware/sistema) |
| **ELB** | O health check do Load Balancer — detecta também quando a **aplicação** parou de responder |

Ao ativar o health check do ELB, uma instância cuja aplicação deixou de responder é marcada como **unhealthy**, **terminada** e **substituída** automaticamente.

## Relação com ELB

O ELB distribui as requisições. O ASG ajusta a quantidade de instâncias. Eles podem trabalhar juntos, mas fazem funções diferentes.

```text
Usuários ──> Load Balancer ──┬──> AZ A: EC2
                             ├──> AZ B: EC2
                             ├──> AZ C: EC2
                             └──> (+ EC2 criada pelo ASG se a carga subir)

Auto Scaling Group = conjunto dessas instâncias, distribuídas nas AZs A, B e C
```

## Custo

O Auto Scaling **não tem cobrança própria**: paga-se apenas pelos recursos criados (instâncias EC2, volumes EBS, Load Balancer, alarmes do CloudWatch).

## Resumo

ASG = gerencia o número de instâncias EC2 conforme capacidade mínima, desejada, máxima e políticas de scaling.

- Usa um **Launch Template** para criar instâncias.
- Distribui instâncias em **várias AZs** para alta disponibilidade.
- Substitui instâncias com problema e se registra no **ELB** automaticamente.
- Serviço gratuito; paga-se pelos recursos.
