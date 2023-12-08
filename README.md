# Projeto de Análise e Visualização de Dados

Este repositório contém um projeto de análise de dados utilizando as bibliotecas Pandas e Matplotlib em Python. O código realiza a leitura de dataframes a partir de um arquivo Excel, realiza pré-processamento dos dados, e gera visualizações significativas.

## Principais Funcionalidades

1. **Leitura de Dataframes:** Utiliza o Pandas para ler os dataframes a partir de um arquivo Excel.

    ```python
    dataframes = pd.read_excel('Teste visualização de dados.xlsx', sheet_name=None)
    ```

2. **Limpeza e Tratamento de Dados:** Remove espaços em branco nos nomes das colunas e nos valores de strings nas células.

    ```python
    for k, v in dataframes.items():
        v = v.rename(columns=lambda x: x.strip() if isinstance(x, str) else x)
        v = v.apply(lambda col: col.str.strip() if col.dtype == "object" else col)
        dataframes[k] = v
    ```

3. **Análise de Vendas:** Calcula a receita total por dia e identifica o melhor dia de vendas.

    ```python
    vendas_df = dataframes['vendas']
    vendas_df['DATA'] = pd.to_datetime(vendas_df['DATA'], errors='coerce')
    vendas_df['VALOR_VENDA'] = vendas_df['VALOR_VENDA'].progress_apply(pd.to_numeric, errors='coerce')
    receita_por_dia = vendas_df.groupby('DATA')['VALOR_VENDA'].sum().reset_index()
    ```

4. **Visualização de Dados:** Plota a evolução da receita total ao longo do tempo.

    ```python
    plt.figure(figsize=(14,7))
    plt.plot(receita_por_dia['DATA'], receita_por_dia['VALOR_VENDA'], marker='o')
    plt.title('Evolução da Receita Total')
    plt.xlabel('Data')
    plt.ylabel('Receita Total')
    plt.grid(True)
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()
    ```

5. **Análise de Produtos:** Identifica o produto mais vendido em termos de quantidade (kg) e o dia em que foram vendidas mais bananas.

    ```python
    vendas_produtos_df = vendas_df.merge(produtos_df, on='ID_PRODUTO')
    total_kg_por_produto = vendas_produtos_df.groupby('NOME_PRODUTO')['TOTAL_KG'].sum().reset_index()
    produto_mais_vendido = total_kg_por_produto.loc[total_kg_por_produto['TOTAL_KG'].idxmax()]
    ```

## Execução

Execute o código em um ambiente Python com as bibliotecas Pandas e Matplotlib instaladas. Certifique-se de ter o arquivo 'Teste visualização de dados.xlsx' disponível.

```bash
pip install pandas matplotlib
python seu_arquivo.py
