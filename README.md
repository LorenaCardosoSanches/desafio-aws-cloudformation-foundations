# ☁️ Desafio AWS CloudFormation Foundations – DIO

## 📘 Visão Geral

Este projeto foi desenvolvido como parte da formação **AWS Cloud Foundations – Santander Code Girls (DIO)**.  
O objetivo é demonstrar a criação e automação de uma infraestrutura em nuvem utilizando o serviço **AWS CloudFormation**, aplicando o conceito de **Infraestrutura como Código (IaC)**.

O desafio consistiu em criar um **template YAML** capaz de provisionar automaticamente múltiplos serviços AWS integrados, sem a necessidade de configuração manual pelo console.

---

## 🧠 Conceito de CloudFormation

O **AWS CloudFormation** é um serviço que permite descrever toda a infraestrutura da nuvem através de **arquivos declarativos**.  
Com ele, é possível criar, atualizar e excluir recursos de forma automatizada e reprodutível, garantindo que diferentes ambientes (como desenvolvimento e produção) tenham a mesma estrutura e configuração.

Essa abordagem é parte fundamental do modelo **DevOps** e do princípio de **automação completa** da infraestrutura.

---

## 🧩 Estrutura do Projeto

```
📁 desafio-aws-cloudformation-foundations/
│
├── 📄 README.md → Documentação técnica e detalhada
├── 📁 templates/
│ └── primeira-stack.yaml → Template principal em YAML
└── 📁 images/ → Evidências do processo de execução
├── template-upload.png
├── stack-create-complete.png
├── resources-list.png
└── ec2-running.png
```


---

## ⚙️ Arquitetura Criada

O template `primeira-stack.yaml` foi desenvolvido para automatizar a criação dos seguintes componentes na AWS:

| Recurso | Serviço AWS | Descrição |
|----------|--------------|------------|
| **S3 Bucket (LogsBucket)** | Amazon S3 | Armazena logs de auditoria e eventos. |
| **Bucket Policy** | S3 Policy | Permite que o CloudTrail grave logs dentro do bucket. |
| **CloudTrail (Trail)** | AWS CloudTrail | Registra todas as ações realizadas na conta AWS. |
| **IAM Role e Instance Profile** | AWS Identity and Access Management | Concede permissões à instância EC2 para enviar métricas e logs ao CloudWatch. |
| **EC2 Instance (DemoInstance)** | Amazon EC2 | Instância criada automaticamente para simulação prática. |
| **CloudWatch Alarm (CpuAlarm)** | Amazon CloudWatch | Cria um alerta quando o uso de CPU da instância ultrapassa 70%. |

Esses recursos se complementam e demonstram como o CloudFormation pode orquestrar **infraestrutura completa**, englobando **armazenamento, segurança, auditoria e monitoramento** em um único arquivo YAML.

---

## 🧱 Estrutura do Template (Explicação Detalhada)

### 🔹 Cabeçalho e Descrição

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: >
  DIO – AWS Cloud Foundations | Primeira Stack com CloudFormation.
  Provisiona: S3 (logs), CloudTrail (auditoria), IAM Role/InstanceProfile para EC2
  e um Alarme do CloudWatch para CPU.
[//]: Define a versão do formato CloudFormation e a descrição geral da pilha, explicando o propósito do projeto.

Parameters:
  ProjectName:
    Type: String
    Default: dio-cloudformation-foundations
  InstanceType:
    Type: String
    Default: t3.micro
[//]: Permitem personalizar a stack no momento da criação, alterando o nome do projeto ou o tipo da instância EC2 sem modificar o código principal.

LogsBucket:
  Type: AWS::S3::Bucket
  Properties:
    BucketName: !Sub '${ProjectName}-logs-${AWS::AccountId}-${AWS::Region}'
[//]: Cria um bucket S3 para armazenar logs.
[//]: O nome é gerado automaticamente com o nome do projeto, número da conta e região, garantindo unicidade global.
[//]: A configuração também inclui:
[//]: - Criptografia AES256
[//]: - Bloqueio de acesso público
[//]: - Controle de versão de objetos

LogsBucketPolicy:
  Type: AWS::S3::BucketPolicy
  Properties:
    Bucket: !Ref LogsBucket
[//]: Define as permissões que permitem ao CloudTrail gravar arquivos dentro do bucket.
[//]: Garante que apenas o serviço autorizado (CloudTrail) possa escrever logs de auditoria.

Trail:
  Type: AWS::CloudTrail::Trail
  Properties:
    S3BucketName: !Ref LogsBucket
    IsLogging: true
[//]: Cria uma trilha de auditoria que registra eventos como login, criação de recursos e exclusões.
[//]: Os arquivos gerados são enviados automaticamente para o bucket de logs.

Ec2Role:
  Type: AWS::IAM::Role
  Properties:
    AssumeRolePolicyDocument:
      Statement:
        - Effect: Allow
          Principal:
            Service: ec2.amazonaws.com
[//]: Cria uma função IAM que permite à instância EC2 enviar métricas e logs para o CloudWatch.
[//]: O Instance Profile associa essa função à máquina virtual durante sua criação.

DemoInstance:
  Type: AWS::EC2::Instance
  Properties:
    ImageId: !Ref LatestAmiId
    InstanceType: !Ref InstanceType
[//]: Provisiona uma instância EC2 Amazon Linux 2023, conectada ao perfil IAM definido anteriormente.
[//]: Essa instância é o “coração” do ambiente, usada para testes e simulações.

CpuAlarm:
  Type: AWS::CloudWatch::Alarm
  Properties:
    MetricName: CPUUtilization
    Threshold: 70
[//]: Cria um alarme de monitoramento que dispara caso a CPU da instância ultrapasse 70% em dois períodos consecutivos.
[//]: Demonstra a capacidade do CloudFormation de gerenciar observabilidade e alertas.

Outputs:
  oInstanceId:
    Description: ID da instância EC2 provisionada.
    Value: !Ref DemoInstance
[//]: Define as informações finais exibidas após a criação da stack, como o ID da instância, nome do bucket e região.
[//]: Essas saídas facilitam a integração com outras stacks e automações.

```

## 🚀 Passos Executados

1. **Criação do arquivo `primeira-stack.yaml`**  
   Template desenvolvido e validado no VS Code, contendo todos os recursos descritos em YAML.

2. **Upload no AWS CloudFormation**  
   Feito diretamente pelo console AWS → *Create Stack → Upload a template file*.

3. **Validação e Execução**  
   Aguardado o status **`CREATE_COMPLETE`** indicando a criação bem-sucedida da stack.

4. **Verificação dos Recursos**  
   Confirmado o provisionamento automático de todos os serviços listados (S3, CloudTrail, IAM, EC2 e CloudWatch).

5. **Testes e Auditoria**  
   Acesso à instância **EC2** para confirmar o estado de execução e inspeção do **bucket S3** para verificar o recebimento dos logs do CloudTrail.

---

## 🖼️ Evidências da Implementação

| Etapa | Descrição | Imagem |
|-------|------------|--------|
| 1️⃣ | Upload do template YAML no CloudFormation | ![Template Upload](images/template-upload.png) |
| 2️⃣ | Stack criada com sucesso (CREATE_COMPLETE) | ![Stack criada](images/stack-create-complete.png) |
| 3️⃣ | Recursos provisionados automaticamente | ![Recursos criados](images/resources-list.png) |
| 4️⃣ | Instância EC2 em execução | ![EC2 Running](images/ec2-running.png) |

---

## 📊 Benefícios da IaC e CloudFormation

- 🚀 **Recriação rápida** de ambientes em minutos.  
- 🧩 **Eliminação de erros humanos** por meio de automação declarativa.  
- 🕓 **Versionamento e histórico** centralizados no GitHub.  
- 🏗️ **Padronização de ambientes** (Desenvolvimento, QA e Produção).  
- 🔄 **Integração com pipelines CI/CD** e controle de mudanças contínuas.

---

## 💡 Conclusões Pessoais

Durante o desenvolvimento deste desafio, foi possível compreender na prática:

- Como estruturar templates **CloudFormation** de forma modular e reutilizável.  
- A importância de **políticas seguras no IAM** e **bloqueio público no S3**.  
- A sinergia entre **CloudTrail, CloudWatch e EC2** para auditoria e monitoramento.  
- O valor da **automação e reprodutibilidade** em ambientes profissionais de nuvem.  

O projeto demonstrou que, com **um único arquivo YAML**, é possível criar uma infraestrutura completa, segura e documentada, aplicando boas práticas de **Infraestrutura como Código (IaC)**.

---

## 🔗 Documentações e Recursos

- [📘 AWS CloudFormation – Documentação Oficial](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/Welcome.html)  
- [📙 AWS CloudWatch – Alarms and Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)  
- [📗 AWS CloudTrail – Logging and Events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)  
- [📓 DIO – Formação AWS Cloud Foundations](https://www.dio.me)

---

## ✨ Autora

**Lorena Cardoso Sanches**  
Formação **AWS Cloud Foundations – DIO & Santander Code Girls**  
📍 São Bernardo do Campo – SP  
🔗 [linkedin.com/in/lorenacardososanches](https://www.linkedin.com/in/lorenacardososanches)
