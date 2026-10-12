# EC2 Hands-on

Esta pasta reúne roteiros para praticar os conceitos no Console AWS. Faça os exercícios em ambiente de estudo, verifique os custos e exclua os recursos ao terminar.

- [01 — Criar e revisar uma instância EC2](01-create-and-review-ec2.md)
- [02 — Security Groups](02-security-groups.md)
- [03 — Conectar por SSH ou Instance Connect](03-connect-to-ec2.md)

## Antes de começar

- [ ] Região selecionada no Console (ex.: `sa-east-1`) — os recursos de EC2 são regionais.
- [ ] Alerta de orçamento configurado ([AWS Budgets](../../05-billing-cost-management/01-aws-budgets.md)).
- [ ] Usuário IAM com permissões de EC2 (evite usar o root).
- [ ] Saber o seu IP público, para restringir a regra de SSH.

## Limpeza ao final

- [ ] Instâncias **terminadas** (não apenas paradas).
- [ ] Volumes EBS em estado `available` excluídos.
- [ ] Elastic IPs liberados (**Release**).
- [ ] Security Groups e key pairs de teste excluídos, se não forem reutilizados.
- [ ] Roles IAM de teste removidas.
- [ ] Conferência em outras regiões, caso tenha trocado de região durante o exercício.
