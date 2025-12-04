
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
| Analysis Scenario | Translation Service Failure During Peak Load |
|---|---|
| **Scenario** | During peak usage the translation service experiences high latency or failure while students are asking questions in multiple languages |
| **Attributes** | Usability, Performance, Reliability |
| **Stimulus** | The Language Detection & Translation Service becomes overloaded and cannot process requests within 2 seconds |
| **Environment** | Peak operation with 5,000 simultaneous users|
| **Response** | The system must detect the translation service degradation and maintain responses within 2 seconds, either through fallback mechanisms or degraded translation quality. |

| Architecture Decision | Sensitivity | Tradeoff | Risk | Nonrisk |
|---|---|---|---|---|
| AD1 - Microservices Architecture with Separate Translation Service | S1, S5 | T3 | R1 | N1 |
| AD2 - Multi-language AI Service with Context Management | S2 | T4 | R2 | - |
| AD3 - Data Filter & Masking Service | S3 | T2 | R3 | N2 |
| AD4 - Translation Caching Mechanism | S5 | T5 | - | N3 |

# ATAM Utility Tree
<img width="1135" height="665" alt="image" src="https://github.com/user-attachments/assets/9cf32471-16f9-462c-b42a-29822440eca6" />
