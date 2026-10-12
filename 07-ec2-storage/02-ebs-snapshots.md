# EBS Snapshots

## O que é um snapshot?

Um **EBS Snapshot** é uma cópia de backup de um volume EBS em um ponto no tempo. Ele permite criar volumes a partir do backup e apoiar estratégias de recuperação.

Em cargas com gravações ativas, planeje o backup para garantir consistência da aplicação. A AWS permite criar snapshots sem desanexar o volume, mas uma preparação da aplicação ou pausa de gravações pode ser necessária para consistência específica.

## Fluxo de recuperação

```text
EC2 + EBS (zona A)
       |
       v
   Snapshot
       |
       v
Restaurar volume EBS (zona B)
       |
       v
   EC2 de destino
```

Um snapshot pode ser usado para criar um volume em outra Availability Zone e, conforme os procedimentos suportados, copiar dados entre regiões.

## Como os snapshots funcionam

- Ficam armazenados de forma redundante pela AWS (no Amazon S3, gerenciado pelo serviço — você não vê os snapshots no seu bucket).
- São **incrementais**: o primeiro copia os blocos usados do volume; os seguintes guardam apenas os **blocos alterados** desde o anterior. Isso economiza armazenamento e tempo.
- Mesmo sendo incrementais, **cada snapshot pode restaurar o volume completo**. Excluir um snapshot antigo não quebra os mais novos: a AWS mantém os blocos ainda referenciados.
- Não é obrigatório desanexar o volume, mas é **recomendado** (ou pausar gravações) para ter consistência.
- Um snapshot de volume criptografado é criptografado; é possível criar uma cópia criptografada de um snapshot não criptografado.

```text
Snapshot 1 (completo)  ->  Snapshot 2 (só mudanças)  ->  Snapshot 3 (só mudanças)
                                                              |
                                                              v
                                                  restaura o volume inteiro
```

## Usos comuns

- **Backup** e recuperação de volumes.
- **Mover** um volume para outra AZ: snapshot → novo volume na AZ de destino.
- **Copiar para outra região** (Copy snapshot) para recuperação de desastres ou migração.
- **Compartilhar** com outra conta AWS.
- **Base para AMIs**: uma AMI baseada em EBS é composta por snapshots.

## Snapshot Archive

A camada de arquivo destina-se a snapshots que precisam ser mantidos por longo prazo e acessados raramente. Ela pode reduzir o custo de armazenamento, mas a recuperação demora mais e pode gerar custos adicionais. Confira os preços e tempos atuais na documentação antes de planejar uma recuperação.

Referência usada em cursos: o Archive custa cerca de **75% menos** que a camada padrão, e restaurar um snapshot arquivado leva de **24 a 72 horas**. Há um período mínimo de permanência no archive.

## Recycle Bin

O Recycle Bin permite configurar regras de retenção para proteger determinados snapshots e outros recursos suportados contra exclusão acidental. Os recursos mantidos na lixeira continuam sujeitos a cobrança enquanto são retidos.

- O período de retenção é configurável (de **1 dia a 1 ano**).
- Também pode proteger **AMIs**.
- Durante a retenção, o recurso excluído pode ser **recuperado**; depois, é apagado definitivamente.

## Fast Snapshot Restore (FSR)

Um volume criado a partir de um snapshot normalmente carrega os blocos sob demanda, e as primeiras leituras podem ser mais lentas. O **FSR** pré-inicializa o snapshot em AZs escolhidas para que o volume restaurado tenha desempenho total desde o início, sem latência na primeira leitura. Tem **custo adicional** significativo.

## Automatizar snapshots

- **Amazon Data Lifecycle Manager (DLM):** cria, retém e exclui snapshots e AMIs EBS por agendamento e tags.
- **AWS Backup:** serviço centralizado de backup para EBS e muitos outros serviços (RDS, EFS, DynamoDB etc.), com planos e políticas de retenção.

## Boas práticas

- Defina uma política de retenção.
- Teste periodicamente a restauração.
- Considere criptografia e permissões de acesso.
- Exclua snapshots antigos apenas depois de confirmar que não são necessários.
- Lembre-se de que snapshots e volumes podem gerar custos.

## Resumo

Snapshot = backup em um ponto no tempo. Archive = armazenamento de longo prazo para acesso raro. Recycle Bin = proteção contra exclusão acidental conforme regras configuradas.

- Snapshots são **incrementais** e permitem mover dados entre **AZs** e **regiões**.
- **FSR** = volume restaurado sem latência inicial (pago).
- **DLM / AWS Backup** = automatizar a criação e a retenção.
