# E-Commerce Growth Intelligence in Power BI
**Revenue, Conversion & Product Economics**

## Overview

This Power BI project analyzes the website-to-purchase journey of an online toy retailer, covering traffic acquisition, conversion, revenue growth, product economics, basket behaviour, and refunds.

The project combines data validation, semantic modeling, KPI design, exploratory analysis, and interactive reporting to identify the main drivers of business performance and translate them into actionable recommendations.

## Dashboard Preview

**Report Pages:** Executive Summary, Revenue & Growth, Conversion & Visitor Behaviour, Product & Basket Performance
- Executive Summary
![](Images/Screenshots/exec_summary.png?raw=true)
<!--
- Revenue & Growth
![](Images/Screenshots/growth_acquisition.png)
- Conversion & Visitor Behaviour
![](Images/Screenshots/conversion_visitor.png)
- Product & Basket Performance
-->
## Business Problem

An online toy retailer is selling products directly through its website. Revenue is generated through online product sales. The dataset captures the customer journey from website visit through purchase and eventually to refund.

Key business questions include:

- What drove revenue growth: traffic, conversion, or changes in order value?
- Which acquisition sources and devices generate the strongest traffic quality and monetization?
- How do new and repeat sessions, devices, and landing pages differ in conversion performance?
- Which products drive revenue and gross profit, and how has the product mix evolved?
- How do bundle purchases and cross-sell behaviour affect basket value?
- Where is refund risk concentrated, and are major refund spikes structural or product-specific anomalies?

## Data & Analytical Scope

The model combines multiple business processes at different analytical grains, including:
- 472K+ website sessions
- 1.18M+ pageviews
- 32K+ orders
- 40K+ order items
- 1.7K+ refund records

The source data was profiled for missing values, duplicates, referential integrity, business-rule consistency and data types before semantic modeling. The dataset does not include marketing spend, geography, customer demographics, inventory, or shipping information. Recommendations are therefore limited to conclusions supported by the available data.

## Technical Implementation

Tools: 
- Power BI
- Power Query
- DAX

The semantic model separates business processes operating at different grains:

- Website Sessions for acquisition and conversion analysis
- Orders for order-level sales economics
- Order Items for product-level performance
- Refunds for return activity
- Calendar for consistent time intelligence

### Semantic Model
![](Images/Screenshots/modelling.png)

A dedicated KPI framework separates Session Date, Order Date, Product Sale Date, and Refund Date, preventing misleading comparisons across different business processes.

### Key techniques include:

- [Data profiling and validation](Docs/01_Data_Audit.md)
- Power Query transformation
- [Fact constellation modeling](Images/Screenshots/modelling.png)
- Dedicated Calendar table
- Explicit DAX measures, including USERELATIONSHIP for alternate date roles
- Time intelligence with previous-month, MoM, and YTD comparisons 
- [KPI Framework](Docs/02_KPI_Framework.md) and business driver decomposition

## Key Business Insights

- **Growth was primarily volume-driven.** Revenue growth was driven mainly by increasing orders and traffic, while conversion became a more important growth driver during the later period.

- **Mobile is the clearest conversion opportunity.** Desktop converts at approximately 8.5% compared with 3.1% on mobile and generates roughly 2.7 times more revenue per session. The gap persists within individual traffic sources and comparable landing pages.

- **Acquisition is heavily concentrated in Google.** Google generates roughly two thirds of website sessions, making it the primary acquisition source but also creating channel concentration risk.

- **Bundle purchasing significantly increases transaction value.** Bundle purchases represent approximately 24% of orders but 36% of revenue, with Average Order Value around 76% higher than single item orders.

- **Product scale, margin, and refund risk tell different stories.** The Original Mr. Fuzzy leads in both revenue and profit, while the other products show different margin and refund patterns.

- **Refund performance differs across products.** Refunds generally increased in line with business growth, but two issues stand out: a sharp Mr. Fuzzy refund spike in September 2014 and a consistently higher refund rate for Birthday Sugar Panda.

For a deeper analysis, see [Insights and Business Recommendations](Docs/04_Insights_and_Business_Recommendations.md).

## Recommended Business Priorities

The analysis points to six priority actions:

- Investigate the mobile conversion journey and identify sources of mobile drop-off.
- Reduce acquisition concentration while preserving Google's contribution to scale.
- Strengthen bundle and cross-sell strategies to increase basket value.
- Analyze refund drivers at the product level rather than assuming the problem affects the whole portfolio.
- Continue product diversification while evaluating revenue, margin, and refund risk together.
- Use controlled testing before attributing landing-page conversion differences to page design.

Detailed findings and recommendations are available in [Insights and Business Recommendations](Docs/04_Insights_and_Business_Recommendations.md).

## Documentation

Detailed project documentation is available in the [Docs](Docs/) folder:

- [Business Understanding](Docs/00_Business_Understanding.md) - business context, analytical questions, scope, and limitations
- [Data Audit](Docs/01_Data_Audit.md) - data profiling, grain, keys, validation, and modeling implications
- [KPI Framework and Metric Dictionary](Docs/02_KPI_Framework.md) - metric definitions, DAX logic, grain, and date semantics
- [Insights and Business Recommendations](Docs/03_Insights_and_Business_Recommendations.md) - detailed findings, interpretation, limitations, and recommended actions