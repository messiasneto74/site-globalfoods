# Plano de implementação — Site Global Foods

## 1. Objetivo

Transformar a apresentação web da Global Foods em um site institucional funcional, com:

- apresentação clara da marca e dos produtos;
- localizador de supermercados;
- consulta de disponibilidade por produto;
- indicação de telefone e disponibilidade de WhatsApp;
- Agente de IA conectado à mesma base de dados;
- caminhos específicos para consumidores e parceiros comerciais.

Este documento é o backlog de referência para organizar o desenvolvimento em milestones e issues.

## 2. Estado atual

- Projeto no ChatGPT/Sites: `Proposta de Site — Global Foods`.
- Publicação atual: [global-foods-proposta.netomessias74.chatgpt.site](https://global-foods-proposta.netomessias74.chatgpt.site).
- A versão atual é uma apresentação interativa/protótipo, não o site institucional final.
- O localizador e o Agente de IA são demonstrações conceituais.
- O site está privado enquanto o conteúdo e as funcionalidades são definidos.
- Não há banco D1, binding ou tabela configurada no projeto neste momento.

## 3. Escopo do MVP

O visitante deverá conseguir:

1. entender o que é a Global Foods;
2. escolher o produto que procura;
3. consultar supermercados que possuem esse produto;
4. visualizar nome, endereço e telefone do supermercado;
5. saber se o telefone também atende pelo WhatsApp;
6. conversar com o Agente de IA para obter a mesma informação;
7. entrar em contato com a Global Foods ou demonstrar interesse como parceiro.

## 4. Regras do produto

### 4.1 Disponibilidade por produto

O sistema não deve usar apenas um campo global `tem_produto`. A disponibilidade pertence à combinação entre supermercado e produto.

Um supermercado pode ter um produto da Global Foods e não ter outro. Toda busca e toda resposta do Agente de IA devem considerar o produto selecionado.

### 4.2 WhatsApp

O cadastro deve informar se o telefone do supermercado também atende pelo WhatsApp. Quando o valor for verdadeiro, o site poderá exibir um botão de conversa.

O sistema não deve afirmar que o número possui WhatsApp quando essa informação não estiver cadastrada.

### 4.3 Agente de IA

O agente deve:

- perguntar qual produto o visitante procura quando essa informação não estiver clara;
- consultar a disponibilidade por produto;
- informar somente dados existentes na base oficial;
- mostrar nome, endereço, telefone e disponibilidade de WhatsApp;
- indicar quando não houver resultado;
- encaminhar o visitante para atendimento humano quando necessário.

O agente não deve inventar produtos, supermercados, endereços, disponibilidade ou contatos.

## 5. Modelo de dados inicial

### 5.1 Tabela `pontos_de_venda`

| Campo | Tipo sugerido | Regra |
|---|---|---|
| `id` | identificador | Chave primária |
| `nome_supermercado` | texto | Obrigatório |
| `endereco` | texto | Obrigatório |
| `telefone` | texto | Obrigatório quando disponível |
| `eh_whatsapp` | sim/não | Obrigatório; indica se o telefone atende pelo WhatsApp |

### 5.2 Tabela `produtos`

| Campo | Tipo sugerido | Regra |
|---|---|---|
| `id` | identificador | Chave primária |
| `nome_produto` | texto | Obrigatório |
| `ativo` | sim/não | Permite retirar produtos sem apagar histórico |

### 5.3 Tabela `disponibilidade`

| Campo | Tipo sugerido | Regra |
|---|---|---|
| `id` | identificador | Chave primária |
| `ponto_de_venda_id` | referência | Liga ao supermercado |
| `produto_id` | referência | Liga ao produto |
| `disponivel` | sim/não | Indica a situação atual |
| `atualizado_em` | data/hora | Mostra quando a informação foi revisada |

Deve existir apenas um registro ativo para cada combinação de supermercado e produto. Se no futuro houver necessidade de controlar estoque, quantidade ou preço, isso será tratado em uma etapa posterior.

## 6. Milestones

| Milestone | Resultado esperado | Depende de |
|---|---|---|
| M0 — Definição do produto | Escopo, públicos, produtos e ações principais aprovados | — |
| M1 — Fundação de dados | Banco, produtos, supermercados e disponibilidade estruturados | M0 |
| M2 — Conteúdo e arquitetura | Apresentação convertida em estrutura de site | M0 |
| M3 — Localizador | Busca funcional por produto e resultados confiáveis | M1, M2 |
| M4 — Agente de IA | Agente consultando a base e respondendo com segurança | M1, M3 |
| M5 — Conversão e confiança | Contatos, parceria, privacidade e chamadas para ação | M2, M3, M4 |
| M6 — Qualidade e lançamento | Testes concluídos, domínio e publicação preparados | M3, M4, M5 |

Um milestone só deve ser considerado concluído quando seus critérios de aceite forem atendidos.

## 7. Backlog de issues

### M0 — Definição do produto

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| GF-001 | P0 | Definir se o produto final será institucional, comercial e localizador | O escopo do MVP está escrito e aprovado |
| GF-002 | P0 | Definir públicos prioritários: consumidor, parceiro, distribuidor ou varejista | Cada público possui objetivo e caminho no site |
| GF-003 | P0 | Definir a ação principal do visitante | Existe uma chamada principal definida para a página inicial |
| GF-004 | P0 | Listar os produtos que estarão disponíveis no MVP | O catálogo inicial está fechado e validado |
| GF-005 | P1 | Definir quem será responsável por atualizar supermercados e disponibilidade | Existe um responsável e uma rotina de atualização |

### M1 — Fundação de dados

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| DB-001 | P0 | Configurar o banco e o binding do projeto | O ambiente possui uma base acessível pelo site |
| DB-002 | P0 | Criar a tabela `pontos_de_venda` | Nome, endereço, telefone e `eh_whatsapp` são armazenados corretamente |
| DB-003 | P0 | Criar a tabela `produtos` | Os produtos do MVP podem ser cadastrados e ativados/desativados |
| DB-004 | P0 | Criar a tabela `disponibilidade` | Cada produto pode ser associado a cada supermercado com status próprio |
| DB-005 | P0 | Impedir combinações duplicadas de supermercado e produto | A base não cria dois registros ativos para a mesma combinação |
| DB-006 | P1 | Cadastrar a primeira base de supermercados, produtos e disponibilidades | Os dados reais de teste estão disponíveis para o localizador |
| DB-007 | P1 | Criar uma forma simples de atualizar os registros | O responsável consegue incluir, editar e desativar dados sem alterar código |
| DB-008 | P1 | Registrar a data da última atualização da disponibilidade | O resultado informa quando a disponibilidade foi revisada |

### M2 — Conteúdo e arquitetura do site

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| SITE-001 | P0 | Substituir o formato de apresentação por uma estrutura de site institucional | O visitante entende que está no site da Global Foods |
| SITE-002 | P0 | Criar uma página inicial com proposta de valor objetiva | A marca, os produtos e a ação principal aparecem sem depender de explicação externa |
| SITE-003 | P1 | Separar os caminhos de consumidor e parceiro comercial | Cada público encontra uma chamada e informação adequada |
| SITE-004 | P1 | Revisar textos, imagens, logo, cores e tom de voz | O conteúdo definitivo foi validado pela Global Foods |
| SITE-005 | P1 | Criar navegação, rodapé e página de contato | Todas as áreas principais são acessíveis pelo menu ou rodapé |

### M3 — Localizador de supermercados

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| LOC-001 | P0 | Criar seletor de produto | O visitante consegue escolher um produto cadastrado |
| LOC-002 | P0 | Buscar disponibilidade por produto | A consulta retorna somente supermercados associados ao produto selecionado e marcados como disponíveis |
| LOC-003 | P0 | Exibir nome, endereço e telefone do supermercado | Cada resultado mostra os quatro dados necessários ao visitante |
| LOC-004 | P0 | Exibir se o telefone possui WhatsApp | O resultado mostra essa informação sem inferência |
| LOC-005 | P1 | Criar botão de WhatsApp quando `eh_whatsapp` for verdadeiro | O botão aparece apenas para números marcados como WhatsApp |
| LOC-006 | P1 | Criar estados sem resultado, erro e carregamento | O visitante recebe orientação clara em todos os estados |
| LOC-007 | P1 | Criar filtros complementares de localização quando necessários | A busca continua simples e reduz resultados irrelevantes |

### M4 — Agente de IA Global Foods

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| IA-001 | P0 | Definir nome, função e personalidade do agente | O agente possui uma apresentação oficial aprovada |
| IA-002 | P0 | Criar a base de conhecimento institucional | O agente utiliza apenas informações oficiais e revisadas |
| IA-003 | P0 | Conectar o agente à consulta de disponibilidade | O agente consulta a mesma base usada pelo localizador |
| IA-004 | P0 | Fazer o agente pedir o produto quando ele não for informado | O agente não realiza busca genérica que possa gerar falso positivo |
| IA-005 | P0 | Responder com supermercado, endereço, telefone e WhatsApp | A resposta reproduz os dados encontrados na base |
| IA-006 | P0 | Criar respostas para produto sem disponibilidade | O agente informa que não encontrou resultado e oferece próximo caminho |
| IA-007 | P0 | Criar limite contra informações inventadas | O agente declara quando não possui informação suficiente |
| IA-008 | P1 | Encaminhar para atendimento humano ou WhatsApp da Global Foods | O visitante possui uma saída clara quando a automação não resolve |
| IA-009 | P1 | Testar perguntas reais de consumidores e parceiros | Os cenários principais passam sem respostas incorretas |
| IA-010 | P2 | Registrar perguntas frequentes e falhas para melhoria | Existe uma forma de revisar o desempenho do agente |

### M5 — Conversão, confiança e privacidade

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| CONV-001 | P0 | Criar chamadas para encontrar produto e falar com a Global Foods | As ações principais são visíveis nas áreas relevantes |
| CONV-002 | P1 | Criar caminho para parceiros, distribuidores e varejistas | O parceiro sabe como demonstrar interesse |
| CONV-003 | P1 | Adicionar WhatsApp, e-mail ou formulário oficial da Global Foods | Os canais de contato foram validados |
| CONV-004 | P1 | Criar política de privacidade e consentimentos necessários | O site explica o uso de dados antes de coletá-los |
| CONV-005 | P1 | Adicionar informações de confiança validadas | Certificações, números e afirmações possuem fonte ou aprovação |

### M6 — Qualidade e lançamento

| ID | Prioridade | Issue | Critério de aceite |
|---|---:|---|---|
| QA-001 | P0 | Testar navegação, localizador e agente em celular e computador | Os fluxos principais funcionam nos tamanhos de tela definidos |
| QA-002 | P0 | Testar produtos disponíveis, indisponíveis e sem cadastro | O sistema apresenta resultados corretos em todos os casos |
| QA-003 | P0 | Testar telefone e links de WhatsApp | Cada ação abre o destino correto quando cadastrada |
| QA-004 | P1 | Revisar acessibilidade básica e legibilidade | Contraste, teclado, textos e mensagens estão adequados |
| QA-005 | P1 | Configurar SEO, título, descrição e compartilhamento | O site possui metadados básicos e prévia correta |
| QA-006 | P0 | Configurar domínio e acesso público quando aprovado | O endereço definitivo abre o site publicado |
| QA-007 | P1 | Configurar métricas essenciais | Visitas, buscas e contatos podem ser acompanhados respeitando a privacidade |
| QA-008 | P1 | Documentar atualização do banco e operação do agente | Existe um procedimento para manter dados e respostas atualizados |

## 8. Critérios de aceite do MVP

O MVP estará pronto quando:

- o visitante puder escolher um produto;
- o sistema retornar apenas supermercados com disponibilidade daquele produto;
- cada resultado mostrar nome, endereço, telefone e indicação de WhatsApp;
- o Agente de IA fizer a mesma consulta sem inventar dados;
- houver tratamento para produto inexistente, indisponível e sem informação;
- os responsáveis conseguirem atualizar produtos, supermercados e disponibilidade;
- os fluxos principais funcionarem em celular e computador;
- privacidade, contatos, domínio e publicação estiverem validados.

## 9. Dependências e decisões pendentes

- lista final de produtos do MVP;
- dados reais dos supermercados;
- responsável pela atualização da base;
- canal oficial de atendimento humano;
- nome definitivo do Agente de IA;
- definição de acesso público ao site;
- domínio definitivo;
- necessidade de mapa ou apenas lista de resultados;
- forma de administração do banco.

## 10. Ordem recomendada de execução

1. Concluir M0 e aprovar o escopo.
2. Criar o banco e cadastrar dados reais de teste em M1.
3. Estruturar o site institucional em M2.
4. Implementar e testar o localizador em M3.
5. Conectar e avaliar o Agente de IA em M4.
6. Finalizar conversão, privacidade e contatos em M5.
7. Executar QA e publicar em M6.

As issues P0 devem ser concluídas antes das issues P1. Issues P2 ficam para melhorias após o MVP estar funcionando.
