<div align="center">

<!-- Header Banner — Spotify gradient wave: black → green → black -->
<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=1DB954&height=160&section=header&text=Spotify%20SQL%20Analysis&fontSize=42&fontColor=ffffff&fontAlignY=55"/>

<!-- Animated typing subtitle -->
[![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=16&pause=1000&color=1DB954&center=true&vCenter=true&width=600&lines=A+Competitor+Intelligence+Study;SQL+%7C+Python+%7C+MySQL;Analyzing+Spotify's+Music+Catalog)](https://git.io/typing-svg)

<!-- Tech Badges — alternating Spotify black & green -->
![MySQL](https://img.shields.io/badge/MySQL-1DB954?style=for-the-badge&logo=mysql&logoColor=191414)
![Python](https://img.shields.io/badge/Python-191414?style=for-the-badge&logo=python&logoColor=1DB954)
![Pandas](https://img.shields.io/badge/Pandas-1DB954?style=for-the-badge&logo=pandas&logoColor=191414)
![Kaggle](https://img.shields.io/badge/Kaggle-191414?style=for-the-badge&logo=kaggle&logoColor=1DB954)
![Status](https://img.shields.io/badge/Status-In%20Progress-1DB954?style=for-the-badge&logo=clockify&logoColor=191414)


<br/>

[![GitHub](https://img.shields.io/badge/GitHub-BabithaRavindra-191414?style=flat-square&logo=github&logoColor=1DB954)](https://github.com/BabithaRavindra)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-babitha--ravindra-1DB954?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/babitha-ravindra)
[![Spotify](https://img.shields.io/badge/Data%20Source-Spotify-191414?style=flat-square&logo=spotify&logoColor=1DB954)](https://www.kaggle.com)

</div>

---

## Table of Contents

- [![](https://img.shields.io/badge/Project%20Overview-1DB954?style=flat-square)](#project-overview)
- [![](https://img.shields.io/badge/Dataset%20Information-191414?style=flat-square)](#dataset-information)
- [![](https://img.shields.io/badge/Database%20Schema-1DB954?style=flat-square)](#database-schema)
- [![](https://img.shields.io/badge/Methodology-191414?style=flat-square)](#methodology)
- [![](https://img.shields.io/badge/SQL%20Analysis-1DB954?style=flat-square)](#sql-analysis)
- [![](https://img.shields.io/badge/Key%20Insights%20%26%20Findings-191414?style=flat-square)](#key-insights--findings)
- [![](https://img.shields.io/badge/Tools%20%26%20Technologies-1DB954?style=flat-square)](#tools--technologies)
- [![](https://img.shields.io/badge/Repository%20Structure-191414?style=flat-square)](#repository-structure)
- [![](https://img.shields.io/badge/Author-1DB954?style=flat-square)](#author)

---

## Project Overview

> *"Leveraging structured query language and data wrangling techniques to extract competitive intelligence from Spotify's music catalog — uncovering genre trends, artist performance, and listener engagement patterns."*

This project applies **Python-based data wrangling** and **advanced SQL analysis** to a Kaggle dataset of Spotify tracks, audio features, and artist metadata. The cleaned data is normalized into a relational MySQL database and queried to derive strategic insights for platform and competitor analysis.

**Core Objectives:**

| Objective | Description |
|-----------|-------------|
| Playlist Optimization | Derive optimal audio feature combinations from top-performing tracks to improve curation |
| Genre Focus | Identify high-performing, low-variance genres for stable listener engagement |
| Artist Discovery | Surface rising or versatile artists with consistent cross-genre growth |
| Avoid Saturation | Analyze genre distribution variance to guide content diversification |
| Platform Strategy | Compare explicit content presence across genres for audience targeting |
| Predictive Hits | Build a framework for forecasting potential hit songs from audio attributes |

---

## Dataset Information

- **Source:** [![Kaggle](https://img.shields.io/badge/Kaggle-Spotify%20Tracks%20Dataset-1DB954?style=flat-square&logo=kaggle&logoColor=191414)](https://www.kaggle.com)
- **Scope:** Tracks, artists, genres, and audio features from Spotify's music catalog
- **File:** `Dataset.xlsx` (sheet: `dataset`)

| Column | Description | Type |
|--------|-------------|------|
| `Track_id` | Unique identifier for each track | Categorical |
| `Track_name` | Title of the musical piece | Categorical |
| `Artists` | Performer or group credited with the track | Categorical |
| `Track_genre` | Musical style (Pop, Hip-Hop, Rock, etc.) | Categorical |
| `Popularity` | Score (0–100) based on plays, skips, and recency | Numerical |
| `Danceability` | Suitability for dancing (0.0–1.0) | Numerical |
| `Energy` | Perceptual measure of intensity and activity (0.0–1.0) | Numerical |
| `Valence` | Musical positiveness conveyed by a track (0.0–1.0) | Numerical |
| `Tempo` | Overall estimated tempo in BPM | Numerical |
| `Duration_ms` | Track length in milliseconds | Numerical |
| `Explicit` | Whether the track contains explicit content | Boolean |
| `Artist_popularity` | Overall popularity score of the artist | Numerical |

> **Data Quality Note:** The raw dataset contained missing values in popularity and audio feature fields, potential duplicate entries due to multi-genre track mappings, and inconsistent text formats in artist and genre columns. All issues were resolved during the data wrangling phase.
---

## Database Schema

The cleaned data was normalized into a **four-table relational schema** in MySQL:

```
Artists_DET                 Genre_DET
──────────────────          ──────────────────
Artist_id  (PK)             Genre_id   (PK)
Artist                      Genre

        Tracks_DET
        ──────────────────────────────────
        Track_id     (PK)
        Artist_id    (FK → Artists_DET)
        Genre_id     (FK → Genre_DET)
        Attribute_id (FK → Attributes_DET)
        Track_name

Attributes_DET
──────────────────────────────────────────────
Attribute_id   (PK)
Popularity, Duration_ms, Explicit
Danceability, Energy, Key, Loudness, Mode
Speechiness, Acousticness, Instrumentalness
Liveness, Valence, Tempo, Time_signature
```

---

## Methodology

The project followed a structured **four-phase pipeline:**

```
Data Collection  ──►  Data Wrangling  ──►  Database Design  ──►  SQL Analysis
   Kaggle              Python/Pandas         MySQL Schema         10 Queries
```

### Phase 1 — Data Collection
- Downloaded the Spotify Tracks Dataset from Kaggle
- Loaded the Excel workbook into a Pandas DataFrame

### Phase 2 — Data Wrangling (Python)
- **Dropped** unnamed index column and standardized all column headers
- **Missing Values:** Identified columns with nulls; removed affected rows (< 10% of total data)
- **Duplicate Handling:** Tracks appearing in multiple genres created duplicate rows — resolved by:
  - Assigning a shared `Genre_id` to all rows belonging to the same track
  - Labelling tracks as `single-genre` or `multi-genre` via a `Genre_count` column
  - Retaining one row per track and moving genre mappings to a dedicated `Genre_DET` table
  - Result: 24,244 duplicate rows (~21.28% of dataset) resolved without data loss
- **ID Generation:** Unique `Genre_id`, `Artist_id`, and `Attribute_id` values generated using random alphanumeric strings
- **Export:** Four normalized DataFrames (`Artists`, `Genre`, `Attributes`, `Tracks`) pushed to MySQL via `mysql-connector-python`

### Phase 3 — Database Design (MySQL)
- Created `Spotify` database with four normalized tables
- Enforced referential integrity through foreign key constraints
- Schema designed to support efficient JOINs across all analysis queries

### Phase 4 — SQL Analysis
- Formulated 10 structured queries across four analytical categories
- Applied window functions (`DENSE_RANK`), aggregations, `HAVING` clauses, and subqueries

---

## SQL Analysis

### Genre Performance & Trends

**Q1. Which genre has the highest average popularity across all tracks?**
```sql
SELECT G.Genre, AVG(AD.Popularity) AS Average_Popularity
FROM Genre_DET G
JOIN Tracks_DET T ON G.Genre_id = T.Genre_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY G.Genre
ORDER BY Average_Popularity DESC
LIMIT 1;
```

**Q2. Which genre has the longest average track duration?**
```sql
SELECT G.Genre, AVG(AD.Duration_ms) AS Average_Duration
FROM Genre_DET G
JOIN Tracks_DET T ON G.Genre_id = T.Genre_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY G.Genre
ORDER BY Average_Duration DESC
LIMIT 1;
```

**Q3. Detect genre fatigue — genres with high energy and tempo variance**
```sql
SELECT G.Genre,
       STDDEV(AD.Energy) AS Energy_STD,
       STDDEV(AD.Tempo) AS Tempo_STD
FROM Genre_DET G
JOIN Tracks_DET T ON G.Genre_id = T.Genre_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY G.Genre
ORDER BY Energy_STD DESC, Tempo_STD DESC;
```

---

### Artist Influence & Visibility

**Q4. Which artists consistently appear in the top 5 most popular tracks across genres?**
```sql
SELECT A.Artist, AVG(AD.Popularity) AS Average_Popularity
FROM Artists_DET A
JOIN Tracks_DET T ON A.Artist_id = T.Artist_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY A.Artist
HAVING COUNT(T.Track_id) >= 5
ORDER BY Average_Popularity DESC;
```

**Q5. Which artists have contributed to the highest number of unique genres?**
```sql
SELECT A.Artist, COUNT(DISTINCT T.Genre_id) AS Number_of_Unique_Genres
FROM Artists_DET A
JOIN Tracks_DET T ON A.Artist_id = T.Artist_id
GROUP BY A.Artist
ORDER BY Number_of_Unique_Genres DESC;
```

**Q6. Which artists have the highest average energy and danceability across their tracks?**
```sql
SELECT A.Artist,
       AVG(AD.Energy) AS Average_Energy,
       AVG(AD.Danceability) AS Average_Danceability
FROM Artists_DET A
JOIN Tracks_DET T ON A.Artist_id = T.Artist_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY A.Artist
ORDER BY Average_Energy DESC, Average_Danceability DESC;
```

---

### Listener Engagement & Interaction

**Q7. Which genre has the highest combined average valence and energy?**
```sql
SELECT G.Genre,
       (AVG(AD.Valence) + AVG(AD.Energy)) AS Combined_Valence_Energy
FROM Genre_DET G
JOIN Tracks_DET T ON G.Genre_id = T.Genre_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY G.Genre
ORDER BY Combined_Valence_Energy DESC
LIMIT 1;
```

**Q8. What is the average danceability score per genre, ranked highest to lowest?**
```sql
SELECT G.Genre, AVG(AD.Danceability) AS Average_Danceability
FROM Genre_DET G
JOIN Tracks_DET T ON G.Genre_id = T.Genre_id
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
GROUP BY G.Genre
ORDER BY Average_Danceability DESC;
```

---

### Track-Level Insights

**Q9. Top 3 most popular tracks per genre using DENSE_RANK**
```sql
SELECT Genre, Track_Name, Popularity
FROM (
    SELECT G.Genre, T.Track_Name, AD.Popularity,
           DENSE_RANK() OVER (PARTITION BY G.Genre ORDER BY AD.Popularity DESC) AS drnk
    FROM Genre_DET G
    JOIN Tracks_DET T ON G.Genre_id = T.Genre_id
    JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
) AS RankedTracks
WHERE drnk <= 3;
```

**Q10. Tracks that are outliers in both tempo and energy (2 standard deviations from mean)**
```sql
SELECT T.Track_Name, AD.Tempo, AD.Energy
FROM Tracks_DET T
JOIN Attributes_DET AD ON T.Attribute_id = AD.Attribute_id
WHERE (AD.Tempo < (SELECT AVG(Tempo) - 2 * STDDEV(Tempo) FROM Attributes_DET)
    OR AD.Tempo > (SELECT AVG(Tempo) + 2 * STDDEV(Tempo) FROM Attributes_DET))
AND   (AD.Energy < (SELECT AVG(Energy) - 2 * STDDEV(Energy) FROM Attributes_DET)
    OR AD.Energy > (SELECT AVG(Energy) + 2 * STDDEV(Energy) FROM Attributes_DET));
```

---

## Key Insights & Findings

| # | Insight |
|---|---------|
| 1 | Genre with the **highest average popularity** identified, enabling content investment prioritization |
| 2 | Genres with **high energy and tempo variance** flagged as candidates for audience fatigue |
| 3 | Artists with **cross-genre presence** surfaced as versatile talents for partnership strategies |
| 4 | **Combined valence + energy** score reveals the most emotionally engaging genre on the platform |
| 5 | **DENSE_RANK** window function used to extract top tracks per genre without ties distorting results |
| 6 | **Outlier tracks** in tempo and energy represent high-risk, high-reward content for playlist experimentation |
| 7 | 24,244 duplicate rows (~21.28% of dataset) resolved without any loss of genre mapping information |

### Challenges Identified
- **Multi-Genre Duplicates** — Required a custom ID-based deduplication strategy rather than simple row removal
- **Missing Audio Features** — Affected rows removed after confirming they represented less than 10% of total data
- **Schema Normalization** — Denormalized source data required careful decomposition into four relational tables
- **Referential Integrity** — Foreign key ordering during MySQL insertion required strict table creation sequence

---

## Tools & Technologies

<div align="center">

| Tool | Purpose |
|------|---------|
| ![Python](https://img.shields.io/badge/Python-191414?style=flat-square&logo=python&logoColor=1DB954) | Data wrangling, ID generation, and MySQL export |
| ![Pandas](https://img.shields.io/badge/Pandas-1DB954?style=flat-square&logo=pandas&logoColor=191414) | DataFrame manipulation, deduplication, and transformation |
| ![MySQL](https://img.shields.io/badge/MySQL-191414?style=flat-square&logo=mysql&logoColor=1DB954) | Relational database design, schema creation, and SQL analysis |
| ![Jupyter](https://img.shields.io/badge/Jupyter-1DB954?style=flat-square&logo=jupyter&logoColor=191414) | Interactive notebook environment for step-by-step wrangling |
| ![Microsoft Excel](https://img.shields.io/badge/Microsoft%20Excel-191414?style=flat-square&logo=microsoftexcel&logoColor=1DB954) | Source dataset format |
| ![Kaggle](https://img.shields.io/badge/Kaggle-1DB954?style=flat-square&logo=kaggle&logoColor=191414) | Primary data source |
| ![PowerPoint](https://img.shields.io/badge/PowerPoint-191414?style=flat-square&logo=microsoftpowerpoint&logoColor=1DB954) | Project presentation and reporting |

</div>

---

## Repository Structure

```
Spotify-SQL-Analysis/
├── [Date]_Spotify-SQL-Analysis_Dataset.xlsx              # Raw Kaggle dataset (Excel)
├── [Date]_Spotify-SQL-Analysis_DataWrangling.ipynb       # Python data wrangling notebook
├── [Date]_Spotify-SQL-Analysis_Presentation.pptx         # Full project presentation
└── README.md                                              # Project documentation
```

> **Note:** Replace `[Date]` placeholders with your actual file dates once confirmed (e.g., `20250403`).

---

## Author

<div align="center">

**Babitha Ravindra**
*Data Analyst | SQL Developer | Python Enthusiast*

[![GitHub](https://img.shields.io/badge/GitHub-BabithaRavindra-191414?style=for-the-badge&logo=github&logoColor=1DB954)](https://github.com/BabithaRavindra)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-1DB954?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/babitha-ravindra)
[![Spotify](https://img.shields.io/badge/Spotify-Project-191414?style=for-the-badge&logo=spotify&logoColor=1DB954)](https://www.kaggle.com)

</div>

---

<div align="center">

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=1DB954&height=100&section=footer"/>

</div>
