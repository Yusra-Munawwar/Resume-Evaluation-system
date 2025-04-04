# Resume Evaluation System

A Python-based system that leverages LLMs to automatically evaluate resumes against job descriptions, streamlining the candidate screening process.

## Overview

This project aims to solve a common challenge in the hiring process: the time-consuming and potentially biased manual review of resumes. The system uses LangChain and Google's Generative AI to assess how well a candidate's resume matches a given job description, providing a score and justification.

## Features

- Automated resume scoring on a scale of 1-10
- Justification for each score
- PDF resume parsing capabilities
- Simple and flexible input/output pipeline

## Requirements

- Python 3.x
- LangChain
- Google Generative AI API access
- PyMuPDF (for PDF parsing)

## Installation

```bash
pip install langchain langchain_google_genai PyMuPDF
```

## Usage

### Basic Example

```python
from langchain.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_core.runnables import RunnablePassthrough

# Setup the prompt template
template = """
You are an HR assistant. Evaluate the resume based on the job description.
Job Description: {job_description}
Resume: {resume}

Provide a score from 1 to 10 and give a short reason for the score.
"""
prompt = PromptTemplate(input_variables=["job_description", "resume"], template=template)

# Initialize the LLM
llm = ChatGoogleGenerativeAI(
    model="gemini-pro",
    verbose=True,
    temperature=0.0,
    google_api_key="YOUR_API_KEY"
)

# Create the evaluation chain
chain = (
    {"job_description": RunnablePassthrough(), "resume": RunnablePassthrough()}
    | prompt
    | llm
)

# Example evaluation
job_description = """
We are looking for a software engineer with experience in Python, machine learning, and cloud computing.
The ideal candidate will have strong problem-solving skills and the ability to work in a team environment.
"""

resume = """
Experienced software engineer with 3 years of experience in Python and machine learning.
Proficient in building scalable solutions and cloud-based architectures.
"""

result = chain.invoke({"job_description": job_description, "resume": resume})
print(result)
```

### PDF Resume Handling

```python
import fitz  # PyMuPDF

# Function to extract text from PDF
def extract_text_from_pdf(pdf_path):
    doc = fitz.open(pdf_path)
    text = ""
    for page in doc:
        text += page.get_text()
    return text

# Extract text from a resume PDF
resume_text = extract_text_from_pdf("path/to/resume.pdf")

# Pass to the evaluation chain
result = chain.invoke({"job_description": job_description, "resume": resume_text})
```

## Project Structure

- `README.md`: Project documentation
- `Resume_Evaluation.ipynb`: Jupyter notebook with implementation details
- Sample resumes and job descriptions for testing

