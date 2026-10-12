# Amazon EC2

A pasta reúne fundamentos, tipos de instância, segurança de rede, conexão e opções de compra.

## Conteúdos

- [01 — EC2 Basics](01-ec2-basics.md)
- [02 — EC2 Instance Types](02-ec2-instance-types.md)
- [03 — Security Groups e portas](03-security-groups-and-ports.md)
- [04 — Opções de compra](04-ec2-purchasing-options.md)
- [05 — Conectar via SSH no Windows](05-ssh-from-windows.md)
- [06 — EC2 Instance Connect e IAM Role](06-instance-connect-and-iam-role.md)
- [Hands-on](hands-on/README.md)

## Mapa mental

```text
                         Amazon EC2
                              │
   ┌──────────┬───────────────┼───────────────┬──────────────┐
   │          │               │               │              │
  AMI   Instance type   Security Group     Storage       IAM Role
 (SO)  (CPU/RAM/rede)  (firewall virtual) (EBS/Instance  (permissões
                                            Store)       de API)
   │
User Data (script no primeiro boot)
```

## O que mais cai na prova

- EC2 é **IaaS**: o cliente cuida do SO, patches, dados e Security Groups.
- Famílias: **T/M** uso geral, **C** computação, **R/X** memória, **I/D** armazenamento, **P/G** GPU.
- Security Group: só regras **allow**, **stateful**, inbound bloqueado por padrão.
- Timeout = Security Group; connection refused = aplicação.
- Portas: **22** SSH, **21** FTP, **22** SFTP, **80** HTTP, **443** HTTPS, **3389** RDP.
- Opções de compra: On-Demand, Reserved (1/3 anos), Savings Plans, Spot (até ~90%, pode ser interrompida), Dedicated Host (BYOL/compliance), Capacity Reservation.
- Credenciais na instância: **sempre IAM role**, nunca `aws configure` com chaves pessoais.

## Próximo tópico

Armazenamento usado pelo EC2: [07 — EC2 Instance Storage](../07-ec2-storage/README.md).
