# Sistema de Cadastro de Parceiros com Google Apps Script

## 📜 Descrição

Aplicação web desenvolvida com Google Apps Script para automatizar e otimizar o processo de cadastro de parceiros (corretores de imóveis e imobiliárias) para uma construtora.

Este projeto nasceu da necessidade de um cliente que enfrentava dificuldades com o processo manual via Google Forms: validação de dados limitada, risco de cadastros duplicados, e o demorado trabalho de transferir informações para o Google Sheets e adicionar contatos manualmente no Google Contacts e em grupos de WhatsApp.

Esta solução oferece um formulário web customizado com validações robustas, integração direta com Planilhas e Contatos Google, e redirecionamento para WhatsApp, tornando o fluxo mais eficiente e confiável.

## ✨ Funcionalidades Principais

* **Formulário HTML Customizado:** Interface web para entrada de dados de Corretores ou Imobiliárias, servida via Google Apps Script (`doGet`).
* **Validação de Dados:** Regras de validação direto no HTML (ex: CPF, CNPJ, formato de telefone, campos obrigatórios).
* **Registro no Google Sheets:** Salva os dados submetidos em abas separadas (`Lista de Corretores`, `Lista de Imobiliarias`) na Planilha Google associada, incluindo um timestamp do cadastro.
* **Verificação de Duplicados (Planilha):** Checa se o telefone já existe na respectiva planilha antes de adicionar um novo registro.
* **Integração com Google Contacts (People API):**
    * Verifica se já existe um contato com o telefone informado.
    * Cria um novo contato automaticamente caso não exista, incluindo nome formatado (Ex: "Corretor CRECI NOME"), telefone, e-mail, aniversário (para corretores), etc.
* **Redirecionamento Pós-Cadastro:** Após o envio bem-sucedido, exibe uma mensagem de sucesso e um botão/link para entrar em um grupo específico do WhatsApp.

## ⚙️ Tecnologias Utilizadas

* **Google Apps Script (JavaScript)**
* **HTML5**
* **CSS3**
* **Google Sheets API** (nativa via `SpreadsheetApp`)
* **Google People API** (via Serviços Avançados do Apps Script)

## 🚀 Configuração e Implantação

Para utilizar ou adaptar este projeto, siga os passos:

1.  **Planilha Google:**
    * Crie uma nova Planilha Google.
    * Crie duas abas (páginas) com os nomes exatos: `Lista de Corretores` e `Lista de Imobiliarias`.
    * Defina as colunas necessárias em cada aba (a ordem importa para o script `salvarNaPlanilha`):
        * `Lista de Corretores`: `Data/Hora`, `Nome`, `Cpf`, `Telefone`, `E-mail`, `Creci`, `Imobiliaria`, `Nascimento`, `Instagram`
        * `Lista de Imobiliarias`: `Data/Hora`, `Nome`, `Cnpj`, `Endereço`, `Celular`, `Fixo`, `E-mail`, `Creci`, `Instagram`

2.  **Editor de Scripts:**
    * Abra o editor de scripts da planilha (Ferramentas > Editor de scripts).
    * Copie o conteúdo do arquivo `Código.gs` (ou o nome que você deu ao seu arquivo `.gs`) e cole no editor.
    * Crie um arquivo HTML (`Arquivo > Novo > Arquivo HTML`). Nomeie-o **exatamente** como `formulario.html`. Copie e cole o conteúdo do seu HTML neste arquivo.

3.  **Serviços Avançados (API People):**
    * No editor, vá em `Serviços (+)` no menu esquerdo.
    * Encontre e adicione o `Google People API`. Certifique-se de que ele está habilitado.

4.  **Manifesto (`appsscript.json`):**
    * Vá em `Configurações do projeto (⚙️)`.
    * Marque a caixa "**Mostrar arquivo de manifesto 'appsscript.json' no editor**".
    * Clique no arquivo `appsscript.json` que apareceu no editor.
    * Garanta que a seção `enabledAdvancedServices` para a People API existe e adicione/edite a seção `oauthScopes` para incluir as permissões necessárias (copie a lista de escopos que usamos anteriormente). Exemplo:
        ```json
        {
          // ... outras configurações ...
          "dependencies": {
            "enabledAdvancedServices": [{
              "userSymbol": "People",
              "serviceId": "people",
              "version": "v1"
            }]
          },
          "oauthScopes": [
            "[https://www.googleapis.com/auth/contacts](https://www.googleapis.com/auth/contacts)",
            "[https://www.googleapis.com/auth/userinfo.profile](https://www.googleapis.com/auth/userinfo.profile)",
            "[https://www.googleapis.com/auth/script.container.ui](https://www.googleapis.com/auth/script.container.ui)",
            "[https://www.googleapis.com/auth/spreadsheets.currentonly](https://www.googleapis.com/auth/spreadsheets.currentonly)"
           // Adicione outros escopos se seu script precisar
          ]
        }
        ```
    * Salve o arquivo `appsscript.json`.

5.  **Link do WhatsApp:**
    * Dentro do arquivo `formulario.html`, localize a função JavaScript `enviarCadastro`.
    * No bloco `.withSuccessHandler`, atualize o `href` da tag `<a>` com o link correto do seu grupo do WhatsApp.

6.  **Implantação como Aplicativo Web:**
    * Clique em `Implantar > Nova implantação`.
    * Escolha o tipo: `Aplicativo da Web`.
    * Configure:
        * **Descrição:** (Opcional) Ex: Formulário Cadastro Parceiros.
        * **Executar como:** `Eu` (Sua conta Google).
        * **Quem pode acessar:** `Qualquer pessoa` (Recomendado para um formulário público).
    * Clique em `Implantar`.
    * **Autorize** o script concedendo as permissões solicitadas na janela pop-up.
    * Copie a **URL do aplicativo da web** gerada. Este é o link público para o seu formulário.

## 👤 Autor

[Maciel Silva]
* **LinkedIn:** https://www.linkedin.com/in/silvamaciel/
* **GitHub:** https://github.com/silvamaciel/
