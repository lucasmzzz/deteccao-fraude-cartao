# Detecção de Fraude em Cartões de Crédito

## 📌 O Problema
Este projeto visa detectar transações fraudulentas em cartões de crédito. O grande desafio é o **desbalanceamento extremo** dos dados: apenas 0,17% das transações são fraudes. Por isso, a métrica de acurácia é inútil aqui (um modelo que diz "não é fraude" para tudo acerta 99,83% das vezes). O foco deve ser em **Recall, Precisão e F1-Score** da classe de fraude.

## 🛠️ Preparação dos Dados
- O dataset foi carregado diretamente via URL (para não ultrapassar o limite de 100MB do GitHub).
- As variáveis `Time` e `Amount` foram padronizadas com `StandardScaler`.
- As variáveis `V1` a `V28` já eram componentes PCA (protegendo a privacidade).
- Foi realizado um `train_test_split` com `stratify=y` para manter a proporção de fraudes no treino e teste.

## 📊 Comparação dos Modelos
Foram treinados três modelos, todos lidando com o desbalanceamento através de pesos (`class_weight='balanced'` ou `scale_pos_weight`):
1. **Regressão Logística**: Baseline.
2. **Random Forest**: Melhor lidou com a complexidade não linear.
3. **XGBoost**: Excelente performance, escolhido para o ajuste final.

*(Insira aqui as tabelas de classificação ou os gráficos de curva PR e ROC gerados no notebook)*

## ⚙️ Ajuste de Limiar e SHAP
- **Limiar de Decisão**: O limiar padrão de 0.5 foi reduzido para **0.3**. Isso aumentou o Recall (pegando mais fraudes reais), mas exigiu um trade-off na Precisão (alguns falsos positivos a mais), o que é aceitável no contexto de segurança.
- **SHAP (SHapley Additive exPlanations)**: Foi utilizado para entender o "peso" de cada variável. O gráfico de summary mostrou que as variáveis V14, V17 e V12 são as mais impactantes para o modelo decidir se uma transação é fraude ou não.

## 🔄 O que mudei em relação à Expert
- Adicionei o teste de limiar (threshold) customizado (0.3) para demonstrar na prática o impacto no Recall.
- Explorei o `scale_pos_weight` no XGBoost, que é a forma nativa da biblioteca lidar com classes desbalanceadas.
