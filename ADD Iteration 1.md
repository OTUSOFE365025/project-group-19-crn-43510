# AIDAP Architecture Design - Iteration 1

## Step 1: Review Inputs

**Design Purpose**: Establish the core architectural foundation for AIDAP by defining the system layers, integration patterns, and deployment structure.

### Primary Functional Requirements
- **UC-1 Ask a question**: A student asks a question about their schedule, grades, or deadlines. The system uses AI to understand the question, gets data from the relevant university systems, and provides a clear answer.
- **UC-3 Post Course Announcements**: A lecturer can post an announcement to all the students in a particular course using a conversational command.
- **UC-8 Securely Login**: A user provides their university credentials to log in. The system verifies their identity and role, then gives the appropriate interface and permissions.

### Quality Attributes
- **QA-1 Performance**: 5,000 students log in and start asking questions at the same time at the beginning of a semester. The system successfully handles all requests and gives answers within 2 seconds.
- **QA-2 Usability**: An international student asks the system a question in their native language and get a clear response without any delays.

### Constraints
- **CON-1**: The system must return the results of a query within 2 seconds on average, for normal loads.
- **CON-2**: The system needs to be available for all users around 99.5% of the time every month.

### Concerns
- **CRN-1**: Having cross-platform accessibility (mobile, web, etc) while maintaining consistent notifications and UI/UX for the different platforms.
- **CRN-5**: Creating the system with high performance, with the response time taking within about 2 seconds.

## Step 2: Selected Drivers
- **Functional requirements**: UC-1, UC-3, UC-8
- **Quality attributes**: QA-1, QA-2
- **Constraints**: CON-1, CON-2
- **Concerns**: CRN-1, CRN-5

## Step 3: Refinement Scope
We want to refine the entire AIDAP system.

## Step 4: Design Concepts

### Layered Architecture
- **For UC-1 and QA-2**: The layered design lets us separate the logic of AI into the business layer. This allows the system to be very modifiable because the AI models can easily be upgraded and new languages can be added as well. This can all happen without touching the question-answering logic.
- **For UC-8**: The layering allows for smooth handling of verification and authorization before going to any of the business logic.

### Rich Client Application
- **For QA-1 and CON-1**: Rich client application can cache frequent responses and store data locally. This means question which have already been asked can be answered immediately without any calls to the server.
- **For CON-2**: Users can access some content even during network outages. The rich client application can function without connection to the internet, which allows system availability.
- **For UC-3**: Lecturers can make drafts of their announcements and course content while being offline.

### Microservices Architecture
- **For QA-1 and CON-1**: Each part can be scaled up independently when needed. When 5000 students login, multiple instances of the AI Query Service can be deployed to handle the load without wasting resources on other parts of the system.
- **For CON-2**: If one service fails, the entire system can still work. A student can still view the course content and ask questions even if the administrator can't post an announcement.
- **For QA-2**: There can be many different tools used for building the complex app based on the requirements for each service.

## Step 5: Architectural Elements

| Component | Responsibilities |
|-----------|------------------|
| **Rich Client Application** | Manages conversational UI for different devices, stores frequently asked questions, allows some offline access to draft announcements |
| **Conversational AI Service** | Understands questions asked by user, handles different languages to process queries |
| **Data Integration Service** | Gets data from various university systems to optimize data-driven queries |
| **Authentication Service** | Checks the credentials and confirms the role of the user and manages permissions based on the role |
| **Announcement Service** | Handles the creation, drafting, and posting of the course and other announcements |

## Step 6: 

## Step 7: Requirements Coverage Analysis

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions Made during the Iteration |
|---------------|---------------------|----------------------|-------------------------------------------|
|               |                     | UC-1 | Rich client saves frequently asked questions, conversational AI service handles questions, data integration service gets data from university platforms |
|               |                     | UC-3 | Rich Client supports offline availability, announcement service handles posting |
|               | UC-8 | | Authentication Service handles credential verification and role-based permissions |
|               |   QA-1                  |  | Microservices allow independent scaling different services, rich client caching reduces load |
|               |                     | QA-2 | Conversational AI Service has multilingual features |
|               |                     | CON-1 | Caching frequently asked questions and independent scaling |
|               |     CON-2|  | Rich client can work offline |
|               |                     | CRN-1 | Rich Client Application ensures consistent UI/UX across various platforms |
|               | CRN-5                    |  | Overall architecture supports horizontal scaling |
