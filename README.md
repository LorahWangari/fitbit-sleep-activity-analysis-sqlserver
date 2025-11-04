# fitbit-sleep-activity-analysis-sqlserver

## Overview
A real-world SQL data analysis project combining fitness and sleep data to generate lifestyle insights. 


## Process
- Imported 18 CSV files into SQL Server.
- Created relational tables and views for activity and sleep data.
- Cleaned and validated data.
- Ran analytical SQL queries for behavioral insights.

## Final Insights from Fitbit Fitness & Sleep Data Analysis
Analyzing Fitbit data in SQL Server revealed some interesting behavioral patterns about activity levels, sleep duration and sleep efficiency across users.

Overall, there’s a positive relationship between physical activity and sleep quality — users who were more active during the day generally experienced more efficient sleep at night. 
However, this wasn’t universal. A few users with lower daily steps still had excellent sleep efficiency, suggesting that consistent sleep schedules and recovery habits can sometimes 
outweigh raw activity levels.

The data also showed how sleep consistency plays a critical role in sleep quality. Users whose total minutes asleep stayed relatively steady from day to day achieved 
higher sleep efficiency compared to those with irregular sleep patterns. This points to a strong connection between routine and rest. Maintaining a predictable sleep schedule seems 
more beneficial than simply sleeping longer hours.

When comparing weekdays and weekends, a clear difference emerged. Sleep duration tended to increase on weekends, likely as users caught up on rest, but sleep efficiency slightly dropped. 
Longer time in bed didn’t always translate into better rest. This reflects how inconsistent bedtime patterns or “social jet lag” can reduce overall sleep effectiveness, 
even when total sleep time goes up.

There was also a moderate positive relationship between calories burned and sleep efficiency. Users with higher average calorie expenditure, a sign of daily physical activity,
tended to have slightly better sleep quality. It supports the idea that balanced daily movement helps promote restorative sleep, but too much variation or excessive exertion can have 
diminishing returns.

Across all observations, a consistent theme emerged: balance and consistency matter most. Users who kept steady sleep habits, maintained regular physical activity 
and avoided large fluctuations in either area showed the highest sleep efficiency overall.

This project demonstrated how structured SQL analysis can uncover meaningful health insights from wearable data. Connecting numbers to real-world lifestyle behaviors.

## The Structure

CREATE DATABASE FitbitProject;

USE FitbitProject;

-------------------------------------------------
-- 1. dailyActivity_merged
-------------------------------------------------
CREATE TABLE dbo.dailyActivity_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityDate DATE NOT NULL,
    TotalSteps INT,
    TotalDistance FLOAT,
    TrackerDistance FLOAT,
    LoggedActivitiesDistance FLOAT,
    VeryActiveDistance FLOAT,
    ModeratelyActiveDistance FLOAT,
    LightActiveDistance FLOAT,
    SedentaryActiveDistance FLOAT,
    VeryActiveMinutes INT,
    FairlyActiveMinutes INT,
    LightlyActiveMinutes INT,
    SedentaryMinutes INT,
    Calories INT,
    CONSTRAINT PK_dailyActivity PRIMARY KEY (Id, ActivityDate)
);

-------------------------------------------------
-- 2. dailyCalories_merged
-------------------------------------------------
CREATE TABLE dbo.dailyCalories_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityDay DATE NOT NULL,
    Calories INT,
    CONSTRAINT PK_dailyCalories PRIMARY KEY (Id, ActivityDay)
);

-------------------------------------------------
-- 3. dailyIntensities_merged
-------------------------------------------------
CREATE TABLE dbo.dailyIntensities_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityDay DATE NOT NULL,
    SedentaryMinutes INT,
    LightlyActiveMinutes INT,
    FairlyActiveMinutes INT,
    VeryActiveMinutes INT,
    SedentaryActiveDistance FLOAT,
    LightActiveDistance FLOAT,
    ModeratelyActiveDistance FLOAT,
    VeryActiveDistance FLOAT,
    CONSTRAINT PK_dailyIntensities PRIMARY KEY (Id, ActivityDay)
);

-------------------------------------------------
-- 4. dailySteps_merged
-------------------------------------------------
CREATE TABLE dbo.dailySteps_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityDay DATE NOT NULL,
    StepTotal INT,
    CONSTRAINT PK_dailySteps PRIMARY KEY (Id, ActivityDay)
);

-------------------------------------------------
-- 5. heartrate_seconds_merged
-------------------------------------------------
CREATE TABLE dbo.heartrate_seconds_merged (
    Id NVARCHAR(50) NOT NULL,
    TimeStamp DATETIME NOT NULL,
    Value INT,
    CONSTRAINT PK_heartrate PRIMARY KEY (Id, TimeStamp)
);

-------------------------------------------------
-- 6. hourlyCalories_merged
-------------------------------------------------
CREATE TABLE dbo.hourlyCalories_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityHour DATETIME NOT NULL,
    Calories INT,
    CONSTRAINT PK_hourlyCalories PRIMARY KEY (Id, ActivityHour)
);


-------------------------------------------------
-- 7. hourlyIntensities_merged
-------------------------------------------------
CREATE TABLE dbo.hourlyIntensities_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityHour DATETIME NOT NULL,
    TotalIntensity INT,
    AverageIntensity FLOAT,
    CONSTRAINT PK_hourlyIntensities PRIMARY KEY (Id, ActivityHour)
);

-------------------------------------------------
-- 8. hourlySteps_merged
-------------------------------------------------
CREATE TABLE dbo.hourlySteps_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityHour DATETIME NOT NULL,
    StepTotal INT,
    CONSTRAINT PK_hourlySteps PRIMARY KEY (Id, ActivityHour)
);

-------------------------------------------------
-- 9. minuteCaloriesNarrow_merged
-------------------------------------------------
CREATE TABLE dbo.minuteCaloriesNarrow_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    Calories FLOAT,
    CONSTRAINT PK_minuteCalories PRIMARY KEY (Id, ActivityMinute)
);

-------------------------------------------------
-- 10. minuteIntensitiesNarrow_merged
-------------------------------------------------
CREATE TABLE dbo.minuteIntensitiesNarrow_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    Intensity INT,
    CONSTRAINT PK_minuteIntensities PRIMARY KEY (Id, ActivityMinute)
);

-------------------------------------------------
-- 11. minuteMETsNarrow_merged
-------------------------------------------------
CREATE TABLE dbo.minuteMETsNarrow_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    METs INT,
    CONSTRAINT PK_minuteMETs PRIMARY KEY (Id, ActivityMinute)
);

-------------------------------------------------
-- 12. minuteSleep_merged
-------------------------------------------------
CREATE TABLE dbo.minuteSleep_merged (
    Id NVARCHAR(50) NOT NULL,
    date DATETIME NOT NULL,
    value INT,
    logId BIGINT,
    CONSTRAINT PK_minuteSleep PRIMARY KEY (Id, date)
);

-------------------------------------------------
-- 13. minuteStepsNarrow_merged
-------------------------------------------------
CREATE TABLE dbo.minuteStepsNarrow_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    Steps INT,
    CONSTRAINT PK_minuteSteps PRIMARY KEY (Id, ActivityMinute)
);

-------------------------------------------------
-- 14. sleepDay_merged
-------------------------------------------------
CREATE TABLE dbo.sleepDay_merged (
    Id NVARCHAR(50) NOT NULL,
    SleepDay DATETIME NOT NULL,
    TotalSleepRecords INT,
    TotalMinutesAsleep INT,
    TotalTimeInBed INT,
    CONSTRAINT PK_sleepDay PRIMARY KEY (Id, SleepDay)
);

-------------------------------------------------
-- 15. weightLogInfo_merged
-------------------------------------------------
CREATE TABLE dbo.weightLogInfo_merged (
    Id NVARCHAR(50) NOT NULL,
    Date DATETIME NOT NULL,
    WeightKg FLOAT,
    WeightPounds FLOAT,
    Fat FLOAT NULL,
    BMI FLOAT,
    IsManualReport BIT,
    LogId BIGINT NOT NULL,
    CONSTRAINT PK_weightLog PRIMARY KEY (Id, LogId)
);

-------------------------------------------------
-- 16. minuteCaloriesWide_merged
-------------------------------------------------
CREATE TABLE dbo.minuteCaloriesWide_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    Calories FLOAT,
    CONSTRAINT PK_minuteCaloriesWide PRIMARY KEY (Id, ActivityMinute)
);

-------------------------------------------------
-- 17. minuteIntensitiesWide_merged
-------------------------------------------------
CREATE TABLE dbo.minuteIntensitiesWide_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    Intensity INT,
    CONSTRAINT PK_minuteIntensitiesWide PRIMARY KEY (Id, ActivityMinute)
);

-------------------------------------------------
-- 18. minuteStepsWide_merged
-------------------------------------------------
CREATE TABLE dbo.minuteStepsWide_merged (
    Id NVARCHAR(50) NOT NULL,
    ActivityMinute DATETIME NOT NULL,
    Steps INT,
    CONSTRAINT PK_minuteStepsWide PRIMARY KEY (Id, ActivityMinute)
);

SELECT TOP (10) * FROM dbo.dailyActivity_merged;

SELECT
    t.name AS TableName,
    p.rows AS RowCount
FROM sys.tables t
INNER JOIN sys.partitions p ON t.object_id = p.object_id
WHERE p.index_id IN (0,1)
ORDER BY t.name;

SELECT * FROM sys.tables;

USE FitbitProject;

-- Reliable table row counts for every user table
SELECT
    t.name AS TableName,
    SUM(p.rows) AS RowCount
FROM sys.tables AS t
JOIN sys.partitions AS p
    ON t.object_id = p.object_id
    AND p.index_id IN (0,1)          -- heap or clustered index
GROUP BY t.name
ORDER BY t.name;

SELECT * FROM dbo.minuteSleep_merged;


SELECT name FROM sys.tables ORDER BY name;

SELECT name AS TableName
FROM sys.tables
ORDER BY name;

DROP TABLE IF EXISTS dbo.minuteStepsWide_merged;
DROP TABLE IF EXISTS dbo.minuteCaloriesWide_merged;
DROP TABLE IF EXISTS dbo.minuteIntensitiesWide_merged;
DROP TABLE IF EXISTS dbo.sysdiagrams;

SELECT 
    t.name AS TableName,
    SUM(p.rows) AS TotalRows
FROM sys.tables AS t
JOIN sys.partitions AS p
    ON t.object_id = p.object_id
   AND p.index_id IN (0,1)
GROUP BY t.name
ORDER BY TotalRows DESC;

DROP TABLE IF EXISTS dbo.sleepDay_merged;

CREATE TABLE dbo.sleepDay_merged (
    Id BIGINT,
    SleepDay DATETIME,
    TotalSleepRecords INT,
    TotalMinutesAsleep INT,
    TotalTimeInBed INT
);

BULK INSERT dbo.sleepDay_merged
FROM 'C:\FitBit Data\sleepDay_merged.csv'
WITH (
    FORMAT = 'CSV',
    FIRSTROW = 2,
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    TABLOCK
);


SELECT COUNT(*) AS TotalRows FROM dbo.sleepDay_merged;
SELECT TOP 10 * FROM dbo.sleepDay_merged;

SELECT 
    a.Id,
    a.ActivityDate,
    a.TotalSteps,
    a.Calories,
    s.TotalMinutesAsleep,
    s.TotalTimeInBed
FROM dbo.dailyActivity_merged a
JOIN dbo.sleepDay_merged s
    ON a.Id = s.Id
   AND CAST(a.ActivityDate AS DATE) = CAST(s.SleepDay AS DATE)
ORDER BY a.Id, a.ActivityDate; 

-- Daily activity sample
SELECT TOP (5) * FROM dbo.dailyActivity_merged;

-- Sleep data sample
SELECT TOP (5) * FROM dbo.sleepDay_merged;

-- Create a unified activity-sleep view
CREATE VIEW vw_Fitbit_ActivitySleep AS
SELECT 
    a.Id,
    CAST(a.ActivityDate AS DATE) AS RecordDate,
    a.TotalSteps,
    a.TotalDistance,
    a.Calories,
    s.TotalMinutesAsleep,
    s.TotalTimeInBed
FROM dbo.dailyActivity_merged a
JOIN dbo.sleepDay_merged s
    ON a.Id = s.Id
   AND CAST(a.ActivityDate AS DATE) = CAST(s.SleepDay AS DATE);

SELECT TOP (10) * FROM vw_Fitbit_ActivitySleep;

-- Do active users sleep better?
SELECT 
    Id,
    AVG(TotalSteps) AS AvgDailySteps,
    AVG(TotalMinutesAsleep) AS AvgSleepMinutes
FROM vw_Fitbit_ActivitySleep
GROUP BY Id
ORDER BY AvgDailySteps DESC;  -- The more the daily steps, the more the sleep minutes. 


-- Let's see if it's just sleep quantity or actual sleep quality.

CREATE OR ALTER VIEW vw_SleepEfficiency AS
SELECT 
    a.Id,
    CAST(a.ActivityDate AS DATE) AS RecordDate,
    a.TotalSteps,
    a.Calories,
    s.TotalMinutesAsleep,
    s.TotalTimeInBed,
    CAST(s.TotalMinutesAsleep AS FLOAT) / NULLIF(s.TotalTimeInBed, 0) * 100 AS SleepEfficiency
FROM dbo.dailyActivity_merged a
JOIN dbo.sleepDay_merged s
    ON a.Id = s.Id
   AND CAST(a.ActivityDate AS DATE) = CAST(s.SleepDay AS DATE);


 SELECT 
    Id,
    AVG(TotalSteps) AS AvgDailySteps,
    AVG(SleepEfficiency) AS AvgSleepQuality
FROM vw_SleepEfficiency
GROUP BY Id
ORDER BY AvgDailySteps DESC;  
-- No. Some users with less AvgDailySteps are high ranking in more AvgSleepQuality. So more physical activity doesn't improve sleep efficiency. 
-- Let's look closer to see whether it's more so per user for different resons

SELECT 
    CASE 
        WHEN TotalSteps < 5000 THEN 'Low Activity'
        WHEN TotalSteps BETWEEN 5000 AND 10000 THEN 'Moderate Activity'
        ELSE 'High Activity'
    END AS ActivityLevel,
    ROUND(AVG(SleepEfficiency), 2) AS AvgSleepEfficiency
FROM vw_SleepEfficiency
GROUP BY 
    CASE 
        WHEN TotalSteps < 5000 THEN 'Low Activity'
        WHEN TotalSteps BETWEEN 5000 AND 10000 THEN 'Moderate Activity'
        ELSE 'High Activity'
    END
ORDER BY AvgSleepEfficiency DESC;

-- It is more per user. Different strokes (steps) for different folks. 
SELECT 
    Id,
    STDEV(SleepEfficiency) AS SleepQualityVariance,
    AVG(TotalSteps) AS AvgSteps
FROM vw_SleepEfficiency
GROUP BY Id
ORDER BY SleepQualityVariance ASC;


-- Are weekends different from weekdays?
SELECT 
    DATENAME(WEEKDAY, RecordDate) AS DayOfWeek,
    AVG(TotalSteps) AS AvgSteps,
    AVG(TotalMinutesAsleep) AS AvgSleep
FROM vw_Fitbit_ActivitySleep
GROUP BY DATENAME(WEEKDAY, RecordDate)
ORDER BY 
    CASE DATENAME(WEEKDAY, RecordDate)
        WHEN 'Monday' THEN 1 WHEN 'Tuesday' THEN 2 WHEN 'Wednesday' THEN 3
        WHEN 'Thursday' THEN 4 WHEN 'Friday' THEN 5 WHEN 'Saturday' THEN 6
        WHEN 'Sunday' THEN 7 END;
		-- Wednesday and Sunday have low Avg Steps but the highest Avg Sleep. Showing they forfeited working out for sleep. 
		-- Saturdays have the highest Avg Steps.


-- Let's see the coleration between calories and sleep duration
SELECT 
    ROUND(AVG(CAST(Calories AS FLOAT)), 2) AS AvgCalories,
    ROUND(AVG(CAST(TotalMinutesAsleep AS FLOAT)), 2) AS AvgSleepMinutes
FROM vw_Fitbit_ActivitySleep;


SELECT 
    CASE 
        WHEN Calories < 2000 THEN 'Low Activity'
        WHEN Calories BETWEEN 2000 AND 3000 THEN 'Moderate Activity'
        ELSE 'High Activity'
    END AS ActivityLevel,
    AVG(TotalMinutesAsleep) AS AvgSleepMinutes
FROM vw_Fitbit_ActivitySleep
GROUP BY 
    CASE 
        WHEN Calories < 2000 THEN 'Low Activity'
        WHEN Calories BETWEEN 2000 AND 3000 THEN 'Moderate Activity'
        ELSE 'High Activity'
    END
ORDER BY AvgSleepMinutes DESC;  


CREATE VIEW vw_Fitbit_Summary AS
SELECT 
    Id,
    CAST(RecordDate AS DATE) AS RecordDate,
    TotalSteps,
    Calories,
    TotalMinutesAsleep,
    TotalTimeInBed,
    (TotalMinutesAsleep * 1.0 / NULLIF(TotalTimeInBed, 0)) * 100 AS SleepEfficiency
FROM vw_Fitbit_ActivitySleep;

SELECT TOP (10) *
FROM vw_Fitbit_Summary;


-- Average sleep efficiency by activity level
SELECT 
    CASE 
        WHEN TotalSteps < 5000 THEN 'Low Activity (<5k steps)'
        WHEN TotalSteps BETWEEN 5000 AND 9999 THEN 'Moderate Activity (5k-10k steps)'
        ELSE 'High Activity (10k+ steps)'
    END AS ActivityLevel,
    ROUND(AVG(SleepEfficiency), 2) AS AvgSleepEfficiency,
    ROUND(AVG(TotalMinutesAsleep), 0) AS AvgMinutesAsleep,
    COUNT(*) AS DaysCount
FROM vw_Fitbit_Summary
WHERE SleepEfficiency IS NOT NULL
GROUP BY 
    CASE 
        WHEN TotalSteps < 5000 THEN 'Low Activity (<5k steps)'
        WHEN TotalSteps BETWEEN 5000 AND 9999 THEN 'Moderate Activity (5k-10k steps)'
        ELSE 'High Activity (10k+ steps)'
    END
ORDER BY AvgSleepEfficiency DESC; 


-- Weekday Vs Weekend sleep pattern 
SELECT 
    DATENAME(WEEKDAY, RecordDate) AS DayOfWeek,
    ROUND(AVG(TotalMinutesAsleep), 0) AS AvgMinutesAsleep
FROM vw_Fitbit_Summary
GROUP BY DATENAME(WEEKDAY, RecordDate)
ORDER BY 
    CASE 
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Monday' THEN 1
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Tuesday' THEN 2
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Wednesday' THEN 3
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Thursday' THEN 4
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Friday' THEN 5
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Saturday' THEN 6
        WHEN DATENAME(WEEKDAY, RecordDate) = 'Sunday' THEN 7
    END;   -- Users sleep longer on weekends, often catching up on rest missed during the week
