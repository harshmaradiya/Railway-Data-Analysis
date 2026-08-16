# Indian Railways Dataset – Data Analysis

A Python-based data analysis project on an Indian Railways route and station dataset. The project explores train routes, station frequency, journey duration, route classification, data quality, and an interactive train-enquiry function.

## Project Overview

This project analyzes railway schedule data at the **train-station level**. Each row represents a station stop for a train and contains train information, station details, fare-related class values, schedule times, route number, and cumulative distance.

The analysis is organized into six levels:

1. **Basic Data Review** – dataset structure, train routes, and number of stops.
2. **Simple Data Processing** – time standardization, journey duration, route classification, and station frequency.
3. **Data Quality Checks** – suspicious schedule values, duplicates, and station-order validation.
4. **Basic Analysis & Visualization** – route-duration comparison and high-traffic stations.
5. **Advanced Analysis & Visualization** – pivot tables, cross-tabulation, and route-type distribution by station.
6. **Final Capstone System** – a route-based train enquiry function for finding direct trains between two stations.

The notebook explicitly limits the tools by level: Pandas for data handling, Matplotlib/Seaborn for visualization, and Python for the final enquiry-system logic.

## Dataset

**File:** `Dataset1_verified.csv`

The verified dataset contains:

- **186,074 records**
- **12 attributes**
- **11,113 unique trains**
- **8,099 unique station names**
- **0 exact duplicate rows**

### Columns

| Column | Description |
|---|---|
| `SN` | Sequential station number within a train route |
| `Train_No` | Train number |
| `Station_Code` | Station code |
| `1A` | First AC class value |
| `2A` | Second AC class value |
| `3A` | Third AC class value |
| `SL` | Sleeper class value |
| `Station_Name` | Station name |
| `Route_Number` | Route identifier |
| `Arrival_time` | Scheduled arrival time |
| `Departure_Time` | Scheduled departure time |
| `Distance` | Cumulative distance from the train's starting point |

> **Note:** The notebook treats the `1A`, `2A`, `3A`, and `SL` fields as numeric dataset values. Their business meaning is not further defined in the notebook.

## Project Files

```text
.
├── Dataset1_verified.csv
├── Railway_Data_Analysis_Project.ipynb
└── README.md
```

## Technologies Used

- Python
- Pandas
- Matplotlib
- Seaborn
- Jupyter Notebook

## Analysis Workflow

### Level 1 – Basic Data Review

The project first examines the overall structure of the dataset and identifies:

- Number of records and attributes
- Dataset columns
- Starting and ending stations for each train
- Number of stops per train
- Trains with maximum and minimum numbers of stops

**Key result:**

- Train **53041** has the maximum number of stops: **118**
- The minimum number of stops is **2**
- **1,249 trains** are tied for the minimum

## Level 2 – Simple Data Processing

### Schedule Standardization

`Arrival_time` and `Departure_Time` are converted into standardized time values using Pandas datetime operations.

### Journey Duration

Journey duration is estimated from the first departure time to the final arrival time for each train. When the final arrival is earlier than the initial departure, the analysis adds one day to account for an overnight journey.

### Route Classification

Routes are classified using total cumulative distance:

| Route Type | Distance Rule | Number of Trains |
|---|---:|---:|
| Short | `< 300 km` | 8,221 |
| Medium | `300–799 km` | 1,346 |
| Long | `>= 800 km` | 1,546 |

This classification is based on the thresholds implemented in the notebook, not on an external railway classification.

### Station Frequency

The analysis calculates how many unique trains serve each station.

Top stations by train frequency include:

| Station | Train Count |
|---|---:|
| CST-MUMBAI | 1,027 |
| KALYAN JN | 828 |
| THANE | 796 |
| SEALDAH | 745 |
| CHENNAI BEAC | 738 |
| HOWRAH JN. | 699 |
| DADAR | 598 |
| DUM DUM JN. | 463 |
| KURLA | 462 |
| TAMBARAM | 434 |

## Level 3 – Data Quality Checks

The notebook performs three main validation checks.

### 1. Suspicious `00:00:00` Schedule Values

`00:00:00` is treated as a legitimate placeholder at a train's origin or terminus. The analysis specifically checks for such values appearing at intermediate stops.

- **63 suspicious mid-route entries** were identified.

These entries are flagged rather than automatically overwritten.

### 2. Duplicate Records

Exact duplicate rows were removed using Pandas `drop_duplicates()`.

- **0 duplicate rows were removed.**

### 3. Station Sequence Validation

The project checks whether station sequence numbers (`SN`) run continuously from 1 through the number of stops for each train.

- **11,107 of 11,113 trains** have correct sequential order.
- **6 trains** have bad station-order sequences.

The notebook nevertheless saves the deduplicated dataset as `Dataset1_verified.csv`; the six ordering issues are reported for validation rather than silently corrected.

## Level 4 – Analysis & Visualization

The project compares journey duration across the three route types.

### Average Journey Duration

| Route Type | Average Duration |
|---|---:|
| Short | 2.11 hours |
| Medium | 11.14 hours |
| Long | 12.15 hours |

The analysis shows that journey duration increases strongly from Short to Medium routes, while Medium and Long routes are relatively close in average duration.

The notebook also visualizes the ten highest-frequency stations.

### Main Observation

Traffic is concentrated around major stations such as **CST-MUMBAI, KALYAN JN, THANE, DADAR, and KURLA**, reflecting a strong concentration of train services around Mumbai-area stations in this dataset.

## Level 5 – Advanced Analysis

The project creates:

- Pivot tables by station and route type
- Cross-tabulations of station and route type
- Stacked comparative charts

### Key Insights

- **Chennai Beach, Kurla, and Dum Dum Jn.** are served almost exclusively by Short-route trains in the notebook's analysis, indicating strong commuter-oriented usage.
- **Vijayawada Jn.** shows predominantly Long-route services, indicating a different role as a long-distance transit hub.
- **Kalyan Jn. and Howrah Jn.** have a mix of Short, Medium, and Long routes, making them multi-purpose junctions in this dataset.

## Level 6 – Train Enquiry System

The notebook implements a Python function called `find_trains()`.

It accepts:

```text
source station
destination station
dataset
```

The function:

1. Normalizes station names to uppercase.
2. Groups records by train.
3. Checks whether both stations occur on the same train.
4. Ensures the source station appears before the destination station.
5. Calculates the estimated journey duration between the two stations.
6. Returns the matching train number, departure time, arrival time, and duration.

A simple interactive wrapper, `train_enquiry_system()`, allows a user to enter source and destination stations.

### Example

The notebook demonstrates a search from:

```text
THANE → CST-MUMBAI
```

The demo finds **206 direct trains** in the supplied dataset.

## Key Business/Analytical Takeaways

- The dataset contains a large number of train-station observations, making it suitable for route and station-level analysis.
- Short routes dominate the dataset under the project's distance thresholds.
- Major metropolitan/junction stations account for a large share of train frequency.
- Journey duration increases substantially when moving from Short to Medium routes.
- Station-order and schedule validation are important because the raw schedule contains anomalies such as suspicious intermediate `00:00:00` values and six trains with non-sequential station numbering.
- Pivot tables and cross-tabulations help distinguish commuter-oriented stations from long-distance transit hubs.
- The enquiry system demonstrates how the analysis can be extended from descriptive analytics into a small practical Python application.

## Important Data-Quality Notes

This README intentionally follows the logic and results implemented in the supplied notebook.

- The project reports **63 suspicious mid-route `00:00:00` values**.
- The project reports **6 trains with bad station-order sequences**.
- No exact duplicate rows were found.
- `00:00:00` at an origin or terminus is treated as a legitimate placeholder by the notebook.
- Journey duration is estimated from schedule times and includes an overnight adjustment when the final arrival time is earlier than the initial departure time.
- Route type is based on the notebook's thresholds: `<300 km`, `300–799 km`, and `>=800 km`.
- The dataset contains schedule and cumulative-distance information; it does not provide passenger counts, actual delays, cancellations, or ticket-sales outcomes. Therefore, conclusions about passenger demand or operational performance should be interpreted as service-frequency observations rather than measured passenger behavior.

## How to Run

### 1. Install dependencies

```bash
pip install pandas matplotlib seaborn jupyter
```

### 2. Keep the files together

Place the following files in the same directory:

```text
Dataset1_verified.csv
Railway_Data_Analysis_Project.ipynb
```

### 3. Start Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
Railway_Data_Analysis_Project.ipynb
```

### 4. Run the notebook

Run the cells from top to bottom so that the data-cleaning, derived fields, visualizations, and enquiry system are created in the intended order.

## Skills Demonstrated

This project demonstrates practical skills in:

- Data loading and inspection
- Pandas filtering and grouping
- Aggregation and transformation
- Datetime handling
- Data validation and quality checks
- Duplicate detection
- Route classification
- Station-level frequency analysis
- Pivot tables and cross-tabulation
- Data visualization
- Exploratory data analysis
- Python function development
- Building a basic interactive data-enquiry system

## Conclusion

The project provides an end-to-end example of railway schedule analysis, starting with raw data inspection and progressing through data validation, exploratory analysis, visualization, advanced station/route analysis, and a functional train-enquiry system. It demonstrates how Python and Pandas can transform structured railway schedule data into useful route, station, and service-frequency insights.
