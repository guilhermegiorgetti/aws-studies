# Hands-on: criar e anexar um volume EBS

## Roteiro

1. Abra **EC2 → Volumes**.
2. Escolha criar um volume de teste, conferindo a Availability Zone e o tipo.
3. Crie o volume.
4. Anexe-o a uma instância compatível na mesma Availability Zone.
5. Observe o estado do volume e a associação com a instância.
6. Não formate nem monte um disco com dados importantes. Em laboratório, use apenas um volume de teste e siga a documentação do sistema operacional para inicializá-lo.

## Passo a passo sugerido

1. Em **EC2 → Instances**, anote a **AZ** da instância de laboratório (aba *Networking*, ex.: `sa-east-1a`).
2. Na aba **Storage** da instância, observe o volume raiz e o atributo **Delete on termination = Yes**.
3. Em **EC2 → Volumes → Create volume**:
   - Volume type: `gp3`
   - Size: `2 GiB`
   - Availability Zone: **a mesma da instância**
4. Quando o volume ficar `available`, selecione-o → **Actions → Attach volume** → escolha a instância.
5. O estado muda para `in-use`.

### Experimento: AZ diferente

Crie outro volume em uma AZ **diferente** da instância e tente anexá-lo. A instância não aparece na lista: volumes EBS só se anexam a instâncias **da mesma AZ**.

## Usar o volume no Linux (apenas volume de teste vazio)

Conecte-se à instância e execute:

```bash
lsblk                                  # lista os discos; o novo aparece sem ponto de montagem (ex.: nvme1n1 ou xvdf)
sudo file -s /dev/nvme1n1              # "data" = volume vazio, sem sistema de arquivos
sudo mkfs -t xfs /dev/nvme1n1          # cria um sistema de arquivos (APAGA o conteúdo do volume!)
sudo mkdir /dados
sudo mount /dev/nvme1n1 /dados
df -h                                  # o volume aparece montado em /dados
echo "teste EBS" | sudo tee /dados/arquivo.txt
```

> **Atenção:** confirme o nome do dispositivo com `lsblk` antes do `mkfs`. Formatar o disco errado apaga os dados dele. O nome varia conforme o tipo de instância (`/dev/xvdf`, `/dev/nvme1n1` etc.).

O ponto de montagem não sobrevive a um reboot, a menos que seja configurado em `/etc/fstab` (consulte a documentação antes de editar esse arquivo).

## O que observar

- tamanho e tipo do volume;
- Availability Zone;
- estado do volume;
- instância anexada;
- opções de exclusão e persistência.

### Experimento: Delete on Termination

1. Termine a instância de laboratório.
2. Em **Volumes**, observe:
   - o volume **raiz** desaparece (foi excluído);
   - o volume **adicional** volta ao estado `available` (continua existindo e gerando custo).

## Limpeza

Desanexe e exclua o volume de teste quando tiver certeza de que não precisa dos dados. Confira se há snapshots que também precisam ser excluídos.

Para desanexar com segurança, primeiro desmonte no sistema operacional (`sudo umount /dados`) e depois use **Actions → Detach volume**.
