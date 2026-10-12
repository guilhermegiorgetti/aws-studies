# EC2 Purchasing Options

A AWS oferece diferentes formas de pagar pela capacidade EC2. A escolha depende da duração, previsibilidade e flexibilidade da carga de trabalho.

> Os percentuais de desconto abaixo são os valores "**até**" divulgados pela AWS em comparação com On-Demand. Servem como referência para a prova; o desconto real depende do tipo, região, prazo e forma de pagamento.

## 1. On-Demand

Paga-se pelo uso sem compromisso de longo prazo. É útil para cargas imprevisíveis, testes e aplicações em que a flexibilidade é importante.

- Linux e Windows: cobrança **por segundo**, após o primeiro minuto. Outros SOs: por hora.
- Maior custo por hora, mas **sem pagamento antecipado** e sem compromisso.
- Indicado para cargas **curtas e sem interrupção**, cujo comportamento não se consegue prever.

## 2. Reserved Instances

Compromisso normalmente de 1 ou 3 anos, com desconto em troca de compromisso. É apropriado para uso estável e previsível. A reserva é um modelo de desconto/capacidade conforme as condições, não simplesmente uma instância física exclusiva.

- Desconto de **até ~72%**.
- Você reserva atributos: **tipo de instância, região, tenancy e SO**.
- Prazo: **1 ano** (menos desconto) ou **3 anos** (mais desconto).
- Pagamento: **No Upfront**, **Partial Upfront** ou **All Upfront** (quanto mais antecipado, maior o desconto).
- Escopo: **regional** ou **zonal** (zonal também reserva capacidade na AZ).
- Podem ser revendidas no **Reserved Instance Marketplace**.
- **Convertible Reserved Instances:** permitem trocar tipo, família, SO e tenancy; desconto menor (**até ~66%**).
- Exemplo clássico: banco de dados que roda 24/7 por anos.

## 3. Savings Plans

Compromisso de gasto por hora durante 1 ou 3 anos em troca de preços reduzidos em uso elegível. A flexibilidade depende do tipo de Savings Plan.

- Você se compromete com um valor, por exemplo **US$ 10/hora por 1 ou 3 anos**.
- O uso acima do compromisso é cobrado como On-Demand.
- **EC2 Instance Savings Plans:** presos a uma família de instância em uma região (ex.: M5 em `us-east-1`), mas flexíveis em tamanho, SO e tenancy; desconto de **até ~72%**.
- **Compute Savings Plans:** mais flexíveis — valem para qualquer família, região, **AWS Fargate** e **AWS Lambda**; desconto de **até ~66%**.
- Hoje a AWS recomenda Savings Plans como alternativa mais flexível às Reserved Instances.

## 4. Spot Instances

Usa capacidade EC2 disponível com grande desconto, mas a AWS pode interromper a instância. Indicado para cargas tolerantes a interrupções, como processamento em lote e tarefas distribuídas.

- Desconto de **até ~90%**: a opção mais barata.
- A AWS avisa com **2 minutos de antecedência** antes de interromper.
- Bons usos: processamento em lote, análise de dados, renderização de imagens/vídeo, CI/CD, cargas distribuídas, workers com checkpoint.
- **Não** indicado para bancos de dados ou cargas críticas que não podem parar.

## 5. Dedicated Hosts

Servidor físico dedicado ao uso de um cliente, útil em cenários que exigem controle de posicionamento de instâncias ou licenciamento específico.

- Permite usar licenças existentes por soquete/núcleo/VM (**BYOL — Bring Your Own License**), por exemplo Windows Server, SQL Server, Oracle.
- Atende requisitos de **compliance** que exigem servidor dedicado.
- Pagamento On-Demand (por segundo) ou com reserva de 1 ou 3 anos.
- É a **opção mais cara**.

## 6. Dedicated Instances

Instâncias executadas em hardware dedicado a um único cliente, com diferenças de controle e visibilidade em relação a Dedicated Hosts.

- O hardware não é compartilhado com outros clientes, mas pode ser compartilhado com outras instâncias **da mesma conta**.
- Você **não** controla o posicionamento das instâncias no host físico.

| | Dedicated Instance | Dedicated Host |
|---|---|---|
| Hardware exclusivo da conta | Sim | Sim |
| Visibilidade de soquetes/núcleos | Não | Sim |
| Controle do posicionamento | Não | Sim |
| BYOL por soquete/núcleo | Não | Sim |
| Cobrança | Por instância | Pelo host inteiro |

## 7. Capacity Reservations

Reserva capacidade EC2 para uma zona de disponibilidade específica, conforme os parâmetros escolhidos. Não é, por si só, um modelo de desconto equivalente a Savings Plans.

- **Sem compromisso de prazo:** cria e cancela quando quiser.
- Cobrado pela **tarifa On-Demand**, **usando ou não** as instâncias.
- Pode ser combinado com Regional Reserved Instances ou Savings Plans para obter desconto.
- Indicado para cargas curtas que **precisam** de capacidade garantida em uma AZ (ex.: evento importante, recuperação de desastres).

## Analogia: um resort

| Opção | No resort |
|---|---|
| On-Demand | Chegar e ficar quando quiser, pagando a diária cheia |
| Reserved | Reservar com antecedência por um longo período e ganhar desconto |
| Savings Plans | Comprometer-se a gastar um valor por hora no resort e escolher o quarto depois |
| Spot | Pegar um quarto vago com grande desconto, mas ser retirado se alguém pagar mais |
| Dedicated Host | Alugar um prédio inteiro do resort |
| Capacity Reservation | Reservar um quarto pagando diária cheia, mesmo sem aparecer |

## Comparação rápida

| Opção | Ideia central | Principal consideração |
|---|---|---|
| On-Demand | Pagar conforme uso | Flexibilidade, sem compromisso longo |
| Reserved Instances | Compromisso de prazo | Uso previsível |
| Savings Plans | Compromisso de gasto | Desconto em uso elegível |
| Spot | Capacidade com desconto | Pode ser interrompida |
| Dedicated Host | Servidor físico dedicado | Controle/licenciamento |
| Dedicated Instance | Hardware dedicado | Isolamento de hardware |
| Capacity Reservation | Reservar capacidade | Garantir capacidade na AZ escolhida |

## Qual escolher? (perguntas típicas de prova)

| Cenário | Resposta |
|---|---|
| Teste curto, carga imprevisível, não pode ser interrompida | **On-Demand** |
| Aplicação estável 24/7 por 3 anos, mesmo tipo de instância | **Reserved Instances** |
| Uso estável, mas a equipe quer mudar família/região ou usar Lambda/Fargate | **Compute Savings Plans** |
| Processamento em lote que tolera interrupções, menor custo possível | **Spot** |
| Licença de software por núcleo/soquete ou exigência regulatória | **Dedicated Host** |
| Garantir capacidade em uma AZ por um período curto | **Capacity Reservation** |

## Ordem aproximada de custo

```text
Mais barato  Spot < Reserved / Savings Plans < On-Demand < Dedicated Instance < Dedicated Host  Mais caro
```

Preços, condições e nomes de opções podem mudar; consulte a tabela oficial antes de tomar uma decisão de compra.
