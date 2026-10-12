# AWS Budgets

## O que é AWS Budgets?

O **AWS Budgets** permite definir orçamentos e acompanhar o gasto da conta AWS. É possível configurar alertas para ser avisado quando o custo ou a previsão de custo atingir limites definidos.

> **Importante:** um orçamento com alerta não interrompe automaticamente os serviços nem limita obrigatoriamente o gasto. Para ações automáticas, são necessárias configurações adicionais compatíveis com o caso.

## Onde encontrar

No Console AWS, abra **Billing and Cost Management** e procure **Budgets**. O nome exato de alguns menus pode mudar com a interface.

## Criar um orçamento simples

1. Abra **Budgets**.
2. Escolha criar um orçamento.
3. Selecione um modelo simplificado, se disponível.
4. Para acompanhar custos, escolha um orçamento de custo.
5. Defina o período, por exemplo mensal.
6. Configure o limite de custo.
7. Informe os e-mails que devem receber os alertas.
8. Revise e crie o orçamento.

## Tipos mencionados na aula

- **Zero spend budget:** alerta quando há gasto acima do limite muito baixo configurado pelo modelo, destinado a perceber gastos inesperados cedo.
- **Monthly cost budget:** acompanha custos mensais.
- **Daily budget:** se disponível no fluxo ou ferramenta usada, acompanha gastos por dia; confirme as opções apresentadas no Console, pois modelos e recursos podem variar.

Um limite de US$ 0,01 pode servir como exemplo de alerta de gasto mínimo, mas não significa que todo uso será bloqueado ao atingir esse valor.

## Tipos de orçamento

| Tipo | O que acompanha | Exemplo |
|---|---|---|
| **Cost budget** | Valor gasto (US$) | "Avise se passar de US$ 10 no mês" |
| **Usage budget** | Quantidade de uso | "Avise se passar de 750 horas de EC2" |
| **Savings Plans budget** | Utilização e cobertura dos Savings Plans | "Avise se a utilização cair abaixo de 80%" |
| **Reservation budget** | Utilização e cobertura de Reserved Instances | "Avise se a cobertura de RIs cair" |

Os orçamentos podem ser **filtrados** por serviço, região, conta vinculada, tag, tipo de instância etc.

## Alertas: real x previsto

- **Actual (real):** dispara quando o gasto real ultrapassa o limite (ex.: 80% do orçamento).
- **Forecasted (previsto):** dispara quando a **previsão** de gasto até o fim do período indica que o limite será ultrapassado — útil para agir antes de estourar.

As notificações podem ser enviadas por **e-mail** ou por um tópico **Amazon SNS** (que pode integrar com Slack/Chatbot, por exemplo). Os dados de custo são atualizados algumas vezes por dia, então há atraso.

## Budget Actions

O AWS Budgets pode executar **ações** quando um limite é atingido, por exemplo:

- aplicar uma IAM policy que restringe a criação de recursos;
- aplicar uma SCP (Service Control Policy) em contas de uma Organization;
- parar instâncias EC2 ou RDS específicas.

As ações podem ser automáticas ou exigir aprovação. Mesmo assim, não "zeram" o gasto: recursos como armazenamento continuam sendo cobrados.

## Custo do Budgets

Há uma quantidade de orçamentos sem custo e cobrança para orçamentos adicionais ou com ações. Os valores mudam; confira a página de preços do AWS Budgets.

## Ferramentas relacionadas

| Ferramenta | Para que serve |
|---|---|
| **AWS Cost Explorer** | Visualizar, filtrar e prever custos históricos |
| **AWS Free Tier alerts** | Avisos quando o uso se aproxima dos limites do nível gratuito |
| **Billing alarms (CloudWatch)** | Alarme sobre a métrica de cobrança estimada (métrica disponível em `us-east-1`) |
| **Cost Anomaly Detection** | Detecta gastos fora do padrão usando machine learning |
| **Cost and Usage Report (CUR)** | Relatório mais detalhado de custos e uso |

> **Para a prova:** "alertar quando o custo ultrapassar/for previsto ultrapassar um valor" → **AWS Budgets**. "Analisar e visualizar gastos passados" → **Cost Explorer**.

## Quem pode acessar?

O acesso depende das permissões IAM e das configurações de acesso ao faturamento da conta. Nem todo usuário IAM tem acesso ao Billing por padrão. Use uma identidade autorizada; não é necessário usar root se as permissões adequadas estiverem concedidas.

## Boas práticas para laboratórios

- Crie alertas antes de começar a testar serviços pagos.
- Use um endereço de e-mail que você verifica.
- Confira a região e os recursos criados.
- Apague recursos de laboratório quando terminar.
- Consulte o painel de custos; alertas podem ter atraso e não substituem a conferência do faturamento.

## Resumo

**AWS Budgets = definir um orçamento e receber alertas sobre custos ou previsões de custo.** É uma ferramenta de monitoramento, não um bloqueio universal de gastos.
