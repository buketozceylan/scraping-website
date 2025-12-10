# Automated University AVESIS System Scanner

> **An automated ETL pipeline that scrapes university data, sanitizes domain information, and verifies the availability of Academic Data Management Systems (AVESIS) across higher education institutions in Turkey.**

---

## Project Overview

This project is designed to automate the discovery and validation of academic sub-systems. It creates a comprehensive dataset of universities in Turkey by scraping official government sources (YÖK Atlas), cleans and normalizes the data, and performs network checks to identify active **AVESIS** portals. 

This tool serves as a practical implementation of **Web Scraping**, **Data Cleaning**, and **Automated Network Validation**, simulating a real-world data engineering task where ensuring data quality and endpoint availability is crucial.

## Key Features

* **Automated Data Extraction:** Scrapes dynamic content from the Council of Higher Education (YÖK) Atlas platform using `BeautifulSoup`.
* **Data Cleaning & Normalization:** Processes raw URLs using `Pandas` to ensure standard domain formatting (stripping protocols, handling trailing slashes).
* **Endpoint Validation:** Constructs hypothetical AVESIS subdomains and validates their status via HTTP requests, handling SSL handshake errors and connection timeouts robustly.
* **Reporting:** Exports the validation results into structured CSV formats for further analysis.

## Technologies & Tools

* **Language:** Python 3.x
* **Data Manipulation:** Pandas
* **Web Scraping:** BeautifulSoup4
* **HTTP Requests:** Requests
* **Environment:** Jupyter Notebook

## Repository Structure

```text
├── Scraping_cleaning.ipynb  # Phase 1: Scraping university list and cleaning URLs
├── avesisCheck.ipynb        # Phase 2: Validating AVESIS subdomains via HTTP requests
├── cleaned.csv              # Output: Processed list of university domains
├── have_avesis.csv          # Final Output: Status report (Working/Not Working)
└── out.csv                  # Intermediate raw data
```
## How It Works

### 1. Extraction (Scraping)
The script `Scraping_cleaning.ipynb` connects to the YÖK Atlas directory. It parses the HTML structure to extract university names and their official website links, overcoming basic SSL verification hurdles during the request phase.

### 2. Transformation (Cleaning)
Raw URLs often contain inconsistencies (e.g., `http://www.`, `https://`, trailing `/`). The pipeline normalizes these into a clean domain format (e.g., `university.edu.tr`) to prepare them for subdomain construction.

### 3. Validation (Network Check)
The `avesisCheck.ipynb` notebook iterates through the cleaned domains. It constructs the target URL pattern:
`https://avesis.[domain]/proxy/search/_search`

It then sends a `HEAD` or `GET` request to verify the service status. The script includes exception handling for:
* `NameResolutionError` (DNS issues)
* `SSLCertVerificationError` (Certificate mismatches)
* `ConnectionTimeout`

### 4. Load (Result Generation)
The final results are tagged as `working` or `not working` and saved to `have_avesis.csv`, providing a clear dataset of active academic portals.

## 📊 Sample Results

| University Name | Domain | Target Endpoint | Status |
|-----------------|--------|-----------------|--------|
| Abdullah Gül University | agu.edu.tr | avesis.agu.edu.tr/... | **Working** |
| Acibadem University | acibadem.edu.tr | avesis.acibadem.edu.tr/... | **Working** |
| X University | x.edu.tr | avesis.x.edu.tr/... | Not Working |

## Future Improvements

* **Asynchronous Requests:** Implementing `aiohttp` or `asyncio` to perform network validations concurrently, significantly reducing runtime.
* **CI/CD Integration:** Automating the script to run weekly via GitHub Actions to keep the active list up-to-date.
* **Dashboarding:** Visualizing the ratio of active systems using Streamlit or PowerBI.

## 🤝 Contact

Feel free to reach out if you have any questions or suggestions about this project!

* **LinkedIn:** [Your LinkedIn Profile](https://www.linkedin.com/in/buketozceylan/)
* **GitHub:** [buketozceylan](https://github.com/buketozceylan)

---
*This project is for educational and portfolio purposes.*
