# Career Compass by InnoMinds Team
## Table of Contents

- [Project Idea & Motivation](#project-idea--motivation)
- [System Architecture](#system-architecture)
- [Data Description](#data-description)
- [Recommendation Engine](#recommendation-engine)
- [Database & APIs](#database--apis)
- [Flutter Mobile Application](#flutter-mobile-application)
- [UI/UX Design](#ui/ux-design)
- [Business Plan & Sustainability](#business-plan--sustainability)
- [Final Predications & Demonstration](#final-predictions--demonstration)
- [Future Work / Next Steps](#future-work--next-steps)
- [Contributors](#contributors)

## Project Idea & Motivation

This project was developed as part of the **Digitopia AI Competition**, conducted under the supervision of the **Ministry of Education and Communications.**

### Problem Statement

Many students struggle with choosing their future careers due to limited traditional counseling and lack of exposure to modern jobs. Most of them only know about a few professions such as doctor, engineer, or lawyer, while emerging fields like AI, sustainability, data science, or digital design remain unknown. When the time comes for them to make a career decision, they often feel lost, confused, and unsure of the right path to take.

### Our Solution (Career Compass)

Our team **InnoMinds** developed Career Compass, an AI-powered career guidance and job recommendation platform. It helps students and job seekers:
  - Explore a Jobs Library with responsibilities, required skills, and salary insights.
  - Receive personalized AI-powered recommendations based on their interests and queries.
  - Discover emerging job trends and align their skills with future opportunities.

Currently, the system implements the Recommendation Engine as the AI core. It acts as both a **Career Matcher** and a **Skill-to-Career Matcher**. 
Users can type in job roles (e.g., “Data Scientist”), skills (e.g., “Python, SQL”), or even a mix with industries/sectors (e.g., “Machine Learning + Healthcare”) then the system then analyzes the query, extracts key skills and context, and returns:
  - **Top 3 most relevant job recommendations** tailored to the query.
  - **Average salary** for each role.
  - **Responsibilities** and tasks associated with the role.
  - **Key skills** required to succeed.

This **makes Career Compass not only a job recommendation system but also a skill-driven career exploration tool**, bridging the gap between personal abilities and labor market needs.


## Data Description

The **Career Compass system** relies on a curated dataset of jobs, salaries, skills, and responsibilities to power the recommendation engine.

We used the **Job Description Dataset** available on Kaggle: [Job Description Dataset](https://www.kaggle.com/datasets/ravindrasinghrana/job-description-dataset).

- **Shape:** `(1,615,940 rows, 23 columns)`  
- **Format:** CSV file (`job_descriptions.csv`)

### Loading Dataset

To load it directly in Python, we used `kagglehub`:
```markdown
import kagglehub
import os
import pandas as pd

# Download dataset from Kaggle
path = kagglehub.dataset_download("ravindrasinghrana/job-description-dataset")

print("Path to dataset files:", path)
print(os.listdir(path))  # View files inside the dataset folder

# Load dataset into a DataFrame
df = pd.read_csv(path + "/job_descriptions.csv")
```

### Columns Description


| Column Name       | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| **Job Id**        | A unique identifier for each job posting.                                   |
| **Experience**    | Required or preferred years of experience.                                  |
| **Qualifications**| Educational qualifications needed.                                          |
| **Salary Range**  | Range of salaries or compensation offered.                                  |
| **Location**      | City or area where the job is located.                                      |
| **Country**       | Country of the job location.                                                |
| **Latitude**      | Latitude coordinate of the job location.                                    |
| **Longitude**     | Longitude coordinate of the job location.                                   |
| **Work Type**     | Type of employment (e.g., full-time, part-time, contract).                  |
| **Company Size**  | Approximate size of the hiring company.                                     |
| **Job Posting Date** | Date when the job was posted.                                            |
| **Preference**    | Special requirements (e.g., Only Male, Only Female, Both).                  |
| **Contact Person**| Name of recruiter/contact person.                                           |
| **Contact**       | Contact information for job inquiries.                                      |
| **Job Title**     | Job title or position being advertised.                                     |
| **Role**          | Role or category (e.g., software developer, marketing manager).             |
| **Job Portal**    | Platform/website where the job was posted.                                  |
| **Job Description** | Detailed description of job responsibilities and requirements.            |
| **Benefits**      | Benefits offered (e.g., health insurance, retirement plans).                |
| **Skills**        | Skills required for the job.                                                |
| **Responsibilities** | Specific duties and responsibilities.                                    |
| **Company Name**  | Name of the hiring company.                                                 |
| **Company Profile** | Brief overview of the company’s background and mission.                   |

Here is a Sample preview of the dataset: 

| Job Id        | Experience     | Qualifications | Salary Range | Location  | Country      | Latitude | Longitude | Work Type | Company Size | Job Posting Date | Preference | Contact Person       | Contact             | Job Title                   | Role                    | Job Portal | Job Description                                                                 | Benefits                                                                             | Skills                                                   | Responsibilities                                                              | Company                     | Company Profile                                               |
|---------------|----------------|----------------|--------------|-----------|--------------|----------|-----------|-----------|--------------|-----------------|------------|----------------------|--------------------|------------------------------|-------------------------|------------|---------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|----------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------|----------------------------------------------------------------|
| 1089843540111562 | 5 to 15 Years | M.Tech         | $59K-$99K    | Douglas   | Isle of Man  | 54.2361  | -4.5481   | Intern    | 26801        | 2022-04-24      | Female     | Brandon Cunningham   | 001-381-930-7517x737 | Digital Marketing Specialist | Social Media Manager    | Snagajob   | Social Media Managers oversee an organization’s...                           | {'Flexible Spending Accounts (FSAs), Relocation...}                               | Social media platforms (e.g., Facebook, Twitter...)        | Manage and grow social media accounts, create...                            | Icahn Enterprises           | {"Sector":"Diversified","Industry":"Diversified"}              |
| 398454096642776  | 2 to 12 Years | BCA            | $56K-$116K   | Ashgabat  | Turkmenistan | 38.9697  | 59.5563   | Intern    | 100340       | 2022-12-19      | Female     | Francisco Larsen     | 461-509-4216       | Web Developer                | Frontend Web Developer  | Idealist   | Frontend Web Developers design and implement u...                           | {'Health Insurance, Retirement Plans, Paid Time Off...}                        | HTML, CSS, JavaScript, Frontend frameworks (e.g., React)  | Design and code user interfaces for websites...                              | PNC Financial Services Group | {"Sector":"Financial Services","Industry":"Commercial Banking"} |





























