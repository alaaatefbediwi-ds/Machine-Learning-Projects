# Career Compass by InnoMinds Team
## Table of Contents

- [Project Idea & Motivation](#project-idea--motivation)
- [System Architecture](#system-architecture)
- [Data Description & Preprocessing Pipeline](#data-description--preprocessing-pipeline)
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


## Data Description & Preprocessing Pipeline

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


### Data Cleaning & Preprocessing

To ensure high-quality inputs for our recommendation engine, we performed several data cleaning and preprocessing steps on the original dataset.  

#### 1. Company Profile Parsing & Career Categorization

- The `Company Profile` column contained **JSON-like strings** with details such as **Sector, Industry, Location, Website, CEO** etc.  
- Some steps are performed:  
  - Converted string objects into dictionaries using `json.loads()`.  
  - Handled malformed rows (unescaped quotes in the CEO field) using regex fixes.  
  - Extracted **Sector** and **Industry** fields into separate columns.  


#### 2. Fixing Missing & Corrupted Values in Sector and Industry 

- The `Company Profile` column had **5,478 null values**.  
So, we followed some steps:  
  - Checked for duplicate company entries with complete profiles and used them to fill missing values.  
  - For high-frequency missing companies, applied **manual imputation mapping**:  
    - *Peter Kiewit Sons → Sector: Construction, Industry: Engineering & Construction*  
    - *Dunkin' Brands Group, Inc. → Sector: Food & Beverage, Industry: Restaurants*  
    - *Estée Lauder → Sector: Consumer Goods, Industry: Personal Care & Cosmetics*  

**Result:** No remaining nulls in `Sector` and `Industry` columns.  


#### 3. Salary Range Normalization  
- Original format: as for example `"$59K-$99K"`.  
- **Preprocessing steps:** 
  - Removed symbols (`$`, `K`) and split into `Salary_min` and `Salary_max`.  
  - Computed `Salary_avg` as midpoint.  
  - Converted salaries to **monthly estimates**:  
    \[
    Salary\_monthly = \frac{(Salary\_avg \times 1000 \times 48)}{12}
    \]  

**Result:** Numerical `Salary_min`, `Salary_max`, `Salary_avg` columns available.  

#### 4. Sector Standardization  
- Initial dataset had **204 unique sectors** with redundant labels.  
- Created a **sector mapping dictionary** to unify similar categories.  
  - Example: `"Information Technology"`, `"IT Services"`, `"Technology and Engineering"` → **Technology**.  
  - Example: `"Construction & Engineering"`, `"Industrial"`, `"Semiconductors"` → **Engineering**.  
- After standardization: **79 unique sectors** are remained.  

**Result:** Canonicalized sector labels for consistent use in recommendation engine.  


#### 5. Job Title & Salary Aggregation  
- Chose `Job Title` (147 unique values) as the primary label instead of `Role` for recommendation.  
- **Aggregated salaries** across `Job Title`, `Sector`, and `Role` using a pivot table (mean min/max/avg salaries).  

**Result:** Clean salary distribution by `Job Title`.  


#### 6. Text Processing for Recommendation Model  
- Created a unified **job_text** column by concatenating:  
  `Job Title + Sector + Role + Job Description + Skills + Responsibilities`  
- Applied text preprocessing:  
  - Lowercasing  
  - Removing punctuation & extra spaces  
  - Stop word removal   
- Including `Sector` in `job_text` column that was then embedded using the embedding model helped refining user query to be more specific.

**Result:** Clean textual representation of each job posting.  


#### 7. Final Dataset for Embedding Model  
- Deduplicated job postings based on (`Job Title`, `Sector`, `Role`, `job_text_clean`).  
- Final dataset shape: **(9,496 rows × 37 columns)**  

**Result:** A clean, well-structured dataset ready for **embedding generation** and recommendation engine training.  



## Recommendation Engine

Our project includes a **Skill-to-Career Matcher** recommendation engine.

Users can input queries describing their **skills, interests, or target careers**, and the engine recommends **top-3 most relevant jobs** based on semantic similarity and industry context.  

### 1. Recommendation Engine Workflow

The following diagram illustrates the steps of the recommendation engine:

![Recommendation Engine Workflow](./assets/recommendation_engine_workflow.png)

1. **User Query** → The user inputs a job-related query.  
2. **Encoding** → Query is transformed into embeddings using Sentence-BERT.  
3. **FAISS Search** → The system searches for the most similar job embeddings.  
4. **Retrieve Top-k Jobs** → Returns the most relevant job titles (Top-3).  
5. **Display Results** → Recommendations are shown with job title, role, sector, salary, job description and job responsibilities.  


### 2. Embedding Generation

- Used **Sentence Transformers** with `multi-qa-mpnet-base-dot-v1`, a retrieval-optimized model.  
- Encoded the cleaned text representation (`job_text_clean`) of each job posting into dense embeddings.  

```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("multi-qa-mpnet-base-dot-v1")
job_embeddings = model.encode(
    trans_df_final['job_text_clean'].tolist(),
    show_progress_bar=True,
    convert_to_numpy=True
)
```
**Result:** Each job posting is represented as a **768-dimensional vector**.

### 3. Building FAISS Index

- Used FAISS (Facebook AI Similarity Search) for efficient nearest-neighbor retrieval.
- Stored all embeddings in a flat L2 index.

**Result:** Fast similarity search enabled for **~9.5K job postings**.

### 4. Prediction Function

Implemented a `predict_top_3(query)` function:

  - Encodes the user’s query into an embedding.

  - Retrieves **top-3 closest jobs** using FAISS.

  - Returns `Job Title`, `Sector`, `Role`, `Aggregated Salary`, `Job Description`, `Responsibilities` and `Distance Score`.


### 5. Example Queries

**Query 1:** *“I like designing buildings and interiors, and I’m creative with spaces”*

**Recommendations:**
- **Architectural Designer** | Sector: *Fashion & Apparel* | Role: *Interior Designer* | Salary ~358K  
- **Architectural Designer** | Sector: *Media & Entertainment* | Role: *Interior Designer* | Salary ~378K  
- **Architectural Designer** | Sector: *Mining* | Role: *Interior Designer* | Salary ~302K  
 
Since the query described the **skills of an Architectural Designer in general** without specifying a particular domain, the system returned the **same job title** (*Architectural Designer*) across **different sectors**.


**Query 2:** *“I enjoy analyzing data and building predictive models in the healthcare field”*

**Recommendations:**
- **Data Analyst** | Sector: *Healthcare Services* | Role: *Data Scientist* | Salary ~270K  
- **Data Analyst** | Sector: *Healthcare* | Role: *Data Scientist* | Salary ~347K  
- **Business Analyst** | Sector: *Healthcare* | Role: *Data Business Analyst* | Salary ~276K  
 
Here, the user explicitly mentioned the **Healthcare domain**. As a result, the system returned **different but related job titles** (*Data Analyst, Business Analyst*) all within the **Healthcare sector**.


### 6. Evaluation

We tested the engine on **9,496 samples** to measure accuracy and response time.

**Summary Metrics**
| Metric               | Score         |
|-----------------------|--------------|
| **Top-1 Accuracy**    | 82.3%        |
| **Top-3 Accuracy**    | 92.8%        |
| **Average Query Time**| 0.11 seconds (~110 ms) |
| **Median Query Time** | 0.10 seconds (~101 ms) |
| **Worst-case Latency**| 0.41 seconds (~415 ms) |

**Detailed Results**

| Success Top-1 | Failed Top-1 | Top-1 Accuracy | Success Top-3 | Failed Top-3 | Top-3 Accuracy | Avg Query Time (s) | Median Query Time (s) | Max Query Time (s) | Min Query Time (s) |
|---------------|--------------|----------------|---------------|--------------|----------------|---------------------|-----------------------|---------------------|---------------------|
| 7816          | 1680         | 0.8231         | 8814          | 682          | 0.9282         | 0.1099              | 0.1012                | 0.4150              | 0.0846              |


The system achieves **high accuracy** while maintaining **near real-time inference speed**, making it suitable for production deployment.

**Note:**

- Accuracy at *k=3* is higher than *k=1* because allowing the top-3 results gives more flexibility even if the best match is ranked 2nd or 3rd, it is still counted as correct.

This reduces the penalty of small ranking variations and better reflects the system’s usefulness to end-users.

### 7. Saving for Inference

To avoid recomputing embeddings during query time, we saved both:

- **Job embeddings** → `job_embeddings.npy`  
- **Index-to-row mapping** → `enc_order.npy`  

```python
np.save("job_embeddings.npy", job_embeddings)
np.save("enc_order.npy", enc_order)
```
## UI/UX Design

#  Career Compass ( User Flow )
![<img width="12750" height="3318" alt="signup" src="https://github.com/user-attachments/assets/10493c92-7d97-4cb7-9b0a-61728c8d1fa7" />]

---

##  Sign Up Flow

### 1. Splash Screen
- User opens the app and sees a welcoming splash screen.  
- Tap on **Get Started** to continue.
- 
### 2. Log in Screen
- Options: **Log in** or **Sign up**.  
- New users select **Sign up**.
- 
### 3. Email Entry
- User enters their email address.  
- App sends a **5-digit verification code**.

### 4. Email Verification
- User enters the code to confirm their email.
  
### 5. Password Creation
- User sets a password following rules:  
  - Minimum **8 characters**  
  - At least **1 number**  
  - At least **1 symbol**  

### 6. Success Screen 
- Confirmation: *“Your account was successfully created!”*  
- User can now access the app.  













## Business Plan & Sustainability

### 1. Key Features

#### Implemented in Current Version
1. **Skill-to-Career Matcher** – Links students’ existing skills to relevant future job opportunities.  
2. **AI-Powered Career Recommendations** – Personalized job suggestions based on students’ interests, skills, and personality.  

#### Planned for Future Work
3. **Comprehensive Jobs Library** – Simple explanations, short videos, responsibilities, and salary insights.  
4. **Career Simulations & Virtual Try-Outs** – Interactive experiences that allow students to try jobs firsthand.  
5. **Gamified Career Discovery** – Quizzes and interactive scenarios to make career exploration engaging.  
6. **Future Job Trends Explorer** – Insights into in-demand and declining careers.  
7. **IQ & Critical Thinking Hub** – Puzzles, logic challenges, and problem-solving exercises.  

### 2. How Users Will Use the Solution?

#### Current Flow
1. **Quick Quiz & Recommendations**  
   - Student provides inputs about their **interests and skills**.  
   - The AI recommends **3 career paths** tailored to the answers.  

2. **Skill-to-Career Matching**  
   - Students can directly check which career paths align with their **current skill set**.  

#### Future Enhancements (Planned)
- Access a **Jobs Library** with videos, responsibilities, and salary insights.  
- Explore **Trending Jobs** and future job demand.  
- Participate in **Career Simulations** and connect with **mentors**.  
- Solve **IQ puzzles & critical thinking challenges** to sharpen problem-solving.  

### 3. Sustainability & Monetization  

We follow a **freemium model** to ensure accessibility while allowing long-term sustainability.  

- **Free Features (already implemented)**  
  - Skill-to-Career Matcher  
  - AI-powered Career Recommendations  

- **Premium Features (Planned)**   
  - Mentorship sessions with professionals  
  - Career simulations & virtual try-outs  
  - Access to extended career insights & advanced analytics  

**Revenue Streams:**  
- **School/University Partnerships** → Institutions adopt the platform for student guidance.  
- **Corporate Sponsorships** → Companies sponsor career content to connect with future talent.  
- **Premium Subscriptions (Future)** → Unlock simulations, mentorship, and advanced features.  

This ensures the platform is already **useful today** with AI-driven recommendations, while maintaining a **clear roadmap for future expansion** toward a complete career guidance ecosystem.  

## Future Work / Next Steps

While the current version focuses on **Skill-to-Career Matcher** and **AI-powered Career Recommendations**, we have a clear roadmap to expand the platform into a full-featured career guidance ecosystem.  

### 1. Planned Enhancements
- **Jobs Library** → Short videos, responsibilities, and salary insights for each career.  
- **Career Simulations & Virtual Try-Outs** → Let students experience jobs in an interactive way.  
- **Gamified Career Discovery** → Quizzes and interactive challenges to make exploration engaging.  
- **Future Job Trends Explorer** → Insights into in-demand and declining careers.  
- **IQ & Critical Thinking Hub** → Logic puzzles and problem-solving scenarios.  
- **Mentorship Sessions (Premium)** → Connect students directly with professionals for real-world advice.  

### 2. Vision

Our long-term goal is to provide **end-to-end career guidance** for students from initial exploration and self-discovery, through interactive learning and mentorship, to prepare them for the most relevant future jobs.  
 

## Contributors 

**InnoMinds Team**
- [Alaa Atef Abdelhaleem](https://www.linkedin.com/in/alaa-atef-587421285/)
- [Mohamed Ahmed Hamdy](https://www.linkedin.com/in/mohamed-ahmed-hamdy-9b8722272/)
- [Mirna Tarek Abdelaziz](https://www.linkedin.com/in/mirna-tarek/)
- [Doaa Khamis Kamal](https://www.linkedin.com/in/doaa-khamis-a04533182/)

