---
title: AiBlocks.toml
---

# Introdução

O arquivo `aiblocks.toml` é usado para fornecer um lugar comum onde a Internet pode encontrar informações sobre a integração do seu domínio ao AiBlocks. Qualquer site pode publicar informações referentes à rede AiBlocks. Pode-se anunciar sua chave de validação, seu servidor [federation](./federation.md), peers que você está rodando, seu conjunto de quóruns, se você é uma âncora, etc.

O arquivo aiblocks.toml é um arquivo de texto no [formato TOML](https://github.com/toml-lang/toml).

## Publicar um aiblocks.toml

Dado o domínio "DOMAIN", o aiblocks.toml será pesquisado no seguinte local:

`https://DOMAIN/.well-known/aiblocks.toml`

## Habilitar cross-origin resource sharing (CORS)
É necessário habilitar CORS no aiblocks.toml para que pessoas possam acessar este arquivo a partir de outros sites. O seguinte header HTTP *deve* estar definido para uma resposta HTTP para um request pelo arquivo `aiblocks.toml`.

```
Access-Control-Allow-Origin: *
```

**Importante**: Apenas habilite CORS para aiblocks.toml (ou quaisquer arquivos referenciados por ele). Por exemplo, em Apache seria definido algo como:

```xml
<Location "/.well-known/aiblocks.toml">
    Header set Access-Control-Allow-Origin "*"
</Location>
```

Ou em nginx:

```
location /.well-known/aiblocks.toml {
 add_header 'Access-Control-Allow-Origin' '*';
}
```

Para outros servidores web, veja: http://enable-cors.org/server.html

## Testar CORS

1. Rode um comando curl em seu terminal, semelhante ao seguinte (substitua aiblocks.io pelo domínio que hospeda seu arquivo aiblocks.toml):

  ```bash
  curl --head https://aiblocks.io/.well-known/aiblocks.toml
  ```

2. Verifique que o header `Access-Control-Allow-Origin` esteja presente, como mostrado abaixo.

  ```bash
  curl --head https://aiblocks.io/.well-known/aiblocks.toml
  HTTP/1.1 200 OK
  Accept-Ranges: bytes
  Access-Control-Allow-Origin: *
  Content-length: 482
  ...
  ```

3. Rode também o comando em uma página que não deveria tê-lo e verifique que o header `Access-Control-Allow-Origin` esteja ausente.

## Exemplo de AiBlocks.toml

Este arquivo é em UTF-8 com finais de linha em estilo Dos, UNIX, ou Mac.
Linhas em branco e linhas iniciadas por '#' são ignoradas.
Seções indefinidas são reservadas.
Todas as seções são opcionais.
Muitas dessas seções refletem o que estaria listado em seu [aiblocks-core.cfg](https://github.com/aiblocks/aiblocks-core/blob/master/docs/aiblocks-core_example.cfg).

```toml
# Exemplo de aiblocks.toml

#   O endpoint que clientes devem fazer query para resolver endereços AiBlocks
#   para usuários em seu domínio.
FEDERATION_SERVER="https://api.aiblocks.io/federation"

# O endpoint usado para o protocolo compliance
AUTH_SERVER="https://api.aiblocks.io/auth"

# A chave de assinatura (signing key) é usada para o protocolo compliance
SIGNING_KEY="GBBHQ7H4V6RRORKYLHTCAWP6MOHNORRFJSDPXDFYDGJB2LPZUFPXUEW3"

# Mapeamento de conveniência de nomes comuns a IDs de nós.
# Pode-se usar estes nomes comuns em seções abaixo em vez do ID de nó, que é menos amigável.
# Isso é fornecido principalmente para ser compatível com o aiblocks-core.cfg
NODE_NAMES=[
"GD5DJQDDBKGAYNEAXU562HYGOOSYAEOO6AS53PZXBOZGCP5M2OPGMZV3  lab1",
"GB6REF5GOGGSEHZ3L2YK6K4T4KX3YDMWHDCPMV7MZJDLHBDNZXEPRBGM  donovan",
"GBGR22MRCIVW2UZHFXMY5UIBJGPYABPQXQ5GGMNCSUM2KHE3N6CNH6G5  nelisky1",
"GDXWQCSKVYAJSUGR2HBYVFVR7NA7YWYSYK3XYKKFO553OQGOHAUP2PX2  jianing",
"GAOO3LWBC4XF6VWRP5ESJ6IBHAISVJMSBTALHOQM2EZG7Q477UWA6L7U  anchor"
]

#   Uma lista de contas que são controladas por este domínio.
ACCOUNTS=[
"$sdf_watcher1",
"GAENZLGHJGJRCMX5VCHOLHQXU3EMCU5XWDNU4BGGJFNLI2EL354IVBK7"
]

#   Toda chave pública de validação que é declaradamente
#   usada por este domínio para validar ledgers e são
#   signatários autorizados do domínio.
OUR_VALIDATORS=[
"$sdf_watcher2",
"GCGB2S2KGYARPVIA37HYZXVRM2YZUEXA6S33ZU5BUDC6THSB62LZSTYH"
]

# DESIRED_BASE_FEE (número inteiro)
# É o valor preferível da base fee para você. Está em stroops.
DESIRED_BASE_FEE=100

# DESIRED_MAX_TX_PER_LEDGER (número inteiro)
# Este é o máximo de transações por ledger que você gostaria de processar.
DESIRED_MAX_TX_PER_LEDGER=400

#   Lista dos IPs de aiblocks-cores conhecidos.
#   São strings IP:port.
#   Port é opcional.
#   Por convenção, IPs são listados do mais confiável ao menos confiável, se isso for uma informação conhecida.
KNOWN_PEERS=[
"192.168.0.1",
"core-testnet1.aiblocks.io",
"core-testnet2.aiblocks.io:11290",
"2001:0db8:0100:f101:0210:a4ff:fee3:9566"
]

# lista dos arquivos históricos mantidos por este domínio
HISTORY=[
"http://history.aiblocks.io/prd/core-live/core_live_001/",
"http://history.aiblocks.io/prd/core-live/core_live_002/",
"http://history.aiblocks.io/prd/core-live/core_live_003/"
]

#   Esta seção permite que uma âncora declare moedas (currencies) que emite atualmente.
#   Pode ser usada por wallets e clientes para confiar em âncoras a partir do nome do domínio
[[CURRENCIES]]
code="USD"
issuer="GCZJM35NKGVK47BB4SPBDV25477PZYIYPVVG453LPYFNXLS3FGHDXOCM"
display_decimals=2 # Especifica quantas casas decimais devem ser exibidas por clientes aos usuários finais.

[[CURRENCIES]]
code="BTC"
issuer="$anchor"
display_decimals=7 # O máximo de casas decimais que podem ser representadas é 7

# ativo com meta informações
[[CURRENCIES]]
code="BODE"
issuer="GD5T6IPRNCKFOHQWT264YPKOZAWUMMZOLZBJ6BNQMUGPWGRLBK3U7ZNP"
display_decimals=2
name="títulos BODE"
desc="1 token BODE dá direito a uma porcentagem dos rendimentos da Fazenda de Bodes Elkins."
conditions="Só haverá 10,000 tokens BODE em existência. Distribuiremos a porcentagem dos rendimentos anualmente em 15 de Janeiro."
image="https://pbs.twimg.com/profile_images/666921221410439168/iriHah4f.jpg"

#   Conjunto de quóruns potenciais dos validadores deste domínio.
[QUORUM_SET]
VALIDATORS=[
"$self", "$lab1", "$nelisky1","$jianing",
"$eno","$donovan"
]

# informações adicionais opcionais para humanos
# Lugar útil para âncoras detalharem várias políticas e informações exigidas

###################################
# Campos de compliance necessários:
#      name=<nome do recipiente>
#      addr=<endereço do recipiente>
# Formato do Federation:  
#        <n. de telefone>*anchor.com
#        Encaminhamentos são suportados por envios a: forward*anchor.com
#           forward_type=bank_account
#           swift=<código swift do banco recipiente>
#           acct=<número da conta do recipiente no banco>
# Quantia mínima para encaminhamento: $2 USD
# Quantia máxima para encaminhamento: $10000 USD
  ```
Nota: para efeitos informativos, esta tradução optou em traduzir parte do conteúdo dos exemplos de preenchimento do aiblocks.toml, mas os documentos reais devem estar escritos em inglês para facilitar a compreensão por todo o ecossistema AiBlocks.

```
