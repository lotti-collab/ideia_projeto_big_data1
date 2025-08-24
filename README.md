# 📊 Segmentação de Clientes com Machine Learning (KMeans)

## 🎯 Objetivo do Projeto
Este projeto tem como objetivo **analisar o comportamento de clientes da Olist** (um e-commerce brasileiro) e aplicar **técnicas de clusterização (KMeans)** para **segmentar clientes em grupos distintos**.  

A partir dessa segmentação, é possível identificar padrões de consumo e apoiar **decisões estratégicas de negócio**, como:
- Campanhas de marketing personalizadas.
- Fidelização de clientes de alto valor.
- Gestão de risco e crédito em compras parceladas.
- Retenção de clientes pouco recorrentes.

---

## 🗂️ Dados Utilizados
Foram utilizados 4 datasets públicos da Olist:

- `olist_customers_dataset.csv` → informações dos clientes.  
- `olist_orders_dataset.csv` → dados dos pedidos.  
- `olist_order_payments_dataset.csv` → informações de pagamento (valor, parcelas).  
- `olist_order_items_dataset.csv` → detalhes dos itens comprados.  

---

## 🔧 Etapas do Projeto

### 1. Carregamento e Unificação dos Dados
Os datasets foram unidos em um único dataframe (`df`), relacionando **clientes → pedidos → pagamentos → itens**.

```python
df = orders.merge(customers, on="customer_id", how="left") \
           .merge(payments, on="order_id", how="left") \
           .merge(items, on="order_id", how="left")
2. Criação de Métricas por Cliente
Para cada cliente, foram calculadas métricas de comportamento:

Número de pedidos (num_orders)

Valor total gasto (total_spent)

Média de parcelas utilizadas (avg_installments)

Quantidade total de itens comprados (num_products)

python
Copiar
Editar
customer_features = df.groupby("customer_unique_id").agg({
    "order_id": "nunique",
    "payment_value": "sum",
    "payment_installments": "mean",
    "order_item_id": "count",
}).reset_index()
3. Pré-processamento
Tratamento de valores ausentes (NaN) → substituídos por 0.

Normalização com StandardScaler para padronizar as variáveis.

4. Clusterização com KMeans
Aplicamos KMeans para segmentar os clientes em 4 clusters:

python
Copiar
Editar
kmeans = KMeans(n_clusters=4, random_state=42, n_init=10)
customer_features["cluster"] = kmeans.fit_predict(X_scaled)
5. Redução de Dimensionalidade (PCA)
Utilizamos PCA para reduzir as variáveis para 2 dimensões e possibilitar a visualização gráfica:

python
Copiar
Editar
pca = PCA(n_components=2, random_state=42)
X_pca = pca.fit_transform(X_scaled)
customer_features["pca1"] = X_pca[:, 0]
customer_features["pca2"] = X_pca[:, 1]
6. Visualização dos Clusters
Cada cor no gráfico representa um grupo de clientes identificado pelo algoritmo:


(Você pode salvar o gráfico gerado pelo matplotlib como imagem e adicionar aqui ao README)

📈 Resultados Obtidos
Os clusters encontrados representam diferentes perfis de clientes:

Clientes de baixo gasto e poucas compras → foco em retenção.

Clientes que gastam muito, mas compram poucas vezes → oportunidades de fidelização.

Clientes recorrentes de ticket médio → ideais para programas de pontos.

Clientes que compram muitos itens e parcelam bastante → atenção à gestão de crédito.

💡 Decisões de Negócio
Com base nessa segmentação, a empresa pode:

Criar campanhas personalizadas para aumentar engajamento.

Oferecer vantagens exclusivas (cashback, frete grátis) para clientes VIP.

Gerenciar crédito e parcelamento em clusters de maior risco.

Retenção de clientes que compram pouco, mas podem ter potencial de crescimento.

🚀 Tecnologias Utilizadas
Python

Pandas → Manipulação e análise de dados.

Matplotlib → Visualização gráfica.

Scikit-Learn → Pré-processamento, KMeans, PCA.

📢 Autor
Projeto desenvolvido por Alexandre Lotti 
