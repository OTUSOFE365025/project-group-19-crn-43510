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
