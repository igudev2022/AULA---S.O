<p align="center">
  <img src="https://github.com/user-attachments/assets/7410e64f-debc-4b91-a30e-7c4dca93ad9b" width="650" alt="Banner VM vs Docker">
</p>





# 📄 Relatório Técnico de Engenharia e Modernização de Sistemas
> **Projeto:** Modernização da Infraestrutura Tecnológica – Case DevStore  
> **Status:** Parecer Técnico Final (Versão Estendida & Arquitetura Detalhada)  
> **Responsável:** [Seu Nome/Grupo] – Arquiteto de Soluções & Engenheiro DevOps  

---

## 📌 1. Resumo Executivo
Este documento detalha o plano de transição da infraestrutura de TI da **DevStore** de um modelo de virtualização local legada para uma arquitetura moderna baseada em computação em nuvem (AWS) e conteinerização (Docker). O objetivo primordial é mitigar a dívida técnica acumulada, reduzir o TCO (Custo Total de Propriedade) e garantir uma escalabilidade elástica capaz de suportar o crescimento exponencial da startup com alta disponibilidade (SLA de 99,99%).

---

## 🔍 2. Diagnóstico da Situação Atual (Cenário *As-Is*)
A auditoria identificou que a DevStore opera sob uma topologia *On-Premise* ineficiente:
* **Déficit de Hardware:** Servidores físicos (*Bare-metal*) com manutenção dispendiosa e sem redundância geográfica.
* **Gargalo de Performance:** Uso de Máquinas Virtuais (VMs) tradicionais que geram um *overhead* excessivo ao exigir um Sistema Operacional completo para cada micro-serviço.
* **Fragmentação de Ambientes:** Falta de paridade entre o desenvolvimento e a produção, causando falhas críticas durante deploys.
* **Risco Operacional:** Ausência de monitoramento preditivo e segurança de rede perimetral automatizada.

---

## 🛠️ 3. Proposta de Nova Arquitetura (Cenário *To-Be*)
A estratégia recomendada é a adoção de uma arquitetura **Cloud-Native**, fundamentada em pilares de imutabilidade e elasticidade. A transição move a DevStore de um gerenciamento focado em hardware para um modelo de "Infraestrutura como Código" (IaC).

---

## 🗺️ 4. Diagrama Estrutural da Nova Infraestrutura
*O fluxograma abaixo compara o modelo monolítico virtualizado (Legado) com a arquitetura em nuvem conteinerizada (Proposta).*

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/50d26042-1c76-4f58-acb6-8016ae93eaed" />



---

## 🏛️ 5. Arquitetura de Sistemas: Análise Profunda de Componentes

Nesta seção, dissecamos o papel fundamental de cada camada computacional, contrastando as limitações do modelo legado com o ganho estratégico da nova arquitetura.

### 5.1. Arquitetura Legada: Virtualização Baseada em Hardware
*A topologia atual amarra a DevStore a limitações físicas e operacionais severas.*

* 💻 **Hardware Físico (*Bare-Metal*):** * **O que faz:** É o servidor físico real (CPU, RAM, Discos) residente no escritório da DevStore.
  * **Importância & Impacto:** É a fundação do processamento, mas no modelo local, atua como um teto rígido. A escalabilidade exige compra e instalação manual (*downtime*). Mais criticamente, a falha de uma fonte de energia ou placa-mãe representa um Ponto Único de Falha (SPOF), podendo paralisar a empresa inteira.
* ⚙️ **Hypervisor (Monitor de Máquina Virtual):** * **O que faz:** Software (ex: VMware, Hyper-V) que intercepta as chamadas de hardware e "engana" os sistemas operacionais convidados, fazendo-os acreditar que possuem um computador físico exclusivo.
  * **Importância & Impacto:** Foi revolucionário nos anos 2000 por permitir consolidação de servidores, mas hoje cobra o "imposto do hypervisor". Ele consome ciclos de CPU apenas para traduzir comandos virtuais para físicos, gerando latência em aplicações web de alta demanda.
* 🧱 **Sistema Operacional Convidado (*Guest OS*):** * **O que faz:** Um Windows ou Ubuntu completo instalado dentro de cada Máquina Virtual para hospedar uma única aplicação.
  * **Importância & Impacto:** É o maior dreno de recursos e segurança da empresa. Se a DevStore tiver 20 serviços rodando, ela gerencia 20 Sistemas Operacionais. Isso significa dezenas de gigabytes de RAM desperdiçados em processos em *background* e, do ponto de vista de segurança, multiplica a "superfície de ataque". A equipe de TI perde horas aplicando patches de segurança em cada VM separadamente.

### 5.2. Arquitetura Proposta: Nuvem e Containerização (Cloud-Native)
*A nova arquitetura elimina o overhead emulatório, adotando o conceito de efemeridade e abstração total de hardware.*

* ☁️ **Infraestrutura Cloud (AWS):** * **O que faz:** Provisão de recursos computacionais distribuídos pela internet (IaaS). O hardware passa a ser responsabilidade da Amazon, gerenciado pela DevStore via painel ou API.
  * **Importância & Impacto:** Transforma CAPEX (compra de servidores) em OPEX (mensalidade sob demanda). Traz a vital capacidade de **Multi-AZ (Zonas de Disponibilidade Múltiplas)**. Se um data center da AWS falhar devido a um desastre natural, a aplicação da DevStore é roteada automaticamente para outro data center a quilômetros de distância, sem queda de serviço.
* 🐧 **Sistema Operacional Hospedeiro (*Host OS*):** * **O que faz:** Um Linux ultra-minimalista (como Amazon Linux 2023 ou Bottlerocket) instanciado na nuvem, cuja única função é rodar o motor de containers.
  * **Importância & Impacto:** Reduz drasticamente a superfície de ataque. Por não possuir interfaces gráficas ou ferramentas desnecessárias, é altamente imune a malwares tradicionais e deixa quase 100% da RAM livre para as aplicações reais da empresa.
* 🐳 **Docker Engine (Motor de Containerização):** * **O que faz:** Em vez de virtualizar hardware como o Hypervisor, o Docker virtualiza o *Sistema Operacional*. Ele utiliza os recursos do kernel do Linux — especificamente *Namespaces* (para garantir que um container não enxergue o outro) e *Cgroups* (para limitar o máximo de CPU e RAM que um container pode usar).
  * **Importância & Impacto:** É o coração da padronização. Ele abstrai toda a infraestrutura subjacente. Para o desenvolvedor da DevStore, não importa se o código vai rodar no seu notebook, em um servidor na Europa ou nos EUA; o Docker garante o mesmo ambiente de execução.
* 🚀 **Containers Leves (Aplicações Imutáveis):** * **O que faz:** Pacotes contendo *apenas* o código compilado da DevStore, a linguagem (ex: Node.js/Python) e as bibliotecas essenciais. Não possuem sistema operacional embutido.
  * **Importância & Impacto:** Adotam a filosofia de *"Cattle, not Pets"* (Gado, não Animais de Estimação). Se um container trava, não perdemos tempo "consertando" ou reiniciando como faríamos com um servidor. O orquestrador simplesmente o destrói e cria um clone idêntico em milissegundos. Isso zera a inconsistência de ambientes e permite a integração perfeita com pipelines CI/CD automatizados.

---

## 📈 6. Análise de TCO (Custo Total de Propriedade) e Viabilidade Econômica


#### Gráficos de Apoio Financeiro


<img width="2214" height="1115" alt="Image" src="https://github.com/user-attachments/assets/d1824644-b7c6-47e6-9f81-1e20e48d7aab" />



<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/ff017c8b-2900-4d1d-9930-3167b928c38b" />


---

### 7.1. Gestão de Identidade e Acesso (IAM)

A primeira camada de defesa é o controle granular de quem pode fazer o quê dentro do ambiente AWS:

* **Princípio do Menor Privilégio (PoLP):** cada membro da equipe recebe apenas as permissões mínimas necessárias para sua função. Um desenvolvedor front-end não tem acesso ao banco de dados de produção.
* **Eliminação de credenciais root compartilhadas:** a conta root da AWS é selada e usada somente para operações de emergência com MFA (Multi-Factor Authentication) obrigatório.
* **Roles por serviço:** cada serviço AWS (EC2, ECS, Lambda) opera com uma IAM Role exclusiva, impedindo que um serviço comprometido acesse recursos de outro.
* **Rotação automática de secrets:** senhas e chaves de API armazenadas no AWS Secrets Manager com rotação automática a cada 30 dias.

---

### 7.2. Segurança de Perímetro e Rede (VPC)

Toda a infraestrutura da DevStore será provisionada dentro de uma **Virtual Private Cloud (VPC)** isolada, seguindo o modelo de sub-redes em camadas:

* **Sub-rede pública:** contém apenas o Application Load Balancer (ALB), que recebe o tráfego externo via HTTPS/TLS 1.3.
* **Sub-rede privada:** onde residem os containers da aplicação (ECS/EC2). Inacessíveis diretamente pela internet.
* **Sub-rede isolada:** reservada ao banco de dados RDS. Sem rota para a internet, acessível apenas pelos containers da camada de aplicação.
* **Security Groups como firewalls stateful:** regras explícitas de entrada/saída por porta e protocolo. Nenhuma porta aberta por padrão (*deny-all* implícito).
* **WAF (Web Application Firewall):** camada adicional no ALB para bloquear ataques OWASP Top 10 (SQL Injection, XSS, DDoS volumétrico).

---

### 7.3. Segurança de Imagens Docker (Supply Chain Security)

Um container comprometido na origem representa um risco crítico. As seguintes práticas garantem a integridade da cadeia de fornecimento de software:

* **Trivy (scanner de vulnerabilidades):** executado automaticamente na pipeline CI/CD. Bloqueia o deploy caso encontre CVEs de severidade `CRITICAL` ou `HIGH` nas imagens Docker.
* **Amazon ECR com scan automático:** o repositório de imagens realiza análise de vulnerabilidades a cada `push`, usando dados do banco NVD (National Vulnerability Database).
* **Imagens base minimalistas:** uso de imagens `distroless` ou `alpine`, eliminando shells, package managers e binários desnecessários que aumentam a superfície de ataque.
* **Assinatura de imagens (Docker Content Trust):** garante que apenas imagens verificadas e assinadas pela equipe de DevOps sejam aceitas em produção.

---

### 7.4. Observabilidade Contínua (Stack de Monitoramento)

A observabilidade vai além de saber se o serviço está no ar. O objetivo é ter visibilidade total sobre o comportamento interno dos sistemas **antes** que falhas afetem os usuários:

| Ferramenta | Função | O que monitora na DevStore |
| :--- | :--- | :--- |
| **Prometheus** | Coleta de métricas | CPU/RAM por container, latência de API, taxa de erros HTTP |
| **Grafana** | Visualização e dashboards | Painéis em tempo real de throughput, saturação e disponibilidade |
| **AWS CloudWatch** | Logs centralizados | Logs de aplicação, eventos de segurança IAM, alarmes automáticos |
| **AWS CloudTrail** | Auditoria de API | Registro imutável de todas as ações realizadas na conta AWS |
| **PagerDuty / SNS** | Alertas e on-call | Notificação da equipe em caso de anomalia crítica (p99 > 2s) |

> **⚡ Alerta preditivo:** o Prometheus será configurado com regras de alerting que disparam quando o uso de CPU ultrapassa **70% por mais de 5 minutos consecutivos** — antes de atingir o limite crítico de 90%. Isso permite escalonamento proativo via Auto Scaling Group.
---

## 📊 8. Síntese Técnica Comparativa

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/9332f1a6-31d6-45aa-8dcc-213f5d3e26df" />
---

## 🚀 9. Conclusão e Roadmap de Implementação


A migração da DevStore será executada em **quatro fases sequenciais**, cada uma com entregáveis mensuráveis e critérios de aceite claros. O modelo incremental garante que a operação do negócio nunca seja interrompida durante a transição.

---

### 📅 Semana 1 — Fundação de Código e Containerização
> **Objetivo:** garantir que toda a aplicação roda em containers localmente antes de tocar na nuvem.

1. Migração de artefatos para repositório Git centralizado (GitHub). Definição de branching strategy: `main` (produção), `develop`, `feature/*`.
2. Inventário de serviços: mapear todos os microserviços existentes, dependências externas, variáveis de ambiente e segredos.
3. Escrita dos `Dockerfiles` para cada serviço seguindo boas práticas: multi-stage build, imagem base `alpine`, usuário não-root.
4. Criação do `docker-compose.yml` para orquestração local completa (app + banco + cache).
5. **Validação:** todos os serviços sobem via `docker-compose up` e passam nos testes de smoke.

> ✅ **Entregável:** repositório Git com Dockerfiles validados, docker-compose funcional e documentação de variáveis de ambiente (sem secrets em plain text).

---

### 📅 Semana 2 — Provisionamento da Infraestrutura AWS (IaC)
> **Objetivo:** ter o ambiente de produção provisionado como código, reproduzível e versionável.

1. Criação da conta AWS com MFA e configuração de IAM Roles por serviço (princípio do menor privilégio).
2. Escrita dos arquivos Terraform para: VPC, subnets públicas/privadas/isoladas, Security Groups, ALB e target groups.
3. Provisionamento do Amazon ECR (repositório de imagens Docker) e do RDS (banco de dados gerenciado com backup automático).
4. Configuração do ECS Fargate ou EC2 com Auto Scaling Group para hospedar os containers.
5. **Validação:** infraestrutura destruída e recriada via `terraform destroy && terraform apply` sem erros.

> ✅ **Entregável:** código Terraform no repositório com estado remoto no S3 + DynamoDB (lock). Diagrama de rede VPC atualizado.

---

### 📅 Semana 3 — Pipeline CI/CD e Ambiente de Staging
> **Objetivo:** automatizar completamente o ciclo de build, teste e deploy.

1. Configuração do GitHub Actions com workflows de: (1) lint + testes unitários a cada PR, (2) build e push de imagem Docker para ECR a cada merge em `develop`.
2. Integração do Trivy na pipeline para scan de vulnerabilidades. Deploy bloqueado em caso de CVE `CRITICAL`.
3. Deploy automático para o ambiente de staging (ECS staging) após aprovação no scan de segurança.
4. Execução de testes de integração e carga no staging (ferramenta: `k6` ou `Locust`). Critério de aceite: p95 < 300ms com 100 usuários simultâneos.
5. Configuração do stack de observabilidade: Prometheus + Grafana + CloudWatch com dashboards e alertas.

> ✅ **Entregável:** pipeline CI/CD completa documentada, ambiente de staging validado com relatório de testes de carga.

---

### 📅 Semana 4 — Cutover para Produção e Descomissionamento Legado
> **Objetivo:** migrar o tráfego real para a nova infraestrutura com risco mínimo.

1. Snapshot completo do ambiente legado (backup de banco de dados, configurações e volumes).
2. Execução do **Blue/Green Deployment:** ambiente AWS (Green) recebe 10% do tráfego via weighted routing no Route 53. Monitoramento por 24h.
3. Após validação, roteamento de 100% do tráfego para o ambiente Green. Ambiente legado (Blue) mantido em standby por 7 dias.
4. **Verificação dos critérios de Go/No-Go:** zero erros 5xx, latência p95 < 300ms, todos os alertas do Grafana no verde.
5. Descomissionamento formal do ambiente On-Premise após período de estabilização. Documentação do post-mortem da migração.

> ✅ **Entregável:** produção 100% na AWS, legado descomissionado, runbook de operações e plano de resposta a incidentes documentados.

---

### 📊 Resumo do Roadmap

<img width="1004" height="627" alt="Image" src="https://github.com/user-attachments/assets/5de08897-b3f4-4056-89bc-b1fda4a869d6" />
---

*Relatório fundamentado em: AWS Well-Architected Framework · CNCF · OWASP Top 10 · NIST Cybersecurity Framework*
