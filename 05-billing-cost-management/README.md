# Billing & Cost Management

Esta pasta reúne anotações sobre controle de custos e orçamento na AWS.

## Conteúdos

- [01 — AWS Budgets](01-aws-budgets.md)

**Regra prática:** configure alertas de orçamento antes de criar recursos de laboratório e confira os custos reais no Billing & Cost Management. Um orçamento e seus alertas ajudam a monitorar gastos, mas não são necessariamente um bloqueio automático de consumo.

## Mapa mental

```text
Billing & Cost Management
│
├── Planejar ........ Pricing Calculator
├── Monitorar ....... Budgets, Free Tier alerts, Billing alarms
├── Analisar ........ Cost Explorer, Cost and Usage Report
├── Detectar ........ Cost Anomaly Detection
└── Organizar ....... Tags de alocação de custo, AWS Organizations
```

## Checklist de uma conta de estudos

- [ ] MFA ativo na conta root.
- [ ] Acesso de usuários IAM ao Billing habilitado (se for usar um usuário IAM).
- [ ] **Zero spend budget** ou orçamento mensal baixo criado.
- [ ] Alertas do Free Tier ativados nas preferências de faturamento.
- [ ] Revisão semanal do painel **Bills** e do **Cost Explorer**.
- [ ] Recursos de laboratório excluídos ao final de cada exercício (instâncias, volumes, snapshots, Elastic IPs, Load Balancers).

## Pegadinhas comuns de custo

- Parar uma instância EC2 não para a cobrança dos **volumes EBS**.
- **Endereços IPv4 públicos** e Elastic IPs têm cobrança por hora.
- **Snapshots** e **AMIs** continuam ocupando armazenamento até serem excluídos.
- **Load Balancers** e **NAT Gateways** cobram por hora mesmo sem tráfego.
- Recursos podem estar em **outra região** — confira o seletor de região ou use o Billing para descobrir onde está o gasto.
