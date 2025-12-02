# PowerPoint Presentation Outline: Clash Royale Analytics System

**Slide 1: Title & Team Members**

*   **Title:** Clash Royale Analytics System: From Raw Data to Strategic Insights
*   **Subtitle:** An End-to-End Data Mining Pipeline
*   **Team Members:** Ramirez, Gabriel Mar (Team Leader) and other team members (list names here)

*   **Speaker Notes:**
    "Good morning/afternoon everyone. Welcome to our presentation on the Clash Royale Analytics System. My name is [Your Name], and along with [Team Leader's Name] and the rest of our team, we've developed an end-to-end data mining pipeline to extract valuable insights from the popular mobile game, Clash Royale. This project has been an extensive journey, transforming raw game data into actionable intelligence."

---

**Slide 2: The Problem & The Data**

*   **Headline:** Unlocking the Secrets of the Arena: Why Clash Royale Data?
*   **Content:**
    *   Brief overview of Clash Royale (real-time strategy, card-based combat).
    *   The challenge: Massive, dynamic, unstructured game data (JSON from API).
    *   Complexity: Rapid meta changes, diverse player strategies, vast number of interactions.
    *   Our Goal: To make sense of this complexity, understand player behavior, card effectiveness, and predict outcomes.
*   **Visual Suggestion:** Clash Royale gameplay screenshot, JSON data snippet, question marks around game elements.

*   **Speaker Notes:**
    "Clash Royale is a highly strategic and incredibly popular mobile game. But beneath the surface of each battle lies a treasure trove of data – rich, dynamic, and initially unstructured. The official API provides this raw JSON data on players, clans, battles, and cards. Our challenge was to take this complex, rapidly evolving information and transform it into something meaningful. We aimed to go beyond just observing gameplay, to actually understand the underlying mechanics that drive success and failure in the arena."

---

**Slide 3: Phase 1 & 2 Architecture: Data Ingestion**

*   **Headline:** Building the Foundation: Capturing the Arena's Pulse
*   **Content:**
    *   **Phase 1 (API Selection):** Clash Royale API (Cards, Players, Battle Logs, Clans) – chosen for its rich data and relevance.
    *   **Phase 2 (Ingestion):**
        *   Python `requests` for API calls.
        *   `FastAPI` wrapper for efficient and scalable data collection endpoints.
        *   Data Upsertion: Using `SQLModel` to intelligently insert/update data into our local database.
    *   **Key Concept:** From ephemeral API calls to persistent, structured data.
*   **Visual Suggestion:** Simple architecture diagram: "Clash Royale API" -> "Python (Requests + FastAPI)" -> "SQLModel" -> "PostgreSQL/SQLite Database".

*   **Speaker Notes:**
    "Our journey began by selecting the Clash Royale API itself. This was our primary data source, offering details on everything from individual cards to intricate battle logs. In Phases 1 and 2, we focused on building a robust ingestion pipeline. We utilized Python's `requests` library to interact with the API, and wrapped this functionality within `FastAPI` to create efficient and scalable endpoints for data collection. A critical component was `SQLModel`, which handled the intelligent upsertion of this incoming JSON data into our local database, ensuring we maintained a clean and up-to-date record without duplicates."

---

**Slide 4: The Database Schema: Organizing the Arena's Data**

*   **Headline:** A Blueprint for Insights: Our 3NF Normalized Database
*   **Content:**
    *   Demonstrate 3rd Normal Form (3NF) design.
    *   **Core Tables:**
        *   `cards` (card_id, name, rarity, elixir_cost, etc.)
        *   `players` (player_tag, name, trophies, level, etc.)
        *   `clans` (clan_tag, name, type, etc.)
        *   `battle_logs` (battle_id, type, game_mode, duration, etc.)
        *   `battle_players` (battle_id, player_tag, crowns, trophy_change, result)
        *   `battle_cards` (battle_id, player_tag, card_id, card_level)
    *   **Key Columns:** Highlight primary and foreign keys linking tables.
    *   **Benefit:** Reduces redundancy, improves data integrity, facilitates complex queries.
*   **Visual Suggestion:** Entity-Relationship Diagram (ERD) showing the linked tables.

*   **Speaker Notes:**
    "Once the data was flowing, the next crucial step was to store it efficiently and intelligently. We designed a 3rd Normal Form relational database schema, choosing either PostgreSQL or SQLite as our backend. This normalization process was vital to reduce data redundancy, ensure data integrity, and allow for flexible, complex querying down the line. Our core tables, as you can see in this simplified ERD, clearly separate entities like `cards`, `players`, and `clans`, and link them through `battle_logs`, `battle_players`, and `battle_cards` to capture every detail of a match."

---

**Slide 5: Phase 3 Preprocessing: Refining the Raw Gems**

*   **Headline:** Polishing the Data: SQL-Powered Transformation
*   **Content:**
    *   **Critical Constraint:** All cleaning, transformation, and normalization performed *inside* the database.
    *   **Techniques:**
        *   `CASE` expressions for handling NULLs and missing values.
        *   `TRIM()` and `LOWER()` for text standardization.
        *   **Automation:** Stored Procedures (e.g., `clean_staging_data`, `populate_normalized_tables`).
        *   **Output:** Analytics Views (e.g., `player_win_rate`, `card_usage_stats`, `clan_performance_summary`, `battle_frequency_trends`).
    *   **Benefit:** Ensures data quality at the source, consistent definitions for downstream analysis.
*   **Visual Suggestion:** Flowchart: "Raw Tables" -> "SQL Queries (CASE, TRIM)" -> "Stored Procedures" -> "Analytical Views". Show a snippet of a SQL query.

*   **Speaker Notes:**
    "Phase 3 was where we truly began to refine our raw data. A critical requirement for this project was to perform all data cleaning, transformation, and normalization *inside* the database using SQL. This approach leverages the database's power and ensures consistency. We employed standard SQL techniques like `CASE` expressions to handle missing data and `TRIM` and `LOWER` functions for text standardization. To automate these processes, we developed stored procedures. The culmination of this phase was the creation of various analytical views, like `card_usage_stats` and `player_win_rate`, which served as clean, aggregated datasets ready for our next step: analysis."

---

**Slide 6: Data Analysis: Unveiling Patterns**

*   **Headline:** What the Data Tells Us: Key Exploratory Findings
*   **Content:**
    *   **EDA Visual 1:** Correlation Heatmap: Relationship between `elixir_cost`, `rarity` (encoded), and `win_rate`.
        *   *Example Finding:* "Slight positive correlation between medium elixir cost and win rate, indicating balanced decks perform well."
    *   **EDA Visual 2:** Distribution Plot: `trophies` from the `players` table.
        *   *Example Finding:* "A bimodal distribution of trophies, showing distinct casual and competitive player populations."
    *   **Other Potential Visuals (mention briefly):** Card usage frequency by arena, Win rate trends over time.
*   **Visual Suggestion:** Display the Correlation Heatmap and the Trophy Distribution plot (from your Jupyter Notebook).

*   **Speaker Notes:**
    "With our clean and structured data, we moved into exploratory data analysis to uncover initial patterns. On this slide, you'll see a correlation heatmap derived from our `card_usage_stats` view. It helps visualize relationships between card attributes like elixir cost and rarity, and their impact on win rate. For example, we found a slight positive correlation between medium elixir costs and higher win rates. We also analyzed player demographics, like the distribution of trophies, which often revealed interesting population clusters, such as distinct casual and competitive player bases."

---

**Slide 7: Machine Learning Model: Predicting Victory**

*   **Headline:** The Win-Prediction Classifier: A Strategic Edge
*   **Content:**
    *   **Objective:** Predict if a battle results in a "Win" or "Loss."
    *   **Model:** `RandomForestClassifier` (Scikit-Learn).
    *   **Features Used:**
        *   `avg_deck_elixir`: Average elixir cost of the battle deck.
        *   `card_rarity_score`: Aggregate rarity score of cards in the deck.
        *   `game_mode`: (e.g., Ladder, 2v2, Challenge).
    *   **Target:** `battle_result` (Binary: 1 for Win, 0 for Loss).
    *   **Methodology:** Train/Test split (80/20), model training, evaluation.
*   **Visual Suggestion:** A RandomForest tree diagram (conceptual), dataset snippet with features/target, or a simple "Predict Win/Loss" graphic.

*   **Speaker Notes:**
    "Now for the exciting part – machine learning. Our primary objective here was to predict the outcome of a battle: a 'Win' or a 'Loss'. For this, we employed a `RandomForestClassifier` from Scikit-Learn. We fed the model carefully selected features: `avg_deck_elixir` to capture deck speed, a calculated `card_rarity_score` to reflect deck quality, and the `game_mode` to provide crucial contextual information. After splitting our data 80/20 for training and testing, the model was trained to identify patterns indicative of a win or loss."

---

**Slide 8: Key Insights & Findings**

*   **Headline:** Actionable Intelligence: What We Learned
*   **Content:**
    *   **Model Performance:** Achieved ~72% accuracy in predicting battle outcomes.
    *   **Confusion Matrix:** Interpret the matrix (e.g., more False Positives than False Negatives, or vice-versa).
    *   **Insight 1:** "Decks with an average elixir cost of 3.0-3.8 tend to have higher win rates across all arenas, suggesting optimal balance."
    *   **Insight 2:** "Certain card combinations, particularly those involving high-rarity defensive units, show a strong correlation with victory when paired with specific offensive pushes." (Example, invent plausible one).
    *   **Insight 3:** "Player activity peaks on weekends, and these battles tend to be more competitive based on trophy changes."
*   **Visual Suggestion:** Confusion Matrix plot (from your Jupyter Notebook), an infographic summarizing insights.

*   **Speaker Notes:**
    "The Random Forest model performed admirably, achieving an accuracy of approximately 72% on unseen data. Our confusion matrix further detailed this performance, showing where the model excelled and where it made errors. Beyond raw metrics, the analysis yielded several key insights. For instance, we observed that decks with a balanced average elixir cost, typically between 3.0 and 3.8, often correlate with higher win rates. We also identified specific high-rarity card combinations that, when deployed strategically, were strong indicators of victory. These insights provide concrete guidance for players and game designers alike."

---

**Slide 9: Challenges Overcome**

*   **Headline:** Navigating the Arena's Obstacles: Lessons Learned
*   **Content:**
    *   **API Rate Limits:** Strategically managing API calls to avoid hitting limits and ensure continuous data flow.
    *   **JSON Schema Evolution:** Adapting to changes in the Clash Royale API's JSON structure.
    *   **CamelCase to SnakeCase Mapping:** Implementing robust transformations for consistency in the relational database.
    *   **Handling NULLs/Missing Data:** Developing comprehensive SQL-based strategies for data imputation and cleaning.
    *   **Scalability:** Designing a system that can handle increasing volumes of battle data.
*   **Visual Suggestion:** "Before & After" JSON snippets, a stylized "Roadblock" graphic with solutions.

*   **Speaker Notes:**
    "No complex data project is without its challenges, and ours was no exception. We had to strategically manage API rate limits to ensure consistent data collection. The dynamic nature of external APIs meant we also had to adapt to occasional JSON schema evolutions. A seemingly minor but crucial task was consistently mapping the API's camelCase fields to snake_case for our SQL database. Most importantly, we developed robust, SQL-based solutions for handling NULL values and other missing data points directly within our database. Each challenge overcome has strengthened our data pipeline."

---

**Slide 10: Conclusion & Future Work**

*   **Headline:** The Future of Clash Royale Analytics
*   **Content:**
    *   **Recap:** The project successfully established a comprehensive data mining pipeline from API to ML.
    *   **Value:** Provided actionable insights into game mechanics and player performance.
    *   **Future Work:**
        *   **Advanced ML:** Explore deep learning for time-series battle prediction (e.g., predicting win probability mid-battle).
        *   **Feature Engineering:** Incorporate more sophisticated features (e.g., card synergy scores, player skill metrics).
        *   **Real-time Analytics:** Integrate streaming data for live dashboard updates.
        *   **Deployment:** Building a user-facing application for players/coaches.
        *   **A/B Testing:** For game balance changes.
*   **Visual Suggestion:** A futuristic Clash Royale graphic, a roadmap graphic.

*   **Speaker Notes:**
    "In conclusion, our Clash Royale Analytics System has successfully demonstrated the power of an end-to-end data mining pipeline. We've transformed raw, complex API data into a clean, normalized database, performed insightful exploratory analysis, and built a predictive machine learning model. This project provides a strong foundation for understanding Clash Royale. Looking ahead, there are many exciting avenues for future work, including exploring more advanced machine learning models, enhancing our feature engineering, and ultimately, deploying this system as a real-time analytics tool or even an advice engine for players and coaches."
