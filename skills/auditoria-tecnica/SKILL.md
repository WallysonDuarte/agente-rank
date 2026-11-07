---
name: auditoria-tecnica
description: Verificar rastreamento, indexacao, desempenho, enderecos e dados estruturados, corrigindo na ordem de impacto.
---

# Auditoria tecnica de busca

## Quando usar

Site novo, queda de trafego organico, migracao, antes de investir em conteudo.

## Ordem de verificacao

### 1. Rastreamento
- Arquivo de regras de rastreamento permitindo o que importa e bloqueando o que nao deve ser
  indexado (area autenticada, busca interna, parametros infinitos).
- Mapa do site declarado, com data de modificacao real, apenas enderecos canonicos e com resposta
  de sucesso.
- Nenhuma pagina importante bloqueada por engano.

### 2. Indexacao
- Relatorio de cobertura na ferramenta do buscador: o que esta indexado, o que foi excluido e por
  que.
- Pagina de confirmacao, agradecimento ou area interna marcada para nao indexar.
- Canonico correto em toda pagina — e **coerente** com a versao servida: canonico com e sem barra
  final, ou apontando para outro dominio, divide o sinal.
- Em site que serve varios dominios, canonico derivado do dominio validado por lista permitida,
  nunca fixo em um deles nem confiando no cabecalho recebido.

### 3. Desempenho

Tres medidas que importam, no dado de campo e nao apenas no laboratorio:

| Medida | Alvo | Alavancas |
|---|---|---|
| Maior conteudo visivel | abaixo de 2,5 s | imagem principal otimizada e com prioridade, fonte com exibicao imediata, resposta rapida do servidor |
| Resposta a interacao | abaixo de 200 ms | menos script na linha principal, divisao de pacote |
| Estabilidade visual | abaixo de 0,1 | dimensao declarada em imagem e video, espaco reservado para anuncio |

Compressao moderna, imagem em formato eficiente com conjunto de tamanhos, carregamento tardio fora
da primeira tela, rede de distribuicao.

Mecanismo que pre-busca todos os pedacos do pacote baixa centenas de arquivos num unico acesso: em
rede lenta isso destroi o desempenho. Meca a quantidade de requisicoes do primeiro carregamento.

### 4. Enderecos e redirecionamentos
- Curto, descritivo, com o termo, separado por hifen.
- Uma unica versao canonica (com ou sem prefixo, com ou sem barra final), com redirecionamento
  permanente das outras.
- Nenhuma cadeia de redirecionamento: aponte direto ao destino final.
- Pagina de erro propria, que responde o codigo correto — e nao devolve sucesso.

**Atencao:** em aplicacao de pagina unica, o retorno padrao devolve a pagina inicial com resposta de
sucesso. Isso faz rota inexistente parecer existente. Verifique o conteudo, nao apenas o codigo.

### 5. Dados estruturados
- Formato de objetos interligados em bloco de script, preferido a marcacao em atributos.
- Tipos conforme o conteudo: organizacao, negocio local, produto, artigo, perguntas frequentes,
  passo a passo, trilha de navegacao, avaliacao.
- Validado na ferramenta de teste oficial; marcacao com erro nao gera resultado enriquecido.
- Somente dado que aparece na pagina. Marcacao de avaliacao sem avaliacao visivel e violacao.

### 6. Pre-visualizacao em compartilhamento
Imagem de pre-visualizacao presente **e acessivel**: a marcacao apontando para arquivo inexistente
passa despercebida por meses. Confira o endereco da imagem diretamente (resposta de sucesso e tipo
de imagem). Formato: 1200 por 630, abaixo de 300 KB, endereco absoluto, com largura, altura, texto
alternativo e tipo declarados.

Ao trocar a imagem, versione o **nome** do arquivo: mensageiros e redes guardam a versao anterior
em cache.

## Comandos de verificacao

```bash
curl -s https://<dominio>/robots.txt
curl -s https://<dominio>/sitemap.xml | head -50
curl -s https://<dominio>/ | grep -i canonical
curl -s https://<dominio>/ | grep -i 'application/ld+json' -A 20
curl -sI https://<dominio>/ | grep -iE 'cache-control|etag|vary'
curl -sI https://<dominio>/pagina-inexistente | head -1
curl -sIL https://<dominio> | grep -iE 'HTTP/|location:'
```

## Checklist

- [ ] Regras de rastreamento corretas; mapa do site valido e declarado
- [ ] Cobertura revisada; paginas internas marcadas para nao indexar
- [ ] Canonico presente e coerente com a versao servida
- [ ] Tres medidas de desempenho dentro do alvo no dado de campo
- [ ] Uma unica versao canonica, sem cadeia de redirecionamento
- [ ] Pagina de erro respondendo o codigo correto
- [ ] Dados estruturados validados
- [ ] Imagem de pre-visualizacao existente e acessivel

## Pronto quando

A cobertura nao tem erro inesperado, as tres medidas de desempenho estao no alvo, e os dados
estruturados passam na validacao oficial.
