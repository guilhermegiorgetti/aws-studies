# Hands-on: Security Groups

## Objetivo

Entender como regras de entrada controlam o acesso de rede à instância.

## Roteiro seguro

1. Abra **EC2 → Security Groups**.
2. Selecione o Security Group da instância de laboratório.
3. Revise as inbound rules e outbound rules.
4. Identifique a porta usada pela atividade: por exemplo, TCP/22 para SSH ou TCP/80 para HTTP.
5. Para SSH, limite a origem ao seu IP público atual, se o cenário permitir.
6. Salve a regra e teste a conexão.
7. Remova regras temporárias quando não forem mais necessárias.

## Experimento 1: provocar um timeout

Use a instância com o servidor web do hands-on anterior.

1. Abra `http://IP_PUBLICO` no navegador e confirme que a página carrega.
2. No Security Group, **remova** a regra de entrada da porta 80.
3. Recarregue a página: ela fica carregando até dar **timeout**.
4. Adicione a regra novamente e confirme que a página volta.

Conclusão: **timeout = tráfego bloqueado na rede**, geralmente Security Group.

## Experimento 2: um Security Group em várias instâncias

1. Crie uma segunda instância usando o **mesmo** Security Group.
2. Altere uma regra no Security Group.
3. Observe que a mudança vale para as duas instâncias imediatamente, sem reiniciar.

## Experimento 3: referenciar outro Security Group

1. Crie um Security Group `sg-app` sem regras de entrada.
2. Crie `sg-db` com uma regra de entrada TCP/3306 cuja origem é **`sg-app`** (não um IP).
3. Observe que, na origem, aparece o ID do Security Group (`sg-...`).

Essa é a base das arquiteturas em camadas: só quem tem o `sg-app` alcança o banco.

## Checklist de revisão

- [ ] Nenhuma regra `0.0.0.0/0` em portas 22 ou 3389 que não seja temporária.
- [ ] Cada regra tem uma **descrição** indicando o motivo.
- [ ] Regras de banco de dados referenciam Security Groups, não a Internet.
- [ ] Security Groups não usados foram excluídos (o grupo `default` da VPC não pode ser excluído).

## Observações

- Não abra todas as portas para a Internet.
- A regra de Security Group não cria usuário nem configura a autenticação do sistema.
- Se a conexão falhar, verifique também rotas, IP público, estado da instância e autenticação.
