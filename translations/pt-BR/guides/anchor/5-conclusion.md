---
title: Próximos Passos
sequence:
  previous: 4-compliance-server.md
---

Parabéns! Se você chegou até aqui, agora devem estar funcionando seus servidores bridge, federation e compliance, com a habilidade de fazer e receber pagamentos seguros e sancionados na rede AiBlocks.

## Testes
Enviar e receber requer que seus servidores interajam com os servidores de outra pessoa. Isso pode fazer com que seja difícil de testar. Por isso, a SDF criou um meio prático para testes onde se pode enviar e receber de nossa âncora teste. É possível simular vários cenários de falha, para você ter certeza de que pode lidar com todos os casos. Dê uma olhada na ferramenta de testes aqui: [goaiblocks.io](http://goaiblocks.io).

## Transicionar para Produção

Enquanto você se prepara para mover seus serviços para produção e suportar transações na rede AiBlocks pública, certifique-se de ter cuidado com algumas coisas:

- Atualize a URL Millennium e a network passphrase nos arquivos de configuração de seus servidores bridge e compliance. O servidor bridge precisará de um servidor millennium na rede Pública (em vez da rede de testes) para enviar transações. Ambos os servidores bridge e compliance também têm uma `network_passphrase` em suas configurações. A rede pública usa uma passphrase separada:

    ```toml
    network_passphrase = "Public Global AiBlocks Network ; September 2015"
    ````

- Verifique que seu servidor bridge e o port interno de seu servidor compliance não sejam publicamente acessíveis. Ambos esses servidores permitem operações privilegiadas, e poderia custar caro se alguém que não devia puder acessá-los.


## E agora?

Embora agora você tenha aprendido a manusear as operações básicas de uma âncora, há muitas outras coisas que âncoras podem procurar aprender ou deveriam considerar:

- [Opere seu próprio nó e servidor millennium na rede AiBlocks](https://aiblocks.io/developers/aiblocks-core/software/admin.html). Fazer isso o torna menos dependente de outros provedores e torna toda a rede AiBlocks mais forte.
- Leia nosso guia sobre [segurança](../security.md).
- Faça ofertas para [comprar e vender ativos na exchange distribuída](../concepts/exchange.md).
- Explore [sistemas multisignature](../concepts/multi-sig.md) para tornar contas importantes mais seguras.
- Use [canais](../channels.md) para submeter mais transações por vez.
- Fale com outros desenvolvedores de AiBlocks na [comunidade do AiBlocks no Slack](http://slack.aiblocks.io/).
- [Contribua](../contributing.md) com seus próprios consertos e melhorias aos softwares do AiBlocks.

<nav class="sequence-navigation">
  <a rel="prev" href="4-compliance-server.md">Anterior: Servidor Compliance</a>
</nav>
