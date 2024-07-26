# Goal:
To evaluate the effectiveness of the bot mitigation system in detecting malicious agent behavior within our current product suite.

## What is a Malicious Agent?
Malicious agents are users exhibiting behaviors that can harm our services. These include:
- Users leveraging automation tools, such as Cypress or Selenium.
- Users exploiting our payment systems (e.g., initiating multiple unfulfilled deposit requests, causing resource exhaustion and potential service denial to legitimate customers).
## Userbase
Our primary market is Russia. Therefore:
- IP reputation-based checks are challenging due to prevalent VPN, TOR, and proxy usage among our users.
- Accurate detection of suspicious traffic is complex but essential.
## Methodology

1. **Detection Mechanism**: Integrate the validation score or response from the casino frontend.
2. **Data Attachment**: Link the response to sessions in Google Analytics and Microsoft Clarity.
3. **Reporting**: Develop a BI report illustrating two key metrics:
	- The percentage of paying users identified as malicious agents and their corresponding financial impact.
    - The accuracy of the system in identifying actual malicious agents.

The decision to implement the bot mitigation system in our backend will be based on these metrics.

## Test Integration Schemas
### Google Tag Manager (GTM) Integration
1. **Script Installation**: Obtain the detection script from the solution provider and add it to our GTM container.
2. **Tag Execution**: Activate this tag on page load or during registration attempts (as per the solution provider's recommendation).
3. **Data Capture**: Capture and send relevant information to Google Analytics.
#### GTM Fields to Send:
- User email
- User ID
#### Expected Outcome:
- Transfer GA data to BigQuery (BQ) for merging with backend data to evaluate the mitigation system’s effectiveness.
- Generate a comprehensive UserID-based report.
### CSV Data Exchange.

1. **Data Export**: Prepare dataset customer data in a form of CSV file containing:
    - User ID
    - Email
    - Registration IP address
    - Phone number (if available)
2. **Data Submission**: Provide this CSV to the solution provider for analysis.
3. **Blind Testing**: Conduct blind tests by comparing the solution provider’s results with actual user behavior based on the User ID.
#### Expected Outcome:
- Generate a comprehensive UserID-based report.