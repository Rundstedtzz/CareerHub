# CareerHub
Mini Project 2 – CareerHub: Building a Mini Job Portal with MongoDB and Flask

## Objectives: 
-	Schema Design and Data Transformation: Understand the differences between relational and NoSQL databases. Design a MongoDB schema and transform data from a "relational" format (provided as multiple CSV files) into MongoDB collections
-	MongoDB and Python Integration: Practice using PyMongo to interact with MongoDB through Python to gain hands-on experience with database operations such as insertion, querying, and updates
-	Web API Development: Explore the basics of web API development using Flask and enhance your understanding of creating and managing endpoints to a modern database. You will practice testing these endpoints using tools like Postman to ensure your API's functionality and reliability

## Assignment Tasks
### Schema Design - take a look at the original data

**1. companies.csv:**

id: A unique identifier for the company. <br>
name: The name of the company. <br>
size: The size of the company (e.g., number of employees).<br>
type: The type of the company (e.g., Enterprise, Multinational, Startup, SME).<br>
location: The location of the company.<br>
website: The company's website URL.<br>
description: A brief description of the company.<br>
hr_contact: The HR contact email for the company.<br>

**2. education_and_skills.csv:**

id: A unique identifier for the education and skill entry.<br>
required_education: The required education level for a job.<br>
preferred_skills: A list of preferred skills for the job (comma-separated).<br>
job_id: A foreign key linking to a specific job.<br>

**3. employment_details.csv:**

id: A unique identifier for the employment detail entry.<br>
employment_type: The type of employment (e.g., Part-time, Contract, Full-time, etc.).<br>
average_salary: The average salary for the job.<br>
benefits: A list of benefits offered for the job.<br>
remote: A boolean indicating whether the job is remote or not.<br>
job_posting_url: The URL link to the job posting.<br>
posting_date: The date when the job was posted.<br>
closing_date: The date when the job application will close.<br>

**4. industry_info.csv:**

id: A unique identifier for the industry information entry.<br>
industry_name: The name of the industry.<br>
growth_rate: The growth rate of the industry.<br>
industry_skills: A list of skills relevant to the industry.<br>
top_companies: A list of top companies in the industry.<br>
trends: Industry-specific trends.<br>

**5. jobs.csv:**

id: A unique identifier for the job entry.<br>
title: The job title.<br>
description: A brief description of the job.<br>
years_of_experience: The years of experience required for the job.<br>
detailed_description: A detailed description of the job.<br>
responsibilities: Responsibilities associated with the job.<br>
requirements: Requirements for the job.<br>

******************
### Schema Design - proposed schema

The MongoDB collection follow the schema outlined below:

- **Root (Object)**
  - `id` (Integer)
  - `title` (String)
  - `description` (String)
  - `years_of_experience` (Integer)
  - `detailed_description` (String)
  - `responsibilities` (String)
  - `requirements` (String)

  - **company (Object)**
    - `name` (String)
    - `size` (String)
    - `type` (String)
    - `location` (String)
    - `website` (String)
    - `description` (String)
    - `hr_contact` (String)
  
  - **education_and_skills (Object)**
    - `required_education` (String)
    - `preferred_skills` (Array of Strings)
  
  - **employment_details (Object)**
    - `employment_type` (String)
    - `average_salary` (Integer)
    - `benefits` (Array of Strings)
    - `remote` (Boolean)
    - `job_posting_url` (String)
    - `posting_date` (String)
    - `closing_date` (String)
  
  - **industry_info (Object)**
    - `industry_name` (String)
    - `growth_rate` (Double)
    - `industry_skills` (Array of Strings)
    - `top_companies` (Array of Strings)
    - `trends` (Array of Strings)

MongoDB schema in JSON Schema format:<br>

<img width="676" alt="image" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/a1ffad99-f990-435a-adf3-05e58a4bb78c">

*******************

### Data Transformation and Import
<img width="727" alt="image" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/e61961a2-ee3b-44e7-b5e1-a272fd18a138"> <br>

<img width="1193" alt="image" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/5f490bb8-932a-4ac0-8ed9-00e560caf7b9">

### Flask App
<img width="831" alt="image" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/e662fbb9-7f0a-4c8f-91c6-50572d12570f">

-	Homepage
    - For the default root URL path (‘/’ or ‘http://localhost:5000/’), display a message welcoming your user.
<img width="1280" alt="image" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/df777214-b073-4ac9-81c9-a651689cf865">

- Create a Job Post
  - When a user visits this page http://localhost:5000/create/jobPost, they can create a new job posting with details, including title, description, industry, average salary, and location.
  - In the corresponding view function, make sure to include logic to validate the data and ensure that essential fields like title and industry are not empty.
  - Insert the validated data into the MongoDB collection.
 <img width="1512" alt="Screenshot 2023-10-16 at 11 45 42 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/b6457053-fda0-4436-a0eb-e35da4762c34">

- View Job Details (part 1)
  - When a user visits this page http://localhost:5000/search_by_job_id /<job_id> users can search by a job id.
<img width="1512" alt="Screenshot 2023-10-16 at 11 44 35 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/23fc2aaf-1a00-49da-b0ef-e7741e99617d">

- View Job Details (part 2)
  - Allow users to input a job title they wish to update via http://localhost:5000/update_by_job_title
  - Search for the job in the database and, if found, display the current details. Provide an option to modify fields like description, average, salary, and location.
  - Update the MongoDB collection with the new details after validation.
<img width="1512" alt="Screenshot 2023-10-16 at 11 47 32 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/ddce3ca6-cc60-4b78-a5f6-6dbab9ee829b">
 
- Remove Job Listing
  - Provide an option for users to input a job title they wish to delete via http://localhost:5000/delete_by_job_title
  - Validate the input and search for the job in the database. If found, display the job details and ask the user for confirmation before deletion.
  - Delete the job from the MongoDB collection upon confirmation
 <img width="1512" alt="Screenshot 2023-10-16 at 11 48 09 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/24fe74d4-cf71-460d-acca-b21789ee3828">

- Salary Range Query
  - Develop an endpoint to query jobs based on a salary range.
  - The user should be able to provide min_salary and max_salary as query parameters, and the API should return jobs within that salary bracket.
<img width="1512" alt="Screenshot 2023-10-16 at 11 48 25 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/9e4cfe3a-97b0-4fee-b5fa-d9c9612e95d1">

- Job Experience Level Query
  - Create an endpoint where users can query jobs based on experience levels such as 'Entry Level', 'Mid Level', 'Senior Level', etc. 
  - Users should be able to provide an experience_level query parameter, and the API should list jobs that match the given experience requirement.
<img width="1512" alt="Screenshot 2023-10-16 at 11 50 10 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/47bfca1b-9dfc-4322-895d-a17b71c98577">

- Top Companies in an Industry
  - Develop an endpoint to fetch top companies in a given industry based on the number of job listings.
  - Users should provide an industry query parameter, and the API should return companies in that industry ranked by the number of job listings they have.
<img width="1512" alt="Screenshot 2023-10-16 at 11 50 38 PM" src="https://github.com/Rundstedtzz/CareerHub/assets/63605514/7394f130-c908-436c-87f3-7b38d9d1b987">


