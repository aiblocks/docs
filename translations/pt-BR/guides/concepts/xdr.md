---
title: XDR
---

**XDR**, também conhecido como _External Data Representation_, é usado por toda a rede e protocolo AiBlocks. O ledger, transações, resultados, histórico, e até as mensagens passadas entre computadores que rodam o aiblocks-core são codificadas usando XDR.

XDR é especificado em [RFC 4506](http://tools.ietf.org/html/rfc4506.html) e é similar a ferramentas como Protocol Buffers ou Thrift. XDR oferece alguns recursos importantes:

- É muito compacto, podendo assim ser transmitido rapidamente e armazenado com espaço mínimo de disco.
- Dados codificados em XDR são armazenados de maneira confiável e previsível. Os campos estão sempre na mesma ordem, o que torna simples assinar criptograficamente e verificar mensagens em XDR.
- Definições em XDR incluem descrições ricas de tipos e estruturas de dados, o que não é possível em formatos mais simples como JSON, TOML, ou YAML.

Como XDR é um formato binário e não é tão bem conhecido quanto formatos mais simples como JSON, os SDKs de AiBlocks incluem ferramentas para parsear XDR e o farão automaticamente ao recuperar dados.

Além disso, o servidor da API Millennium geralmente expõe as partes mais importantes dos dados XDR em JSON, o que torna mais fácil para parsear caso não esteja usando um SDK. Os dados XDR ainda estão incluídos (codificados como um string base64) dentro do JSON caso precise acessá-los diretamente.

## Arquivos .X

Estruturas de dados em XDR são especificadas em um _interface definition file_ (IDL).
Os arquivos IDL usados para a rede AiBlocks estão disponíveis [no GitHub](https://github.com/aiblocks/aiblocks-core/tree/master/src/xdr).
