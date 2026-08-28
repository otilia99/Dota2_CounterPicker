# Dota 2 Hero Counter Picker and Situational Item Recommendation Project (in progress)

Dota 2 counter picker and item timing analysis using OpenDota API and Databricks.

## Project Structure

### Python Utility Modules

* **`dota2_utils.py`** - Core utilities and data fetching
  * API configuration (BASE_URL, HEADERS)
  * Hero image display functions with color correction
  * Data fetching functions (heroes, matchups)
  * Item validation utilities

* **`dota2_analysis.py`** - Analysis functions
  * Counter picking analysis (`get_best_counters`)
  * Item timing analysis (`check_item_timing`)
  * Mid-game decision support (`midgame_advice`)
  * Item popularity by phase

### Notebooks

1. **`1_Data_Setup`** - Initial data collection
   * Fetches hero metadata from OpenDota
   * Fetches hero matchup statistics
   * Creates Delta tables: `dota_heroes`, `dota_hero_matchups`
   * **Run this one first**

2. **`2_Counter_Picker`** - Counter picking analysis
   * Analyze enemy team composition
   * Get hero recommendations based on win rates
   * Visualize top counters with images

3. **`3_Item_Analysis`** - Item timing and purchase optimization
   * Check optimal item purchase timing
   * Get mid-game advice based on matchups
   * Visualize win rates by purchase minute

4. **`4_Hero_Browser`** - Hero visualization and browsing
   * Browse heroes by attribute (STR, AGI, INT)
   * Browse heroes by role
   * Display hero images with proper color rendering

### Info

* **`Dota2_Daily_Data_Collection`** - Scheduled ETL pipeline (still active)

## Getting Started

### Step 1: Set Up Data

Run the **`1_Data_Setup`** notebook first to fetch and store hero data:

```python
# This will create two Delta tables:
# - dota_heroes (hero metadata)
# - dota_hero_matchups (win rates between hero pairs)
```

This takes 10-15 minutes due to API rate limiting.

### Step 2: Use the Analysis Notebooks

Once data is set up, you can use any of the analysis notebooks:

* **Counter Picking**: Open `2_Counter_Picker` to find counters for enemy teams
* **Item Analysis**: Open `3_Item_Analysis` to check optimal item timings
* **Hero Browser**: Open `4_Hero_Browser` to browse and visualize heroes

## Delta Tables

### `dota_heroes`

| Column | Type | Description |
|--------|------|-------------|
| hero_id | int | Unique hero identifier |
| name | string | Hero display name |
| primary_attr | string | Primary attribute (str/agi/int) |
| attack_type | string | Melee or Ranged |
| roles | string | Comma-separated roles |
| img | string | OpenDota image URL |

### `dota_hero_matchups`

| Column | Type | Description |
|--------|------|-------------|
| hero_id | int | Hero identifier |
| against_hero_id | int | Enemy hero identifier |
| games_played | int | Number of games in sample |
| wins | int | Number of wins |
| win_rate | double | Win rate percentage |


## Configuration

API configuration is centralized in `dota2_utils.py`:

```python
BASE_URL = "https://api.opendota.com/api"
HEADERS = {"User-Agent": "Databricks-Dota2-Pipeline/1.0"}
```

## Hero Image Display

Hero images are fetched from Steam CDN with proper color correction:

* RGB channel swapping to fix color inversion
* Alpha channel handling for transparency
* Base64 encoding for notebook display
* Special case mapping for hero name variations

## Notes

* The OpenDota API has rate limits (1 request per second)
* Hero matchup data requires ~15 minutes to fetch for all heroes
* Item timing data is fetched on-demand from the API
* All analysis functions work with Spark DataFrames for scalability



## Dependencies

* PySpark
* pandas
* requests
* matplotlib
* Pillow (PIL)

All dependencies are available in standard Databricks Runtime.
