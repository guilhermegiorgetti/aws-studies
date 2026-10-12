# Hands-on: criar uma AMI

## Roteiro

1. Abra **EC2 → Instances**.
2. Selecione uma instância de laboratório que possa ser usada como base.
3. No menu **Actions**, procure a opção de criar imagem/AMI.
4. Dê um nome e uma descrição que indiquem o propósito.
5. Revise as configurações de volumes incluídos.
6. Inicie a criação e acompanhe o estado da AMI em **AMIs**.
7. Quando estiver pronta, use-a para iniciar uma instância de teste, se isso fizer parte do exercício.

## Exercício completo: AMI com servidor web pré-instalado

### 1. Preparar a instância base

Lance uma instância Amazon Linux com este User Data (instala o Apache, mas sem criar a página):

```bash
#!/bin/bash
dnf update -y
dnf install -y httpd
systemctl start httpd
systemctl enable httpd
```

### 2. Criar a AMI

1. Selecione a instância → **Actions → Image and templates → Create image**.
2. Nome: `lab-ami-httpd`.
3. Aguarde o estado da AMI mudar de `pending` para `available` em **Images → AMIs**.
4. Observe em **Snapshots** que um snapshot foi criado automaticamente para a AMI.

### 3. Lançar uma instância a partir da AMI

1. Selecione a AMI → **Launch instance from AMI**.
2. Use o mesmo Security Group (HTTP liberado).
3. Em User Data, coloque **apenas** a criação da página — o Apache já está instalado:

```bash
#!/bin/bash
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

4. Acesse `http://IP_PUBLICO` no navegador.

Compare o tempo até a página responder com o do hands-on de EC2: o boot fica **mais rápido**, porque os pacotes já estavam na imagem. É exatamente por isso que Auto Scaling Groups costumam usar AMIs personalizadas.

## Cuidados

A criação de uma AMI pode envolver snapshots e custos de armazenamento. Não crie imagens de máquinas com dados pessoais ou segredos sem avaliar as implicações de segurança. Limpe AMIs e snapshots de teste quando não forem mais necessários.

- Antes de criar a AMI, remova arquivos como `~/.aws/credentials`, chaves privadas e históricos de comandos que possam conter segredos.
- Ao criar a AMI, a instância é **reiniciada por padrão** para garantir consistência.

## Limpeza (na ordem)

1. Termine as instâncias de teste.
2. **Images → AMIs → Actions → Deregister AMI**.
3. **Snapshots:** exclua o snapshot que pertencia à AMI (veja a descrição `Created by CreateImage... for ami-...`).
