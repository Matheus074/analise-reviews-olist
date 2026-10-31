# 🛒 O que explica a insatisfação dos clientes? Análise de avaliações Olist com IA generativa

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?logo=googlegemini&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c)

Análise de cerca de **98 mil avaliações** de clientes do e-commerce Olist para responder a uma pergunta de negócio:
**o que leva um cliente a dar nota 1 ou 2 para uma compra?**

O projeto tem duas etapas:
1. **Análise exploratória:** cruza a nota com atraso na entrega, categoria do produto e estado do cliente.
2. **Classificação com IA generativa:** usa o Google Gemini para ler os comentários negativos e identificar o motivo
   principal de cada reclamação.

> Este projeto complementa o [NPS Preditivo em E-commerce](https://github.com/Matheus074/NPS-Preditivo-em-E-commerce):
> lá o foco é prever **quem** vai ficar insatisfeito; aqui, entender **por quê**.

## 💡 Principais achados

### 1. Com 4 ou mais dias de atraso, a maioria das avaliações vira negativa

![Atraso x avaliações negativas](img/1_atraso_x_negativas.png)

Pedidos entregues no prazo têm **9%** de avaliações negativas. Com 1 a 3 dias de atraso, a taxa sobe para **32%**; a
partir de 4 dias, passa de **dois terços**.

### 2. Mas 57% das avaliações negativas vêm de pedidos entregues no prazo

![Origem das avaliações negativas](img/2_origem_negativas.png)

**Este é o achado central.** O atraso explica menos da metade da insatisfação. Para entender o restante, é preciso
ler o que os clientes escreveram, e é aí que entra a etapa com IA.

### 3. Mesmo no prazo, algumas categorias concentram insatisfação

![Categorias no prazo](img/3_categorias_no_prazo.png)

Entre pedidos entregues no prazo, **móveis de escritório** têm 18% de avaliações negativas, o dobro da média,
indicando problemas de produto, montagem ou expectativa.

### 4. Estados com mais atrasos têm clientes mais insatisfeitos

![Estados](img/4_estados.png)

A correlação entre a taxa de atraso e a de avaliações negativas por estado é de **0,88**. Alagoas, Maranhão e
Sergipe lideram nos dois indicadores; SP, PR e MG ficam no extremo oposto.

## 🤖 Classificação dos comentários com Gemini

O notebook [`02_classificacao_gemini.ipynb`](notebooks/02_classificacao_gemini.ipynb) envia uma amostra
estratificada de 600 comentários negativos (300 de pedidos no prazo, 150 com atraso e 150 não entregues) para o
Gemini, que escolhe **um motivo principal** em uma lista fechada:

| Motivo | O que entra aqui |
|---|---|
| Atraso na entrega | O produto chegou, mas depois do esperado |
| Produto não recebido | O cliente diz que o produto não chegou |
| Pedido incompleto | Chegou só parte dos itens ou faltou alguma peça |
| Produto errado ou diferente do anúncio | Outro produto, cor, tamanho, modelo ou marca |
| Produto com defeito ou danificado | Quebrado, não funciona, avariado no transporte |
| Qualidade abaixo do esperado | Chegou certo, mas o material ou acabamento decepcionou |
| Atendimento e pós-venda | Dificuldade de contato, troca, devolução ou reembolso |
| Outro ou sem motivo claro | O texto não permite identificar o motivo |

**Boas práticas aplicadas**
- **Saída estruturada:** a resposta vem em JSON validado por um esquema Pydantic, sem interpretar texto livre.
- **Processamento em lotes** de 30 comentários, com pausa entre chamadas para respeitar o limite do plano gratuito.
- **Retomada automática:** o progresso é salvo a cada lote; se a cota acabar, a execução continua de onde parou.
- **Chave da API protegida** nos *Secrets* do Colab, nunca no código.
- **Conferência manual** de uma amostra das classificações antes de tirar conclusões.

> 🚧 **Resultados da classificação em breve.**

## 📁 Estrutura

```
.
├── notebooks/
│   ├── 01_analise_exploratoria.ipynb   # Análise exploratória (com resultados)
│   └── 02_classificacao_gemini.ipynb   # Classificação dos comentários com Gemini
├── img/                                # Gráficos gerados pelos notebooks
├── dados/                              # Coloque aqui os CSVs do Kaggle (não versionados)
└── requirements.txt
```

## 🚀 Como executar

1. Baixe o [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
   e coloque os arquivos `.csv` na pasta `dados/`.
2. Instale as dependências: `pip install -r requirements.txt`
3. Rode `notebooks/01_analise_exploratoria.ipynb`.
4. Para a etapa com IA, abra `notebooks/02_classificacao_gemini.ipynb` no Google Colab, salve sua chave do
   [Google AI Studio](https://aistudio.google.com/apikey) nos *Secrets* com o nome `key_gemini` e execute.

## 📚 Dados

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce): cerca de
100 mil pedidos reais (anonimizados) feitos entre 2016 e 2018, disponibilizados pela Olist sob licença
CC BY-NC-SA 4.0. Os dados não são versionados neste repositório.

## 👤 Autor

**Matheus Cadoni** · Analista de Dados · [LinkedIn](https://www.linkedin.com/in/matheus-cadoni-aab94a208) · [GitHub](https://github.com/Matheus074)
