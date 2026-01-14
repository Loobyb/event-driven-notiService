# event-driven-notiService
Notification Service for Payments and Inquiries

A Billing App that notifies the user when events happen such as a payment, a monthly invoice, or even a missed payment

##Features
- Event Driven Architecture
- Notification Channel: send notification via Email or SMS
- Template-based message: Reusable message template with variable
- Idempotency & deduping: No duplicate notifications when same event is processed
- Retry: Automatic retry for transient failures and dead-letter queue for repeated failures
- Aduit trail: Stores delivery attempts, provider responses, timestamps (QUEUED, SENT, FAILED)
- REST API: End point to publish events and view notifications
- Configurable rules: Route different event types(PaymentFailed -> SMS + Email)
- Observed: Structured logs + metrics-ready design

 ## Tech Stack
 Backend:
  -  Java and Spring Boot (REST API, Event publish, dependency injection)
  -  Spring Web( HTTP endpoint)
  -  Spring Data JPA (persistence layer)

Messaging/Event Bus:
- AWS SQS SNS
-   SQS for async processing
-   SNS for fan-out/ multiple subs

Database
 - MySQL (notification history + audit logs)

Frontend
 - Django( simple dashboard)

DevOps/ Tooling
- Docker
- Git
- GItHUb

Architecture: 
Produce publish an event (FailPayment, InvoiceCreated) 
Event goes to a Queue/Topic
Consumer sees event:
  - validate
  - check idempotency (duplicates consistently)
  - routing rules( if/then instructions for info being sent
  - creates audit record

Data Model 
notification
  -id(UUID)
  -eventid(string/UUID)
  -eventType(string)
  channel(EMAIL,SMS)
  recipient(email, phone)
  status(QUEUED, SENT, FAILED)
  attemptCount(int)
  lastError(text)
  createdAt,updatedAt

  notifcation_attempt
  - id
  - notificaitonId
  - providerREsponse
  - success
  - attemptedAt
