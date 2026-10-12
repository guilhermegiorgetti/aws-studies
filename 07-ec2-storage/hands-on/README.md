# Storage Hands-on

- [01 — EBS e volume](01-ebs-volume.md)
- [02 — Snapshots](02-ebs-snapshots.md)
- [03 — AMI](03-create-ami.md)

Faça os exercícios com recursos de teste, confira a cobrança e exclua o que não for mais necessário.

## Ordem sugerida

```text
01 EBS volume  ──>  02 Snapshot do volume  ──>  03 AMI a partir de uma instância
(criar/anexar)      (mover entre AZs/regiões)   (boot mais rápido com software pronto)
```

## Ideias de exercícios extras

- **EFS:** criar um sistema de arquivos EFS, montá-lo em duas instâncias em AZs diferentes e verificar que um arquivo criado em uma aparece na outra (Security Group do EFS liberando TCP/2049 a partir do Security Group das instâncias).
- **Instance Store:** lançar um tipo de instância com disco local (ex.: com sufixo `d`), gravar um arquivo, fazer **reboot** (arquivo mantido) e depois **stop/start** (arquivo perdido). Confira o custo antes: esses tipos não costumam estar no nível gratuito.

## Checklist de limpeza

- [ ] Volumes EBS em estado `available` excluídos.
- [ ] Snapshots excluídos em **todas as regiões** usadas.
- [ ] AMIs com **deregister** + snapshots associados excluídos.
- [ ] Itens retidos no **Recycle Bin** e suas regras revisados.
- [ ] Sistemas de arquivos EFS de teste excluídos.
