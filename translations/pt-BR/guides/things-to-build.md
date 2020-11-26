---
title: Construir Aplicações com AiBlocks
---

Construa coisas bacanas no AiBlocks! Esta lista esboça algumas ideias para aplicações. Sinta-se à vontade para adicionar a suas próprias ideias ou pegar uma daqui e mandar ver.
Como sempre, se precisar de ajuda para construir algo com AiBlocks, é só perguntar no [chat do Slack](http://slack.aiblocks.io/), [IRC](https://kiwiirc.com/client/irc.freenode.net/#aiblocks-dev), ou no e-mail developers@aiblocks.io.

Se você não está buscando um projeto mais completo mas ainda quer dar uma ajuda, procure por issues com o rótulo `help wanted` em qualquer um de nossos [repositórios](https://github.com/aiblocks).

# Slack Bot
- Reportar um stream de todas as transações AiBlocks em um canal.
- *Avançado*: Permitir que pessoas enviem dinheiro/ponto/"+1"s a outro membros da equipe no Slack, ao estilo de `/send @zezinho $5`.

# API Mashups
- Twilio com AiBlocks: alertas SMS para transações (veja um exemplo [aqui](https://github.com/aiblocks/aiblocks-sms-client))
- Twitter com AiBlocks: envie dinheiro com tweets ou alertas do Twitter
- Reddit com AiBlocks: Tipbot! (um bot que dá gorjetas por upvotes)
- Muitas outras possibilidades

# Gráfico de dados do Millennium
Um projeto relativamente simples que exibe graficamente informações retiradas do Millennium e que poderia consultar contas e transações. Seria também legal ver:
 - Árvore de criação de contas. Todas as contas são criadas por outras contas, então seria possível mostrar essa árvore genealógica.
 - Gráfico das informações do ledger header ao longo do tempo:
   - Número de transações
   - Número de operações
   - Horas de fechamento do ledger
   - Pool de tarifas

# Serviço Federation
Implementar um [servidor Federation ](https://www.aiblocks.io/developers/guides/concepts/federation.html) simples e configurar uma página web onde qualquer pessoa possa pegar um endereço AiBlocks do tipo nome*seudomínio.com e associar o ID de sua conta AiBlocks a esse endereço. O truque é que esse serviço apenas irá federar para contas que definirem seu [destino de inflação](https://www.aiblocks.io/developers/guides/concepts/inflation.html) como uma conta fornecida pelo seu domínio.

Você também pode contribuir ao [servidor federation ](https://github.com/aiblocks/go/tree/master/services/federation) mantido pela AiBlocks Development Foundation.

# Delos para qualquer endereço de e-mail
Permitir que qualquer pessoa envie delos de seu cliente AiBlocks para qualquer endereço de e-mail. A ideia é simplesmente inserir algo como `<endereçodeemail>*dominio.com` e já dar para enviar delos ao endereço. Se o recipiente ainda não tiver uma conta AiBlocks, será criada uma para ele e ele receberá um e-mail avisando que possui delos.

Isso seria um serviço hospedado em `dominio.com` que faz o seguinte:
- Roda um servidor federation.
- Irá federar endereços de pagamento com um prefixo de e-mail como `jed@aiblocks.io*dominio.com`.
- Se houver uma request de federation para um endereço que você não conheça que comece com um e-mail válido:
  - Gera um par de chaves
  - Retorna a chave pública gerada como o accountID
  - Vigiar a rede para ver se a conta foi criada.
  - Se a conta tiver sido criada, você envia um e-mail para o endereço de e-mail dado com a chave privada e links a um cliente AiBlocks.

*Avançado*: permitir que pessoas gerenciem a conta AiBlocks que você acabou de criar por meio de e-mails enviados a control@domínio.com. Isso torna do inbox de alguém um cliente AiBlocks. Por exemplo: `enviar 100 DLO a zezinho@gmail.com`

[Adicionar isso a uma wallet](https://galactictalk.org/d/37-project-idea-sending-delos-to-any-address)

# Exchange Distribuída
Descrição e discussão [aqui.](https://galactictalk.org/d/26-project-idea-distributed-exchange)


# Paywall para Recursos
Digamos que você tem um serviço disponível ao público, talvez para streaming ou wi-fi aberto. Você pode permitir que outras pessoas usem esse serviço sob a condição de te pagar pequenas quantias. Esses pagamentos poderiam ser usados para prevenir spam ou para dar suporte ao seu negócio. Esse um trabalho para o **toll collector**...

## Toll Collector
Um serviço simples que monitora qualquer DLO enviado a um `endereço de pedágios`, ou toll address. O toll collector tem uma base de dados de chaves públicas e quantidades de DLO enviadas ao toll address. Ele escuta pagamentos ao toll address na rede AiBlocks e os adiciona a essa base de dados.

O serviço de toll collector tem um RPC ou endpoint que pode ser chamado:

  - `charge(publicKey, qtde. de DLO)` retorna
    - `qtde. de DLO cobrada`
    - `qtde. de DLO que resta nesta chave`

Sua aplicação pode publicar seu toll address do AiBlocks para pagamentos. Quando alguém tenta usar seu serviço, o servidor os faz autenticar sua chave pública e chama `charge` no Toll Collector para decrementar o saldo do consumidor na base de dados. Você pode enviar uma mensagem ao consumidor quando seu saldo for zero.

# Coordenador Multisig
Uma aplicação web que facilita a criação de transações multisig. Tipicamente, é preciso coordenar entre várias partes para gerar uma transação para uma conta protegida por multisig. Este site tornaria esse processo muito mais fácil e o permitiria coordenar em casos que você não conhece a outra parte.

Idealmente, o coordenador multisig incluiria os seguintes recursos:
- Associar um endereço de e-mail à sua chave pública
- Criar uma tx que você gostaria que fosse assinada por várias partes
- Inserir as chaves públicas que você gostaria que assinassem a tx
- Se alguma dessas chaves tiver associado previamente seu endereço de e-mail, será enviada uma mensagem
- Ao entrar no site, será exibida uma lista de todas as transações pendentes:
  - Pode-se ver os detalhes de cada transação
  - Pode-se ver quem iniciou a transação
  - Pode-se ver quem mais assinou a transação
  - Pode-se assinar qualquer transação que esteja aguardando a sua assinatura
- Quando uma transação pendente for assinada por pessoas suficientes, a transação é submetida à rede
- Quando a transação for submetida, todos os signatários são notificados

# Feed de Mercado
Feed de dados para a exchange distribuída dentro do AiBlocks. Algo equivalente à [API da Poloniex](https://poloniex.com/public?command=returnTicker).
Isso será útil para aplicações como [aiblocksTerm](http://aiblocksterm.com), assim como para adicionar o volume comercial do AiBlocks a sites como [CoinMarketCap](http://coinmarketcap.com).

# Monitor de Quorums
Uma página web que mostra o estado da rede de quorums em um gráfico. Idealmente, o monitor de quorums mostra:
- Um gráfico ao vivo de como a rede está conectada
- Que servidores estão tendo problemas
- Quaisquer servidores que discordem do resto da rede
- Talvez um histórico do uptime de cada validador

 Deveria ser possível ver o gráfico de quorums do ponto de vista de qualquer validador. Você provavelmente teria que rodar o aiblocks-core. Pode-se pegar os dados dos logs do aiblocks-core e do comando /quorum.

*Avançado*: Construir um servidor que se conecta ao aiblocks-core e monitora as mensagens externalizadas e os vários broadcasts dos validadores.

# Bibliotecas
Crie uma biblioteca em sua linguagem favorita:
- C#
- PHP
- Haskell
- Outras linguagens

Ou contribua a nossos SDKs já existentes:
- [JavaScript](https://github.com/aiblocks/js-aiblocks-sdk)
- [Go](https://github.com/aiblocks/go)
- [Java](https://github.com/aiblocks/java-aiblocks-sdk)
- [Python](https://github.com/AiBlocksCN/py-aiblocks-base/)

# Ideias que ouvimos para produtos e serviços
- Conta de microeconomias para escola, saúde, seguros
- Microsseguros
- Empréstimos P2P
- Transferências de dinheiro condicionais
- Sistemas de doação para organizações de fins não lucrativos
- Programas de pontos de fidelidade
- Moedas para comunidades
- Bancos de tempo
- Monitoramento de horas voluntárias
- Caixa eletrônico em qualquer lugar ou aplicativos mobile de caixas eletrônicos

# Facilitador de Atomic Swaps Cross-chain
- Software para usuários finais que facilita atomic swaps cross-chain com
várias outras criptomoedas (entre tanto Delos como outras
  moedas AiBlocks).
- Um serviço de rendezvous que estabelece um marketplace para swaps cross-chain.
