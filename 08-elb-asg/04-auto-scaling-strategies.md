# Auto Scaling Groups: estratégias de scaling

## 1. Manual Scaling

O operador altera manualmente a capacidade desejada do grupo. É simples, mas exige intervenção humana.

Exemplo: mudar a capacidade desejada de 1 para 2 instâncias no Console.

## 2. Dynamic Scaling

Responde a mudanças observadas na demanda, por exemplo usando métricas e alarmes.

Estratégias comuns:

- **Simple/Step Scaling:** ajusta capacidade conforme alarmes e limites configurados. Step Scaling permite diferentes ajustes conforme a intensidade da métrica.
- **Target Tracking Scaling:** tenta manter uma métrica em torno de um valor-alvo, como utilização média de CPU.
- **Scheduled Scaling:** agenda alterações para horários conhecidos. É útil quando a demanda segue um padrão previsível, mas depende de uma programação definida.

> Na documentação da AWS, o **Scheduled Scaling** aparece como uma categoria própria, separada do Dynamic Scaling. Alguns cursos o agrupam junto das estratégias dinâmicas, como acima — o conceito é o mesmo.

### Exemplos

**Simple / Step Scaling** (baseado em alarme do CloudWatch):

```text
Quando a CPU média > 70%  ->  adicionar 2 instâncias
Quando a CPU média < 30%  ->  remover 1 instância
```

Com Step Scaling, o ajuste varia conforme o tamanho do desvio:

```text
CPU 70%–85%  ->  +1 instância
CPU > 85%    ->  +3 instâncias
```

**Target Tracking** (o mais simples de configurar):

```text
Manter a CPU média do ASG em torno de 40%
```

O próprio ASG cria e gerencia os alarmes necessários. Outras métricas comuns: requisições por destino no ALB e tráfego de rede.

**Scheduled Scaling:**

```text
Toda sexta-feira às 17h  ->  capacidade mínima = 10
Toda sexta-feira às 23h  ->  capacidade mínima = 2
```

## 3. Predictive Scaling

Usa análise preditiva e aprendizado de máquina da AWS para antecipar a demanda e planejar capacidade antes de aumentos esperados, conforme os dados e a configuração disponíveis.

```text
Histórico de carga (dias/semanas)  ->  previsão para as próximas horas  ->  agenda ações de scaling antecipadamente
```

Funciona melhor para cargas com **padrões cíclicos** (ex.: mais acesso em dias úteis e horário comercial). Pode ser usado em conjunto com o Dynamic Scaling.

## Conceitos complementares

- **Cooldown / warm-up:** depois de uma ação de scaling, o ASG aguarda um período antes de agir novamente, para que as novas instâncias inicializem e as métricas se estabilizem. Usar uma AMI com software pré-instalado reduz esse tempo.
- **Scale out rápido, scale in conservador:** é comum adicionar capacidade rapidamente e remover com mais cautela, para evitar oscilações.

## Como escolher?

| Estratégia | Quando faz sentido |
|---|---|
| Manual | Laboratórios ou mudanças pontuais |
| Dynamic | Demanda muda conforme métricas atuais |
| Target tracking | Manter uma métrica perto de um alvo |
| Step scaling | Ajustes diferentes conforme a intensidade do alarme |
| Scheduled | Picos conhecidos em horários específicos |
| Predictive | Padrões recorrentes que podem ser previstos |

## Perguntas típicas de prova

| Cenário | Estratégia |
|---|---|
| "Manter a CPU média em 50%" | **Target Tracking** |
| "Promoção começa toda segunda às 9h" | **Scheduled** |
| "Prever a demanda com base no histórico e escalar antes" | **Predictive** |
| "Adicionar instâncias quando um alarme do CloudWatch disparar" | **Simple/Step** |

## Resumo

- Manual: você muda a capacidade.
- Dynamic: reage às métricas.
- Scheduled: segue horários definidos.
- Predictive: antecipa demanda com previsões.
- Target tracking: mantém uma métrica próxima de um alvo.
