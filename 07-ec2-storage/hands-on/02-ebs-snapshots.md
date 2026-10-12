# Hands-on: EBS Snapshots

## Roteiro

1. Abra **EC2 → Snapshots** ou selecione um volume EBS e procure a opção de criar snapshot.
2. Use somente um volume de laboratório.
3. Crie um snapshot e acompanhe seu estado até ficar concluído.
4. Observe os metadados e a origem do snapshot.
5. Para testar recuperação, crie um novo volume a partir do snapshot e selecione a Availability Zone de destino.
6. Anexe o volume restaurado a uma instância de teste compatível, se necessário.

## Parte A — Mover um volume para outra AZ

1. Selecione o volume de 2 GiB do hands-on anterior → **Actions → Create snapshot**.
2. Descrição: `lab-snapshot-ebs`. Aguarde o estado `Completed`.
3. Selecione o snapshot → **Actions → Create volume from snapshot**.
4. Escolha uma **AZ diferente** da original (ex.: `sa-east-1b`).
5. Observe em **Volumes**: agora existe uma cópia do volume em outra AZ.

```text
Volume (sa-east-1a) ──snapshot──> Snapshot (regional) ──create volume──> Volume (sa-east-1b)
```

Se o volume tinha arquivos (como `/dados/arquivo.txt`), anexe o volume novo a uma instância na AZ de destino, monte-o e confirme que o arquivo está lá.

## Parte B — Copiar para outra região

1. Selecione o snapshot → **Actions → Copy snapshot**.
2. Escolha a região de destino (ex.: `us-east-1`) e, se quiser, ative a criptografia.
3. Troque a região no Console e veja o snapshot copiado em **Snapshots**.

Esse é o mecanismo básico para **recuperação de desastres** entre regiões.

## Parte C — Recycle Bin

1. Abra **EC2 → Recycle Bin** (ou pesquise "Recycle Bin").
2. **Create retention rule**:
   - Resource type: **EBS snapshots**
   - Apply to all resources: sim
   - Retention period: **1 dia**
3. Volte a **Snapshots** e **exclua** o snapshot de teste.
4. Em **Recycle Bin → Resources**, o snapshot aparece. Selecione-o e clique em **Recover**.
5. Confirme que ele voltou para **Snapshots**.

## Parte D — Archive (opcional)

Selecione um snapshot → **Actions → Archive snapshot**. Observe o aviso sobre o período mínimo de retenção e o tempo de restauração (horas a dias). Em laboratório, avalie se vale a pena, pois há período mínimo cobrado.

## Atenção

- Snapshots podem gerar custos.
- Um snapshot em andamento não deve ser tratado como backup pronto.
- Para aplicações com gravações ativas, planeje consistência antes do backup.
- Exclua recursos de teste apenas depois de verificar que não precisa deles.

## Limpeza

- [ ] Volumes criados a partir de snapshots excluídos.
- [ ] Snapshots excluídos **nas duas regiões**.
- [ ] Regra de retenção do Recycle Bin removida (ou lembre-se de que snapshots excluídos ficarão retidos e cobrados até o fim do período).
