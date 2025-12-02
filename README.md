# GA_per90 Prediction – Modelo Preditivo com FastAPI + Machine Learning

Este projeto implementa um sistema completo de **predição de GA_per90 (Gols + Assistências por 90 minutos)** para jogadores de futebol, combinando:

- **Machine Learning (scikit-learn)**
- **API moderna usando FastAPI**
- **Frontend com HTML + Bootstrap + JS**
- **Deploy com Docker**
- **Acesso externo via Ngrok**
- **Dataset real da temporada 2024–2025**

A solução permite:

Prever GA_per90 com base em métricas avançadas  
Comparar GA_real vs GA_previsto  
Visualizar os Top 10 jogadores da temporada  
Testar via frontend ou chamadas externas (requests)
---

## 📁 Estrutura do Projeto
deploy/
│── app/
│ ├── main.py # API FastAPI
│ ├── model_loader.py # Carregamento do modelo ML
│── model/
│ ├── modelo_ga_per90.pkl # Modelo treinado
│ ├── scaler_ga_per90.pkl # Scaler usado no treino
│ ├── columns_ga_per90.pkl # Colunas usadas no treino
│── requirements.txt
│── Dockerfile
│── docker-compose.yml
frontend/
│── index.html # Interface web
│── script.js # Consumo da API
│── styles.css # Estilização
players_stats_limpo.csv # Dataset de entrada


---

## 📡 Endpoints da API

### 🔹 POST `/predict`
Recebe estatísticas do jogador e retorna a previsão do GA_per90.

#### Exemplo de payload:
```json
{
  "KP_per90": 2.1,
  "PrgP_per90": 3.0,
  "Carries_per90": 27.4,
  "Touches_per90": 41.6,
  "npxG": 7.3,
  "Age": 25,
  "Pos": "FW",
  "Comp": "Bundesliga"
}
Resposta:
{
  "GA_per90_pred": 0.72
}
````
GET /top-players
Retorna os 10 jogadores com maior GA_real, acompanhados da previsão do modelo.
Exemplo:
```json
{
  "players": [
    {
      "Player": "Harry Kane",
      "Squad": "Bayern Munich",
      "Comp": "Bundesliga",
      "GA_per90": 1.49,
      "Predicted_GA_per90": 0.72
    }
  ]
}
````
Como o GA_previsto é calculado
O pipeline segue estes passos:
*Recebe dados do jogador (JSON)
*Converte em DataFrame
*Aplica one-hot encoding em:
*Pos (FW, MF, DF, GK)
*Comp (ligas)
*Garante o formato igual ao do treinamento (columns_ga_per90.pkl)
*Normaliza variáveis numéricas com o scaler salvo (scaler_ga_per90.pkl)
*Aplica o modelo (modelo_ga_per90.pkl)
*Retorna o valor previsto

Features utilizadas no modelo:

*KP_per90
*PrgP_per90
*Carries_per90
*Touches_per90
*npxG
*Age
*Pos (one-hot)
*Comp (one-hot)

*GA_real (do dataset)
*GA_real = Gls_per90 + Ast_per90

### Executando Localmente (sem Docker)
Instalar dependências:
pip install -r deploy/requirements.txt

Rodar API:
uvicorn deploy.app.main:app --reload

Abrir o frontend:
http://localhost:8000/app/

Executando com Docker
Build:
docker build -t ga-api ./deploy

Run:
docker run -p 8000:8000 ga-api

Ou com docker-compose:

cd deploy
docker-compose up --build

A API ficará disponível em:
http://localhost:8000
E o frontend em:

http://localhost:8000/app/

Deploy com Ngrok (Google Colab)
from pyngrok import ngrok
ngrok.set_auth_token("SUA_CHAVE")
ngrok.connect(8000)

Resultado:

https://xxxxx.ngrok-free.app

Frontend
Construído com:
Bootstrap 5
HTML + CSS
Javascript puro
fetch() para consumo da API
Recursos:
Formulário de predição
Exibição de GA_per90 previsto
Tabela dinâmica com Top 10 jogadores
Comparação GA_real vs GA_previsto
Visualização do request e response da API

Dataset

O dataset players_stats_limpo.csv contém estatísticas avançadas de jogadores da temporada 2024–2025, incluindo:
KP_per90
PrgP_per90
Carries_per90
Touches_per90
Gls_per90
Ast_per90
npxG
Idade
Liga
Posição

E é a base tanto para o treino quanto para a análise.

👤 Autor
João Antonio David
Engenharia de Software — Católica de Santa Catarina - Joinville
