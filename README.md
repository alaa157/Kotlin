# CareBrief — AI-Assisted Care Plan Assistant

CareBrief is an **offline-first Android application** that transforms unstructured caregiver notes into structured observations, evidence-linked summaries, editable care-plan drafts, and actionable tasks.

The project demonstrates practical Android engineering, local-first architecture, human-in-the-loop AI workflows, data modeling, testing, and responsible AI UX.

> **Important:** CareBrief is an assistive software prototype, not a medical device. AI-generated content is always presented as a draft requiring human review and must not be used as medical advice, diagnosis, or treatment.

---

## Project Overview

Caregivers often record observations as free-form notes. Relevant patterns can become difficult to identify when information is scattered across multiple days.

CareBrief addresses this workflow by turning those notes into a structured review process:

```text
Daily Notes
    ↓
AI Analysis
    ↓
Structured Insights + Evidence
    ↓
Care-Plan Draft
    ↓
Human Review & Editing
    ↓
Approved Plan
    ↓
Actionable Tasks
    ↓
Progress Dashboard
```

The application is designed around a simple principle:

**AI assists with organization and drafting; humans remain responsible for review and approval.**

---

## Key Features

### Dashboard
- Today's overview and key metrics
- Attention items
- Recent activity
- Care-plan and task status

### Care Recipient Management
- Searchable recipient list
- Plan status and pending-task indicators
- Detailed recipient profiles
- Activity history

### Daily Notes
- Free-text caregiver observations
- Categorization using structured chips
- Optional mood, mobility, appetite, and sleep fields
- Filterable chronological timeline

### AI-Assisted Analysis
- Offline deterministic demo AI provider
- Structured summaries generated from notes
- Pattern detection across recent observations
- Evidence-linked insights
- Transparent pattern counts instead of fabricated confidence scores

### Care-Plan Drafting
- Generated draft goals and actions
- Editable plan content
- Add, edit, delete, and reorder actions
- Monitoring indicators
- Priority and review date
- Explicit human approval before activation

### Task Management
- Tasks generated from approved care plans
- Today / upcoming / completed filters
- Task completion with undo
- Progress reflected in the dashboard

### Local-First Experience
- Works without a backend or internet connection
- Local Room database
- DataStore-based preferences
- Local reminders
- JSON export
- Demo data reset
- Privacy-focused local data handling

### Responsive Android UI
- Jetpack Compose and Material 3
- Phone bottom navigation
- Large-screen side navigation
- Responsive layouts for larger devices

---

## Architecture

CareBrief follows a pragmatic **MVVM + Repository architecture** with clear separation between presentation, domain logic, and data access.

```text
┌──────────────────────────────────────────────┐
│                Presentation                   │
│  Compose Screens • ViewModels • UI State     │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│                   Domain                      │
│ Validators • Timeline Logic • Plan Operations│
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│                    Data                       │
│ Room • DataStore • Repositories • AI Layer  │
└──────────────────────────────────────────────┘
```

### Main Layers

**Presentation**
- Jetpack Compose screens
- Screen-specific ViewModels
- Explicit `StateFlow` UI states
- Loading / Content / Empty / Error handling

**Domain**
- Validation rules
- Timeline grouping
- Care-plan editing operations
- Business logic independent of Android UI

**Data**
- Room persistence
- Repository implementations
- DataStore preferences
- AI providers
- Demo dataset
- JSON export

**Core**
- Navigation
- Design system
- Database entities and DAOs
- Responsive UI helpers
- Connectivity observation

### State Management

Data follows a predictable unidirectional flow:

```text
Room / Repository Flow
        ↓
ViewModel StateFlow
        ↓
Compose UI
        ↓
User Action
        ↓
ViewModel
        ↓
Repository
        ↓
Local Database
```

Database operations are performed asynchronously using Kotlin coroutines, keeping the UI responsive.

---

## AI Provider Architecture

The AI layer is intentionally provider-agnostic through the `AiCareAssistant` interface.

It supports operations such as:

- `summarizeNotes`
- `analyzePatterns`
- `generateCarePlanDraft`
- `generateSuggestedTasks`

### Demo AI Provider

`DemoAiCareAssistant` provides:

- Deterministic results
- Offline execution
- Evidence-linked output
- No API keys
- No network dependency

This makes the complete product workflow reproducible during demonstrations and testing.

### Remote AI Provider

`RemoteAiCareAssistant` provides the integration seam for a future production backend.

A real deployment should connect the application to a secure backend rather than embedding provider credentials inside the APK.

---

## Engineering Highlights

This project demonstrates several production-oriented engineering practices:

- **Offline-first application design**
- **MVVM and repository separation**
- **Reactive state management with Kotlin Flow**
- **Room database persistence**
- **DataStore preferences**
- **Asynchronous operations with Kotlin Coroutines**
- **WorkManager-based local reminders**
- **Dependency boundaries around AI functionality**
- **Explicit UI state modeling**
- **Human-in-the-loop AI workflow**
- **Evidence-linked generated content**
- **Responsive Compose layouts**
- **Database migrations**
- **Unit and UI testing**
- **Local data export**
- **Error-state handling**

---

## Technology Stack

| Category | Technologies |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose, Material 3 |
| Architecture | MVVM, Repository Pattern |
| Navigation | Navigation Compose |
| Local Database | Room |
| Preferences | DataStore Preferences |
| Async / Reactive | Kotlin Coroutines, Flow, StateFlow |
| Background Work | WorkManager |
| Build System | Gradle Kotlin DSL |
| Testing | JUnit 4, kotlinx-coroutines-test, Compose UI Tests |
| Android | minSdk 26, compile/target SDK 34 |

---

## Testing

Testing is treated as part of the application rather than an afterthought.

The project includes:

- Unit tests for application logic
- Coroutine-based tests
- Compose UI tests
- Database-related behavior
- Validation and domain logic coverage

Current unit-test suite: **66 tests**.

Run unit tests with:

```bash
./gradlew :app:testDebugUnitTest
```

Run connected Android tests with an emulator or physical device:

```bash
./gradlew :app:connectedDebugAndroidTest
```

---

## Getting Started

### Prerequisites

- Android Studio
- JDK 17 or JDK 21
- Android SDK
- Android emulator or physical Android device for connected tests

### Clone the Repository

```bash
git clone https://github.com/alaa157/Kotlin.git
cd Kotlin
```

### Build the Debug APK

```bash
./gradlew :app:assembleDebug
```

The generated APK is located at:

```text
app/build/outputs/apk/debug/app-debug.apk
```

### Install on a Connected Device

```bash
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

> **Build note:** Use JDK 17 or 21. JDK 25 is not compatible with the Kotlin Gradle Plugin version currently used by the project.

---

## Demo Workflow

The application includes fictional demo data so the complete product flow can be demonstrated without external services.

A typical walkthrough is:

```text
Open App
  ↓
Select Sarah Johnson
  ↓
Review Daily Notes
  ↓
Run Analysis
  ↓
Inspect Evidence-Linked Summary
  ↓
Review Care-Plan Draft
  ↓
Edit / Reorder Actions
  ↓
Approve Plan
  ↓
Generate Tasks
  ↓
Complete Tasks
  ↓
Review Dashboard Progress
```

All demo profiles are fictional.

---

## APK Builds

Debug APK:

```text
app/build/outputs/apk/debug/app-debug.apk
```

Release APK:

```text
app/build/outputs/apk/release/app-release.apk
```

The current prototype release build uses a debug signing key for development/demo purposes. A production release must be signed with a dedicated release keystore.

---

## Responsible AI Design

CareBrief intentionally avoids presenting generated content as authoritative medical decisions.

The application uses several safeguards:

- Generated content is labeled **DRAFT — REVIEW REQUIRED**
- Evidence is shown alongside generated insights
- Pattern counts are preferred over unsupported confidence percentages
- A human approval step is required before a care plan becomes active
- The demo AI is deterministic and reproducible
- The application does not diagnose or prescribe treatment
- A production AI integration should keep provider credentials on a secure backend

These design decisions make the project useful not only as an Android application, but also as an example of **human-in-the-loop AI product design**.

---

## Project Goals

The project was designed to explore how modern Android applications can combine:

- Mobile-first UX
- Local data persistence
- Reactive application architecture
- AI-assisted workflows
- Responsible AI interaction design
- Automated testing
- Offline reliability

The result is a portfolio project focused on **real software-engineering concerns rather than a simple CRUD demonstration**.

---

## Disclaimer

CareBrief is an **assistive documentation prototype**.

AI-generated content is provided only as a draft requiring appropriate human review. It must not be interpreted as medical advice, diagnosis, or treatment.

All demo care recipients and observations are fictional. Data is stored locally unless the user explicitly exports it.

---

## License

This project is currently provided for educational and portfolio purposes.
