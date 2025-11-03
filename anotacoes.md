# 🧠 Anotações e Insights – Desafio AWS CloudFormation

## 💡 Objetivo Pessoal

Registrar aprendizados, reflexões e resultados práticos durante a criação da primeira infraestrutura automatizada na AWS, aplicando o conceito de **Infraestrutura como Código (IaC)** com o **AWS CloudFormation**.

---

## ⚙️ Etapas do Processo

1. Desenvolvi o template `primeira-stack.yaml` para criar automaticamente uma arquitetura completa na nuvem.  
2. Fiz o upload do modelo no serviço **CloudFormation** e validei a criação da stack até o status `CREATE_COMPLETE`.  
3. Verifiquei os recursos provisionados (S3, CloudTrail, IAM, EC2 e CloudWatch).  
4. Testei o monitoramento com **CloudWatch Alarm** e confirmei logs de auditoria no **S3 via CloudTrail**.  
5. Registrei as evidências visuais no repositório (pasta `/images`).

---

## 📚 Aprendizados Principais

- A **IaC facilita a padronização e o controle de ambientes** em diferentes estágios (dev, teste, produção).  
- O **CloudFormation** reduz erros humanos e torna o provisionamento muito mais rápido.  
- Entendi a importância da **integração entre segurança, auditoria e monitoramento** (IAM + CloudTrail + CloudWatch).  
- Documentar o processo é essencial — tanto para reuso quanto para aprendizado contínuo.  

---

## 🪄 Destaques Técnicos

- Nomeação dinâmica com `!Sub` para evitar conflitos entre recursos.  
- Bucket S3 configurado com bloqueio público e versionamento.  
- Alarme automatizado de CPU via CloudWatch.  
- Auditoria contínua com CloudTrail.  

---

## 🔍 Resultados

A stack foi criada com sucesso e todos os recursos ficaram operacionais.  
As imagens comprovam a execução completa do ciclo:  
- Upload do template  
- Criação da stack  
- Recursos provisionados  
- EC2 em execução  
- CloudWatch monitorando métricas  
- CloudTrail registrando logs  

---

## ✨ Reflexão Final

Este desafio foi uma excelente introdução à **automação de infraestrutura e boas práticas de DevOps**.  
Percebi o poder da **documentação e versionamento com GitHub** e a importância de sempre trabalhar com **segurança e rastreabilidade** na nuvem.

---

📅 **Autora:** Lorena Cardoso Sanches  
📚 *Formação AWS Cloud Foundations – DIO & Santander Code Girls*  
🔗 [linkedin.com/in/lorenacardososanches](https://www.linkedin.com/in/lorenacardososanches)
