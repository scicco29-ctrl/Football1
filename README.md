# ⚽ Football Match & Goal Predictor (Serie A & Serie B)

Un sistema di **Machine Learning e Modellazione Statistica** sviluppato in Python per predire l'esito finale (1X2), il numero di goal attesi ($xG$), i mercati *Over/Under 2.5*, *Goal/NoGoal (BTTS)* e i risultati esatti più probabili per le partite di **Serie A** e **Serie B**.

Il progetto unisce l'analisi delle serie storiche calcistiche, il calcolo di un **Elo Rating dinamico cross-lega** (che traccia la forza delle squadre attraverso promozioni e retrocessioni tra A e B) e modelli di **Gradient Boosting** integrati con la correzione matematica di **Dixon-Coles**.

---

## 🚀 Caratteristiche Principali

* **Pipeline ETL Automatizzata (`pathlib` & `pandas`):** Caricamento massivo, pulizia, normalizzazione delle date e deduplicazione automatica di decine di file CSV storici divisi per campionato.
* **Elo Rating Cross-Lega (Serie A ↔ Serie B):** Algoritmo custom che aggiorna la forza storica di ogni club dopo ogni giornata (con moltiplicatore per la differenza reti e fattore campo), mantenendo la memoria storica quando una squadra sale in Serie A o retrocede in Serie B.
* **Feature Engineering *Leakage-Free*:** Calcolo delle medie mobili sulle ultime 5 partite (*Rolling Averages* per goal fatti, goal subiti, tiri nello specchio e punti) tramite `.shift(1)` per garantire l'assenza totale di *data leakage* (nessun dato futuro viene mai usato per predire il passato).
* **Modellazione Ibrida (Gradient Boosting + Dixon-Coles):**
  * Stima non-lineare dei tassi di realizzazione attesi ($\lambda_{home}$, $\lambda_{away}$) tramite `HistGradientBoostingRegressor(loss='poisson')`.
  * Costruzione della matrice di probabilità congiunta $8 \times 8$ basata sulla distribuzione di Poisson con **correzione di Dixon-Coles** ($\rho = -0.10$) per calibrare la frequenza reale dei pareggi a basso punteggio (`0-0`, `1-1`).
* **Integrazione Fattore Calciomercato (Rose 2026):** Modulo di calibrazione che pondera le previsioni storiche del modello con il valore attuale dei reparti offensivi e difensivi post-trasferimenti (da integrare nel prossimo progetto).
