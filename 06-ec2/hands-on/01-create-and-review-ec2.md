# Hands-on: criar e revisar uma instância EC2

## Objetivo

Conhecer as principais opções de criação de uma instância e entender o papel de cada configuração.

## Roteiro

1. Abra **EC2 → Instances → Launch instances**.
2. Escolha um nome identificável para o laboratório.
3. Escolha uma AMI compatível com o exercício.
4. Escolha um tipo de instância pequeno e adequado ao laboratório, conferindo custo e elegibilidade de nível gratuito na sua conta.
5. Selecione ou crie um key pair se for usar SSH.
6. Revise a VPC e a subnet.
7. Configure um Security Group com apenas as portas necessárias.
8. Revise as opções de armazenamento.
9. Se desejar testar User Data, use apenas um script simples e compreendido.
10. Revise tudo antes de iniciar a instância.

## Configuração sugerida para o laboratório

| Opção | Valor sugerido |
|---|---|
| Name | `lab-web-01` |
| AMI | Amazon Linux 2023 |
| Instance type | um tipo elegível ao nível gratuito na sua conta (ex.: `t2.micro` ou `t3.micro`) |
| Key pair | criar `EC2Tutorial` (`.pem` para OpenSSH, `.ppk` para PuTTY) |
| Network | VPC padrão, subnet pública, **Auto-assign public IP: Enable** |
| Security Group | SSH (22) do **seu IP** + HTTP (80) de `0.0.0.0/0` |
| Storage | volume raiz gp3 de 8 GiB (padrão) |

## User Data: servidor web simples

Em **Advanced details → User data**, cole:

```bash
#!/bin/bash
# Executado como root, apenas no primeiro boot
dnf update -y
dnf install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

Após a instância ficar em **running** e passar nos **status checks** (aguarde 1–2 minutos), abra no navegador:

```text
http://IP_PUBLICO_DA_INSTANCIA
```

Use **http://**, não https:// — não configuramos certificado nem a porta 443. Se der timeout, confira a regra da porta 80 no Security Group.

## Depois de criar

Observe:
- Instance ID e estado;
- tipo de instância;
- AMI;
- IP público/privado;
- Security Groups;
- volumes EBS anexados;
- zona de disponibilidade.
- abas **Status and alarms** (status checks), **Monitoring** e **Storage**.

## Experimento: parar e iniciar

1. Anote o IP público e o IP privado.
2. **Instance state → Stop instance** e aguarde `stopped`.
3. **Instance state → Start instance**.
4. Compare: o **IP público mudou**, o **IP privado continua** o mesmo.
5. Acesse o novo IP no navegador: o site continua funcionando porque o `httpd` foi habilitado com `systemctl enable`.

## Encerramento

Quando terminar, **termine a instância** se não precisar mais dela e revise volumes, snapshots, endereços IP e outros recursos que possam continuar gerando custos. Parar uma instância não elimina necessariamente todos os custos associados.
