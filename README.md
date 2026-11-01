# RachaFrete 2.0 — ambiente de demonstração

Evolução do protótipo original para fretes individuais e compartilhados. Interface responsiva, em português, feita com HTML, CSS e JavaScript, sem dependências de produção e sem etapa de compilação.

## Abrir e publicar

**Mais rápido:** extraia o ZIP e abra `index.html` em um navegador moderno. Para armazenamento consistente e testes de hospedagem, prefira HTTP:

```sh
npm start
```

Com Node.js 20 ou superior, abra `http://127.0.0.1:4173`. Não precisa executar `npm install`.

Para publicar em uma hospedagem estática, envie `index.html`, `styles.css`, `core.js`, `app.js` e a pasta `assets/` para a mesma pasta pública. O `index.html` deve ficar na raiz publicada. Não é necessário subir `tests/`, `server.cjs` ou `package.json`. Pode ser usado em hospedagem estática com HTTPS; os caminhos são relativos, inclusive para subpastas. Esta entrega não foi publicada automaticamente.

## O que está implementado

- Perfis de cliente e motorista, entrada rápida e criação de perfis fictícios que podem ser reabertos na tela de login.
- Dashboard, busca por cidade/bairro/carga, categorias, filtros de status e histórico.
- Pedido com origem, destino, data, horário, distância estimada, descrição, quantidade, peso, volume e observações.
- Modalidade individual ou compartilhada, com grupos de 2 a 4 clientes.
- Entrada e saída de grupos abertos; novas cargas invalidam propostas antigas para recálculo.
- Rateio proporcional ao volume, conservando o total em centavos. Reserva fecha o grupo e fixa o valor.
- Motoristas compatíveis por categoria, volume e peso; envio de propostas e aceitação pelo cliente.
- NPCs clientes e motoristas com nomes fictícios, retratos ilustrativos, capacidades e avaliações de exemplo.
- NPCs entram em grupos, enviam propostas, aceitam valores compatíveis ou solicitam revisão de ofertas acima de 135% da referência.
- Chat contextual por regras para horário, rota, preço, documentos e acesso; respostas após aproximadamente 1,8 segundo.
- Reserva e pagamento fictícios, confirmação de coleta, código de entrega, avaliação e cancelamento antes do transporte.
- Painel para pausar a geração automática de propostas/participantes, exportar JSON e restaurar exemplos.
- Persistência em `localStorage`; imagens com fallback local se a fonte externa falhar.

## Roteiro de teste

### Cliente → motorista NPC

1. Entre como Marina ou crie um cliente.
2. Solicite um frete, informe a carga e endereços fictícios diferentes, escolha uma data futura.
3. Escolha individual ou compartilhado; confirme os requisitos e publique.
4. Com a simulação ativa, aguarde alguns segundos. Em grupos, NPCs entrarão até o limite enquanto houver capacidade. Propostas serão recalculadas a cada entrada.
5. Compare as propostas e selecione uma; revise o rateio e simule a reserva.
6. Use o chat: “Qual horário?”, “Quais documentos?” ou “Qual o valor?”.
7. Clique em “Simular coleta pelo NPC”, anote o código mostrado e simule a entrega usando esse código.
8. Avalie a entrega; veja o histórico em Meus fretes.

### Motorista → cliente NPC

1. Troque para Rafael, motorista de furgão.
2. Abra uma oportunidade, como a bicicleta, e envie sua proposta.
3. O NPC aceita um valor dentro do orçamento. Para testar negociação, ofereça mais de 135% da estimativa; ele pedirá revisão e permitirá nova oferta.
4. Confirme a coleta e converse com o cliente NPC.
5. Para entregar, use o código fornecido pelo cliente NPC no painel de teste.
6. Confira o recebimento fictício no dashboard.

### Grupo existente e alternância de perfis

Como cliente, entre no grupo de sofá e declare carga da mesma categoria, pontos e horário. O organizador NPC aceita uma proposta compatível. O participante pode avançar o motorista NPC usando os controles de simulação. Para testar duas pessoas manuais, crie outro perfil e use “Trocar perfil” no mesmo navegador e na mesma aba.

## Categorias e documentos

Móveis, eletrodomésticos, caixas, bicicletas, motos, automóveis e cães/gatos. O pedido trabalha com uma categoria e volumes iguais por vez; cargas mistas devem ser descritas como um lote da categoria, com o volume/peso total correto, ou separadas em pedidos.

Pets, motos e automóveis ficam apenas no individual neste protótipo. Esta é uma restrição de produto para os testes, não uma afirmação de proibição legal geral. Automóveis recebem propostas de plataforma; pets, de veículo especializado. Outros animais, animais silvestres/de produção, viagens internacionais, cargas perigosas e itens ilícitos não são oferecidos.

A caixa de confirmação registra uma declaração, **não valida documentos**. Para animais, o guia considera trânsito nacional brasileiro de cães e gatos: atestado sanitário veterinário e medidas sanitárias aplicáveis, incluindo imunização antirrábica; o MAPA informa dispensa de GTA para essas espécies. As exigências locais e da operação precisam ser verificadas por profissional responsável.

Fontes oficiais consultadas em 01/11/2026:

- [MAPA — Cães e gatos](https://www.gov.br/agricultura/pt-br/assuntos/sanidade-animal-e-vegetal/saude-animal/cgtqa/t_nacional/caes-e-gatos)
- [ANTT — Requisitos do RNTRC](https://www.gov.br/antt/pt-br/assuntos/cargas/rntrc-1/requisitos)

## Dados e limites do protótipo

Não existe backend, autenticação real, senha, conta financeira, PIX, custódia, seguro, geocodificação, GPS, upload/verificação de documentos nem contratação efetiva. Não coloque dados pessoais sensíveis ou documentos reais. O armazenamento pode ser lido e alterado pelas ferramentas do navegador. Os controles por perfil são comportamentos de interface, não barreiras de segurança.

Os NPCs usam regras determinísticas e temporizadores, não IA generativa ou pessoas reais. Executam enquanto a página está aberta; respostas podem atrasar em abas em segundo plano. O aplicativo evita atualizar a tela enquanto você preenche um campo ou mantém um modal aberto. “Pausar” interrompe novos participantes/propostas; mensagens enviadas no chat continuam recebendo respostas contextuais.

Cada navegador/origem possui uma base independente. Não há sincronização multiusuário ou entre abas; use uma aba por teste. O JSON exportado é um relatório, sem função de importação nesta versão. O servidor local é apenas para desenvolvimento e escuta em loopback.

Distâncias são informadas manualmente. O preço é uma fórmula demonstrativa (base + km + volume + adicional de categoria), sem referência de mercado ou garantia de economia. Compartilhamento exige os mesmos pontos, horário e categoria neste teste; não há otimização de múltiplas paradas. Há um código de entrega por pedido/grupo; uma operação real precisa de confirmações separadas por destinatário. A disponibilidade de agenda dos motoristas não é controlada.

## Retratos e recursos externos

Retratos carregados de [Random User](https://randomuser.me/), com nomes e atividades fictícios; não representam cadastro ou vínculo das pessoas retratadas. A ilustração de rota, ícones e fallback de avatar são SVG locais. Fontes DM Sans e Manrope são carregadas do Google Fonts, com fontes do sistema como alternativa. Sem internet, o app mantém os fluxos locais e usa o fallback de imagens/fontes. Não é um PWA offline: não há service worker ou instalador nesta versão.

## Estrutura e validação

```text
index.html       entrada
styles.css       interface e layouts responsivos
core.js          domínio, regras, cálculos, dados e NPCs
app.js           interface, formulários, persistência e eventos
assets/          marca e avatar alternativo
tests/           testes de domínio
server.cjs       servidor local opcional
```

```sh
npm run check
npm test
```

Os testes cobrem permissões de papel, jornada completa, código de entrega, valores do rateio, limite de grupo, recálculo após entrada, compatibilidade de veículo, documentação declarada, NPCs e chat. Verificações de navegador complementam esses testes; não substituem testes de segurança ou homologação de produção.

## Próxima etapa para operação real

Separar a persistência em uma API com banco de dados e controle transacional de vagas/propostas; implementar autenticação e autorização no servidor; integrar mapas/roteamento e agenda; criar validação de transportadores, documentação e seguros; integrar pagamento e repasse com provedor; registrar entrega por destinatário; implementar privacidade, retenção, moderação, suporte e revisão jurídica conforme a operação. O domínio em `core.js` serve como ponto de partida, mas as regras precisam ser novamente validadas no servidor.
