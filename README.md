# Career Track Analysis with SQL and Tableau

## Project Overview

This project involves analyzing student enrollments and completions in data-related career tracks offered by 365 Data Science. The analysis is performed using SQL for data extraction and Tableau for visualization. The goal is to gain insights into student behavior, track popularity, and completion rates to help 365 Data Science improve their educational offerings.

---

## Project Structure

- **SQL Data Extraction**: Extract necessary data from the provided SQL database.
- **Tableau Visualizations**: Create insightful visualizations to interpret the data.
- **Analysis and Recommendations**: Analyze the results and provide recommendations for improvement.

---

## 🗃️ Files in the Repository

| File | Description |
|------|-------------|
| `career_track_completions.csv` | Final cleaned data for Tableau |
| `sql_and_tableau.sql` | SQL logic used to generate the dataset |
| `mysql.sql` | Database schema creation |
| `.twb / .twbx` | Tableau workbook (Dashboard) |
| `README.md` | Project documentation |

---


## SQL Data Extraction

### Database Tables

- **career_track_info**
  - `track_id`: The unique identification of a track.
  - `track_name`: The name of the track.

- **career_track_student_enrollments**
  - `student_id`: The unique identification of a student.
  - `track_id`: The unique identification of a track.
  - `date_enrolled`: The date the student enrolled in the track.
  - `date_completed`: The date the student completed the track (NULL if not completed).
 
  ---

### SQL Query

The following SQL query extracts the required data and prepares it for visualization in Tableau:

```sql
SELECT 
    ROW_NUMBER() OVER (ORDER BY e.student_id, i.track_name DESC) AS student_track_id,
    e.student_id,
    i.track_name,
    e.date_enrolled,
    IF(e.date_completed IS NULL, 0, 1) AS track_completed,
    DATEDIFF(e.date_completed, e.date_enrolled) AS days_for_completion
FROM
    career_track_student_enrollments e
    JOIN
    career_track_info i ON e.track_id = i.track_id;

SELECT 
    a.*,
    CASE
        WHEN days_for_completion = 0 THEN 'Same day'
        WHEN days_for_completion BETWEEN 1 AND 7 THEN '1 to 7 days'
        WHEN days_for_completion BETWEEN 8 AND 30 THEN '8 to 30 days'
        WHEN days_for_completion BETWEEN 31 AND 60 THEN '31 to 60 days'
        WHEN days_for_completion BETWEEN 61 AND 90 THEN '61 to 90 days'
        WHEN days_for_completion BETWEEN 91 AND 365 THEN '91 to 365 days'
        WHEN days_for_completion > 365 THEN '366+ days'
        ELSE NULL
    END AS completion_bucket
FROM
(
    SELECT 
        ROW_NUMBER() OVER (ORDER BY e.student_id, i.track_name DESC) AS student_track_id,
        e.student_id,
        i.track_name,
        e.date_enrolled,
        IF(e.date_completed IS NULL, 0, 1) AS track_completed,
        DATEDIFF(e.date_completed, e.date_enrolled) AS days_for_completion
    FROM
        career_track_student_enrollments e
        JOIN
        career_track_info i ON e.track_id = i.track_id
) a;
```

### Export to CSV

The result of the above query is exported as `career_track_completions.csv`.

---


### Combo Chart

- **Bar Chart**: Represents the number of track enrollments per month.
- **Line Chart**: Displays the fraction of track completions per month as a percentage of enrollments.

### Steps to Create Combo Chart in Tableau

1. **Connect to Data Source**: Import `career_track_completions.csv` into Tableau.
2. **Create Bar Chart**: Drag `Date Enrolled` to Columns (set to MONTH), drag `student_track_id` to Rows (set to COUNT).
3. **Create Line Chart**: Drag `track_completed` to Rows next to `student_track_id` and change to Line Chart.
4. **Dual Axis**: Right-click the axis and select Dual Axis.
5. **Format Line Chart Axis**: Set to Percentage.
6. **Filter by Career Track**: Drag `track_name` to Filters and show filter.

### Completion Bucket Bar Chart

- Each bar represents a different completion bucket with their height corresponding to the number of track completions.

---


## Dashboard Preview

Below is a preview of the dashboard created for the Bike Store project:

![Dashboard Preview](https://github.com/Strik3r10/Career-Analysis-SQL-Tableau-/blob/main/Dashboard.png)

![Dashboard Preview](https://github.com/user-attachments/assets/bd9d16d6-ddd5-4f11-9d6d-2eb6a4c2b531)

![Dashboard Preview](https://github.com/user-attachments/assets/3eeac4eb-0406-44aa-87ab-e6c00361de51)


---


### ❓ Key Inisghts

**Q1: Which month has the highest learner enrollment?**  
📌 **August**, across all tracks.

**Q2: How many learners typically finish the program each month?**  
📌 Only **~1% of enrolled learners** complete their program every month.

**Q3: How long does it usually take students to complete a track?**  
📌 Most learners require **between 91–365 days** to finish a career program.

**Q4: What strategy can improve program completion rates?**  
📌 Targeting long-duration learners can drive a **~30% boost in completions**.

**Q5: Which career track is the most popular among learners?**  
📌 **Data Analyst** is the most popular, followed by **Data Scientist**.

---


### 🧩 Strategic Actions (Based on Insights)

➡️ **Is completion rate low among long-duration learners (91–365 days)?**
✔ Launch mentorship + scheduled reminders  
🎯 Expected Impact: **~30% completion boost**

➡️ **Do learners lack motivation to finish?**
✔ Add tiered certifications (badges → exams → interview prep)  
🎯 Builds urgency + skill credibility

➡️ **Is engagement dropping mid-track?**
✔ Set weekly goals + app/email nudges  
🎯 Consistent habit formation → higher completion

➡️ **Which track should be prioritized for marketing?**
✔ Focus promo on **Data Analyst Track** (highest enrollment)  
🎯 Maximum ROI on acquisition campaigns

➡️ **Why are some learners stuck for months?**
✔ Implement AI learning paths + weak-topic suggestions  
🎯 Faster progression, fewer drop-offs


---


### 📈 Business Impact

If implemented, these recommendations could:
✔ Improve overall completion rates by **25–30%**  
✔ Increase learner retention in long-duration tracks  
✔ Strengthen brand value through higher certification numbers  
✔ Boost revenue via demand-driven program promotion (Data Analyst)### 📈 Business Impact


---


## Conclusion

This analysis provides clear visibility into learner habits and bottlenecks.
By focusing on slow-track learners and optimizing content delivery time, course providers can improve completion rates and engagement efficiently.

---


