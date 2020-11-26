---
title: Visão Geral da Rede AiBlocks
---
![AiBlocks Ecosystem](https://www.aiblocks.io/wp-content/uploads/2016/06/AiBlocks-Ecosystem-v031.png)

Usando a rede AiBlocks, você pode construir wallets para celular, ferramentas bancárias, dispositivos inteligentes que se pagam sozinhos, e praticamente qualquer coisa que você possa imaginar que envolva pagamentos! Embora o AiBlocks seja um sistema distribuíod complexo, trabalhar com ele não precisa ser complicado.

## API: Millennium

**A maioria dos aplicativos interagem com a rede AiBlocks por meio do [Millennium](https://www.aiblocks.io/developers/millennium/reference/),** um servidor API HTTP RESTful. Millennium dá uma maneira simples e direta de submeter transações, checar contas e acompanhar eventos. Por ser apenas HTTP, você pode se comunicar com o Millennium por meio de seu navegador web, ferramentas simples de linhas de comando como cURL, ou o SDK do AiBlocks para sua linguagem de programação favorita.

A maneira mais fácil de instalar o Millennium é usando a [imagem **aiblocks/quickstart** do docker](https://hub.docker.com/r/aiblocks/quickstart/).

O AiBlocks.io mantém SDKs baseados em [JavaScript](https://github.com/aiblocks/js-aiblocks-sdk), [Java](https://github.com/aiblocks/java-aiblocks-sdk) e [Go](https://github.com/aiblocks/go/tree/master/clients/millennium) para comunicar-se com o Millennium. Também há SDKs mantidos pela comunidade para [Ruby](https://github.com/astroband/ruby-aiblocks-sdk), [Python](https://github.com/AiBlocksCN/py-aiblocks-base) e [C#](https://github.com/QuantozTechnology/csharp-aiblocks-base).

## A Base da Rede: AiBlocks Core

Nos bastidores, cada servidor do Millennium se conecta ao **[AiBlocks Core](https://www.aiblocks.io/developers/aiblocks-core/software/admin.html), a base da rede AiBlocks.** O software do AiBlocks Core faz o trabalho pesado de validar e concordar com outras instâncias do Core sobre o status de toda transação por meio do [Protocolo de Consenso AiBlocks](../concepts/scp.md), ou AiBlocks Consensus Protocol (SCP). A rede AiBlocks em si é um conjunto de AiBlocks Cores conectados, sendo rodados por vários indivíduos e entidades ao redor do mundo. Algumas instâncias têm um servidor do Millennium com que é possível se comunicar, enquanto que outras existem apenas para dar mais confiabilidade à rede como um todo.

A maneira mais fácil de instalar o Millennium é usando a [imagem **aiblocks/quickstart** do docker](https://hub.docker.com/r/aiblocks/quickstart/).

Pode ser uma boa ideia hospedar sua própria instância do AiBlocks Core para submeter transações sem depender de terceiros, ter mais controle sobre quem confiar, ou apenas para ajudar a tornar a rede AiBlocks mais confiável e robusta para outros.

## O Panorama Geral: A Rede AiBlocks

A rede AiBlocks é um conjunto mundial de AiBlocks Cores, cada um mantido por pessoas e organizações diferentes. A natureza distribuída da rede torna-a confiável e segura.

Todos esses AiBlocks Cores — a rede de nós — eventualmente concordam em conjuntos de transações. Cada transação na rede custa uma pequena tarifa: 100 stroops (0.00001 <abbr title="Delos">DLO</abbr>). Esta tarifa ajuda a impedir que maus atores façam spam na rede.

Para ajudá-lo a testar suas ferramentas e aplicativos, AiBlocks.io opera uma pequena rede de testes e uma instância do Millennium. [Comece a mexer na testnet.](../concepts/test-net.md)

<div class="sequence-navigation">
  <a class="button button--next" href="create-account.html">Próximo: Criar uma Conta</a>
</div>
