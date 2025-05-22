# Integracao-Azure-Data-Factory-com-Azure-DevOps-DIO-
![image](https://github.com/user-attachments/assets/6a4d523f-e94f-4935-8ada-8a7e02d81a9b)

## ⚙️ Funcionamento do Fluxo

Este fluxo foi projetado para garantir **versionamento confiável**, **governança de dados** e **entregas contínuas (CI/CD)** com o Azure Data Factory e Azure DevOps.

---

### 1. 🏁 Configuração Inicial
- **Azure DevOps**:
  - Criação da **organização** e do **projeto** usando uma conta vinculada ao tenant da empresa.
  - Inicialização de um **repositório Git** com `.gitignore` configurado para recursos ARM/ADF.
- **Azure Data Factory**:
  - O ADF é conectado ao repositório Git para controle de versão dos artefatos (pipelines, datasets, linked services, triggers).

---

### 2. 🌿 Estratégia de Branches
- Uso de uma estratégia de Git Flow adaptada para dados:
  - `feature/*` → Desenvolvimento de novos pipelines ou alterações isoladas.
  - `develop` → Homologação/testes integrados.
  - `main` → Produção estável.

- A branch `main` é protegida por **políticas de pull request (PR)**:
  - Revisão obrigatória (code review).
  - Validação automatizada de sintaxe (linting dos JSONs).

---

### 3. 🔗 Integração ADF ↔ DevOps
- No portal do ADF, é feita a **integração com o repositório Git do Azure DevOps**.
- Todo o desenvolvimento ocorre no modo "Git-integrado", onde alterações:
  - São salvas como arquivos `.json`.
  - São commitadas na branch `feature`.
- As alterações não vão diretamente para produção: exigem merge + aprovação via PR.

---

### 4. 🔄 CI/CD Automatizado
- Ao abrir um **pull request**, o Azure DevOps executa o pipeline de validação:
  - **Validação da sintaxe JSON** usando ferramenta como `adf-validator`.
  - Checagem de consistência dos artefatos.
- Após o merge aprovado na branch `main`, um pipeline de **Release** é acionado:
  - Deploy automático dos pipelines para o **Azure Data Factory de produção**.
  - Utilização de scripts PowerShell (ex: `deploy_adf.ps1`) para importar os JSONs via `Import-AzDataFactoryV2Pipeline`.

---

### 5. 💾 Backup & 📊 Monitoramento
- Todos os artefatos (pipelines, datasets, triggers etc.) são versionados no Git e também **armazenados em um Blob Storage** como backup adicional.
- Logs de deploy e execução de pipelines são coletados com:
  - **Azure Monitor**: análise de execução, falhas, latência.
  - **Log Analytics**: centralização e alertas.
- Essa estrutura garante **rastreabilidade completa** e **conformidade com a LGPD e ISO 27001**.

---

✅ **Benefícios da Arquitetura**:
- Padronização do ciclo de vida dos pipelines.
- Deploys reproduzíveis e auditáveis.
- Redução de erros manuais em produção.
- Governança de dados com versionamento e rollback.


---

## 🚦 Fluxo Detalhado


| Etapa | Ação | Resultado |
|------|------|-----------|
| **1. Configuração DevOps** | Crie organização, projeto e repositório Git. | Base para versionamento. |
| **2. Branch Strategy** | `feature` (dev) → `develop` (homolog) → `main` (prod). | Segregação de ambientes. |
| **3. Integração ADF** | Conecte o ADF ao repositório Git. | Commits automáticos de JSON. |
| **4. Desenvolvimento** | Crie/edite pipelines na branch `feature`. | Entregas incrementais. |
| **5. Pull Request** | PR de `feature` → `develop` com reviewers & validações. | Código revisado. |
| **6. Build** | Pipeline de validação executa testes & lint JSON. | Qualidade garantida. |
| **7. Release** | Pipeline YAML faz deploy no ADF de destino. | Publicação sem intervenção. |
| **8. Backup & Monitor** | JSONs salvos no Blob; logs no Azure Monitor. | Rastreamento e conformidade. |


| Tema | Link |
|------|------|
| Criar organização no Azure DevOps | <https://learn.microsoft.com/azure/devops/organizations/accounts/create-organization> |
| Políticas de branch (PR/approvals) | <https://learn.microsoft.com/azure/devops/repos/git/branch-policies> |
| Git Ignore para ARM/ADF | <https://github.com/github/gitignore/blob/main/VisualStudio.gitignore> |
| Integração Git no Azure Data Factory | <https://learn.microsoft.com/azure/data-factory/source-control> |
| ADF Utilities (CLI) | <https://github.com/Azure/data-factory-v2-python-manager> |
| Exemplo de repositório ADF + DevOps | <https://github.com/Azure-Samples/azure-datafactory-cicd-guidance> |
| Cmdlet **Import-AzDataFactoryV2Pipeline** | <https://learn.microsoft.com/powershell/module/az.datafactory/import-azdatafactoryv2pipeline> |
| Azure PowerShell (instalação) | <https://learn.microsoft.com/powershell/azure/install-az-ps> |
| Azure Monitor – Logs & Métricas | <https://learn.microsoft.com/azure/azure-monitor/> |
| Azure Blob Storage (overview) | <https://learn.microsoft.com/azure/storage/blobs/> |
| LGPD – Guia ANPD (PT-BR) | <https://www.gov.br/anpd/pt-br/documentos-e-publicacoes/guias-e-orientacoes> |
| ISO 27001 Overview | <https://www.iso.org/isoiec-27001-information-security.html> |

---

## 📚 Referências Oficiais

- [Azure Data Factory + Git Integration](https://learn.microsoft.com/azure/data-factory/source-control)  
- [Azure DevOps Pipelines](https://learn.microsoft.com/azure/devops/pipelines)  
- [DataOps & CI/CD Guidelines](https://techcommunity.microsoft.com/)  

---


