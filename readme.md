# GitSetGo Airline Booking System
# Technical Specifications Document

## 1. Title Page
- **Project Name**: Parallox Airline Booking System
- **Version**: 1.0
- **Date**: November 21, 2025
- **Author(s)**: 

  1. Rick San Mateo
  2. Leigh Tamayo
  3. Roldan Estobio
  4. Jan Michael Agapay
  5. Janel Bianca San Jose
  6. Pedro Jose Siapuatco
  7. Jomari Angel Jacinto

## 2. Table of Contents
1. [Introduction](#3-introduction)
2. [Overall Description](#4-overall-description)
3. [Visual Mockup Reference](#5-visual-mockup-reference)
4. [Features](#6-features)
5. [Functional Requirements](#7-functional-requirements)
6. [Non-Functional Requirements](#8-non-functional-requirements)
7. [Data Requirements](#9-data-requirements)
8. [External Interface Requirements](#10-external-interface-requirements)
9. [Glossary](#11-glossary)
10. [Appendices](#12-appendices)

## 3. Introduction
### 3.1 Purpose:
  
The fundamental purpose of this Technical Specifications Document (TSD) is to establish the definitive technical blueprint for the Parallox Airline Booking and Management System (P-ABMS). This document specifies the complete functional and non-functional requirements to guide the development team. The P-ABMS must be implemented on the specified MEAN/MERN stack (MongoDB, Express.js, Node.js, and Vue/React) to create a modern, scalable, and efficient platform supporting Parallox Airline's strategy for direct customer distribution and operational efficiency.

### 3.2 Scope:
  
The scope of the P-ABMS is defined by the Customer-Facing Booking Portal and the Internal Administration Platform, with all data operations mapped directly to the provided Entity-Relationship Diagram (ERD).

**In-Scope Functionality (WILL DO)**:
**1. Customer Transactional Core:** End-to-end management of the booking lifecycle, including flight search based on Airports.iataCode and date. The system handles the creation of the Booking document, linking it to Customers, FlightSchedules, and FareClasses. It includes processing Payments via an external gateway and managing AncillaryServices purchases.
**2. Inventory Management:** Real-time synchronization and tracking of seat availability for every flight instance via the FlightSeats collection, with strict consistency checks on the isAvailable status.
**3. Administrative CRUD:** Provision of an interface for authorized users to perform CRUD (Create, Read, Update, Delete) operations on core master data collections, including FlightSchedules, Aircrafts, Airports, and FareClasses.

**Out-of-Scope Functionality (WILL NOT DO)**:
**1. Legacy Systems Integration:** The system will not integrate with external Global Distribution Systems (GDS) or Departure Control Systems (DCS).
**2. Operational Logging:** No storage or management of operational data such as crew duty rosters, aircraft maintenance, or fuel planning logs.
**3. Financial & Accounting:** No complex financial modeling, revenue management, or general ledger (GL) integration. The Payments collection is strictly for transaction audit reference (storing the external transactionRef).

- **Definitions, Acronyms, and Abbreviations**: List any terms or acronyms used.
- **References**: List any documents or sources referenced.

### 3.3 Definitions, Acronyms, and Abbreviations:

| Term/Acronym | Definition |
|--------------|------------|
| P-ABMS | Parallox Airline Booking and Management System. |
| TSD | Technical Specifications Document. |
| MEAN/MERN Stack | The required architecture: MongoDB (Database), Express.js (Backend Framework), Node.js (Runtime Environment), and Vue/React (Frontend Frameworks). |
| PNR | **Passenger Name Record** — the unique booking identifier and itinerary reference (represented in the system by the *Bookings* collection). |
| NoSQL | Non-relational database architecture (MongoDB), known for its document-based flexible schema and horizontal scalability. |
| CRUD | Create, Read, Update, Delete — the fundamental database operations. |
| IATA Code | International Air Transport Association three-letter airport code. |

### 3.4 References:

- Airline Booking System Entity-Relationship Diagram (ERD).drawio.png
- External Payment Gateway API Specification v2.1.
- OWASP Top 10 Security Guidelines.


## 4. Overall Description
### 4.1 Product Perspective:
The **Parallox Airline Booking and Management System (P-ABMS)** is architected as a modern, three-tier, cloud-native application built using the **MEAN/MERN stack**. It serves as the airline’s direct sales channel and core inventory data repository.

**System Architecture Overview**

**1. Presentation Tier (Frontend)**
- Built with **Vue.js** or **React**
- Runs in the user's browser
- Responsible for rendering the dynamic **User Interface (UI)** and handling user interactions

**2. Application Tier (Backend)**
- Implemented using **Node.js** with **Express.js**
- Acts as the central **REST API**
- Implements all **business logic**, request validation, authentication, and security controls

**3. Data Tier (Database)**  
- Powered by **MongoDB**
- Stores all application data using a **document-based model**
- Utilizes document references to maintain the relationships defined in the **Entity Relationship Diagram (ERD)**

### 4.2 Product Functions:
The **P-ABMS** provides comprehensive functionality, segmented into four key technical areas:

**1. Customer Transactional Core**
- Supports end-to-end management of the booking lifecycle.
- Handles flight search and real-time seat inventory display by querying the **FlightSeats** and **FlightSchedules** collections.
- Facilitates secure payment integration through the dedicated API service layer.
- Generates and issues the **PNR (Booking Reference)** once a booking is successfully confirmed.

**2. Data Persistence Layer**
- Manages and optimizes queries within the **MongoDB** document database.
- Stores, updates, and retrieves core data collections such as **FlightSchedules**, **Aircrafts** specifications, **Customers** profiles, and **Bookings**.
- Ensures data integrity across collections using cross-document references and transactional logic when applicable.

**3. Service Integration**
- Implements the **Node.js/Express.js** API for secure, high-throughput communication.
- Integrates with the external **Payment Gateway** to process financial transactions.
- Connects with an **Email Service Provider** to deliver customer notifications, such as booking confirmations.

**4. Administrative CRUD**
- Provides an interface for authorized Administrator users.
- Supports Create, Read, Update, and Delete (CRUD) operations for master data collections, including **FlightSchedules**, **Airports**, **FareClasses**, and **AncillaryServices**.

### 4.3 User Classes and Characteristics:
The system supports two main classes of users, with access rights enforced by the Express.js backend.

| **User Class** | **Characteristics** | **Authorization Level** |
|----------------|----------------------|---------------------------|
| **Customer** | General public, focused on efficient self-service. Requires authentication to manage their own bookings and profile information. | **Limited:** Access restricted only to their own data (`customerId`). |
| **Administrator (Admin)** | Airline staff with operational responsibility, requiring full data visibility and modification capabilities. Authenticated through the internal Admin dashboard. | **High:** Full access to Admin-specific API endpoints. |

### 4.4 Operating Environment:
The operating environment is defined by the following required technologies:

| **Component** | **Technology** | **Deployment Environment** | **Purpose** |
|---------------|----------------|-----------------------------|-------------|
| **Runtime** | Node.js (LTS) & Express.js | Cloud Hosting (generic) using container orchestration (e.g., Docker/Kubernetes) | Executes the backend API and business logic; ensures high availability and scalability. |
| **Frontend** | Vue.js/React (HTML5, CSS, Bootstrap) | Static Web Hosting with a Content Delivery Network (CDN) | Provides a fast, responsive, and secure customer-facing interface. |
| **Database** | MongoDB | Managed Cloud Database Service | Stores all persistent data, indexed for high throughput and low-latency queries. |
| **Networking** | N/A | Load Balancer & Virtual Private Cloud (VPC) equivalent | Distributes traffic securely and ensures application resilience across multiple nodes. |

### 4.5 Authentication Procedure in Express.js
The Express.js backend implements a stateless, token-based authentication mechanism using JSON Web Tokens (JWTs), ensuring secure and efficient API access.

| **Procedure** | **Description** | **Express.js Implementation** |
|---------------|-----------------|-------------------------------|
| **Authentication** | User logs in, and the system validates the submitted password hash (using bcrypt) against the `Customers` collection. | The `/api/auth/login` route handles credential verification. |
| **Token Generation** | When authentication succeeds, a JWT is generated and signed. Its payload includes the `userId` and `isAdmin` role flag. | The `jsonwebtoken` library is used to produce the secure, signed token. |
| **Access Control** | The client includes the JWT in all protected API requests. Middleware verifies the signature and enforces Role-Based Access Control (RBAC). | Custom Express middleware checks the extracted role against route-level permissions before executing the handler. |

- **Product Perspective**: Describe how this application fits into a larger system or context.
- **Product Functions**: Provide a high-level overview of the main functionalities.
- **User Classes and Characteristics**: Describe the different types of users.
- **Operating Environment**: Specify the hardware, software, and network environment.
- **Assumptions and Dependencies**: List any assumptions made and dependencies on other projects or technologies.

### 4.6 Assumptions and Dependencies
**Assumption:**
The data relationships defined in the ERD will be effectively modeled in MongoDB using a combination of references and embedded documents, with the Express.js application layer responsible for maintaining transactional integrity.

**Dependency:**
A fully documented, stable, and PCI DSS–compliant external Payment Gateway API must be available for integration.

**Dependency:**
A reliable email service API (SMTP or cloud-based provider) is required to send confirmation emails and system notifications.

## 5. Visual Mockup Reference
- **Link or Screenshot**: Provide a link to the visual mockup or include a screenshot.

## 6. Features
- **Feature 1**: Description
- **Feature 2**: Description
- **Feature 3**: Description
- (Add more features as necessary)

## 7. Functional Requirements
### Use Cases
- **Use Case 1**:
  - **Title**: 
  - **Description**: 
  - **Actors**: 
  - **Preconditions**: 
  - **Postconditions**: 
  - **Main Flow**: 
  - **Alternate Flows**: 

### System Features
- **Feature 1**:
  - **Description**: 
  - **Priority**: 
  - **Inputs**: 
  - **Processing**: 
  - **Outputs**: 
  - **Error Handling**: 

## 8. Non-Functional Requirements
- **Performance**: Describe performance requirements.
- **Security**: Outline security needs.
- **Usability**: Detail user interface and experience considerations.
- **Reliability**: Define reliability and availability requirements.
- **Supportability**: Specify maintenance and support requirements.

## 9. Data Requirements
- **Data Models**: Include simple diagrams if possible.
- **Database Requirements**: Describe tables and relationships.
- **Data Storage and Retrieval**: Explain how data will be stored and accessed.
- **ERD**: Add the ERD for the database models based on Specifications.

## 10. External Interface Requirements
- **User Interfaces**: Provide sketches or descriptions of the user interface.
- **API Interfaces**: Briefly describe any APIs.
- **Hardware Interfaces**: Mention any required hardware interactions.
- **Software Interfaces**: Note any software interactions.

## 11. Glossary
- **Term 1**: Definition
- **Term 2**: Definition

## 12. Appendices
- **Supporting Information**: Add any additional information here.
- **Revision History**: Record any changes made to the document with dates and descriptions.
