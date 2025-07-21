## Casos de Teste para Validação de Campos Cadastrais (Incluindo CPF)

### **CT01 - Validação do Campo Nome Completo**
- **Descrição**: Verificar se o sistema aceita apenas nomes completos válidos.  
- **Pré-condições**: Usuário está na tela de alteração de cadastro.  
- **Passos**:  
  1. Inserir um nome válido (ex: "Ana Silva").  
  2. Inserir um nome com 1 caractere (ex: "A").  
  3. Inserir caracteres especiais ou números (ex: "João @123").  
- **Resultados Esperados**:  
  1. Aceito.  
  2. Mensagem de erro: "Nome deve conter pelo menos 2 caracteres".  
  3. Mensagem de erro: "Nome não pode conter caracteres especiais ou números".  

---

### **CT02 - Validação do Campo CPF (Novo)**
- **Descrição**: Verificar se o sistema valida CPF no formato correto (11 dígitos, válido ou máscara).  
- **Pré-condições**: Usuário está na tela de alteração de cadastro.  
- **Passos**:  
  1. Inserir CPF válido com máscara (ex: "408.199.938-40").  
  2. Inserir CPF válido sem máscara (ex: "40819993840").  
  3. Inserir CPF inválido (ex: "111.111.111-11").  
  4. Inserir menos de 11 dígitos (ex: "123").  
  5. Deixar o campo vazio.  
- **Resultados Esperados**:  
  1 e 2. Aceitos (com ou sem máscara).  
  3. Mensagem de erro: "CPF inválido".  
  4. Mensagem de erro: "CPF deve ter 11 dígitos".  
  5. Mensagem de erro: "CPF é obrigatório" (se for campo obrigatório).  

---

### **CT03 - Validação do Campo E-mail**
- **Descrição**: Verificar se o sistema valida o formato do e-mail.  
- **Passos**:  
  1. Inserir e-mail válido (ex: "usuario@exemplo.com").  
  2. Inserir e-mail sem "@" (ex: "usuarioexemplo.com").  
  3. Inserir e-mail com espaços (ex: "usu ario@exemplo.com").  
- **Resultados Esperados**:  
  1. Aceito.  
  2 e 3. Mensagem de erro: "E-mail inválido".  

---

### **CT04 - Validação do Campo Telefone**
- **Descrição**: Validar formato e obrigatoriedade do telefone.  
- **Passos**:  
  1. Inserir telefone válido (ex: "(11) 98765-4321").  
  2. Inserir número incompleto (ex: "119876").  
  3. Inserir caracteres não numéricos (ex: "(11) ABCD-1234").  
- **Resultados Esperados**:  
  1. Aceito.  
  2 e 3. Mensagem de erro: "Telefone inválido".  

---

### **CT05 - Validação do Campo Data de Nascimento**
- **Descrição**: Verificar datas válidas e futuras.  
- **Passos**:  
  1. Inserir data válida (ex: "01/01/1990").  
  2. Inserir data futura (ex: "01/01/2030").  
  3. Inserir formato inválido (ex: "32/13/1990").  
- **Resultados Esperados**:  
  1. Aceito.  
  2. Mensagem de erro: "Data não pode ser futura".  
  3. Mensagem de erro: "Data inválida".  

---

### **CT06 - Validação do Campo Endereço (Rua, Cidade, Estado, CEP)**
- **Descrição**: Validar subcampos do endereço.  
- **Passos**:  
  1. Inserir CEP válido (ex: "12345-678").  
  2. Inserir CEP incompleto (ex: "123").  
  3. Deixar cidade ou estado vazios.  
- **Resultados Esperados**:  
  1. Aceito.  
  2. Mensagem de erro: "CEP inválido".  
  3. Mensagem de erro: "Cidade/Estado são obrigatórios".  

---

### **CT07 - Validação de Campos Obrigatórios**
- **Descrição**: Impedir salvamento com campos obrigatórios vazios.  
- **Passos**:  
  1. Deixar "CPF" ou "Nome" vazios e tentar salvar.  
  2. Preencher todos os campos corretamente e salvar.  
- **Resultados Esperados**:  
  1. Mensagem de erro: "Preencha todos os campos obrigatórios".  
  2. Mensagem de sucesso: "Cadastro atualizado".  

---

### **CT08 - Persistência dos Dados Alterados**
- **Descrição**: Verificar se as alterações são salvas corretamente.  
- **Passos**:  
  1. Alterar o CPF para um valor válido (ex: "987.654.321-00") e salvar.  
  2. Recarregar a página e verificar se o CPF foi atualizado.  
- **Resultados Esperados**:  
  1 e 2. CPF exibido corretamente após recarregar.  
