import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

st.title("📊 Análise de Dados com Pandas e Streamlit")

# Opção de carregar arquivo ou usar dados fictícios
opcao = st.radio("Escolha a fonte de dados:", ["Usar dados fictícios", "Fazer upload de arquivo"])

# Função para criar dados fictícios
def criar_dados_ficticios():
    return pd.DataFrame({
        'Produto': ['Camiseta', 'Calça', 'Tênis', 'Boné', 'Jaqueta'],
        'Quantidade': [10, 5, 8, 12, 4],
        'Preço Unitário': [50, 120, 250, 40, 300]
    })

# Leitura dos dados
if opcao == "Usar dados fictícios":
    df = criar_dados_ficticios()
else:
    arquivo = st.file_uploader("Faça upload do arquivo CSV ou Excel", type=['csv', 'xlsx'])
    if arquivo:
        if arquivo.name.endswith('.csv'):
            df = pd.read_csv(arquivo)
        else:
            df = pd.read_excel(arquivo)
    else:
        st.warning("Por favor, envie um arquivo para continuar.")
        st.stop()

# Exibir dados
st.subheader("📄 Dados")
st.dataframe(df)

# Informações básicas
st.subheader("ℹ️ Informações básicas")
st.write(f"Número de linhas: {df.shape[0]}")
st.write(f"Número de colunas: {df.shape[1]}")
st.write("Estatísticas descritivas:")
st.write(df.describe())

# Cálculo de campo novo
if 'Quantidade' in df.columns and 'Preço Unitário' in df.columns:
    df['Total Venda'] = df['Quantidade'] * df['Preço Unitário']
    st.subheader("💰 Dados com Total da Venda")
    st.dataframe(df)

    # Agrupamento
    resumo = df.groupby('Produto')['Total Venda'].sum().reset_index()

    st.subheader("📊 Total de Vendas por Produto")
    st.dataframe(resumo)

    # Gráfico
    fig, ax = plt.subplots()
    ax.bar(resumo['Produto'], resumo['Total Venda'], color='skyblue')
    plt.xticks(rotation=45)
    plt.ylabel("Total em R$")
    plt.title("Total de Vendas por Produto")
    st.pyplot(fig)

    # Exportação
    st.subheader("⬇️ Exportar Dados")
    exportar = st.radio("Deseja exportar os dados?", ["Não", "Sim"])
    if exportar == "Sim":
        tipo = st.selectbox("Escolha o formato", ["CSV", "Excel"])
        if tipo == "CSV":
            st.download_button("Baixar CSV", df.to_csv(index=False), file_name="dados_exportados.csv", mime="text/csv")
        else:
            from io import BytesIO
            buffer = BytesIO()
            with pd.ExcelWriter(buffer, engine='xlsxwriter') as writer:
                df.to_excel(writer, index=False, sheet_name='Dados')
            st.download_button("Baixar Excel", buffer.getvalue(), file_name="dados_exportados.xlsx", mime="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
else:
    st.warning("As colunas 'Quantidade' e 'Preço Unitário' são necessárias para cálculo e agrupamento.")
