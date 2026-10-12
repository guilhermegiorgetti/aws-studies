# EC2 Instance Storage

Esta pasta reúne os serviços de armazenamento usados com EC2.

## Conteúdos

- [01 — EBS](01-ebs-overview.md)
- [02 — EBS Snapshots](02-ebs-snapshots.md)
- [03 — AMI](03-ami-overview.md)
- [04 — EC2 Image Builder](04-ec2-image-builder.md)
- [05 — EC2 Instance Store](05-ec2-instance-store.md)
- [06 — EFS](06-efs-overview.md)
- [07 — FSx](07-fsx-overview.md)
- [Hands-on](hands-on/README.md)

## Mapa mental

```text
Armazenamento para EC2
│
├── Bloco
│   ├── EBS ............... disco de rede, persistente, 1 AZ
│   │   └── Snapshots ..... backup incremental; mover entre AZs/regiões
│   └── Instance Store .... disco físico local, muito rápido, efêmero
│
├── Arquivos (compartilhados)
│   ├── EFS ............... Linux, NFS, multi-AZ, paga pelo uso
│   └── FSx ............... Windows (SMB), Lustre (HPC), ONTAP, OpenZFS
│
└── Imagens
    ├── AMI ............... molde para lançar instâncias (regional)
    └── Image Builder ..... automatiza criação/teste/distribuição de AMIs
```

## Comparação rápida

| Serviço | Tipo | Compartilhado? | Escopo | Persistente? |
|---|---|---|---|---|
| EBS | Bloco | Não (exceto Multi-Attach io1/io2) | 1 AZ | Sim |
| Instance Store | Bloco local | Não | Host físico | Não |
| EFS | Arquivos (NFS) | Sim | Região | Sim |
| FSx | Arquivos (SMB/Lustre/NFS/iSCSI) | Sim | Depende do tipo | Sim |

## Responsabilidade compartilhada (armazenamento)

| AWS | Cliente |
|---|---|
| Infraestrutura física e replicação interna dos volumes | Backups e snapshots |
| Substituição de hardware com falha | Criptografia dos dados |
| Isolamento entre clientes | Quem tem acesso aos dados |
| | Cópia dos dados do Instance Store antes de perdê-los |
