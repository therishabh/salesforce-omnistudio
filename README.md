Table of Content : 
- [OmniStudio](#omnistudio)
- [Data Mapper Extract](#data-mapper-extract)
- [Data Mapper Turbo Extract](#data-mapper-turbo-extract)
- [Data Mapper Load](#data-mapper-load)
- [Data Mapper](#data-mapper)


# OmniStudio

**OmniStudio** is a low-code Salesforce framework used to create guided experiences, integrate systems, transform data, and automate business processes without writing much Apex code.

It is commonly used in industries like:

* Telecom
* Healthcare
* Insurance
* Banking

Main purpose:

* Collect data from users
* Retrieve data from multiple systems
* Transform data
* Apply business rules
* Display data in UI
* Save/update records

---

## OmniStudio Architecture (High Level)

```text
User/UI Layer
     ↓
OmniScript
     ↓
Integration Procedures
     ↓
DataRaptors / External APIs
     ↓
Business Logic Layer
(FlexCards + Decision Matrix + Calculations)
     ↓
Salesforce Objects / External Systems
```

Let's understand each layer.

---

### 1. Presentation Layer / Digital Experience Layer (User Interface Layer)

Contains:

* OmniScript
* FlexCards

Purpose:

* Show data to users
* Collect user input
* Display results

---

#### OmniScript

OmniScript creates guided step-by-step flows.

Think of it like:

```text
Step 1 → Personal details
Step 2 → Address details
Step 3 → Product selection
Step 4 → Review
Step 5 → Submit
```

##### Example

Suppose customer wants to apply for health insurance.

OmniScript screens:

```text
Screen 1:
Name
DOB
Phone

↓

Screen 2:
Address

↓

Screen 3:
Choose Insurance Plan

↓

Screen 4:
Review and Submit
```

User fills data.

OmniScript sends data to Integration Procedure.

---

### 2. Service Layer

Contains:

* Integration Procedures
* DataRaptors

Purpose:

* Process data
* Call APIs
* Reduce server trips

---

#### Integration Procedure (IP)

IP is server-side processing logic.

Think:

```text
OmniScript
    ↓
Integration Procedure
```

IP can:

* Call DataRaptors
* Call APIs
* Apply conditions
* Merge data
* Transform responses

No UI exists here.

---

##### Example

User submits insurance application.

IP performs:

```text
Step 1:
Get customer information

↓

Step 2:
Check eligibility

↓

Step 3:
Get available plans

↓

Step 4:
Calculate premium

↓

Step 5:
Save record
```

---

#### Why use Integration Procedure?

Without IP:

```text
OmniScript
    ↓
Call DR
    ↓
Call API
    ↓
Call DR
    ↓
Call API
```

Many server trips.

With IP:

```text
OmniScript
     ↓
Single Integration Procedure
     ↓
Multiple operations internally
```

Benefits:

* Faster
* Less API calls
* Better performance

---

### 3. Data Layer

Contains:

* DataRaptor Extract
* DataRaptor Load
* DataRaptor Transform
* DataRaptor Turbo Extract

Purpose:

* Read and write data

---

#### DataRaptor Extract

Reads Salesforce records.

Example:

Get Account information.

```text
Account:

Name: ABC Ltd
Phone: 999999999
Industry: IT
```

Output:

```json
{
   "Name":"ABC Ltd",
   "Phone":"999999999",
   "Industry":"IT"
}
```

---

#### DataRaptor Load

Insert/update records.

Input:

```json
{
   "FirstName":"John",
   "LastName":"Doe"
}
```

Creates:

```text
Contact record
```

---

#### DataRaptor Transform

Changes data structure.

Input:

```json
{
   "fname":"John"
}
```

Output:

```json
{
   "FirstName":"John"
}
```

---

#### DataRaptor Turbo Extract

Fast data extraction.

Restrictions:

* Single object only
* No formulas
* No transformations

Use when speed matters.

---

Example:

```text
Fetch Account data quickly
```

---

For senior Salesforce interviews, remember this one-line summary:

> OmniStudio architecture follows a layered approach where OmniScript handles UI, Integration Procedures orchestrate services, DataRaptors manage data operations, and Decision/Calculation components implement business logic.


# Data Mapper Extract

**Purpose:**

Retrieve data from Salesforce with **complex logic and transformations**.

Supports:

- ✅ Multiple objects
- ✅ Relationships
- ✅ Formulas
- ✅ Output transformations
- ✅ Nested JSON
- ✅ Calculated fields

---

## Example Scenario

Suppose we have:

```text
Account
   |
   |-- Contact
           |
           |-- Cases
```


You want output like:

```json
{
    "owner": {
      "email_id": "therishabhagrawal+omni@gmail.com",
      "name": "Rishabh Agrawal"
    },
    "contact": {
      "phone": "(650) 867-3450",
      "name": "Edna Frank",
      "id": "003g7000008J9D0AAK",
      "email": "efrank@genepoint.com",
      "case": [
        {
          "number": "00001006",
          "subject": "Generator assembly instructions unclear",
          "type": "Other"
        },
        {
          "number": "00001016",
          "subject": "Maintenance guidelines for generator unclear",
          "type": "Other"
        }
      ]
    },
    "name": "GenePoint",
    "type": "Customer - Channel"
}
```

### Screenshots
<img width="2880" height="3400" alt="screencapture-dyninno6-dev-ed-develop-lightning-force-builder-omnistudio-omnistudioBuilder-app-2026-05-27-05_51_14" src="https://github.com/user-attachments/assets/225af7dc-bafd-4f26-9c3e-41bf296eef67" />

<img width="1436" height="754" alt="Screenshot 2026-05-27 at 5 30 19 AM" src="https://github.com/user-attachments/assets/396c55a8-a4c9-4df7-83e8-51710c5f4818" />

<img width="721" height="379" alt="Screenshot 2026-05-27 at 5 30 40 AM" src="https://github.com/user-attachments/assets/11cd8703-a298-4558-a62a-b42e780a83fb" />

---
# Data Mapper Turbo Extract

Purpose:

Retrieve data **extremely fast**.

Designed for:

* Simple queries
* Single object retrieval
* High performance

Supports:

- ✅ Single object
- ✅ Basic field retrieval

Does not support:

- ❌ Multiple objects
- ❌ Formulas
- ❌ Transformations
- ❌ Complex relationships

---

## Example

Suppose you only need Account details:

```text
Account:

Name = ABC Ltd
Phone = 999999999
Industry = IT
```

Output:

```json
{
   "Name":"ABC Ltd",
   "Phone":"999999999",
   "Industry":"IT"
}
```

---

## Internal Processing Difference

### Extract

```text
Request
   ↓
Query multiple objects
   ↓
Apply formulas
   ↓
Transform response
   ↓
Generate JSON
```

---

### Turbo Extract

```text
Request
   ↓
Single SOQL query
   ↓
Return data
```

Less processing = faster.

---

### Comparison Table

| Feature          | Extract | Turbo Extract |
| ---------------- | ------: | ------------: |
| Multiple Objects |       ✅ |             ❌ |
| Relationships    |       ✅ |       Limited |
| Formulas         |       ✅ |             ❌ |
| Transformations  |       ✅ |             ❌ |
| Nested JSON      |       ✅ |             ❌ |
| Performance      |  Medium |     Very Fast |
| Complex Logic    |       ✅ |             ❌ |

---

### Interview Question

**When should you use Turbo Extract instead of Extract?**

Answer:

> Use Turbo Extract when data retrieval is simple and involves a single object because it avoids transformation and additional processing, resulting in better performance. Use Extract when complex relationships, transformations, or nested JSON structures are required.

---
# Data Mapper Load

**Data Mapper Load (previously DataRaptor Load)** is used to **insert, update, or upsert data into Salesforce objects** from JSON input.

Think of it as:

```text
Input JSON
      ↓
Data Mapper Load
      ↓
Salesforce Record
```

Instead of writing Apex:

```apex
Account acc = new Account();
acc.Name='ABC Ltd';
insert acc;
```

You can do it using low-code configuration.

---

### What operations can Load perform?

- ✅ Insert records
- ✅ Update records
- ✅ Upsert records
- ✅ Multiple object record creation
- ✅ Parent-child record creation
- ✅ Field mapping
- ✅ Conditional mapping

---

### Interview Question

**Q: Why use Data Mapper Load instead of Apex DML?**

Answer:

> Data Mapper Load provides a low-code approach to create, update, or upsert Salesforce records without writing Apex. It reduces custom code, improves maintainability, and integrates easily with OmniScripts and Integration Procedures.

**Q: Can Data Mapper Load create parent-child records?**

Answer:

> Yes. It can create multiple related records and map IDs between parent and child objects.

One important distinction: **Load writes records to Salesforce**, while **Extract/Turbo Extract only read records.**

### Screenshots: 
<img width="1434" height="753" alt="Screenshot 2026-05-27 at 2 54 59 PM" src="https://github.com/user-attachments/assets/0ff8ba0f-380a-4983-a4d9-da3feaf2abee" />


##### Lookup Field Mapping 

<img width="1434" height="753" alt="Screenshot 2026-05-27 at 3 41 01 PM" src="https://github.com/user-attachments/assets/30bb1266-9ffb-45ca-9998-51f19b078a8e" />
<img width="1438" height="754" alt="Screenshot 2026-05-27 at 3 41 14 PM" src="https://github.com/user-attachments/assets/907f6d39-85d9-4995-a59b-400bb353216c" />

Ye images **Data Mapper Designer** ki configuration dikhati hain jahan JSON input ko Salesforce-style domain objects me map kiya ja raha hai. Is configuration me ek **Contact record create** ho raha hai aur us contact ko existing **Account** se connect kiya gaya hai.

###### Summary

* Input JSON me Contact related fields diye gaye hain jaise:

  * FirstName
  * LastName
  * Email
  * Phone
  * Department
  * MailingAddress
  * AccountName = `"Google"`

* Mapping screen me JSON fields ko `Contact` object ke fields ke saath map kiya gaya hai.

* Specially `AccountName` ko `Contact.AccountId` field ke saath map kiya gaya hai.

###### Important Configuration

`AccountName → Contact.AccountId`

Yahan direct AccountId pass nahi kiya gaya.
Instead, **Lookup configuration** use ki gayi hai:

* Lookup Object = `Account`
* Lookup Field = `Name`
* Lookup Requested Field = `Id`

###### Flow Samajh

Data Mapper pehle input JSON se `AccountName = Google` lega, phir:

1. `Account` object me search karega jahan `Name = Google`
2. Us account ka `Id` fetch karega
3. Wahi `Id` automatically `Contact.AccountId` me set karega
4. Final result: naya Contact create hoga aur existing Google account se linked hoga.

> JSON input ke basis par automated Contact creation ho raha hai, aur Account relationship manually Id diye bina sirf Account Name ke through lookup mechanism se establish ki gayi hai.

#### Parent Child record insert using data mapper

<img width="1438" height="789" alt="Screenshot 2026-05-28 at 3 08 26 PM" src="https://github.com/user-attachments/assets/94d75858-9075-4be3-b48b-9804f71a7b1c" />

Tumne 2 Load Objects banaye hain:

1. `Account`
2. `Contact`

Aur unke beech relation define kiya hai:
```text id="y8m7a6"
Contact.AccountId = Account.Id
```
---

##### Data Flow Samjho

Suppose input JSON ye hai:

```json id="c8dbyy"
{
  "account": {
    "name": "TCS"
  },
  "contacts": [
    {
      "firstName": "Rishabh",
      "lastName": "Sharma",
      "email": "r@test.com"
    },
    {
      "firstName": "Aman",
      "lastName": "Verma",
      "email": "a@test.com"
    }
  ]
}
```

Ab Data Mapper internally kya karega:

---

##### STEP 1 → Parent Insert

Sabse pehle:

```text id="ijz83j"
Account
```

insert hoga.

Generated Id:

```text id="jqc1n0"
001XXXXXXXXXXXX
```

---

##### STEP 2 → Relationship Memory

Ab Data Mapper us inserted Account ka Id memory me store karta hai.

```text id="u8p4rz"
Account.Id
```

---

##### STEP 3 → Child Insert

Ab Contact object insert hoga.

Har contact ke liye automatically:

```text id="i1ckd9"
Contact.AccountId = inserted Account.Id
```

set kar diya jayega.

Example:

```json id="9dkb0l"
{
   "FirstName":"Rishabh",
   "LastName":"Sharma",
   "AccountId":"001XXXXXXXXXXXX"
}
```

---

##### Tumhare Screenshot Ka Exact Meaning

Ye jo line hai:

```text id="6gbh9z"
Contact AccountId = 1 Account Id
```

iska matlab:

| Left Side         | Meaning                                |
| ----------------- | -------------------------------------- |
| Contact.AccountId | Child lookup field                     |
| 1 Account         | Parent object reference                |
| Id                | Parent inserted record ka generated Id |

Matlab:

```text id="j62qcc"
Child.AccountId = Parent.Id
```

---

##### Real Execution Order

Internally execution aise hota hai:

```text id="8b5j8e"
1. Insert Account
2. Get Account.Id
3. Insert Contact 1 with AccountId
4. Insert Contact 2 with AccountId
5. Insert Contact 3 with AccountId
```

---
