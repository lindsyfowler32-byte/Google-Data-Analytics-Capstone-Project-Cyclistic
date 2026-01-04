# Cyclistic Bike-Share Case Study (Google Data Analytics Capstone)

This project analyzes Divvy bike-share data from Q1 2019 and Q1 2020 to understand how annual members and casual riders use Cyclistic bikes differently. Using R, I cleaned, standardized, and analyzed real-world data to identify trends in ride frequency and duration, helping inform marketing strategies to convert casual riders into annual members.

google-data-analytics-capstone-project-cyclistic/
│
├── README.md                # Project documentation
├── data/
│   ├── raw/                 # Original datasets
│   │   ├── Divvy_Trips_2019_Q1.csv
│   │   └── Divvy_Trips_2020_Q1.csv
│   └── cleaned/
│       └── cleaned_divvy_q1_2019_2020.csv
├── scripts/
│   ├── 01_prepare_process_cleaning.R
│   ├── 02_analysis.R
│   └── 03_visualizations.R
├── visuals/
│   ├── rides_by_weekday.png
│   └── avg_ride_length.png
└── .gitignore


---

## Business Task

The main objective is to compare the behavior of **annual members** and **casual riders** to answer:

- How do ride patterns differ by day of the week?
- Do casual riders take longer rides than members?
- How can these insights inform marketing and membership strategies?

---

## Case Study Roadmap – Prepare

**Guiding Questions:**
- Where is the data located?  
- How is it organized?  
- Are there issues with bias or credibility? Does it meet ROCCC criteria?  
- How are licensing, privacy, security, and accessibility addressed?  
- How did you verify data integrity?  
- How does it help answer your question?  
- Are there any problems with the data?

**Key Tasks:**
- Download Q1 2019 and Q1 2020 datasets and store in `data/raw/`
- Inspect and document the structure of each dataset
- Identify column differences between datasets
- Verify credibility and consistency (Divvy/Cyclistic, public, historical data)
- Check for missing or invalid values

**Deliverable:**  
Description of all data sources and their structure.

---

## Case Study Roadmap – Process

**Guiding Questions:**
- What tools are used and why?  
- How is data integrity ensured?  
- What steps were taken to clean the data?  
- How do you verify the data is clean and ready to analyze?  
- Has the cleaning process been documented?

**Key Tasks:**
- Inspect raw data for errors
- Standardize column names and formats
- Transform data for consistency
- Document all cleaning steps in `scripts/01_prepare_process_cleaning.R`

**Deliverable:**  
Documentation of data cleaning and manipulation.

---

## Case Study Roadmap – Analyze

**Guiding Questions:**
- How should data be organized for analysis?  
- Has it been properly formatted?  
- What trends or surprises are in the data?  
- How do insights answer the business question?

**Key Tasks:**
- Aggregate rides by `member_casual` and `weekday`
- Summarize ride counts and average ride lengths
- Identify trends and patterns
- Organize data for visualization

**Deliverable:**  
Summary of trends and differences between rider types.

---

## Case Study Roadmap – Share

**Guiding Questions:**
- Were you able to answer the question of rider behavior differences?  
- What story does the data tell?  
- Who is the audience and how is the story communicated?  
- How do visualizations support the findings?  

**Key Tasks:**
- Create charts comparing rides by weekday and rider type
- Highlight differences in ride length and frequency
- Present insights clearly for stakeholders
- Ensure accessibility of visualizations

**Deliverable:**  
Supporting visualizations and key findings.

---

## Code Examples

Below are key steps from my R analysis. Full scripts are available in the `scripts/` folder.

### 1. Import and Clean Data

```r
library(tidyverse)
library(lubridate)
library(janitor)

# Import Q1 2019 and Q1 2020 datasets
trips_2019 <- read_csv("data/raw/Divvy_Trips_2019_Q1.csv")
trips_2020 <- read_csv("data/raw/Divvy_Trips_2020_Q1.csv")

# Standardize column names
trips_2019 <- clean_names(trips_2019)
trips_2020 <- clean_names(trips_2020)

# Combine datasets
all_trips <- bind_rows(trips_2019, trips_2020)

all_trips <- all_trips %>%
  mutate(
    ride_length = as.numeric(difftime(ended_at, started_at, units = "mins")),
    weekday = wday(started_at, label = TRUE)
  ) %>%
  filter(ride_length > 0)

ride_summary <- all_trips %>%
  group_by(member_casual, weekday) %>%
  summarise(
    number_of_rides = n(),
    average_ride_length = mean(ride_length)
  )

library(ggplot2)

# Rides by weekday
ggplot(ride_summary, aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge") +
  labs(title = "Number of Rides by Weekday and Rider Type",
       x = "Weekday", y = "Number of Rides", fill = "Rider Type")

# Average ride length by weekday
ggplot(ride_summary, aes(x = weekday, y = average_ride_length, color = member_casual, group = member_casual)) +
  geom_line(size = 1.2) +
  geom_point(size = 2) +
  labs(title = "Average Ride Length by Weekday",
       x = "Weekday", y = "Average Ride Length (minutes)", color = "Rider Type")

scripts/01_prepare_process_cleaning.R
scripts/02_analysis.R
scripts/03_visualizations.R

Skills Demonstrated:
R · Data Cleaning · Data Analysis · Data Visualization · Tidyverse · Lubridate · Business Insights
