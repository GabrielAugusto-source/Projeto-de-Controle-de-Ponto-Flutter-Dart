# Sistema Multiplataforma de Controle de Ponto e Horas de Ordem de Serviço (OS) 

---

## Visão Geral do Projeto

Aplicação multiplataforma (**Android** e **Windows Desktop**) desenvolvida em **Flutter** para registro de jornada de trabalho (Entrada, Pausa, Retomada e Saída) vinculada a Ordens de Serviço (OS), com persistência local offline-first e sincronização automatizada em nuvem diretamente para planilhas estruturadas no **Google Sheets / Google Drive** via **Google Apps Script**.

O sistema foi projetado para eliminar apontamentos manuais em papel, garantir rastreabilidade por colaborador e por OS, e centralizar os relatórios gerenciais em tempo real sem custos de infraestrutura de servidor dedicado.

---

## Stack Tecnológica

| Camada | Tecnologias Utilizadas |
| :--- | :--- |
| **Front-end / Mobile & Desktop** | Flutter (Dart 3.14+), Material Design, `shared_preferences`, `http` |
| **Build Nativo Windows** | CMake, MSVC / Visual Studio Build Tools (C++ Desktop Workload), Runner Win32 customizado |
| **Backend Serverless & API** | Google Apps Script (`doPost` / `doGet` Web App REST JSON) |
| **Armazenamento em Nuvem** | Google Sheets API (`SpreadsheetApp`) & Google Drive API (`DriveApp`) |
| **Ferramentas de Validação** | PowerShell (`Invoke-RestMethod`), DevTools, testes automatizados de payload JSON |

---

## Arquitetura e Fluxo de Dados

```text
+-----------------------------------+
|   App Flutter (Android / Windows) |
|  - Autenticacao & Sessao Local    |
|  - Apontamento de Ponto + OS      |
|  - Fila / Persistencia Local      |
+-----------------+-----------------+
                  |
                  | HTTPS POST (JSON + Token de Seguranca)
                  v
+-----------------------------------+
|   Google Apps Script (Web App)    |
|  - Validacao de Token             |
|  - Parse e Tratamento de Linhas   |
|  - Roteamento por Colaborador     |
+-----------------+-----------------+
                  |
                  | SpreadsheetApp / DriveApp
                  v
+-----------------------------------+
|   Google Drive (/Relatorios_OS)   |
|  - Criacao automatica de planilha |
|  - Append sequencial de registros |
|  - Exportacao nativa para .xlsx   |
+-----------------------------------+
