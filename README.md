# Enhancing-Brand-Recognition-Through-Effective-Social-Media-Monitoring
Analysis of how 1 company leverages it's social media marketing tools to manage and enhance brand recognition. 

# Brand Reputation Management: Data Analytics Project  

## Table of Contents
- [Introduction](#introduction)  
- [Business Problem](#business-problem)  
- [Data Sources](#data-sources)  
- [Key Metrics and Visualizations](#key-metrics-and-visualizations)       
- [Insights & Recommendations](#insights--recommendations)  
- [Conclusion](#conclusion)  

---

## Introduction
AfriTech Electronics Ltd. has faced growing **brand reputation challenges** caused by negative customer reviews, product recalls, and competitor pressure. This project leverages **PostgreSQL** and **social media analytics** to uncover insights that can guide AfriTech in improving its brand perception, customer trust, and market competitiveness.

---

## Business Problem
AfriTech’s challenges include:  
- Rising **negative social media buzz** damaging brand image.  
- Escalating **customer complaints** about defects and support delays.  
- Frequent **product recalls** leading to public relations crises.  
- **Competitive pressure** from rivals capitalizing on weaknesses.  

The goal is to use **data-driven analysis** to restore reputation and enhance customer trust.

---

## Data Sources
Data was ingested into **PostgreSQL** from transaction logs, customer records, and social media monitoring. The schema included:  
- **CustomerData** (demographics, segmentation)  
- **Transactions** (sales, product recalls)  
- **SocialMedia** (brand mentions, sentiment, engagement, influencer metrics, crisis events)  
---
## Key Metrics and Visualizations  

### Average Crisis Response Time  

- **Result:** 9.34 hours on average to respond to crises.  
- **Implication:** The company is slower than the ideal **< 6 hours** benchmark, risking escalation.  

```sql
SELECT AVG(DATE_PART('epoch', (CAST(FirstResponseTime AS TIMESTAMP) 
         - CAST(CrisisEventTime AS TIMESTAMP))) / 3600) AS AverageResponseTimeHours
FROM SocialMedia
WHERE CrisisEventTime IS NOT NULL AND FirstResponseTime IS NOT NULL;
```
---

### Average Influence Score  
<img width="1000" height="500" alt="Average_Influence_Score" src="https://github.com/user-attachments/assets/1a71fc41-3e28-4ac2-8d4d-57485ba5ffc4" />
  
- **Result:** 3,086 average followers per brand-related post.  
- **Implication:** AfriTech has **strong reach potential**, but must focus on converting reach into positive advocacy.  

```sql
SELECT AVG(InfluencerScore) AS AverageInfluenceScore
FROM SocialMedia;
```
---

### Brand vs Competitor Mentions  
<img width="1979" height="1180" alt="Brand_vs_Competitor_Mentions" src="https://github.com/user-attachments/assets/4cb55bd4-4900-4698-83b6-40ce4b732371" />
 
- **Result:** AfriTech leads in mentions (2,487), followed by Competitor A (2,163).  
- **Implication:** High visibility but **brand narrative control is at risk** — mentions can skew negative.  

```sql
SELECT
  SUM(CASE WHEN BrandMention = TRUE THEN 1 ELSE 0 END) AS BrandMentions,
  SUM(CASE WHEN CompetitorMention = TRUE THEN 1 ELSE 0 END) AS CompetitorMentions
FROM SocialMedia;
```
---

### Content Effectiveness  
<img width="1979" height="1180" alt="Content_Effectiveness_by_PostType" src="https://github.com/user-attachments/assets/31087991-28cd-4cb7-aee5-df187786860a" />
  
- **Result:** Text posts had the highest engagement (74 avg likes+shares), followed by video and image.  
- **Implication:** **Content strategy should prioritize text posts** (e.g., product announcements, direct customer updates).  

```sql
SELECT PostType, AVG(EngagementLikes + EngagementShares + EngagementComments) AS Engagement
FROM SocialMedia
GROUP BY PostType;
```
---

### Engagement Rate  
<img width="1000" height="500" alt="Engagement_Rate" src="https://github.com/user-attachments/assets/61136a79-b538-463a-9ecf-877e4e667733" />
 
- **Result:** 3.78% average engagement rate per post.  
- **Implication:** Slightly above the **industry benchmark (~3%)**, suggesting active but not maximized community engagement.  

```sql
SELECT AVG((EngagementLikes + EngagementShares + EngagementComments) / 
            NULLIF(UserFollowers, 0)) AS EngagementRate
FROM SocialMedia;
```
---

### Resolution Rate  
<img width="1000" height="500" alt="Resolution_Rate" src="https://github.com/user-attachments/assets/a2e35e30-a3e2-4aed-8a9b-f19ec0b613a3" />
  
- **Result:** 95.24% of crises resolved.  
- **Implication:** AfriTech’s crisis **completion rate is strong**, but improvements in **response speed** are required.  

```sql
SELECT COUNT(*) * 100.0 / (SELECT COUNT(*) 
                           FROM SocialMedia 
                           WHERE CrisisEventTime IS NOT NULL) AS ResolutionRate
FROM SocialMedia
WHERE ResolutionStatus = TRUE;
```
---

### Sentiment Analysis  
<img width="1979" height="1180" alt="Sentiment_Distribution" src="https://github.com/user-attachments/assets/d654ba16-d4e1-467b-9436-b1bf737f5b5a" />
  
<img width="1979" height="1180" alt="Sentiment_Percentage" src="https://github.com/user-attachments/assets/09a17e7e-a1b9-44a3-a7ca-f9455cd4064a" />
  
- **Result:**  
  - 55.6% Positive  
  - 24.6% Neutral  
  - 19.9% Negative  
- **Implication:** While **positive sentiment dominates**, nearly **1 in 5 mentions are negative**, posing a brand risk.  

```sql
SELECT Sentiment, COUNT(*) * 100.0 / (SELECT COUNT(*) FROM SocialMedia) AS Percentage
FROM SocialMedia
GROUP BY Sentiment;
```
## SQL Code Samples  
The project relied heavily on **PostgreSQL queries** for analysis, data transformation, and visualization preparation.   
---

## Insights & Recommendations  

1. **Reduce Crisis Response Time** – Implement AI-powered chatbots and crisis alert dashboards to reduce from **9.34h → <6h**.  
2. **Amplify Influencers** – Partner with high-influence advocates to boost positive messaging.  
3. **Content Strategy Shift** – Prioritize **text posts** with strong calls-to-action while keeping video for high-value launches.  
4. **Sentiment Intervention** – Deploy **predictive sentiment alerts** to prevent negative buzz spikes.  
5. **Competitor Benchmarking** – Use comparative mentions to identify where AfriTech loses ground and craft targeted campaigns.  

---

## Conclusion  
AfriTech Electronics has strong visibility and influence but faces risks in **crisis speed** and **negative buzz control**. By improving **real-time monitoring**, **content strategy**, and **crisis management systems**, the company can rebuild customer trust and transform reputation into a competitive advantage.  
