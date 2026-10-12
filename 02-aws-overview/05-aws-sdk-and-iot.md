# AWS SDKs, Boto3 e IoT

## 1. AWS SDK

**SDK (Software Development Kit)** é um conjunto de bibliotecas que permite usar serviços AWS diretamente no código de uma aplicação.

Em vez de executar comandos manualmente no terminal, o programa pode chamar APIs AWS usando uma biblioteca da linguagem escolhida.

A AWS oferece SDKs para linguagens e plataformas como JavaScript, Python, Java, .NET, Go e aplicações móveis para Android e iOS, entre outras.

## 2. Boto3

**Boto3** é o SDK da AWS para Python. Ele permite que um programa Python interaja com serviços AWS, por exemplo:
- listar objetos de um bucket S3;
- enviar ou baixar arquivos;
- consultar informações de recursos;
- automatizar tarefas.

O programa ainda precisa de credenciais válidas e permissões IAM adequadas. Em uma instância EC2, normalmente é preferível usar uma IAM role em vez de guardar chaves permanentes no código.

### Exemplo mínimo com Boto3

```python
import boto3

s3 = boto3.client("s3", region_name="sa-east-1")

for bucket in s3.list_buckets()["Buckets"]:
    print(bucket["Name"])
```

Instalação: `pip install boto3`.

Repare que o código **não contém chaves**. O SDK procura credenciais automaticamente, em uma ordem definida (cadeia de credenciais), por exemplo:

1. variáveis de ambiente (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_PROFILE`);
2. arquivos `~/.aws/credentials` e `~/.aws/config` (criados pelo `aws configure`);
3. credenciais temporárias de uma IAM role, quando o código roda em EC2, Lambda, ECS etc.

A ordem exata pode variar entre SDKs; consulte a documentação do SDK utilizado.

## Formas de acessar a AWS

| Forma | Como se autentica | Uso típico |
|---|---|---|
| **AWS Management Console** | Usuário + senha (+ MFA) | Uso manual pelo navegador |
| **AWS CLI** | Access Keys ou credenciais temporárias | Scripts e comandos no terminal |
| **AWS SDK** | Access Keys ou credenciais temporárias | Código de aplicações |

Os três chamam, no fundo, as mesmas **APIs da AWS**. A CLI, por exemplo, é construída sobre o SDK para Python.

> **Para a prova:** Access Keys são para CLI/SDK; senha + MFA são para o Console. Access Keys são secretas como uma senha e não devem ser compartilhadas.

## 3. IoT — Internet of Things

**IoT** significa **Internet of Things**, em português, Internet das Coisas.

É o conceito de conectar dispositivos físicos — como sensores, máquinas e eletrodomésticos — a redes para coletar ou trocar dados e, em alguns casos, receber comandos.

### Exemplo com Arduino

Um Arduino conectado a um sensor de temperatura pode coletar a temperatura de uma sala. Com um módulo ou placa que ofereça conectividade de rede, ele pode enviar os dados para um serviço na nuvem. A aplicação pode guardar os dados, mostrar um gráfico ou emitir um alerta quando a temperatura ultrapassar um limite.

O Arduino sozinho não significa necessariamente que há IoT: o dispositivo precisa ter uma forma de comunicação e um sistema que envie ou receba dados.

### IoT na AWS

A AWS tem uma família de serviços para IoT. O principal é o **AWS IoT Core**, que permite conectar dispositivos à nuvem de forma segura e trocar mensagens com eles, normalmente usando o protocolo **MQTT** (leve, pensado para dispositivos com poucos recursos).

```text
Sensor / Arduino ──MQTT──> AWS IoT Core ──regras──> S3 / DynamoDB / Lambda / alertas
```

Cada dispositivo se autentica com certificados, e as permissões são controladas por policies — os mesmos princípios de segurança do restante da AWS. Existem SDKs específicos para dispositivos (AWS IoT Device SDKs).

## Resumo

- **SDK:** biblioteca para acessar serviços AWS a partir de código.
- **Boto3:** SDK da AWS para Python.
- **Credenciais no SDK:** nunca no código; use a cadeia de credenciais (perfil local ou IAM role).
- **Console / CLI / SDK:** três formas de chamar as mesmas APIs da AWS.
- **IoT:** dispositivos físicos conectados que coletam, enviam ou recebem dados.
- **AWS IoT Core:** serviço gerenciado para conectar dispositivos à AWS (ex.: via MQTT).
