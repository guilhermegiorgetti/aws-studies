# IAM Best Practices

## Práticas recomendadas

1. **Menor privilégio:** conceda apenas as permissões necessárias para a tarefa.
2. **Evite usar root no dia a dia:** reserve a conta root para tarefas que realmente exijam esse nível de acesso.
3. **Ative MFA:** especialmente para a conta root e para identidades privilegiadas.
4. **Prefira roles e credenciais temporárias:** evite chaves permanentes embutidas em código.
5. **Revise credenciais:** use o IAM Credentials Report para identificar credenciais antigas ou inativas.
6. **Audite o uso de permissões:** use Access Advisor / Last accessed como evidência para revisar acessos.
7. **Teste policies:** o Policy Simulator pode ajudar a entender se uma ação seria permitida.
8. **Revise acessos externos:** use o IAM Access Analyzer quando apropriado.
9. **Audite periodicamente:** pessoas, aplicações e serviços podem deixar de precisar de permissões que antes eram necessárias.
10. **Um usuário físico = uma identidade:** nunca compartilhe usuários IAM, senhas ou Access Keys entre pessoas.
11. **Permissões por grupo:** coloque usuários em grupos e associe as policies aos grupos, em vez de a cada usuário.
12. **Política de senhas forte:** defina tamanho mínimo, complexidade, expiração e bloqueio de reutilização.
13. **Access Keys só para CLI/SDK:** e rotacione-as periodicamente; desative chaves sem uso.
14. **Use IAM Identity Center para pessoas:** é a recomendação atual da AWS para acesso humano, com credenciais temporárias e login único em várias contas.
15. **Use condições nas policies:** por exemplo, exigir MFA (`aws:MultiFactorAuthPresent`) ou restringir IPs de origem para ações sensíveis.

## O que só o root pode fazer (exemplos)

Por isso o root deve ser protegido com MFA e guardado — não apagado:

- alterar as configurações da conta (nome, e-mail, senha do root);
- fechar a conta AWS;
- restaurar permissões de administradores IAM que perderam acesso;
- alterar o plano de suporte (AWS Support);
- algumas configurações de faturamento e de registro como vendedor no Marketplace.

## IAM e o Modelo de Responsabilidade Compartilhada

| AWS é responsável por | Você é responsável por |
|---|---|
| Infraestrutura global e segurança da rede | Usuários, grupos, roles e policies |
| Disponibilidade e funcionamento do serviço IAM | Ativar MFA em todas as identidades relevantes |
| Análise de configuração e testes de vulnerabilidade | Rotacionar chaves e revisar acessos |
| Compliance da infraestrutura | Usar as ferramentas para aplicar permissões adequadas |

## Auditoria usando Credentials Report e Access Advisor

A recomendação da aula é combinar as duas visões:

- **Credentials Report:** examina o estado das credenciais dos usuários IAM da conta.
- **Access Advisor / Last accessed:** mostra o último acesso aos serviços para uma identidade.

Uma ferramenta não substitui a outra: credenciais e permissões são assuntos relacionados, mas diferentes.

## Exemplo

Um usuário mantém uma Access Key antiga e também possui permissões para serviços que não utiliza. O Credentials Report ajuda a revisar a chave; o Access Advisor ajuda a investigar o uso dos serviços. Depois da análise, o administrador decide se deve desativar a chave ou reduzir permissões, seguindo o processo de mudança da organização.

## Revisão geral de IAM

| Conceito | Definição curta |
|---|---|
| **Users** | Identidade de uma pessoa; pode ter senha (Console) e Access Keys (CLI/SDK) |
| **Groups** | Conjunto de usuários; não contém outros grupos |
| **Policies** | Documento JSON com permissões (`Effect`, `Action`, `Resource`, `Condition`) |
| **Roles** | Identidade assumível por serviços ou pessoas, com credenciais temporárias |
| **Security** | MFA + política de senhas |
| **AWS CLI / SDK** | Acesso programático usando Access Keys |
| **Auditoria** | Credentials Report (conta) e Access Advisor (usuário) |

## Resumo

Audite regularmente credenciais e permissões, habilite MFA, evite root no cotidiano e aplique o menor privilégio.
