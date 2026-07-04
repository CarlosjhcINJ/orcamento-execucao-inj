# Dashboard de Orçamento & Execução — INJ Brasil

Painel financeiro que lê automaticamente a planilha "Fechamento Mensal" do Google Sheets (abas por mês) e mostra KPIs, evolução mensal, projeção de fechamento do ano e detalhamento de entradas/saídas.

Os dados **não são editados aqui** — a edição continua sendo feita na planilha do Google Sheets (botão "✏️ Editar dados na planilha" no topo abre ela direto). O dashboard só sincroniza e exibe.

## Como publicar no GitHub Pages

1. Crie um repositório novo no GitHub (ex: `dashboard-orcamento-inj`), público.
2. Suba os dois arquivos desta pasta (`index.html` e `logo-inj.png`) para esse repositório.
3. No repositório, vá em **Settings → Pages** e em "Branch" selecione `main` (pasta `/root`) → Save.
4. Em alguns minutos o link fica disponível em `https://SEU-USUARIO.github.io/dashboard-orcamento-inj/`.

Se preferir, dá pra fazer isso pelo terminal (com Git instalado e já autenticado no GitHub):

```
git remote add origin https://github.com/SEU-USUARIO/dashboard-orcamento-inj.git
git add index.html logo-inj.png README.md
git commit -m "Dashboard de orçamento e execução INJ"
git branch -M main
git push -u origin main
```

## Como testar localmente antes de publicar

Basta abrir o arquivo `index.html` direto no navegador (duplo clique). Como ele busca os dados do Google Sheets pela internet, precisa estar conectado.

## Como acrescentar um novo ano (ex: 2027)

1. Duplique a planilha do ano atual no Google Sheets (Arquivo → Fazer uma cópia), mantendo os nomes das abas mensais (Janeiro, Fevereiro, ...).
2. Compartilhe a nova planilha como **"Qualquer pessoa com o link" → Leitor**.
3. Copie o ID da planilha (a parte da URL entre `/d/` e `/edit`).
4. Abra `index.html`, procure o bloco `CONFIG.anos` no início do `<script>` e acrescente uma linha, por exemplo:

```js
anos: {
  2026: { sheetId: "1t9lsG5CAJqHjrJ0VyjvPLcetOZaMtfr3z6K_Yb6xhCE" },
  2027: { sheetId: "COLE_AQUI_O_ID_DA_NOVA_PLANILHA" }
}
```

Nenhuma outra alteração é necessária — o seletor de ano no topo do dashboard passa a mostrar os dois anos, e o mês mais recente com dados é detectado automaticamente conforme o ano vai sendo preenchido.

## Como ajustar as categorias dos gráficos

As categorias usadas nos gráficos de composição ("Dízimos", "Ofertas", "Administrativo", etc.) são definidas por palavras-chave em `CONFIG.categoriasEntrada` e `CONFIG.categoriasSaida`, no início do `<script>`. Para mudar o agrupamento de um item, ajuste a expressão regular (`re`) correspondente.

## Observação técnica

O dashboard sempre lê a **última aba de mês existente** na planilha (ela já acumula o histórico de todos os meses anteriores em colunas). Por isso, ao fechar um novo mês na planilha (nova aba, ex: "Julho"), o dashboard passa a mostrá-lo automaticamente na próxima sincronização — não é preciso alterar nada no código.
