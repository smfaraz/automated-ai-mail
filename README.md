# automated-ai-mail
Using openai/chatgpt-3 to automate emails with personalized emails to job postings 
import openai
import requests

# Use OpenAI API key to access the language model
openai.api_key = "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"

# Define the job postings data
job_postings = [
    {
        "email": "syedfaraaz876@gmail.com",
        "title": "Data Scientist",
        "company": "ABC Inc."
    },
    {
        "email": "syedfarazman@gmail.com",
        "title": "Software Engineer",
        "company": "XYZ Ltd."
    }
]

# Create a template for the email content
email_template = "Hello,\n\nI am writing to express my interest in the {title} position at {company}. I believe my skills and experience make me a strong candidate for this role.\n\nI look forward to hearing back from you.\n\nBest regards,\n[Your Name]"

# Use GPT-3 to generate personalized email content for each job posting
for job_posting in job_postings:
    response = openai.Completion.create(
        engine="text-davinci-002",
        prompt=email_template.format(title=job_posting["title"], company=job_posting["company"]),
        max_tokens=1024,
        n=1,
        stop=None,
        temperature=0.5,
    )

    email_content = response["choices"][0]["text"].strip()

    # Send the email to the recipient
    # Replace with your email service provider's API for sending emails
    response = requests.post(
        "https://mail.google.com/gmail/v1/users/syedfaraaz876@gmail.com/drafts/send",
        data={
            "to": job_posting["email"],
            "subject": "Application for {title} at {company}".format(title=job_posting["title"], company=job_posting["company"]),
            "body": email_content
        }
    )

    # Check the response from the email service provider
    if response.status_code == 200:
        print("Email sent successfully to {}".format(job_posting["email"]))
    else:
        print("Failed to send email to {}".format(job_posting["email"]))
