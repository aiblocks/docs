Documentação AiBlocks
============

Neste repositório se encontra a documentação AiBlocks. Estes documentos alimentam o [builder do site AiBlocks para desenvolvedores](https://github.com/aiblocks/developers) e estão expostos em [aiblocks.io/developers](https://www.aiblocks.io/developers/).

## Como escrever documentos

Há algumas convenções ao escrever documentos que vão para o site de AiBlocks para Desenvolvedores. A maioria dos documentos são escritos em formato Markdown. Para uma introdução ao Markdown, dê uma olhada no [Guia do Github "Mastering Markdown"](https://guides.github.com/features/mastering-markdown/).

### Headers

Linhas que começam com um símbolo hash (`#`) são consideradas como headers. Veja abaixo um exemplo de alguns e do nome do header:

```
# h1
## h2
### h3
#### h4
```

- NÃO usar h1, pois é reservado para o título da página gerado a partir da front matter.
- NÃO pular tamanhos de headers (não vá de h2 para h4).
- NÃO usar headers menores (mais símbolos hash) para representar que uma seção está inserida dentro de uma seção pai com um header maior.
- Adicionar SIM um espaço após os símbolos hash. Alguns parsers de Markdown não irão renderizar o texto como header se não houver o espaço.

### Front matter

No topo da maioria dos documentos há algo chamado de "front matter" (conteúdo antes do texto). Trata-se de metadados para o arquivo escrito em [formato YAML](https://en.wikipedia.org/wiki/YAML).

Aqui está um exemplo da front matter em ação:
```
---
title: Referência de Millennium
---
```

As chaves sendo usadas na front matter são:
- title

### Título do documento

Não comece uma página com um header em Markdown (`# Title`). Em vez disso, deixe-o na front matter. O site para desenvolvedores irá pegar o título a partir da front matter.

### Links

Use links em linha e não referencie links.

Há três tipos diferentes de links, e cada tipo de link tem seu próprio significado. Alguns desses links são transformados durante a geração do site para desenvolvedores.

<table>
  <tbody>
    <tr>
      <th>tipo de link</th>
      <th>onde usar</th>
      <th>exemplo de link em markdown</th>
      <th>link resultante (após processamento pelo portal)</th>
    </tr>
    <tr>
    <tr>
      <td>Relativo</td>
      <td><ul><li>links dentro do mesmo repositório</li></ul></td>
      <td>../reference/accounts-all.md</td>
      <td>../reference/accounts-all.html</td>
    </tr>
    <tr>
      <td>Relativo à raiz</td>
      <td><ul><li>quando quiser usar o visualizador de arquivos do GitHub (ex.: para arquivos fonte)</li></ul></td>
      <td>/src/ledger/AccountFrame.cpp</td>
      <td>https://github.com/aiblocks/REPOSITÓRIO-ATUAL/tree/master/src</td>
    </tr>
    <tr>
      <td>Links absolutos</td>
      <td>
        <ul>
          <li>links entre repositórios (deve linkar ao portal para desenvolvedores em www.aiblocks.io/developers/)</li>
          <li>links a sites externos (como https://www.google.com/)</li>
        </ul>
      </td>
      <td>https://www.aiblocks.io/developers/js-aiblocks-base/reference/building-transactions.html</td>
      <td>https://www.aiblocks.io/developers/js-aiblocks-base/reference/building-transactions.html</td>
    </tr>
  </tbody>
</table>

### Arquivos que não estejam em Markdown

Às vezes vamos incluir outros tipos de conteúdo, como em `.pdf`. Para adicionar front matter ao PDF, crie um arquivo irmão com o nome do arquivo PDF e uma extensão adicional de `.metadata`. Esse arquivo pode então definir metadados para o título do `.pdf`.

Veja um exemplo na [pasta software do aiblocks-core](https://github.com/aiblocks/aiblocks-core/tree/master/docs/software).

## Contribuições

Suas contribuições à rede AiBlocks vão ajudar a melhorar a infraestrutura financeira mundial mais rapidamente.

Queremos que seja o mais fácil possível para contribuir alterações que ajudem a rede AiBlocks a crescer e prosperar. Há algumas orientações que pedimos que os contribuidores sigam para podermos realizar merge com suas mudanças rapidamente. Por favor leia nosso [Guia de Contribuições](https://github.com/aiblocks/docs/blob/master/CONTRIBUTING.md) e assine nosso [Acordo da Licença do Contribuidor](https://docs.google.com/forms/d/1g7EF6PERciwn7zfmfke5Sir2n10yddGGSXyZsq98tVY/viewform).
