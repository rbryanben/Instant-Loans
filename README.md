# 🤖 1. Instant-Loans
AI Instant Loans is a digital lending solution that uses transaction analysis 📊 and AI-based salary classification 🤖💰 to determine a customer's eligibility for an instant loan ⚡.

The solution analyses the customer's historical account transactions 🧾 to determine whether a regular salary is being received. The identified salary information is then used to determine which loan policy 📋 should be applied.

If a salary cannot be reliably identified 🔍, the customer is not necessarily rejected ❌. Instead, the system evaluates the customer against alternative loan policies, such as the Recurrence Policy 🔄, allowing eligible customers to access lending based on other patterns in their account activity.

The solution is accessed through the USSD channel 📱, providing customers with a simple and accessible way to apply for loans. The Virtual Service Center (VSC) 🎧 also plays an important role by providing support and assistance to customers who encounter issues or require help during the loan journey.

## 🚀 2. High-Level Loan Decision Flow
The AI Instant Loans journey is illustrated below.

```mermaid
flowchart LR
    A[🏦 Select Account] --> B[💰 Display Salary]
    B --> C{🔍 Existing Loan?}

    C -->|Yes| D[💳 Existing Loan]
    C -->|No| E[⚡ Calculate Eligibility]

    D --> F[👀 View Loan]
    D --> G[🏠 Home]

    E --> H{🎯 Eligible?}
    H -->|No| I[🆘 Assistance]
    H -->|Yes| J{🧠 Determine Policy}

    J -->|Salary Based| K[💰 Salary Based]
    J -->|Recurrence| L[🔄 Recurrence]

    K --> M[🆕 New Loan]
    K --> N[📈 Request Increase]

    L --> O[🆕 New Loan]
    L --> P[💼 I Have a Salary]
```

## 💳 3. Loan Policies

The AI Instant Loans solution uses loan policies to determine the appropriate lending option for each customer.

Once the customer's eligibility has been calculated, the system determines which policy should be applied based on the information available for the customer's account.

Currently, two primary loan policies are supported:

### 💰 3.1 Salary Based Policy

The Salary Based Policy is applied when the customer's salary has been successfully identified through the AI transaction classification process and the customer meets the applicable eligibility criteria.

The customer is presented with the following options:

- 🆕 New Loan — Apply for a new loan.
- 📈 Request Increase — Request an increase where applicable.

The salary identified by the AI is used as part of the loan decisioning process and remains visible to the customer throughout the loan journey.

### 🔄 3.2 Recurrence Policy

The Recurrence Policy provides an alternative lending path when a reliable salary cannot be identified.

Instead of automatically rejecting the customer, the system evaluates the customer's account activity against recurrence-based eligibility criteria.

The customer is presented with:

- 🆕 New Loan — Apply for a new loan based on the Recurrence Policy.
- 💼 I Have a Salary — Indicate that they receive a salary, allowing the customer to proceed through the appropriate salary-related process.

## 4. 🔌 API Requests

### 4.1 ⚡ Calculate Eligibility

The `calculate-eligible` endpoint is called after the customer selects an account and the system determines that the customer does not have an existing loan.

```bash
curl --location 'http://10.50.30.217:9091/api/v1/loans/calculate-eligible' \
--header 'X-Request-Id: <unique-request-id>' \
--header 'X-Api-Key: <api-key>' \
--header 'Content-Type: application/json' \
--data '{
    "account_number": "500007903379"
}'
```

### 4.2 💰 Get Salary

The `get-salary` endpoint retrieves the salary identified for the selected account.

```bash
curl --location 'http://10.50.30.217:9091/api/v1/accounts/get-salary' \
--header 'X-Request-Id: <unique-request-id>' \
--header 'X-Api-Key: <api-key>' \
--header 'Content-Type: application/json' \
--data '{
    "account_number": "500007903379"
}'
```

### 4.3 🆕 New Loan

The `new-loan` endpoint is called when an eligible customer selects **New Loan**.

```bash
curl --location 'http://10.50.30.217:9091/api/v1/loans/new-loan' \
--header 'X-Request-Id: <unique-request-id>' \
--header 'X-Api-Key: <api-key>' \
--header 'Content-Type: application/json' \
--data '{
    "amount": 50,
    "account_number": "500007903387"
}'
```

### 4.4 👀 View Loan

The `view-loan` endpoint is called when a customer with an existing loan selects **View Loan**.

```bash
curl --location 'http://10.50.30.217:9091/api/v1/loans/view-loan' \
--header 'X-Request-Id: <unique-request-id>' \
--header 'X-Api-Key: <api-key>' \
--header 'Content-Type: application/json' \
--data '{
    "account_number": "500007903379"
}'
```

### 🔐 Common Headers

All AI Instant Loans API requests require the following headers:

| Header         | Description                                 |
| -------------- | ------------------------------------------- |
| `X-Request-Id` | Unique identifier used to trace the request |
| `X-Api-Key`    | API authentication key                      |
| `Content-Type` | Specifies that the request body is JSON     |

