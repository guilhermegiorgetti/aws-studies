# EC2 Image Builder

## O que é?

O **EC2 Image Builder** automatiza a criação, personalização, teste e distribuição de imagens de máquinas, como AMIs.

## Como funciona conceitualmente?

```text
Receita / componentes
        |
        v
Instância de build
        |
        v
Imagem criada
        |
        v
Testes
        |
        v
Distribuição da imagem
```

Uma receita pode descrever atualizações do sistema operacional, instalação de pacotes, configurações e testes. O pipeline pode ser executado de acordo com um agendamento ou sob demanda.

## Peças principais

| Componente | O que define |
|---|---|
| **Image recipe** | Imagem de origem + componentes de build (o que instalar/configurar) e de teste |
| **Components** | Passos reutilizáveis (ex.: instalar Java, atualizar o SO, rodar testes). A AWS oferece componentes prontos |
| **Infrastructure configuration** | Tipo de instância, subnet, Security Group e IAM role usados no build |
| **Distribution settings** | Em quais **regiões** e **contas** a imagem será disponibilizada |
| **Image pipeline** | Junta tudo e define o agendamento (ex.: semanal, ou quando há atualização de pacotes) |

## Fluxo detalhado

```text
1. Image Builder lança uma instância de build (EC2 Builder)
2. Aplica os componentes (atualizações, pacotes, configurações)
3. Cria a AMI
4. Lança uma instância de teste a partir da nova AMI
5. Executa os testes (a aplicação sobe? os serviços respondem?)
6. Distribui a AMI para as regiões/contas configuradas
```

As instâncias temporárias de build e teste são encerradas automaticamente ao final.

## Custo

O serviço **não tem cobrança própria**: paga-se apenas pelos recursos usados (instâncias EC2 durante o build/teste e o armazenamento das AMIs/snapshots gerados).

## Outros recursos

- Também cria **imagens de contêiner** (Docker) além de AMIs.
- Pode integrar verificações de vulnerabilidade com o **Amazon Inspector**.
- Permite compartilhar imagens com outras contas por meio do **AWS RAM** ou das configurações de distribuição.

## Benefícios

- reduz etapas manuais repetitivas;
- ajuda a padronizar imagens;
- permite incorporar atualizações e configurações;
- favorece testes antes de distribuir uma imagem.

## Exemplo

Uma equipe quer manter uma AMI com atualizações de segurança e um conjunto padrão de pacotes. O Image Builder pode automatizar o processo de criação e teste da imagem.

## Resumo

EC2 Image Builder = automatização do processo de construir, testar e distribuir imagens de máquina.

- Serviço **gratuito**; paga-se pelos recursos usados.
- **Regional**, mas distribui imagens para várias regiões e contas.
- **Para a prova:** "automatizar a criação e manutenção de AMIs" → **EC2 Image Builder**.
