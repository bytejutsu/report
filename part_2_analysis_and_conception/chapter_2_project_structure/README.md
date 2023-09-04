# Chapter 2: Project Structure

## Invoicing App

**Use-Case Diagram:**

![Use-Case Diagram for Invoicing App](./invoicing_app_use_case.png)

**Class Diagram:**

![Class Diagram for Invoicing App](./invoicing_app_class_diagram.svg)

**State Diagram:**

- User Profile Creation

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> SignUp: User initiates sign up
    SignUp --> AddDetails: User adds essential details
    AddDetails --> UploadLogo: User uploads business logo
    UploadLogo --> ProfileCreated: Profile successfully created
    ProfileCreated --> [*]
```

- Invoice Generation

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> SelectInvoiceDetails: User selects invoice details
    SelectInvoiceDetails --> GenerateInvoice: User initiates invoice generation
    GenerateInvoice --> BackgroundProcessing: Invoice generation in background
    BackgroundProcessing --> InvoiceGenerated: Invoice successfully generated
    InvoiceGenerated --> [*]
```

- VAT Calculation

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> EnterDetails: User enters invoice details
    EnterDetails --> CalculateVAT: System calculates VAT
    CalculateVAT --> DisplayVAT: VAT displayed to user
    DisplayVAT --> [*]
```

- Admin Monitoring

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> AdminLogin: Admin logs in
    AdminLogin --> ViewUsers: Admin views user activity
    ViewUsers --> AdminLogout: Admin logs out
    AdminLogout --> [*]
```

---

- App Access without Registration

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> AccessApp: User accesses the app
    AccessApp --> UseBasicFeatures: User uses basic features
    UseBasicFeatures --> Exit: User exits the app
    Exit --> [*]
```

**Sprint 1 Backlog for the Invoicing App:**

| Priority | User Story | Acceptance Criteria                                                           | Estimated Time |
|----------|------------|-------------------------------------------------------------------------------|----------------|
| 1 | As a user, I want to create a profile so that I can personalize my invoicing experience. | User can sign up, add essential details, and upload a business logo.          | 6 days         |
| 2 | As a user, I want to generate an invoice without any delays. | Invoices are generated in the background, ensuring the UI remains responsive. | 6 days         |
| 3 | As a user, I want to see the calculated VAT automatically. | VAT is automatically calculated based on entered details.                     | 2 days         |

**Total Estimated Time for Sprint 1:** 14 days

---

**Sprint 2 Backlog for the Invoicing App:**

| Priority | User Story | Acceptance Criteria                                                           | Estimated Time |
|----------|------------|-------------------------------------------------------------------------------|----------------|
| 4 | As an admin, I want to monitor user activity. | Admin can log in and view users.                                              | 7 days         |
| 5 | As a user, I want to access the app without mandatory registration. | Users can access basic features without obligatory registration.              | 7 days         |

**Total Estimated Time for Sprint 2:** 14 days

---

## Messaging App:

**Use-Case Diagram:**

![Use-Case Diagram for Messaging App](./messaging_app_use_case.svg)

**Class Diagram:**

![Class Diagram for Messaging App](./messaging_app_class_diagram.svg)

**State Diagram:**

- User Profile Creation for Messaging

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> SignUp: User initiates sign up
    SignUp --> AddDetails: User adds essential details (name, profile picture)
    AddDetails --> ProfileCreated: Profile successfully created
    ProfileCreated --> [*]
```

- Sending and Receiving Messages

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> SelectContact: User selects a contact
    SelectContact --> TypeMessage: User types a message
    TypeMessage --> SendMessage: User sends the message
    SendMessage --> MessageDelivered: Message delivered to recipient
    MessageDelivered --> ReceiveMessage: Recipient receives the message
    ReceiveMessage --> [*]
```

- Viewing Online Status of Contacts

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> OpenApp: User opens the app
    OpenApp --> ViewContacts: User views list of contacts
    ViewContacts --> CheckStatus: System checks online status
    CheckStatus --> DisplayStatus: Display online/offline status
    DisplayStatus --> [*]
```

- 4. Ensuring Message Privacy and Security

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> TypeMessage: User types a message
    TypeMessage --> EncryptMessage: System encrypts the message
    EncryptMessage --> SendMessage: User sends the encrypted message
    SendMessage --> DecryptMessage: Recipient decrypts the message
    DecryptMessage --> ReadMessage: Recipient reads the message
    ReadMessage --> [*]
```

- Finding and Starting Conversations with Other Users

```mermaid
stateDiagram-v2
    [*] --> Start
    Start --> SearchUser: User searches for another user
    SearchUser --> SelectUser: User selects a user from search results
    SelectUser --> InitiateConversation: User starts a conversation
    InitiateConversation --> Chat: User chats with the selected user
    Chat --> [*]
```

**Sprint 1 Backlog for the Messaging App:**

| Priority | User Story                                                     | Acceptance Criteria                                                       | Estimated Time |
|----------|----------------------------------------------------------------|---------------------------------------------------------------------------|----------------|
| 1 | As a user, I want to create a profile to start messaging.      | User can sign up and add essential details like name and profile picture. | 6 days         |
| 2 | As a user, I want to send and receive messages in real-time.   | Messages are sent and received instantly using WebSockets.                | 4 days         |
| 3 | As a user, I want to know the online status of my contacts.    | Users can see real-time status (online/offline) of their contacts.        | 4 days         |

**Total Estimated Time for Sprint 1:** 14 days

---

**Sprint 2 Backlog for the Messaging App:**

| Priority | User Story                                                     | Acceptance Criteria                                    | Estimated Time |
|----------|----------------------------------------------------------------|--------------------------------------------------------|----------------|
| 4 | As a user, I want my messages to be private and secure.        | Messaging channels are secure ensuring privacy.        | 10 days        |
| 5 | As a user, I want to find and start conversations with other users. | Users can find other users and initiate conversations. | 4 days         |

**Total Estimated Time for Sprint 2:** 14 days

