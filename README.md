# Part 1 (Python): User Behavior Analysis in a Mobile Marketplace App: From Sessions to Conversion
This is the graduation project of the Data Analyst track at the Yandex Practicum platform. In this project I worked as an analyst at the mobile application "Unused Items," a marketplace where users sell their unwanted items by posting them on the platform. Event-level data on user actions, install sources, and timestamps was provided for users whose first action took place after October 7, 2019. The task was to study the relationship between user behavior — the sequence of actions performed in the app — and the final conversion to viewing seller contact information, and to derive UX hypotheses for the product team.

## Technical Project Description
This project focused on identifying which user paths drive conversion to the target action ("view seller contact") in a mobile marketplace, using Python-based behavioral and statistical analysis. The objective was to map user journeys, quantify funnel performance for the most popular scenarios, and validate hypotheses about how event behavior and acquisition channel affect conversion.
The analysis was conducted using Python, primarily with:

- pandas and NumPy for data cleaning, transformation, and aggregation
- plotly for Sankey diagrams and funnel charts
- matplotlib for additional visualizations
- scipy.stats for hypothesis testing

First, I cleaned and prepared the dataset: standardized column names, converted timestamps to datetime, removed ~1.5% of duplicate rows, and consolidated semantically equivalent events (contacts_show / show_contacts, and search_1 … search_7 collapsed into a single search event using a regular expression). Sources were merged onto the event log with a left join.
I then sessionized the event log: a new session was opened whenever the user changed or the gap between consecutive events of the same user exceeded 30 minutes. On top of sessionized data I built a Sankey diagram of the first three steps of each session and identified the most popular paths leading to show_contacts (e.g. tips_show → show_contacts, photos_show → show_contacts, search → tips_show → show_contacts).
For each of the most popular paths I built a 2- or 3-step conversion funnel using plotly's Funnel chart and compared step-to-step conversion rates. I also computed event-share distributions for two cohorts — users who reached the contact view and users who did not — to highlight behavioral differences between converters and non-converters.
Finally, I tested two hypotheses using a z-test for proportions:

Conversion to contact view differs between users who perform both tips_show and tips_click and users who perform only tips_show.
Conversion to contact view differs depending on the install source (Google vs. Yandex).

The results supported the first hypothesis (clear difference in conversion) and failed to reject the null for the second (24.36% vs. 24.72% — practically identical).

## Key Technical Skills Applied:
- Data cleaning and transformation with pandas
- Event consolidation with regular expressions
- User sessionization with vectorized pandas operations
- Funnel analysis (plotly Funnel)
- Path analysis with Sankey diagrams (plotly Sankey)
- Cohort comparison and distribution analysis
- Statistical hypothesis testing with a z-test for proportions
- Interpretation of p-values and significance levels
- Translating analytical findings into UX/product recommendations


# Part 2 (SQL): Book Subscription Service: SQL-Based Catalog and User Analysis
This is the SQL portion of the graduation project of the Data Analyst track at the Yandex Practicum platform. In this project I worked as an analyst for a company that had just acquired a subscription-based book reading service. The PostgreSQL database contained five tables — books, authors, publishers, ratings, and reviews — covering 1,000 books published between 1952 and 2020, 636 authors, 340 publishers, 6,456 ratings, and 2,793 user reviews. The task was to explore this database with SQL in order to inform a value proposition for the new product.

## Technical Project Description
This project focused on extracting catalog- and user-level insights from a relational database using SQL, and translating those insights into actionable recommendations for the new subscription product. The objective was to characterize the catalog, identify high-performing authors and publishers, and quantify engagement among the most active users.
The analysis was conducted in Python on top of a PostgreSQL database, primarily with:

- SQL (PostgreSQL dialect) for all data manipulation
- SQLAlchemy for the database connection
- pandas for delivering query results into a Jupyter notebook

First, I connected to the database with SQLAlchemy and wrote a small helper that runs a query inside a managed connection and returns a pandas.DataFrame. I then explored the schema and produced descriptive summaries of every table (row counts, date ranges, sample rows) to validate the data and build intuition.
After that I answered five business questions, each with a single SQL query:

The number of books published after January 1, 2000 (to see how modern the catalog is).
For every book — the number of reviews and the average rating, joining books, reviews, and ratings and using COUNT(DISTINCT review_id) to avoid inflation from the cross-join.
The publisher with the largest number of books longer than 50 pages, using a CTE that filters out brochures and ranks publishers.
The author with the highest average book rating, restricted to books with at least 50 ratings — implemented with two stacked CTEs and a HAVING filter.
The average number of reviews from users who left more than 48 ratings, again with two CTEs that first identify the active users and then count their reviews.

All queries follow a consistent SQL style guide: keywords in upper case, snake_case identifiers, explicit AS aliases, and aligned multi-line indentation. Results were converted into business recommendations covering catalog focus, publisher-led collections, author-anchored personalization, and engagement programs for power users.

## Key Technical Skills Applied:
- Writing analytical SQL on a PostgreSQL database
- Multi-table joins (INNER JOIN, LEFT JOIN)
- Common Table Expressions (CTEs) for staged analysis
- Aggregations with COUNT, AVG, MIN, MAX, ROUND
- GROUP BY / HAVING for filtered aggregates
- COUNT(DISTINCT …) to avoid double-counting after joins
- Connecting to a relational database from Python via SQLAlchemy
- Returning query results into pandas for further exploration
- Following a consistent SQL style guide for readability
- Translating query output into business recommendations
