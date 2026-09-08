# StaffSpy 🕵️‍♂️

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![GitHub](https://img.shields.io/badge/GitHub-EbxdMalix%2FStaffSpy--main-blue?logo=github)](https://github.com/EbxdMalix/StaffSpy-main.git)

**StaffSpy** is a powerful Python library designed to scrape employee directories, profiles, companies, post comments, and connection networks directly from LinkedIn. It provides structured Pandas DataFrames for quick data processing, lead generation, talent sourcing, and market intelligence.

---

## ✨ Features

- 🏢 **Scrape Company Staff**: Find employees by company name, role/title keywords, and location.
- 👤 **Detailed Profile Scraping**: Extract comprehensive profile details (experience history, education, skills, certifications, contact info, and emails).
- 💬 **Post Comments & Lead Extraction**: Scrape comments on LinkedIn posts and automatically parse potential email addresses.
- 🤝 **Connection Scraping**: Export your 1st-degree connections and their contact details.
- ⚡ **Bypass Search Limits**: Built-in auto-blocking and search variation strategies to navigate LinkedIn's 1,000-result search limit.
- 🤖 **Automated Connection Requests**: Send connection invites automatically while scraping.
- 🧩 **CAPTCHA Solvers**: Integrated support for CapSolver and 2Captcha.
- 📊 **Pandas DataFrames**: All scraping methods return structured Pandas DataFrames ready for CSV, Excel, or CRM exports.

---

## 🚀 Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/EbxdMalix/StaffSpy-main.git
cd StaffSpy-main
pip install poetry
poetry install
```

Or install in editable mode with pip:

```bash
pip install -e .
```

---

## 🔑 Authentication & Quick Start

You can authenticate using an existing session file (`session.pkl`) or with your LinkedIn username and password:

### 1. Using a Session File (Recommended)
```python
from staffspy import LinkedInAccount

account = LinkedInAccount(
    session_file="session.pkl",
    log_level=1
)
```

### 2. Using Login Credentials & CAPTCHA Solver
```python
from staffspy import LinkedInAccount, SolverType

account = LinkedInAccount(
    username="your_email@example.com",
    password="your_password",
    solver_service=SolverType.CAPSOLVER, # or SolverType.TWO_CAPTCHA
    solver_api_key="YOUR_CAPTCHA_API_KEY",
    log_level=1
)
```

---

## 📖 Usage Examples

### 1. Scrape Staff from a Company
```python
from staffspy import LinkedInAccount

account = LinkedInAccount(session_file="session.pkl", log_level=1)

# Scrape software engineers at OpenAI
staff_df = account.scrape_staff(
    company_name="openai",
    search_term="Software Engineer",
    location="San Francisco",
    extra_profile_data=True,
    max_results=50,
)

print(staff_df[["name", "current_position", "location", "profile_link"]].head())
staff_df.to_csv("openai_staff.csv", index=False)
```

### 2. Scrape Profiles by User IDs
```python
from staffspy import LinkedInAccount

account = LinkedInAccount(session_file="session.pkl", log_level=1)

user_ids = ["billgates", "satyanadella"]

users_df = account.scrape_users(
    user_ids=user_ids,
    block=False,
    connect=False
)

print(users_df[["name", "headline", "location"]].head())
users_df.to_csv("profiles.csv", index=False)
```

### 3. Scrape Post Comments & Extract Emails
```python
from staffspy import LinkedInAccount

account = LinkedInAccount(session_file="session.pkl", log_level=1)

post_ids = ["7182938491029384756"]

comments_df = account.scrape_comments(post_ids=post_ids)
print(comments_df[["name", "text", "emails", "created_at"]].head())
comments_df.to_csv("comments.csv", index=False)
```

### 4. Scrape Company Details
```python
from staffspy import LinkedInAccount

account = LinkedInAccount(session_file="session.pkl", log_level=1)

company_df = account.scrape_companies(company_names=["microsoft", "google"])
print(company_df.head())
company_df.to_csv("companies.csv", index=False)
```

### 5. Scrape 1st-Degree Connections & Contact Info
```python
from staffspy import LinkedInAccount

account = LinkedInAccount(session_file="session.pkl", log_level=1)

connections_df = account.scrape_connections(
    max_results=100,
    extra_profile_data=True
)

print(connections_df[["name", "connection_email", "connection_phone_numbers"]].head())
connections_df.to_csv("my_connections.csv", index=False)
```

---

## 📊 Extracted Data Fields

When `extra_profile_data=True` is enabled, StaffSpy extracts rich fields including:

| Field | Description |
|---|---|
| `name`, `first_name`, `last_name` | Full and parsed names |
| `headline`, `current_position` | Headline and current job title |
| `current_company`, `past_company_1`, `past_company_2` | Current and previous workplaces |
| `location` | Geographical location |
| `profile_link`, `profile_id`, `urn` | Public profile URL and LinkedIn identifiers |
| `emails_in_bio`, `potential_emails` | Extracted email addresses |
| `connection_email`, `connection_phone_numbers` | Contact info (for 1st-degree connections) |
| `top_skill_1`, `top_skill_2`, `top_skill_3` | Top endorsed skills |
| `school_1`, `school_2`, `estimated_age` | Educational background and estimated age |
| `followers`, `connections`, `mutuals` | Network statistics |
| `open_to_work`, `is_hiring`, `premium`, `creator` | Profile badges and status |
| `experiences`, `schools`, `skills`, `certifications` | Full JSON structured history lists |

---

## 👤 Author

- **EbxdMalix**
- GitHub: [@EbxdMalix](https://github.com/EbxdMalix)
- Repository: [https://github.com/EbxdMalix/StaffSpy-main.git](https://github.com/EbxdMalix/StaffSpy-main.git)

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2026 EbxdMalix
```

---

## ⚠️ Disclaimer

This tool is intended for educational, research, and personal data analysis purposes only. Please adhere to LinkedIn's Terms of Service and applicable privacy regulations (including GDPR and CCPA) when scraping and handling personal data.
