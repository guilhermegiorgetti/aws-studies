# Elastic Load Balancing (ELB)

## O que é um Load Balancer?

Um **Load Balancer** distribui requisições recebidas entre vários destinos, como instâncias EC2. Isso pode melhorar a distribuição de carga e ajudar a evitar que uma única instância receba todo o tráfego.

Ele também pode verificar a saúde dos destinos e deixar de encaminhar tráfego para destinos considerados não saudáveis, conforme a configuração.

## Diagrama

```text
Usuário 1 ──┐                    ┌──> EC2 A
Usuário 2 ──┼──> Load Balancer ──┼──> EC2 B
Usuário 3 ──┘                    └──> EC2 C
```

## Por que usar um Load Balancer?

- **Distribuir a carga** entre várias instâncias.
- Oferecer **um único ponto de acesso** (um nome DNS) para a aplicação, mesmo que as instâncias mudem.
- **Lidar com falhas** de instâncias de forma transparente, por meio de **health checks**.
- Fazer **terminação SSL/TLS** (HTTPS) no Load Balancer, com certificados do AWS Certificate Manager.
- **Alta disponibilidade** entre várias AZs.
- Separar o tráfego público do tráfego privado.

## Por que um Load Balancer *gerenciado* (ELB)?

O **ELB** é um serviço gerenciado: a AWS garante que ele funcione, faz atualizações, manutenção e cuida da alta disponibilidade do próprio balanceador. Você só configura o comportamento. Montar um balanceador próprio em EC2 seria mais barato em teoria, mas daria muito mais trabalho para manter.

## Health checks

O Load Balancer verifica periodicamente cada destino, por exemplo fazendo uma requisição para `HTTP:80/health`. Se a resposta não for a esperada (ex.: status 200), o destino é marcado como **unhealthy** e deixa de receber tráfego até se recuperar.

```text
Load Balancer ──GET /health──> EC2 A  -> 200 OK      (healthy)
Load Balancer ──GET /health──> EC2 B  -> sem resposta (unhealthy: sai da rotação)
```

## Tipos principais de ELB

### Application Load Balancer (ALB)

Trabalha na camada de aplicação, sendo comum para HTTP e HTTPS. Pode encaminhar solicitações conforme regras como host e caminho.

- **Camada 7** (HTTP/HTTPS/gRPC).
- Roteamento por **caminho** (`/api` → um grupo, `/imagens` → outro), **host** (`api.site.com`), query string e cabeçalhos.
- Destinos: instâncias EC2, IPs, **funções Lambda** e contêineres.
- Nome DNS fixo (IP não é fixo).
- Ideal para **aplicações web e microsserviços**.

### Network Load Balancer (NLB)

Trabalha na camada de transporte e é adequado a cenários de alto desempenho com TCP, UDP e outros protocolos suportados conforme a configuração.

- **Camada 4** (TCP/UDP/TLS).
- Altíssimo desempenho: **milhões de requisições por segundo** com latência muito baixa.
- Oferece **IP estático por AZ** (é possível associar Elastic IPs).
- Ideal para jogos, IoT, aplicações que exigem IP fixo ou protocolos não-HTTP.

### Gateway Load Balancer (GWLB)

Ajuda a implantar, escalar e administrar appliances virtuais de rede, trabalhando no nível de rede apropriado para esse uso.

- **Camada 3** (pacotes IP), usa o protocolo **GENEVE** (porta 6081).
- Encaminha o tráfego por **appliances de terceiros** — firewalls, sistemas de detecção/prevenção de intrusão (IDS/IPS), inspeção profunda de pacotes — antes de chegar à aplicação.

```text
Usuários ──> GWLB ──> Firewalls/appliances (inspeção) ──> GWLB ──> Aplicação
```

### Classic Load Balancer (CLB)

É uma opção legada. Não é correto dizer simplesmente que “não existe mais”; pode aparecer em ambientes antigos, mas para novos projetos normalmente se avaliam os tipos modernos.

Funcionava nas camadas 4 e 7 ao mesmo tempo, com menos recursos. A AWS recomenda migrar para ALB ou NLB.

## Comparação rápida

| | ALB | NLB | GWLB |
|---|---|---|---|
| Camada OSI | 7 (aplicação) | 4 (transporte) | 3 (rede) |
| Protocolos | HTTP, HTTPS, gRPC | TCP, UDP, TLS | IP (GENEVE) |
| Destaque | Roteamento por caminho/host | Desempenho extremo, IP estático | Appliances de segurança |
| Palavra-chave na prova | "HTTP", "rotas", "microsserviços" | "alto desempenho", "milhões de req/s", "IP estático", "UDP" | "firewall de terceiros", "inspeção de tráfego" |

## Custos

O ELB é cobrado **por hora** de funcionamento e pela quantidade de tráfego/conexões processadas (unidades de capacidade). Ele cobra **mesmo sem tráfego** enquanto existir — exclua os Load Balancers de laboratório ao terminar.

## Resumo

ELB distribui tráfego e monitora a saúde dos destinos. ALB é comum para HTTP/HTTPS; NLB para tráfego de transporte de alto desempenho; GWLB para appliances de rede; CLB é legado.

- Um único ponto de acesso (DNS), health checks e alta disponibilidade multi-AZ.
- Serviço **gerenciado** pela AWS.
- ALB = L7, NLB = L4, GWLB = L3.
