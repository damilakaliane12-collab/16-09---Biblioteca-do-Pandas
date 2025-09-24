import pandas as pd

# Dados de exemplo para as despesas
dados_despesas = [
    {"Categoria": "Alimentação", "Descrição": "Supermercado", "Valor": 150.00, "Data": "2025-09-15"},
    {"Categoria": "Transporte", "Descrição": "Uber", "Valor": 25.50, "Data": "2025-09-15"},
    {"Categoria": "Lazer", "Descrição": "Cinema", "Valor": 40.00, "Data": "2025-09-16"},
    {"Categoria": "Moradia", "Descrição": "Aluguel", "Valor": 1200.00, "Data": "2025-09-10"},
    {"Categoria": "Alimentação", "Descrição": "Restaurante", "Valor": 75.00, "Data": "2025-09-16"},
    {"Categoria": "Transporte", "Descrição": "Ônibus", "Valor": 8.80, "Data": "2025-09-17"},
    {"Categoria": "Lazer", "Descrição": "Parque", "Valor": 0.00, "Data": "2025-09-17"},
]

# Criar o DataFrame
df = pd.DataFrame(dados_despesas)

# Calcular totais por categoria
resumo_por_categoria = df.groupby("Categoria")["Valor"].sum().reset_index()

# Exibir informações
print("\nDataFrame Completo de Despesas:")
print(df)

print("\nResumo de Despesas por Categoria:")
print(resumo_por_categoria)

# Salvar arquivos Excel
df.to_excel("despesas.xlsx", index=False)
resumo_por_categoria.to_excel("resumo_despesas.xlsx", index=False)

print("\nArquivos 'despesas.xlsx' e 'resumo_despesas.xlsx' foram salvos com sucesso.")
