# Hands-on: conectar a uma instância EC2

## Opção A — EC2 Instance Connect

1. Abra **EC2 → Instances**.
2. Selecione uma instância compatível.
3. Clique em **Connect**.
4. Selecione o método disponível e siga as instruções.

## Opção B — SSH pelo PowerShell

1. Confirme que tem a chave privada correta.
2. Confirme que o Security Group permite TCP/22 apenas a partir da origem necessária.
3. Abra o PowerShell na pasta onde está a chave.
4. Execute, substituindo os valores:

```powershell
ssh -i .\EC2Tutorial.pem ec2-user@IP_PUBLICO_DA_INSTANCIA
```

Se aparecer `UNPROTECTED PRIVATE KEY FILE`, ajuste as permissões da chave com `icacls` (veja [05 — SSH no Windows](../05-ssh-from-windows.md)).

Na primeira conexão, o SSH pergunta se você confia no host (`Are you sure you want to continue connecting?`). Digite `yes`.

## Opção C — Session Manager (sem porta 22)

1. Associe à instância uma role com a policy `AmazonSSMManagedInstanceCore`.
2. Aguarde alguns minutos para a instância aparecer em **Systems Manager → Fleet Manager**.
3. Em **EC2 → Connect**, escolha a aba **Session Manager** e conecte.
4. Opcional: remova a regra da porta 22 do Security Group e confirme que a conexão continua funcionando.

## Depois de conectar

Execute comandos básicos do sistema operacional para verificar que está dentro da máquina remota. Não copie credenciais pessoais para a instância.

```bash
whoami                 # usuário atual (ec2-user, ou ssm-user no Session Manager)
hostname -f            # nome DNS interno da instância
uname -a               # versão do kernel
cat /etc/os-release    # distribuição Linux
df -h                  # discos e uso de espaço
free -h                # memória
curl -s localhost      # testa o servidor web local, se configurado
exit                   # sai da sessão
```

## Teste com IAM role

1. Na instância, execute `aws iam list-users` → deve falhar por falta de credenciais.
2. Associe uma role com `IAMReadOnlyAccess` à instância.
3. Execute novamente → deve listar os usuários.
4. Remova a role ao terminar.

Veja os detalhes em [06 — EC2 Instance Connect e IAM Role](../06-instance-connect-and-iam-role.md).

## Limpeza

Termine a instância de laboratório quando acabar e confira outros recursos faturáveis.
