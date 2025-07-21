# Análise e Identificação do Problema: Anúncios "Pausados" vs "Pausado sem Estoque" no Mercado Livre

## Contexto do Problema

O cliente reportou que, quando o estoque dos seus produtos zera no Magazord, os anúncios no Mercado Livre são marcados como "Pausado" e não como "Pausado sem estoque". Isso faz com que os anúncios permaneçam disponíveis para venda, permitindo pedidos para itens sem estoque real. O cliente precisa cancelar essas vendas, prejudicando sua reputação.

Atualmente, existem 38 anúncios na situação "Pausado" que não foram pausados manualmente pelo cliente. Ao realizar uma atualização manual de estoque (com estoque zero), o anúncio muda corretamente para "Pausado sem estoque" e fica indisponível para venda.

No log, não há registros de envio da informação de estoque zerado quando o estoque se esgota automaticamente no Magazord.

Um exemplo afetado: código do anúncio MLB3097510082.

---

## Objetivo

Descrever os passos para análise e identificação da causa raiz do problema, com base na documentação oficial do Mercado Livre e nas informações disponíveis, mesmo sem acesso direto ao painel do cliente no Seller Center ou ao ERP Magazord.

---

## Passos para Análise e Identificação

### 1. Levantamento e Revisão da Documentação do Mercado Livre

- Revisar a documentação oficial da API do Mercado Livre, especialmente os endpoints relacionados à atualização de anúncios, controle de estoque e status de anúncios.
- Entender o funcionamento esperado da API para situações em que o estoque chega a zero e como o status do anúncio deve ser atualizado automaticamente.
- Verificar as regras específicas do Mercado Livre para status "Pausado" e "Pausado sem estoque" e quais parâmetros ou flags acionam essa distinção.
- Analisar exemplos de payloads para atualização de estoque e status de anúncios.

### 2. Análise dos Logs do Sistema

- Revisar os logs de integração entre Magazord e Mercado Livre para identificar se, no momento em que o estoque do produto zera, está sendo enviada alguma requisição para o Mercado Livre atualizando o estoque para zero.
- Verificar se as requisições que atualizam o estoque possuem o formato e campos corretos conforme a documentação da API Mercado Livre.
- Confirmar se os erros ou exceções de envio estão ocorrendo quando o estoque atinge zero (ex: timeouts, rejeições, erros de validação).

### 3. Validação dos Processos de Sincronização de Estoque

- Confirmar se o Magazord está configurado para enviar atualizações automáticas de estoque para o Mercado Livre sempre que o estoque for alterado.
- Verificar se existe alguma regra ou configuração que impede o envio do estoque zero automaticamente (ex: para evitar suspender anúncios automaticamente).
- Analisar se há algum filtro ou lógica que só envia atualizações para estoque maior que zero.

### 4. Revisão do Código/Implementação da Integração

- Caso possível, analisar a rotina ou serviço responsável por enviar as atualizações de estoque ao Mercado Livre.
- Checar se o código trata corretamente os casos de estoque zero, incluindo o envio da informação e alteração do status do anúncio.
- Verificar se o sistema registra o evento de estoque zerado para disparar a atualização do anúncio como "Pausado sem estoque".

### 5. Testes Manuais e Simulações

- Realizar testes controlados onde o estoque de um produto é zerado no Magazord e monitorar se a requisição de atualização é enviada ao Mercado Livre.
- Testar a atualização manual (que já funciona) para comparar a diferença entre o processo manual e o automático.
- Analisar as diferenças no payload, headers ou parâmetros entre as duas formas de atualização.

### 6. Comunicação com o Cliente

- Confirmar com o cliente se houve alguma alteração recente na configuração do Magazord que possa afetar a sincronização automática.
- Verificar se o cliente ou equipe possui algum procedimento manual para pausar anúncios que possa estar conflitando.
- Obter informações adicionais sobre frequência das atualizações e horário em que os anúncios apresentam o status incorreto.

---

## Possíveis Hipóteses para o Problema

1. **Não envio automático da atualização de estoque quando o estoque zera:**  
   O Magazord pode não estar enviando atualização para estoque zero, por configuração, erro de código ou falha na rotina.

2. **Envio incorreto do payload ou parâmetros errados para a API Mercado Livre:**  
   Pode haver erro no formato do JSON, campos obrigatórios omitidos ou uso incorreto dos valores que impedem a atualização do status para "Pausado sem estoque".

3. **Restrição ou limitação na API do Mercado Livre:**  
   Pode haver limites de requisição, bloqueios ou regras internas do Mercado Livre que impedem atualização automática em determinados casos.

4. **Diferença entre atualização manual e automática:**  
   O processo manual de atualização pode enviar comandos diferentes (ex: flag específica) para marcar "Pausado sem estoque", enquanto o automático não.

5. **Problema de sincronização ou atraso:**  
   A atualização pode estar ocorrendo, mas com atraso ou falha de sincronização entre Magazord e Mercado Livre, causando inconsistência no status exibido.

6. **Falta de registro ou monitoramento do evento de estoque zero no Magazord:**  
   O sistema não está capturando o evento de estoque zerado para disparar a atualização correspondente.

---

## Recomendações

- Implementar monitoramento e alertas para falhas de atualização de estoque zero.  
- Ajustar logs para detalhar envios e respostas da API Mercado Livre no evento de estoque zerado.  
- Validar e corrigir o código que envia atualização automática para garantir que o anúncio seja pausado corretamente.  
- Validar a consistência entre o comportamento manual (funcional) e o automático.  
- Testar em ambiente de homologação usando a sandbox da API Mercado Livre para confirmar o comportamento esperado.  

---