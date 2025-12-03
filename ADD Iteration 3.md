# AIDAP Architecture Design - Iteration 3

## Step 2: Establish the iteration goal by selecting drivers
QA-2 (Usability): Multi-language support for international students

QA-3 (Modifiability): Easy addition of new data source connectors

QA-4 (Security): Protection of private student data

## Step 3: Choose one or more elements of the system to refine

We want to refine the entire AIDAP system.

## Step 4: Choose one or more design concepts that satisfy the selected drivers

| Design Concept | Description |
|----------------|-------------|
| Multi-language AI Pipeline | Uses language detection and translation services before processing, allowing questions in any language |
| Data Masking and Filtering | Automatically filters query results to only show data the user is authorized to see |

## Step 5: Instantiate architectural elements, allocate responsibilities and define interfaces

| Component | Responsibilities |
|-----------|------------------|
| **Language Detection & Translation Service** | The service enables the system to understand and produce multiple languages. This service determines what language a student is using, tells the Student how to translate their question in English so that they can be processed by the AI, and then translate the answer back into the student's native language. Frequent translations are cached so that performance can improve, while also providing terminological consistency for Academic Terms. |
| **Multi-language AI Service** | The Enhanced AI Service translates a question that has already been translated to English. The AI Service also keeps track of the original context of the query through a series of translations so that conversations remain consistent from one language to another. For example, if a user continually switches between English and Spanish, the AI retains its ability to connect those two conversations and keep the correct flow of a conversation. |
| **Data Filter & Masking Service** | The security component also serves as a gatekeeper to all the data sent back to Users. It employs filtering rules based on roles to ensure that Students only see the data they are authorized to see. For instance, a student will only see their own grades and not those of other students. It hides sensitive fields such as Personal Identification Numbers, Private Contact Details, and Academic Records. Every filtering decision made is logged for audits and compliance. |

## Step 6: Sketch Views and Record Design Decisions
Refined Deployment Diagram
<img width="1432" height="559" alt="Screenshot 2025-12-02 213912" src="https://github.com/user-attachments/assets/339aa8b8-28db-490a-ab76-e1f66948c31c" />

Element Responsibility Table for New Elements
| Element | Description / Responsibilities |
| :--- | :--- |
| **Language Detection & Translation Service (LDTS)** | Determines the user's input language and then translates the query to English for the AI, and translates the AI's final answer back to the student's native language. Implements translation caching to improve response time (R1) |
| **Multi-language AI Service (MAS)** | Processes English translated queries but maintains conversational context across multiple language switches to ensure conversation coherence (R2) |
| **Data Filter & Masking Service (DFMS)** | Applies fine-grained, role-based filtering rules to ensure data segregation and hides sensitive fields such as IDs or contact info to protect private student data (QA-4) |


## Step 7
Driver Analysis Table
| Driver | Status | Design Decision Made in Iteration 3 |
| :--- | :--- | :--- |
| **QA-2 (Usability):** Multi-language support for international students | **Completely Addressed** | Implemented the language detection and translation Service (LDTS) and the Multi-language AI Service (MAS) to handle translation and context tracking |
| **QA-3 (Modifiability):** Easy addition of new data source connectors | **Completely Addressed** | The Microservices Architecture and the Data Integration Service's isolated interface enable the addition of new connectors as independent modules without affecting the core AI or security services |
| **QA-4 (Security):** Protection of private student data | **Completely Addressed** | Implemented the Data Filter & Masking Service (DFMS) to enforce centralized, fine-grained, role-based filtering and masking of sensitive information |

# **ATAM Risks, Non-Risks, Sensitivities and Trade-offs** 
## Sensitivities

**S1: Sensitivity to translation service latency**  
Translation service latency has a large impact on the delay experienced by the university users of the multi-language AI Service. As translation processing time increases by even 500ms, the < 2-second requirement for the ability to function according to the requirements is no longer being met, significantly impacting usability for international students.

**S2: Sensitivity to context management complexity**  
The increased complexity of managing context across various language translations increases cognitive overload as additional languages are added; therefore, it is anticipated that maintaining conversation coherence will degrade the quality of the conversations taking place on the multi-language AI Service.

**S3: Sensitivity to filtering rule granularity**  
The improvement in security from fine-grained filtering for user role, adds significantly to the processing overhead for each role added to the multi-language AI Service. Each role added with an additional filtering condition also exponentially increases the processing time for processing and responding to each of those roles.

**S4: Sensitivity to academic terminology translation accuracy**  
Since the majority of interactions are academic based, and the accuracy of academic terms is vital to ensure that users get information that is accurate and relevant, the multi-language AI Service is heavily dependent on being able to accurately translate the academic terms, and if there is even one slight inaccuracy with the translation of an academic term, there would be a significant misunderstanding for international students.

**S5: Sensitivity to cache effectiveness for translations**  
If the translation cache hit rates continue to fall below optimal levels, response times for queries will increase dramatically because every user query would have to be freshly translated; this would violate the performance constraint of responding within 2 seconds.

## Tradeoffs

**T1: Translation Accuracy (+) vs. Response Time (–)**  
Translating accurately will take longer because the translator must analyze the context and that is in direct contrast to the 2-second maximum available to respond to an international student.

**T2: Security Compliance (+) vs. System Performance (–)**  
Comprehensive audit logs and fine-grained filtering of audit logs will improve compliance, however, the added processing overhead from audit logging will have a negative impact on the efficiency of the overall system.

**T3: Modifiability (+) vs. Network Efficiency (–)**  
The increased amount of inter-process communication that results from using a Microservices architecture increases the number of network hops and introduces latency into the overall architecture due to the number of services that reside on separate systems.

**T4: Multi-language Support (+) vs. Conversation Complexity (–)**  
Supporting multiple languages provides functionality to international students, but it creates significant complexities and significantly increases the probability of errors related to managing the context of conversations in those languages.

**T5: Offline Capability (+) vs. Data Freshness (–)**  
Rich client caching improves the availability of translations, however, users may end up viewing etc., stale, old translations or filtered data that was cached inadvertently.

## Risks

**R1: Translation service becomes performance bottleneck**  
The translation service is a single point of performance risk since the translation layer is vastly different from the other services. If the service fails to process a request in the required timeframe, it will affect all the international student queries.

**R2: Context loss during cross-language conversations**  
The multiple language AI service is designed to keep the context of the conversation intact when a student switches from one language to another. However, should the multiple language AI service lose its context while translating, it will provide a response that does not provide continuity and will produce inconsistencies that may confuse the user and decrease usability.

**R3: Data filtering inconsistencies across services**  
This risk exists because different services that filter student data will apply different filtering rules, creating the potential for exposing sensitive data to unauthorized users and wrongly limiting authorized users from accessing the information.

## Non-Risks

**N1: Modular service design supports easy connector addition**  
The clear division of services and defined interfaces allow for the addition of new data source connectors without interfering with current functionality.

**N2: Role-based filtering ensures proper data segregation**  
The Data filtering and masking services are designed to use strict role-based access control to ensure that students are only able to view their own data.

**N3: Caching mechanism improves frequent translation performance**  
The caching mechanism stores common translations of words and phrases and multiple academic terminologies so that frequently asked questions can be quickly answered in all types of languages, without the need to repeatedly translate them.






# ATAM Risk Assessment Table

# ATAM Utility Tree
