# AI-Powered Recruitment Automation with n8n

An intelligent recruitment automation workflow built with **n8n**, **Google Gemini AI**, **Google Sheets**, and **Gmail**.

This workflow automates the initial candidate screening process by receiving candidate information and a resume, extracting the resume content, analyzing the candidate using AI, generating a structured evaluation, storing the results, and sending an appropriate HR email based on the candidate's score.

## 🚀 Overview

The workflow is designed to reduce manual effort during the initial recruitment screening stage.

A candidate submits their information and resume through a webhook. The workflow then:

1. Receives candidate information through a **POST Webhook**.
2. Retrieves the submitted resume.
3. Extracts text from the PDF resume.
4. Sends the candidate information and resume content to a **Google Gemini AI Agent**.
5. Evaluates the candidate's strengths and weaknesses.
6. Compares skills provided in the application form with skills found in the CV.
7. Identifies missing or relevant skills.
8. Generates interview questions.
9. Assigns a candidate score.
10. Estimates an appropriate salary range.
11. Generates an HR email subject and response.
12. Stores the evaluation in **Google Sheets**.
13. Routes the candidate based on their score.
14. Sends the appropriate email automatically.

The AI response is returned using a structured JSON format to make the workflow easier to process and integrate with other systems.

## ✨ Features

* **Automated Candidate Intake**

  * Receives candidate information through a webhook.
  * Supports candidate name, email, phone, experience, and skills.

* **Resume Processing**

  * Retrieves the submitted resume.
  * Extracts text from PDF files before AI analysis.

* **AI Resume Analysis**

  * Uses Google Gemini 2.5 Flash.
  * Evaluates candidate strengths and weaknesses.
  * Reviews experience and technical skills.
  * Identifies missing skills.
  * Provides hiring recommendations and reasoning.

* **Candidate Scoring**

  * Generates a candidate score.
  * Uses the score to determine the next recruitment action.

* **Automated Decision Routing**

  * **80+** → Shortlisted
  * **60–79** → Requires further review
  * **Below 60** → Rejection workflow

  The score-based routing is implemented using an n8n Switch node.

* **Interview Preparation**

  * Generates two interview questions based on the candidate profile and identified skill gaps.

* **HR Email Automation**

  * Automatically generates an email subject and body.
  * Sends different responses depending on the candidate's evaluation.

* **Candidate Data Storage**

  * Saves candidate evaluation data to Google Sheets for record keeping and further review.

* **Structured AI Output**

  * Produces consistent JSON containing candidate information, evaluation results, score, recommendation, interview questions, and email content.

## 🧩 Workflow Architecture

```text
Candidate Form
      │
      ▼
   Webhook
      │
      ▼
 HTTP Request
      │
      ▼
Resume PDF Extraction
      │
      ▼
 Google Gemini AI
      │
      ▼
Structured JSON Output
      │
      ▼
   Edit Fields
      │
      ▼
 Google Sheets
      │
      ▼
 Score-Based Switch
   ┌──────┼──────┐
   ▼      ▼      ▼
80+    60–79    <60
   │      │      │
   ▼      ▼      ▼
Shortlist Review  Reject
   │      │      │
   └──────┴──────┘
          │
          ▼
      Gmail Email
```

The workflow connections follow this sequence from webhook and resume processing through AI analysis, data storage, score routing, and email delivery.

## 🛠️ Technologies Used

| Technology                   | Purpose                               |
| ---------------------------- | ------------------------------------- |
| **n8n**                      | Workflow automation and orchestration |
| **Google Gemini 2.5 Flash**  | AI-powered candidate analysis         |
| **Google Sheets**            | Candidate evaluation storage          |
| **Gmail**                    | Automated HR communication            |
| **Webhook**                  | Candidate data intake                 |
| **PDF Extraction**           | Resume text extraction                |
| **Structured Output Parser** | Consistent AI response format         |

## 📋 AI Evaluation

For each candidate, the workflow can generate:

* Candidate name
* Email
* Phone number
* Years of experience
* Skills listed in the application
* Skills identified in the CV
* Strengths
* Weaknesses
* Missing skills
* Hiring recommendation
* Recommendation reasoning
* Interview questions
* Candidate score
* Estimated salary
* HR email subject
* HR email body
* AI response

These fields are mapped into the workflow before being stored in Google Sheets.

## ⚙️ Setup

### 1. Import the Workflow

Import the provided n8n workflow JSON into your n8n instance.

### 2. Configure Google Gemini

Add your Google Gemini credentials and configure the Gemini Chat Model used by the AI Agent.

The workflow currently uses:

```text
Google Gemini 2.5 Flash
```

### 3. Configure Google Sheets

Connect your Google Sheets account and select the spreadsheet where candidate evaluations should be stored.

Create columns for the candidate information and AI-generated evaluation fields.

### 4. Configure Gmail

Connect your Gmail account and configure the email nodes used for recruitment notifications.

**Important:** Replace any example or personal email addresses in the workflow with your organization's HR email address before deployment.

### 5. Configure the Webhook

Connect your application or candidate form to the n8n webhook using a POST request.

The workflow expects candidate information and a resume URL in the incoming data.

### 6. Activate the Workflow

After testing the workflow with sample candidate data, activate the workflow and connect your recruitment form/application to the webhook endpoint.

## 🔄 Candidate Decision Logic

The workflow uses a score-based recruitment routing system:

|        Score | Action           |
| -----------: | ---------------- |
|   **80–100** | Shortlisted      |
|    **60–79** | Manual HR Review |
| **Below 60** | Rejection        |

This allows HR teams to automate repetitive screening tasks while keeping a review stage for borderline candidates.

## 📊 Example Output

```json
{
  "candidate_name": "Candidate Name",
  "email": "candidate@example.com",
  "phone": "+92XXXXXXXXXX",
  "years_of_experience": "3",
  "strengths": [
    "Strong technical background",
    "Relevant professional experience"
  ],
  "weakness": [
    "Limited experience with a required technology"
  ],
  "recommendation": "Shortlist",
  "reasons": "Strong alignment with the required skills and experience.",
  "interview_questions": [
    "Describe a project where you used your primary technical skill.",
    "How would you approach a challenging problem in this role?"
  ],
  "skills_in_cv": "Python, SQL, n8n",
  "missing_skills": "Advanced cloud deployment",
  "candidate_score": "85",
  "estimated_salary": "Based on experience and market requirements",
  "HR_email_subject": "Candidate Shortlisted",
  "HR_email_body": "The candidate has been shortlisted for the next stage.",
  "ai_response": "Candidate evaluation completed."
}
```

## 🔐 Security Considerations

Before using this workflow in production:

* Do not expose API keys or OAuth credentials.
* Use n8n's credential management system.
* Replace personal email addresses with appropriate organizational accounts.
* Avoid committing sensitive candidate information to GitHub.
* Do not upload real resumes or personal candidate data to the repository.
* Review AI-generated hiring recommendations before making final employment decisions.
* Ensure the workflow complies with applicable employment, privacy, and data-protection requirements.

## ⚠️ Important Note

This project is intended to **assist recruiters with candidate screening**, not to completely replace human decision-making.

AI-generated scores and recommendations should be treated as decision-support information. Final hiring decisions should be made by qualified human recruiters and hiring managers.

## 📁 Repository Structure

```text
.
├── recruitment-automation.json
└── README.md
```

## 🎯 Use Cases

This workflow can be adapted for:

* Resume screening
* Automated candidate evaluation
* HR recruitment pipelines
* Skill-gap analysis
* Interview preparation
* Candidate shortlisting
* Recruitment email automation
* Applicant tracking workflows
* AI-assisted HR operations

## 🔮 Future Improvements

Potential improvements include:

* Add a dedicated applicant tracking system.
* Add duplicate candidate detection.
* Introduce configurable scoring criteria.
* Add job-description matching.
* Support multiple resume formats.
* Add recruiter approval before automated rejection.
* Create a recruitment dashboard.
* Add candidate status tracking.
* Implement stronger data validation.
* Add audit logs for AI-generated decisions.
* Introduce role-specific evaluation criteria.

## 👨‍💻 Author
  Muneeb
Built as an **AI-powered n8n recruitment automation project** demonstrating workflow automation, document processing, LLM integration, structured AI outputs, data storage, conditional routing, and automated email communication.

---

⭐ If you find this project useful, consider giving the repository a star.
