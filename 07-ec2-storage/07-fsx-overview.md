# Amazon FSx Overview

## O que é Amazon FSx?

**Amazon FSx** oferece sistemas de arquivos gerenciados e especializados. A família inclui opções para diferentes necessidades, como compatibilidade com Windows, desempenho para cargas de computação e sistemas de arquivos empresariais.

## FSx for Windows File Server

- sistema de arquivos gerenciado compatível com Windows;
- usa SMB (Server Message Block) para compartilhamento de arquivos;
- integra-se a cenários que precisam de compartilhamentos de arquivos Windows.
- usa o sistema de arquivos **NTFS** e permissões do Windows (ACLs);
- integra-se ao **Microsoft Active Directory** para autenticação de usuários;
- pode ser implantado em **Multi-AZ** para alta disponibilidade, com backups diários para o S3;
- acessível por instâncias na AWS e também por servidores **on-premises** (via VPN/Direct Connect);
- apesar do nome, também pode ser montado por instâncias Linux via SMB.

```text
Usuários Windows on-premises ──SMB──┐
                                    ├──> FSx for Windows File Server (AZ A + AZ B)
EC2 Windows na AWS ──────────SMB────┘           integrado ao Active Directory
```

## FSx for Lustre

- sistema de arquivos de alto desempenho;
- indicado para cargas que precisam processar grandes volumes de dados;
- pode ser usado em computação de alto desempenho, machine learning e processamento de mídia.
- O nome vem de **Linux + cluster**.
- Projetado para **HPC (High Performance Computing)**: latência abaixo de milissegundos, centenas de GB/s de throughput e milhões de IOPS.
- **Integração com o Amazon S3:** pode apresentar objetos de um bucket como arquivos e gravar os resultados de volta no S3.
- Usos: treinamento de machine learning, análise financeira, simulações, renderização, genômica.

```text
Amazon S3 (dados) <──── leitura/gravação ────> FSx for Lustre <──── cluster de computação (EC2)
```

## FSx for NetApp ONTAP

Oferece recursos de armazenamento gerenciado baseados na tecnologia NetApp ONTAP, para cenários empresariais compatíveis.

- Suporta vários protocolos: **NFS, SMB e iSCSI**.
- Útil para **migrar** cargas que já usam NetApp ONTAP on-premises para a AWS.
- Recursos como snapshots, replicação, compressão e deduplicação.

## FSx for OpenZFS

Sistema de arquivos gerenciado baseado em **OpenZFS**, acessado via NFS. Útil para migrar cargas que já usam ZFS ou que precisam de baixa latência com recursos como snapshots e clonagem.

## Qual FSx escolher?

| Necessidade | Escolha |
|---|---|
| Compartilhamento de arquivos Windows, SMB, Active Directory | **FSx for Windows File Server** |
| HPC, ML, processamento massivo, integração com S3 | **FSx for Lustre** |
| Migrar NetApp ONTAP, múltiplos protocolos (NFS/SMB/iSCSI) | **FSx for NetApp ONTAP** |
| Migrar ZFS, NFS com baixa latência | **FSx for OpenZFS** |
| Compartilhar arquivos entre instâncias Linux, sem gerenciar capacidade | **Amazon EFS** |

## EFS versus FSx

- **EFS:** sistema de arquivos elástico muito usado para compartilhamento via NFS, especialmente em Linux.
- **FSx:** família de sistemas de arquivos especializados; a escolha depende do protocolo, desempenho e compatibilidade exigidos.

## Conceitos de protocolo

- **SMB:** protocolo comum para compartilhamento de arquivos em ambientes Windows.
- **NFS:** protocolo de compartilhamento de arquivos comum em ambientes Unix/Linux.

## Resumo

FSx não é um único sistema de arquivos: é uma família de serviços gerenciados para necessidades específicas, incluindo Windows File Server, Lustre, NetApp ONTAP e OpenZFS.

- **Para a prova:** "Windows + SMB + Active Directory" → **FSx for Windows File Server**.
- **Para a prova:** "HPC / computação de alto desempenho / ML com S3" → **FSx for Lustre**.
