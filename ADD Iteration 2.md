# Iteration 2

## Step 1: Review Inputs
**Objective:** Select the drivers from the previous iteration that haven't been fully addressed

### Use Cases
**UC-8 Securely Login**
A user provides their university credentials to log in. The system verifies their identity and role, then gives the appropriate interface and permissions.

### Quality Attributes
**QA-1 Performance**  
5,000 students log in and start asking questions at the same time at the beginning of a semester. The system successfully handles all requests and gives answers within 2 seconds.

### Constraints
**CON-2**  
The system needs to be available for all users around 99.5% of the time every month.

### Concerns
**CRN-5**  
Creating the system with high performance, with the response time taking within about 2 seconds.

## Step 2: Establish the iteration goal by selecting drivers

- **UC-8:** Build a secure login system. Make sure only authorized users can access the system
- **QA-1:** Handle 5,000 users at once. Design the system so it can support many students asking questions simultaneously without slowing down
- **CON-2:** Keep the system running reliably. Ensure the platform is available almost all the time
- **CRN-5:** Make the system fast. Implement strategies to keep response times quick, especially during busy periods

## Step 3: Choose one or more elements of the system to refine
We want to refine the entire AIDAP system.

## Step 4: Choose one or more design concepts that satisfy the selected drivers

- **Web Application Architecture** - Browser-based system that works on any device without downloads. Uses scalable web servers and load balancing to handle 5000 users
- **Domain Model Design** - Defines system objects and their relationships. Provides foundation for secure login and role-based permissions.
- **Layered Architecture** - Organizes system into separate presentation, business logic, and data layers. Enables performance optimization through targeted caching and independent scaling
- **Rich Client Architecture** - Web interface with offline capabilities. Allows drafting announcements and accessing cached data when an internet connection is not available

## Step 5: Instantiate architectural elements, allocate responsibilities and define interfaces

| Design Concept | Rationale |
|---------------|-----------|
| **Authentication Service** | Connects directly to the university's login system to verify users and control what they can access based on their role |
| **Load Balancer & Auto-scaling** | Splits the load from thousands of simultaneous users across multiple servers and automatically adds more computing power when things get busy |
| **Caching Layer** | Keeps often-requested data in super-fast memory, so the system doesn't have to repeatedly ask the main database, making everything feel quicker |
| **API Gateway with Circuit Breaker** | Acts as the main front door for all requests. It blocks traffic spikes and cuts off broken services so a failure in one part doesn't crash the whole system |
| **Health Monitoring Service** | Constantly watches all the system's parts and can automatically switch to backups if something stops working, keeping the platform running smoothly |

## Step 6: Sketch Views and Record Design Decisions
Initial Object Model:

<img width="589" height="460" alt="Screenshot 2025-11-20 231707" src="https://github.com/user-attachments/assets/4abe4e2b-4202-47c6-a884-e62812230616" />

Domain Objects Associated with the Use Case Model:
<img width="649" height="727" alt="Screenshot 2025-11-20 231752" src="https://github.com/user-attachments/assets/96dea87f-4678-4c6a-b390-530a5f811f5f" />

Modules that Support the Primary Use Case:
<img width="1194" height="732" alt="Screenshot 2025-11-20 230145" src="https://github.com/user-attachments/assets/910a4c0d-acf7-4614-8392-f88bdf1230a1" />

Sequence Diagram for UC-1:

<img width="638" height="314" alt="Screenshot 2025-11-20 234053" src="https://github.com/user-attachments/assets/f39f7565-cddf-4b3d-9b19-4eee489d738c" />

Table to describe the methods for each element in the sequence diagram
## Methods / Responsibilities for UC-8 Secure Login Sequence Diagram

| **Element** | **Method / Responsibility** | **Description** |
|------------|------------------------------|-----------------|
| **User** | `clickLogin()` | User initiates login by selecting the login with SSO option |
| | `enterCredentials()` | User enters institutional login credentials at the SSO portal |
| **Client App (PWA / Mobile / Web)** | `requestLogin()` | Sends a login request to the API Gateway after the user clicks Login |
| | `receiveSession()` | Receives confirmation of a successful login/session |
| **API Gateway** | `forwardLoginRequest()` | Forwards the login request to the Authentication Service |
| | `returnLoginSuccess()` | Returns the successful login response to the client |
| **Authentication Service** | `redirectToSSO()` | Redirects user to the University Login System |
| | `receiveLoginConfirmation()` | Receives confirmation from the SSO after successful authentication |
| | `retrieveUserRole()` | Fetches the user's roles and permissions from the User Database |
| | `confirmLogin()` | Confirms successful login and sends the result back to the API Gateway |
| **University Login System (SSO Provider)** | `authenticateUser()` | Validates the user's credentials |
| | `sendLoginSuccess()` | Sends successful authentication confirmation to the Authentication Service |
| **User Database (Roles & Permissions)** | `getUserRoles()` | Returns the user's system roles and access permissions |

