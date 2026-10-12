# Elastic Load Balancing e Auto Scaling Groups

Esta pasta reúne escalabilidade, alta disponibilidade, balanceamento de carga e ajuste automático de capacidade.

## Conteúdos

- [01 — Escalabilidade e alta disponibilidade](01-scalability-and-high-availability.md)
- [02 — Elastic Load Balancing](02-elastic-load-balancing.md)
- [03 — Auto Scaling Groups](03-auto-scaling-groups.md)
- [04 — Estratégias de scaling](04-auto-scaling-strategies.md)

## Arquitetura típica

```text
                         Usuários
                            │
                            ▼
              Application Load Balancer (multi-AZ)
                 │                         │
   ┌─────────────┼──── Auto Scaling Group ─┼─────────────┐
   │   AZ A      ▼                 AZ B    ▼             │
   │        [EC2] [EC2]               [EC2] [EC2]        │
   └─────────────────────────────────────────────────────┘
          ▲
          │ instâncias criadas a partir de um Launch Template (AMI + User Data)
```

- **ELB:** distribui o tráfego e verifica a saúde.
- **ASG:** ajusta a quantidade e substitui instâncias com problema.
- **Múltiplas AZs:** alta disponibilidade.

## O que mais cai na prova

- Vertical = **scale up/down** (tipo de instância maior); horizontal = **scale out/in** (mais instâncias).
- **Elasticidade** = ajuste automático à demanda; **agilidade** = obter recursos rápido.
- **ALB** = L7/HTTP; **NLB** = L4/alto desempenho/IP estático; **GWLB** = L3/appliances de segurança; **CLB** = legado.
- ASG: **mínimo ≤ desejado ≤ máximo**; usa **Launch Template**; ELB e ASG são cobrados apenas pelos recursos usados (o ELB cobra por hora).
- Estratégias: **Manual, Dynamic (Simple/Step, Target Tracking), Scheduled, Predictive**.

## Limpeza após laboratórios

- [ ] Excluir o **Auto Scaling Group** primeiro (ele termina as instâncias; se você terminar só as instâncias, o ASG cria outras).
- [ ] Excluir o **Load Balancer** e os **Target Groups**.
- [ ] Excluir o **Launch Template**, se não for reutilizado.
