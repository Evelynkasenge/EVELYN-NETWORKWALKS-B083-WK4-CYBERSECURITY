# Mediroza General Hospital --- Week 4 Penetration Testing

## Project Overview

This repository documents my **Week 4 penetration-testing assessment**
of the Mediroza General Hospital lab environment.

The assessment followed an authorised security-testing workflow
covering:

-   Reconnaissance
-   Username enumeration
-   SQL injection testing
-   Authentication bypass
-   Patient PDF access
-   PDF password recovery
-   PDF metadata analysis
-   Discovery of an exposed backup directory
-   Database backup retrieval
-   Review of exposed staff and shareholder data
-   Risk identification and remediation recommendations

> **Authorisation / Responsible Use:** This project was completed as an
> authorised educational security assessment. The techniques documented
> here must not be used against systems without explicit permission from
> the system owner.

## Key Findings

  -----------------------------------------------------------------------------------------
  \#                Finding           Location                            Risk
  ----------------- ----------------- ----------------------------------- -----------------
  1                 Username          `patient/login.php`                 Medium
                    enumeration                                           

  2                 SQL injection     `patient/login.php`                 Critical
                    login bypass                                          

  3                 Patient PDFs      `patient/reports/`                  High
                    accessible after                                      
                    login bypass                                          

  4                 Weak PDF          `patient_report_*.pdf`              High
                    passwords                                             

  5                 Sensitive PDF     `patient_report_3.pdf`              Medium
                    metadata                                              

  6                 Exposed backup    `old/`                              Critical
                    directory /                                           
                    directory listing                                     

  7                 Confidential      `old/mediroza_db_backup_2019.sql`   Critical
                    staff and                                             
                    shareholder data                                      
                    exposed                                               
  -----------------------------------------------------------------------------------------

## Assessment Chain

``` text
Reconnaissance
      ↓
robots.txt discovery
      ↓
Patient login page
      ↓
Username enumeration
      ↓
SQL injection
      ↓
Authentication bypass
      ↓
Patient PDF reports accessed
      ↓
PDF password recovery
      ↓
Report 3 metadata analysis
      ↓
/old/ directory discovered
      ↓
Database backup exposed
      ↓
Confidential database information identified
```

## Evidence Highlights

### 1. Username Enumeration

The login application returned different responses for:

-   An invalid username: `Username not found`
-   An existing username with an incorrect password:
    `Incorrect password`

This demonstrated that valid usernames could be enumerated.

### 2. SQL Injection

A single quote in the username input produced a MySQL syntax error. The
authorised lab test then demonstrated that the authentication control
could be bypassed.

### 3. Patient Reports

After the authorised authentication bypass, three patient PDF reports
were accessible from the portal.

### 4. PDF Password Recovery

The assessment demonstrated that the PDF passwords were susceptible to
wordlist-based recovery.

The recovered passwords are **not reproduced in this public repository**
to avoid publishing credential material.

### 5. Metadata Disclosure

Report 3 contained metadata that identified an author value and included
an internal comment pointing toward the `/old/` backup location.

### 6. Exposed Backup Directory

Directory listing was enabled on `/old/`, exposing the database backup
file:

``` text
mediroza_db_backup_2019.sql
```

The backup was successfully retrieved during the authorised assessment.

### 7. Database Exposure

The backup contained staff and shareholder records. The public
repository should **not** contain raw personal information, salaries,
national ID information, email addresses, telephone numbers, or other
sensitive records.

Any screenshots or extracted database evidence published publicly should
therefore be redacted.

## Evidence

The evidence archive contains screenshots documenting the assessment,
including:

-   Login testing
-   SQL injection testing
-   Patient report access
-   PDF hash extraction
-   Wordlist password recovery
-   Report 3 cracking
-   PDF metadata inspection
-   `/old/` directory listing
-   Database backup retrieval

For a public GitHub repository, screenshots should be reviewed and
redacted before publication if they contain passwords, personal
information, email addresses, telephone numbers, national ID
information, or other confidential data.

## Remediation Recommendations

### Authentication

Use the same generic authentication message for invalid usernames and
incorrect passwords.

### SQL Injection

Use parameterised queries or prepared statements. Never build SQL
statements directly from untrusted user input.

### Patient Documents

Store patient reports outside the public web root or enforce strong
server-side access controls.

### PDF Encryption

Use strong, unique passwords rather than common or predictable
passwords.

### Metadata

Remove unnecessary metadata from patient documents before distribution.

### Directory Listing

Disable directory listing on web directories and review public
directories for forgotten files.

### Backup Security

Store database backups outside publicly accessible web directories and
protect them with appropriate access controls.

## Skills Demonstrated

This assessment demonstrates practical experience with:

-   Web reconnaissance
-   Web application authentication testing
-   SQL injection identification
-   Password auditing
-   Wordlist-based password recovery
-   PDF security analysis
-   Metadata analysis
-   Sensitive file discovery
-   Database backup exposure analysis
-   Evidence collection
-   Risk assessment
-   Security remediation

## Repository Structure

A recommended public repository structure is:

``` text
.
├── README.md
├── GitHub_Post.md
├── evidence/
│   ├── login-testing/
│   ├── sql-injection/
│   ├── pdf-password-recovery/
│   ├── metadata/
│   └── backup-discovery/
└── report/
    └── Week4_Mediroza_Final_Penetration_Test_Report.pdf
```

Do **not** upload:

-   Raw database backups
-   Patient reports
-   Recovered passwords
-   National ID information
-   Personal contact information
-   Unredacted screenshots containing confidential data
-   Credentials or authentication secrets

## Final Outcome

The assessment demonstrated how several individual weaknesses could be
chained together to reach increasingly sensitive information. The main
lesson from the exercise was that secure authentication, safe database
queries, controlled document access, strong passwords, clean metadata,
and secure backup storage all contribute to preventing a single weakness
from becoming a larger data-exposure incident.

------------------------------------------------------------------------

**Project:** Mediroza General Hospital --- Week 4\
**Type:** Authorised Educational Penetration Testing Assessment\
**Status:** Completed
