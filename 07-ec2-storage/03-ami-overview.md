# AMI Overview

## O que é uma AMI?

**AMI (Amazon Machine Image)** é uma imagem-base usada para lançar instâncias EC2. Ela pode incluir um sistema operacional e configurações e softwares preparados para a aplicação.

Uma AMI contém, entre outros:

- um ou mais **snapshots EBS** (ou um template para o volume raiz, em AMIs instance-store);
- **permissões de lançamento** (quais contas podem usá-la);
- o **mapeamento de dispositivos de bloco** (quais volumes anexar e com que tamanho).

## Por que usar?

Uma AMI ajuda a criar instâncias com uma base consistente, reduzindo a repetição de configuração manual.

- **Boot mais rápido:** o software já vem instalado, em vez de ser instalado por User Data a cada lançamento.
- **Padronização:** todas as instâncias partem da mesma configuração ("golden AMI").
- **Escalabilidade:** Auto Scaling Groups usam AMIs para lançar novas instâncias rapidamente.

## Tipos de AMI

| Tipo | Origem | Exemplo |
|---|---|---|
| **Pública (AWS)** | Fornecida pela AWS | Amazon Linux 2023, Windows Server |
| **Própria** | Criada e mantida por você | AMI com sua aplicação pré-instalada |
| **AWS Marketplace** | Criada por terceiros, às vezes com cobrança de licença | Appliance de firewall, softwares comerciais |
| **Community** | Compartilhada publicamente por outros usuários | Use com cuidado: verifique a origem |

## Exemplo

1. Você prepara uma instância com o sistema operacional e os pacotes necessários.
2. Cria uma AMI a partir dessa configuração, seguindo o processo suportado.
3. Usa a AMI para iniciar outras instâncias com uma base semelhante.

```text
EC2 (personalizada) ──Create image──> AMI ──Launch──> EC2 nova (AZ A)
                                       │
                                       └──Launch──> EC2 nova (AZ B)
                                       │
                                       └──Copy AMI──> AMI em outra região
```

Para garantir integridade dos dados, é recomendado **parar a instância** antes de criar a AMI. Por padrão, a AWS reinicia a instância durante a criação para obter uma imagem consistente (opção *No reboot* desativada).

## AMI e Availability Zones

Uma AMI pode ser usada para iniciar instâncias em locais compatíveis; a disponibilidade e o compartilhamento entre regiões dependem da AMI e de sua configuração. Copiar uma AMI para outra região pode ser necessário para disponibilizá-la lá.

Em resumo: a AMI é um **recurso regional** — pode lançar instâncias em qualquer AZ da sua região, e é preciso **copiá-la** para usar em outra região.

## Custos e limpeza

A AMI em si não tem cobrança, mas os **snapshots EBS** associados sim. Para remover completamente:

1. **Deregister** (cancelar o registro) da AMI.
2. Excluir os **snapshots** associados.

Excluir só um dos dois deixa sobras: AMI sem snapshot não funciona; snapshot sem AMI continua sendo cobrado.

## Resumo

AMI = molde para lançar instâncias EC2 com uma base de sistema e software preparada.

- Recurso **regional**; copie para usar em outra região.
- Pública, própria, Marketplace ou community.
- Cobrança pelos snapshots; limpe com **deregister + delete snapshots**.
