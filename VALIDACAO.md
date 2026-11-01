# Validação da entrega — 01/11/2026

- `npm run check`: aprovado, sintaxe de `app.js` e `core.js`.
- `npm test`: 13 testes aprovados, nenhuma falha.
- Navegador: login de cliente, pedido compartilhado, revisão, publicação, entrada de dois NPCs, propostas, rateio, pagamento simulado, chat de horário, coleta e entrega por código concluídos.
- Navegador: login de motorista, proposta para pedido individual de bicicleta, aceitação do cliente NPC, chat de endereço, coleta e entrega por código concluídos.
- Persistência: pedido e sessão preservados após recarregar a página.
- Responsividade: dashboard inspecionado em 1440 × 1000 e 390 × 844; sem transbordamento horizontal nesses tamanhos.
- Imagens: retratos exibidos no navegador; fallback local disponível.

Os testes ocorreram no servidor local incluído. A entrega não foi publicada em provedor externo. Não foram realizadas transações, validações documentais, testes de autenticação real, tráfego concorrente ou homologação de transporte. Veja o README para limites de simulação e próximos passos.
