# KenGen Liaison System Flowcharts

This document outlines the operational and technical flows of the KenGen Liaison system, bridging the Community Portal and the Admin Dashboard.

---

## 1. System-Wide Overview
This flowchart demonstrates the high-level interaction between community members and the Liaison team.

```mermaid
graph TD
    User((Community Member))
    Staff((Liaison Officer))

    subgraph Portal [Public Community Portal]
        A[Submit Request / Complaint]
        B[Check Request Status]
    end

    subgraph Dashboard [KenGen Liaison Dashboard]
        C[Review Pending Queue]
        D[Assess & Update Status]
        E[Generate Reports]
    end

    User -->|Submits| A
    User -->|Searches ID| B
    
    A -->|Saves to| Database[(Central Database)]
    Database -->|Populates| C
    
    Staff -->|Logs into| Dashboard
    C --> D
    D -->|Updates| Database
    D -->|Triggers| SMS[Automated SMS Alert]
    
    SMS -->|Notify| User
    Database -.->|Results| B
```

---

## 2. Community Submission Flow
Detailed logic for how the website handles different types of requests (Project vs. Water vs. Complaint).

```mermaid
graph TD
    Start([Landing Page]) --> Action{User Intent}
    
    Action -->|Track| Status[Enter Request ID]
    Action -->|Submit| Step1[Step 1: Choose Type]
    
    Step1 --> Type{Select Category}
    
    Type -->|Church / Community| FormA[Project Name, Budget, Location]
    Type -->|Water / Maji| FormB[Liters, Transport, Location]
    Type -->|Complaint| FormC[Category, Description, Urgency]
    
    FormA --> Step2[Step 2: Contact Info]
    FormB --> Step2
    FormC --> Step2
    
    Step2 --> Submit[Submit to System]
    Submit --> Success[Generate ID & Send SMS]
```

---

## 3. Liaison Processing Workflow (Internal)
The steps taken by KenGen staff after receiving a request, derived from the organizational workflow.

```mermaid
flowchart LR
    Rec[Log & Acknowledge] --> Screen[Screen & Categorize]
    
    Screen --> Assess{Field Assessment Needed?}
    
    Assess -->|Yes| Visit[Site Visit & Committee Review]
    Assess -->|No| Desk[Desktop Feasibility Check]
    
    Visit --> Report[Liaison Report]
    Desk --> Report
    
    Report --> Mgmt{Management Approval}
    
    Mgmt -->|Approved| Exec[Assign Responsibility & Execute]
    Mgmt -->|Rejected| Deny[Reject & Archive]
    
    Exec --> Notify[Final Success Update]
    Deny --> Notify
```

---

## 4. Notification & Status Loop
How the system ensures transparency and keeps the community informed.

```mermaid
graph TD
    Update[Liaison Officer Updates Status] --> DB[(Database Update)]
    DB --> Trigger{Trigger Event}
    
    Trigger -->|Status Change| SMS[Send Automated SMS]
    Trigger -->|New Request| Acknowledgement[Send Receipt SMS]
    
    SMS --> Phone[User Receives Message]
    DB -.-> Search[Update Website Status History]
    
    User[Resident] -->|Input ID| Search
    Search -->|Display| UI[Show Progress: Pending/Review/Complete]
```
