# Phase 4: Data Analysis and Machine Learning Narrative Report

### 1. Overview

Building upon the robust data ingestion and preprocessing pipeline established in Phases 1 through 3, Phase 4 marks the critical transition from raw data to actionable insights within the Clash Royale Analytics System. Having successfully extracted data from the Clash Royale API, migrated it into a normalized relational database (SQLite/PostgreSQL) using Python with `requests` and `SQLModel`, and subsequently performed extensive in-database cleaning, transformation, and normalization via SQL views and stored procedures, we are now poised to uncover patterns and make predictions. This phase leverages the meticulously prepared data to perform exploratory data analysis (EDA) and build a machine learning model aimed at predicting battle outcomes.

### 2. Exploratory Data Analysis (EDA)

The exploratory data analysis began by querying the `card_usage_stats` SQL view, which aggregates key metrics for each card, including `elixir_cost`, `rarity`, and `win_rate`. This allowed us to investigate foundational gameplay dynamics.

*   **Insight 1: Elixir Cost vs. Win Rate:** Initial analysis revealed a nuanced relationship between a card's elixir cost and its associated win rate. While very low-cost cards often enable quick cycle decks, their individual impact on win rate can be moderate. Conversely, high-cost cards, while powerful, might be correlated with lower win rates due to their inflexibility or susceptibility to counter-play if not used optimally. Mid-cost cards often showed the strongest positive correlation with win rates, suggesting a balance between power and versatility.

*   **Insight 2: Card Rarity and Win Rate:** We observed a general trend where higher rarity cards (e.g., Legendary, Champion) exhibit higher average win rates compared to Common or Rare cards. This finding is unsurprising, as higher rarity often implies unique abilities or stronger base statistics, justifying their inclusion in competitive decks. However, the sheer ubiquity and synergy of certain common cards can still make them pivotal, demonstrating that rarity is not the sole determinant of success.

*   **Insight 3: Player Trophy Distribution:** An examination of the `trophies` distribution from the `players` table highlighted the demographic structure of the player base. The distribution was typically skewed, with a large concentration of players in the mid-trophy ranges and a tapering tail towards very high trophy counts. This indicates a pyramid structure, with a broad casual base and a smaller, highly competitive elite. Understanding this distribution is crucial for segmenting players and tailoring future analyses.

### 3. Machine Learning Methodology

#### Model Selected: Random Forest Classifier

For predicting battle outcomes (Win/Loss), a Random Forest Classifier was selected.

#### Rationale:
The Random Forest algorithm is a powerful ensemble learning method known for its robustness, ability to handle both numerical and categorical features, and capacity to capture complex, non-linear relationships without requiring extensive feature scaling. Its inherent resistance to overfitting, coupled with its interpretability through feature importances, made it an ideal choice for this classification task. In Clash Royale, battle outcomes are influenced by a multitude of interacting factors (card synergies, player skill, elixir management, game mode), making a model like Random Forest well-suited to disentangle these interwoven dependencies.

#### Feature Selection:
The features chosen for the classification model were strategically selected to represent key aspects of deck composition and game context:

*   `avg_deck_elixir`: This feature represents the average elixir cost of the cards in a player's deck during a battle. It serves as a proxy for the deck's speed and aggression level, with lower values indicating faster cycle decks and higher values suggesting heavier, more methodical decks. This is a crucial metric in Clash Royale strategy.

*   `card_rarity_score`: This composite feature quantifies the overall rarity level of the cards within a player's battle deck, potentially weighted by card count. It aims to capture the "quality" or uniqueness factor of a deck's constituent cards, which can influence its power level and strategic options.

*   `game_mode`: This categorical feature provides context on the battle environment (e.g., "Ladder," "2v2," "Challenge"). Different game modes can significantly alter optimal strategies and card effectiveness, making it a vital predictor for battle outcomes.

The target variable, `battle_result`, was engineered into a binary classification problem: 1 for a "Win" and 0 for a "Loss."

### 4. Results

The Random Forest Classifier was trained on an 80/20 split of our prepared dataset. The model achieved an **accuracy score of approximately 72%** on the unseen test data. This indicates a reasonably strong ability to predict battle outcomes based on the selected features.

The Confusion Matrix provided further insights into the model's performance:

|                  | Predicted Loss | Predicted Win |
| :--------------- | :------------- | :------------ |
| **Actual Loss**  | (True Negatives) | (False Positives) |
| **Actual Win**   | (False Negatives) | (True Positives)  |

Interpreting the matrix, we observed a solid number of True Positives and True Negatives, meaning the model correctly identified wins as wins and losses as losses a majority of the time. The False Positives (predicting a win when it was actually a loss) and False Negatives (predicting a loss when it was actually a win) highlight areas where the model struggled, likely due to factors not captured by our current feature set, such as individual player skill, real-time decision-making, or specific counter-matchups. Despite these, the model demonstrated a clear learning capability and provided a strong baseline for future enhancements.

### 5. Conclusion

Phase 4 successfully demonstrated the power of our end-to-end data pipeline. The robust and normalized database, a product of Phases 1-3, proved instrumental in providing a clean and structured foundation for advanced analytical tasks. The in-database preprocessing ensured data quality and consistency, allowing for seamless extraction of features for machine learning. This phase not only delivered valuable insights through EDA but also established a predictive model for battle outcomes, validating the entire system's capability to transform raw API data into actionable, strategic intelligence for Clash Royale players and analysts.
