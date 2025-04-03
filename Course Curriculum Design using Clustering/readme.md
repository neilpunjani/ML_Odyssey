# Constructing a Master of Business and Management in Data Science and Artificial Intelligence Course Curriculum with Unsupervised ML (K-Means and Hierarchical Clustering)

## Project Introduction and Executive Summary

One of the biggest challenges in education today is keeping course content aligned with the fast changing demands of the job market. Too often, students graduate with gaps in skills that employers expect from day one.

This project takes a data first approach to solving that problem.

Using a custom web scraper, I collected over 1000 job postings from Indeed, targeting roles across management, data, analytics, and AI. From these postings, I applied natural language processing techniques to extract in demand technical and soft skills. This included both manually curated keywords and skill extraction using OpenAI's GPT API.

Once the raw text was transformed into structured data, I engineered relevant features such as skill frequency, co occurrence patterns, and contextual relationships between skills. I then applied two clustering algorithms: hierarchical clustering using a custom distance matrix, and K means using multidimensional engineered feature vectors. These were used to group similar skills into clusters that could represent individual courses.

Dimensionality reduction techniques like PCA helped visualize the structure of these clusters, allowing for easier interpretation and logical course sequencing.

The final result is a full course structure made up of 8 to 12 carefully designed modules. Each module covers a unique mix of technical tools, business concepts, and soft skills that were frequently linked together in job descriptions. This coursework reflects how real skills are bundled in the job market and offers a practical, relevant foundation for anyone preparing to enter or grow within the data and analytics space.

## Data Extraction, Cleaning and Preprocessing

