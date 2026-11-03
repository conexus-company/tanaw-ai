# TANAW
## Complete Technical Specification Document

**Project Name:** Tanaw  
**Project Type:** Multimodal AI Personal Assistant  
**Document Version:** 1.2  
**Status:** Proposed  
**Primary Platform:** Android  
**Mobile Framework:** React Native + TypeScript  
**Native Integration:** Kotlin  
**AI Architecture:** Tool-based Multimodal AI Agent

---

# 1. Project Overview

## 1.1 Description

Tanaw is a voice-activated, multimodal personal AI assistant designed to interact naturally with users through voice, camera, and AI-powered device capabilities.

Unlike traditional chatbot applications, Tanaw aims to provide a hands-free assistant experience similar to Google Assistant and Siri, while incorporating visual intelligence, security analysis, contextual interfaces, and AI-powered tool execution.

Users can activate Tanaw using a custom wake word, communicate through natural speech, request visual analysis, scan potentially malicious links, and perform supported device actions.

Tanaw will use a hybrid architecture combining React Native for the mobile application and Kotlin for Android-specific functionality.

Android will be the primary development platform because it offers more flexibility for native background services and system integrations.

## 1.2 Vision

Build a personal AI assistant that can hear, see, understand, and assist users through natural interactions.

Tanaw should not simply answer questions. It should understand requests, identify required information, invoke appropriate tools, and present useful results through voice and interactive interfaces.

## 1.3 Core Capabilities

1. System-wide wake-word detection.
2. Natural voice conversations.
3. AI-powered reasoning and intent recognition.
4. Camera-based object and scene identification.
5. Optical character recognition (OCR).
6. Suspicious URL and scam detection.
7. Contextual assistant interfaces.
8. Tool-based device interactions.
9. Spoken AI responses.
10. Conversational context management.

---

# 2. Project Scope

## 2.1 In Scope

The initial project will implement:

- Custom wake-word activation.
- Background voice assistant infrastructure.
- Voice recognition.
- AI-powered conversations.
- Text-to-speech.
- Camera activation through voice requests.
- Image capture and analysis.
- Object identification.
- Text extraction from images.
- LinkGuard security analysis.
- Dynamic input interfaces.
- AI tool orchestration.
- Android permissions and privacy controls.

## 2.2 Out of Scope for Initial MVP

- Full replacement of Google Assistant.
- Full replacement of Siri.
- Universal background camera activation.
- Continuous visual monitoring.
- Autonomous financial transactions.
- Unrestricted device control.
- Offline large language model inference.
- iOS system-wide wake-word support.
- Advanced smart home integrations.

These capabilities may be considered in future versions.

---

# 3. Technology Stack

| Layer | Technology |
|---|---|
| Mobile framework | React Native CLI |
| Application language | TypeScript |
| Native Android | Kotlin |
| Wake-word detection | Picovoice Porcupine |
| Camera | React Native VisionCamera / CameraX |
| Speech recognition | Android SpeechRecognizer / Whisper |
| AI conversation | Gemini API |
| Vision intelligence | Gemini multimodal models |
| Text-to-speech | Android TextToSpeech |
| Backend | Node.js or FastAPI |
| Database | Supabase PostgreSQL |
| Authentication | Supabase Auth, if required |
| Security intelligence | Google Safe Browsing / Web Risk |
| Testing | Jest, React Native Testing Library, native Android tests |

All AI providers should be replaceable through provider abstractions.

The application must not expose private API keys in its client-side code.

---

# 4. System Architecture

## 4.1 High-Level Architecture

```text
                       USER
                         |
                         v
                   "HEY TANAW"
                         |
                         v
              NATIVE ANDROID LAYER
              --------------------
              Kotlin Foreground Service
              Wake Word Detection
              Audio Session Management
                         |
                         v
                   VOICE PIPELINE
              --------------------
              Speech Recognition
              Speech Processing
                         |
                         v
                  AI ORCHESTRATOR
              --------------------
              Intent Recognition
              Context Management
              Tool Selection
              Response Generation
                         |
              +----------+----------+
              |          |          |
              v          v          v
            VISION     LINKGUARD   DEVICE
            MODULE     MODULE     MODULE
              |          |          |
              +----------+----------+
                         |
                         v
                  RESPONSE ENGINE
                         |
              +----------+----------+
              |                     |
              v                     v
          TEXT RESPONSE        VOICE RESPONSE
              |                     |
              +----------+----------+
                         |
                         v
                    TANAW UI
```

## 4.2 Architectural Layers

### A. Mobile Application Layer

Responsible for:

- Assistant interface.
- Conversation display.
- Camera preview.
- Contextual input components.
- Settings.
- Permission management.
- User preferences.

### B. Native Android Layer

Responsible for:

- Wake-word detection.
- Foreground microphone service.
- Audio lifecycle.
- Native microphone access.
- Android assistant integration.
- Native device interactions.

### C. AI Orchestration Layer

Responsible for:

- Intent recognition.
- Context management.
- Tool selection.
- Tool execution coordination.
- AI provider communication.
- Response generation.

### D. Intelligence Modules

Independent modules responsible for:

- Visual intelligence.
- Link security analysis.
- Speech processing.
- Device capabilities.

### E. Backend Layer

Responsible for:

- Secure AI provider communication.
- API credential protection.
- Optional conversation persistence.
- Rate limiting.
- Security intelligence integration.
- Backend URL inspection.

---

# 5. Functional Requirements

## F-001: System-Wide Wake Word Detection

**Description:**

The system shall detect the custom wake word "Hey Tanaw" when the listening service is enabled.

Requirements:

- Use an on-device wake-word engine.
- Support a custom wake-word model.
- Operate independently of the React Native UI lifecycle.
- Activate voice interaction after detection.
- Provide listening enable/disable controls.
- Display a persistent notification while the microphone foreground service is active.
- Handle microphone permission denial.
- Handle service interruption and recovery.

**Technology:** Picovoice Porcupine + Kotlin.

### Acceptance Criteria

- Detects the configured wake word.
- Works under supported background conditions.
- Does not require the application screen to remain open.
- Handles listening interruptions.
- Stops listening when disabled.

**Platform limitation:** Android restricts background microphone access and microphone foreground-service startup. Screen-off and background operation must be validated on actual devices. Force-stopped applications cannot be assumed to keep listening.

---

## F-002: Voice Recognition

**Description:**

The system shall convert spoken commands into text.

Requirements:

- Activate speech recognition after wake-word detection.
- Support English initially.
- Design for Filipino and Taglish.
- Handle recognition failures.
- Support natural questions.
- Maintain active conversation context.

Example:

User: "Hey Tanaw, what is Kubernetes?"

Expected result:

Recognized speech is passed to the AI orchestration layer.

**Technology options:**
- Android SpeechRecognizer.
- Whisper.
- Other compatible speech recognition providers.

---

## F-003: AI Conversation Engine

**Description:**

The system shall process user requests and generate contextual responses.

Requirements:

- Accept recognized speech.
- Understand user intent.
- Retrieve relevant conversation context.
- Determine whether a tool is necessary.
- Generate natural responses.
- Support text and multimodal inputs.
- Support replaceable AI providers.

Initial provider: Gemini.

The conversation engine must be separated from individual intelligence modules.

---

## F-004: Continuous Conversation

**Description:**

The system shall support multiple conversational exchanges without requiring the user to repeat the wake word after every response.

Example:

User: "Hey Tanaw, what is Kubernetes?"

Tanaw: "Kubernetes is a container orchestration platform."

User: "What is it used for?"

Tanaw: "It's commonly used to manage and scale containerized applications."

Requirements:

- Maintain an active conversation session.
- Listen after the assistant finishes speaking.
- Handle conversation timeouts.
- Return to wake-word listening when the session ends.
- Support voice commands for ending a conversation.
- Support interruption during speech playback.

---

# 6. Visual Intelligence Module

**Module ID:** TANAW-MOD-001

## F-005: Camera Activation

**Description:**

The system shall allow users to request camera-based analysis through voice commands.

Example:

User: "Hey Tanaw, ano 'to?"

Expected behavior:

1. Recognize the visual identification request.
2. Check camera permission.
3. Present the camera interface.
4. Display a live camera preview.
5. Capture an image after appropriate user interaction.
6. Send the image to the vision analysis service.
7. Process the AI-generated response.
8. Speak the result.

Requirements:

- Support front and rear cameras.
- Display camera preview.
- Handle camera permission denial.
- Handle capture failures.
- Show analysis progress.
- Avoid unnecessary image storage.
- Provide an explicit way to close the camera.

**Technology:** React Native VisionCamera or Android CameraX.

## F-006: Image Understanding

The system shall analyze images using a multimodal AI model.

Initial capabilities:

- Object identification.
- Object description.
- Product recognition.
- Scene description.
- Visual question answering.
- Basic visual troubleshooting.
- Identification of common signs and labels.

Example:

User: "What is this device used for?"

Tanaw captures the device and asks the vision model to describe it.

The response is presented through text and speech.

## F-007: Optical Character Recognition

The system shall extract readable text from captured images.

Supported use cases:

- Reading signs.
- Reading labels.
- Extracting printed text.
- Reading documents.
- Reading product information.

Possible technologies:

- Google ML Kit Text Recognition.
- Vision-capable Gemini models.

OCR and general image understanding should remain independently replaceable.

---

# 7. LinkGuard – Security Intelligence Module

**Module ID:** TANAW-MOD-002

## 7.1 Description

LinkGuard is an AI-assisted security feature that allows users to inspect URLs received through messaging applications, SMS, email, and other sources.

The system identifies potential phishing websites, malicious redirects, suspicious domains, and other online threats.

The primary objective is to help users determine whether a link may be dangerous before opening it.

## 7.2 User Scenarios

### Scenario A: Voice-Activated Link Scanning

User:

"Hey Tanaw, I want to scan this URL if it's a scam."

Tanaw:

"Sure! Paste the URL here and I'll check it for you."

The assistant displays a contextual URL input interface.

The user pastes the link and presses Scan.

Tanaw analyzes the link and provides the result.

### Scenario B: Direct Link Sharing

The user receives a URL through SMS or Messenger.

1. Opens Android's share menu.
2. Selects Tanaw.
3. Tanaw receives the URL.
4. LinkGuard performs the analysis.
5. Tanaw presents the verdict.

### Scenario C: Clipboard-Based Checking

User:

"Hey Tanaw, check mo nga kung safe itong link."

Tanaw asks the user to paste or explicitly share the URL.

Clipboard content must not be monitored continuously or accessed without an appropriate user action.

## 7.3 F-101: URL Input

The system shall accept URLs through:

- Contextual assistant input.
- Android Share Intent.
- Manual URL entry.
- User-initiated clipboard interaction.

Requirements:

- Validate URL syntax.
- Accept HTTP and HTTPS URLs.
- Reject unsupported URL schemes.
- Detect malformed URLs.
- Support multiple URLs.
- Preserve original URLs for analysis.
- Never automatically navigate to submitted URLs.

## 7.4 F-102: URL Security Analysis

The system shall evaluate URLs through security intelligence and deterministic heuristics.

Analysis criteria:

1. Known phishing reputation.
2. Known malware reputation.
3. Domain reputation.
4. Suspicious domain similarity.
5. URL shortening services.
6. Redirect destinations.
7. Suspicious URL patterns.
8. Brand impersonation.
9. Certificate and HTTPS information where available.
10. Potential credential collection behavior when safely observable.

## 7.5 F-103: Threat Intelligence Integration

Initial provider candidates:

- Google Safe Browsing.
- Google Web Risk.
- Other replaceable threat intelligence providers.

Provider terms and commercial licensing must be reviewed before production use.

Reference documentation:

https://developers.google.com/safe-browsing/reference

https://cloud.google.com/web-risk

An LLM must not independently determine whether a URL is safe. Security verdicts must be based on the security analysis pipeline.

## 7.6 F-104: Redirect Inspection

Requirements:

- Detect known URL shorteners.
- Inspect redirect chains through a controlled backend scanner.
- Identify unexpected destination domains.
- Detect excessive redirects.
- Prevent access to localhost, private IP ranges, internal services, and cloud metadata endpoints.
- Apply DNS rebinding protections.
- Apply network isolation, timeouts, and request limits.

The application must not open suspicious links in the user's default browser during passive inspection.

## 7.7 F-105: Risk Classification

The system shall provide four possible classifications.

| Classification | Meaning |
|---|---|
| No known threats detected | No threat was identified by the completed checks |
| Suspicious | Potential malicious indicators were found |
| Malicious | A confirmed or strongly identified threat was found |
| Unknown | Insufficient information to establish a verdict |

The system must never guarantee that a link is safe simply because no threat was detected.

## 7.8 F-106: AI Explanation

The system shall provide understandable explanations.

Example:

"Warning! This link appears suspicious. The domain resembles a legitimate banking website. I recommend not opening it or entering your password or OTP."

Requirements:

- Explain relevant indicators.
- Distinguish confirmed intelligence from heuristics.
- Communicate uncertainty.
- Recommend protective actions.
- Avoid unsupported claims.

## 7.9 F-107: User Protection

The system shall:

- Recommend avoiding malicious links.
- Warn against sharing OTPs.
- Warn against entering credentials.
- Warn against downloading unknown files.
- Warn against providing financial information.
- Never automatically submit credentials.
- Never automatically download files.
- Never automatically interact with suspicious websites.

## 7.10 Initial LinkGuard Tool Registry

| Tool | Responsibility |
|---|---|
| linkguard.receiveUrl | Accept submitted URLs |
| linkguard.validateUrl | Validate URL structure |
| linkguard.checkReputation | Query threat intelligence |
| linkguard.inspectDomain | Analyze domain indicators |
| linkguard.inspectRedirects | Inspect redirects safely |
| linkguard.assessRisk | Aggregate security findings |
| linkguard.explainResult | Explain the assessment |

---

# 8. Contextual Assistant Interface

**Module ID:** TANAW-MOD-003

## 8.1 Description

Tanaw shall provide a Siri-like assistant interface capable of displaying contextual UI components based on the user's voice requests.

Rather than requiring users to navigate through the main application, Tanaw should present relevant input fields, buttons, and results when additional user interaction is required.

The interface should be reusable across multiple modules.

## 8.2 F-108: Dynamic Input Interface

Requirements:

1. Recognize requests requiring additional user input.
2. Determine the appropriate interface component.
3. Display the contextual assistant UI.
4. Support interactive inputs.
5. Pass submitted values to the appropriate module.
6. Display processing status.
7. Display results.
8. Return to voice interaction when appropriate.
9. Allow the user to dismiss the interface.

## 8.3 URL Scanning Interaction

Example:

User:

"Hey Tanaw, I want to scan this URL if it's a scam."

System behavior:

1. Detects link-scanning intent.
2. Activates the assistant interface.
3. Displays the LinkGuard component.

Tanaw:

"Sure! Paste the URL here and I'll check it for you."

The interface displays:

**LinkGuard**

Paste the URL you want to inspect.

`[ URL INPUT FIELD ]`

`[ SCAN URL ]`

The user pastes the URL and presses Scan URL.

The system then:

1. Validates the submitted URL.
2. Sends it to LinkGuard.
3. Performs security analysis.
4. Displays the scan progress.
5. Displays the final classification.
6. Speaks the result.

## 8.4 Contextual UI Components

The assistant UI must support reusable components such as:

- Text input.
- URL input.
- Confirmation dialog.
- Camera preview.
- Image analysis result.
- Loading indicator.
- Security assessment report.
- Error message.
- Action buttons.

## 8.5 Android Integration

Potential implementation approaches:

**Assistant-style interface**
- Dedicated React Native activity.
- Android assistant integration through VoiceInteractionService.
- User-visible presentation where permitted.

**Floating interface**
- Android WindowManager.
- SYSTEM_ALERT_WINDOW permission when genuinely necessary.
- Explicit user authorization.

The preferred initial approach is an assistant-style interface or dedicated user-visible activity.

Do not depend on unrestricted background activity launches or overlay permissions.

## 8.6 Acceptance Criteria

- Voice requests can trigger the appropriate contextual UI.
- URL scanning displays a URL input field.
- Users can paste URLs.
- Scanning starts only after explicit submission.
- Results are displayed and spoken.
- The interface can be dismissed safely.
- The contextual UI does not interrupt unrelated assistant operations.

---

# 9. AI Agent and Tool Orchestration

## 9.1 Description

Tanaw shall use a tool-based AI architecture rather than a chatbot with hardcoded functionality.

The AI orchestration layer determines the user's intent and selects the appropriate registered tool.

The application validates and executes tool requests.

## 9.2 Initial Tool Registry

| Tool | Responsibility |
|---|---|
| camera.capture | Capture an image |
| vision.analyze | Analyze an image |
| ocr.extractText | Extract text |
| linkguard.receiveUrl | Accept a URL |
| linkguard.validateUrl | Validate a URL |
| linkguard.assessRisk | Assess security findings |
| assistant.speak | Generate spoken output |
| conversation.getContext | Retrieve relevant context |
| device.openApp | Request an allowed app launch |

## 9.3 Tool Execution Rules

- Only registered tools may be invoked.
- Validate all arguments.
- Enforce permission checks.
- Require confirmation for sensitive actions.
- Do not execute arbitrary operating-system commands.
- Prevent duplicate tool execution.
- Handle failures gracefully.
- Log execution metadata without unnecessarily retaining private content.
- Treat external content as untrusted input.

## 9.4 Example: Camera Tool Execution

User:

"Hey Tanaw, ano 'to?"

Intent:

`identify_object`

Proposed execution:

1. camera.capture
2. vision.analyze
3. assistant.speak

## 9.5 Example: LinkGuard Tool Execution

User:

"Hey Tanaw, check this link if it's a scam."

Intent:

`scan_url`

Proposed execution:

1. Display contextual URL input.
2. Wait for user submission.
3. linkguard.validateUrl
4. linkguard.checkReputation
5. linkguard.inspectDomain
6. linkguard.inspectRedirects, where supported.
7. linkguard.assessRisk
8. linkguard.explainResult
9. assistant.speak

The AI must not bypass the security assessment pipeline.

---

# 10. Conversation and Session Management

The system shall maintain conversation state across multiple exchanges.

Requirements:

- Maintain active conversation sessions.
- Preserve relevant conversation context.
- Support voice and UI interactions.
- Associate tool results with the appropriate session.
- Handle conversation timeouts.
- Allow users to end sessions.
- Prevent unrelated requests from sharing sensitive context.
- Support optional conversation persistence.

Example:

User: "Hey Tanaw, what is this?"

Tanaw: "That's an apple."

User: "Is it healthy?"

Tanaw understands that "it" refers to the previously identified apple.

Conversation memory should be separated from persistent personal memory.

---

# 11. User Interface Requirements

## 11.1 Main Screen

Elements:

- Tanaw branding.
- Listening status.
- Microphone activation control.
- Conversation transcript.
- Camera activation.
- Settings.
- Listening controls.

## 11.2 Assistant States

The interface shall distinguish:

1. Idle – Waiting for activation.
2. Listening – Receiving speech.
3. Processing – Understanding the request.
4. Executing – Running a tool.
5. Awaiting Input – Waiting for user interaction.
6. Speaking – Playing the response.
7. Error – Handling a failure.

## 11.3 Camera Interface

Elements:

- Live camera preview.
- Capture control.
- Analysis indicator.
- AI response.
- Close camera action.

## 11.4 LinkGuard Interface

Elements:

- Security module identifier.
- URL input field.
- Paste support.
- Scan button.
- Loading indicator.
- Risk classification.
- Security findings.
- Recommended actions.

## 11.5 Assistant UI Design Principle

The interface should be contextual and lightweight.

The user should not need to navigate to separate application pages every time Tanaw needs input.

The assistant should present only the components necessary for the current task.

---

# 12. Android Permissions and System Integration

The following permissions and integrations must be evaluated.

| Permission / API | Purpose |
|---|---|
| RECORD_AUDIO | Speech recognition and wake-word detection |
| FOREGROUND_SERVICE | Foreground service infrastructure |
| FOREGROUND_SERVICE_MICROPHONE | Microphone foreground service |
| CAMERA | Camera access |
| POST_NOTIFICATIONS | Listening service notification |
| SYSTEM_ALERT_WINDOW | Optional floating overlay |
| VoiceInteractionService | System assistant integration |

Additional requirements:

- Respect Android foreground-service restrictions.
- Handle microphone while-in-use restrictions.
- Request runtime permissions appropriately.
- Avoid unrestricted background activity launches.
- Provide a clear listening indicator.
- Handle manufacturer-specific battery restrictions.
- Do not assume the application can operate after force-stop.
- Evaluate assistant-role eligibility and VoiceInteractionService separately.

Camera access must be user-visible and permission-compliant. Tanaw must never silently capture images.

---

# 13. Non-Functional Requirements

## 13.1 Performance

- Wake-word detection should operate locally.
- Minimize unnecessary network requests.
- Keep audio processing independent of UI rendering.
- Minimize camera startup latency.
- Optimize image size before AI transmission where appropriate.
- Provide responsive contextual interfaces.
- Use reasonable battery resources.

## 13.2 Security

- Protect provider credentials.
- Use HTTPS.
- Validate all external inputs.
- Restrict tool execution.
- Apply least-privilege permissions.
- Protect conversation data.
- Secure URL inspection against SSRF.
- Treat webpage content as untrusted.

## 13.3 Privacy

- Perform wake-word detection locally.
- Do not continuously upload microphone audio.
- Capture images only after an appropriate request.
- Provide listening controls.
- Avoid unnecessary image retention.
- Provide conversation deletion controls.
- Request consent before submitting sensitive URLs to third-party services.
- Clearly communicate when the microphone or camera is active.

## 13.4 Reliability

- Handle network interruptions.
- Recover from speech recognition failures.
- Handle AI provider errors.
- Handle permission changes.
- Handle Android service interruptions.
- Prevent duplicate tool execution.
- Return Unknown when security analysis is incomplete.

---

# 14. Testing Strategy

## 14.1 Unit Testing

Test:

- Wake-word event handling.
- Intent recognition.
- Conversation state transitions.
- Tool registration.
- Tool input validation.
- Permission checks.
- URL validation.
- URL risk classification.
- Redirect validation.
- AI response parsing.

## 14.2 Integration Testing

Test:

- React Native and Kotlin communication.
- Kotlin and Porcupine integration.
- Speech recognition and AI orchestration.
- Camera capture and vision analysis.
- URL input and LinkGuard.
- Threat intelligence integration.
- AI response and text-to-speech.
- Android Share Intent.
- Contextual assistant UI.

## 14.3 Security Testing

Test:

- Malicious URL fixtures.
- Phishing URL fixtures.
- URL shorteners.
- Redirect loops.
- Private IP and localhost targets.
- DNS rebinding.
- Prompt injection in webpage content.
- API rate limiting.
- Invalid tool calls.
- Unauthorized camera and microphone activation.

## 14.4 Device Testing

Test:

1. Application open.
2. Application in background.
3. Screen locked.
4. Network unavailable.
5. Microphone permission denied.
6. Camera permission denied.
7. Service manually stopped.
8. Battery optimization enabled.
9. Incoming phone calls.
10. Other applications using the microphone.
11. Android Share Intent.
12. Contextual interface presentation.

Testing must document actual device and Android-version limitations.

---

# 15. Proposed Repository Structure

```text
tanaw/
│
├── android/
│   └── app/
│       └── src/main/java/
│           └── .../
│               ├── service/
│               ├── wakeword/
│               ├── audio/
│               ├── camera/
│               ├── assistant/
│               └── modules/
│
├── src/
│   ├── app/
│   ├── components/
│   │
│   ├── features/
│   │   ├── assistant/
│   │   ├── conversation/
│   │   ├── camera/
│   │   ├── linkguard/
│   │   └── settings/
│   │
│   ├── services/
│   │   ├── ai/
│   │   ├── speech/
│   │   ├── vision/
│   │   ├── security/
│   │   └── native/
│   │
│   ├── agent/
│   │   ├── orchestrator/
│   │   ├── tools/
│   │   ├── registry/
│   │   ├── schemas/
│   │   └── policies/
│   │
│   ├── hooks/
│   ├── stores/
│   ├── types/
│   └── utils/
│
├── backend/
│   ├── api/
│   ├── providers/
│   ├── conversation/
│   ├── linkguard/
│   ├── security/
│   └── middleware/
│
├── specs/
│   ├── features/
│   ├── decisions/
│   └── tasks/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── AGENTS.md
├── SPEC.md
├── TEST.md
└── README.md
```

The structure is a proposed baseline. Adjust it according to actual module boundaries and implementation requirements.

---

# 16. Development Roadmap

Development shall follow Specification-Driven Development and Test-Driven Development.

Each phase must have its own implementation specification, acceptance criteria, and test plan.

## Phase 1: Project Foundation

Deliverables:

- React Native CLI initialization.
- Kotlin integration.
- Native module communication.
- Initial application interface.
- Permission handling.
- Project architecture.

Exit criteria:

- Application launches.
- Native communication works.
- Permissions are handled correctly.

## Phase 2: Wake Word Infrastructure

Deliverables:

- Porcupine integration.
- Custom wake-word model.
- Kotlin foreground microphone service.
- Listening notification.
- Listening controls.

Exit criteria:

- Wake-word detection works.
- Background operation works under documented conditions.
- Service interruption is handled.

## Phase 3: Voice Conversation

Deliverables:

- Speech recognition.
- AI provider integration.
- Conversation management.
- Text-to-speech.
- Continuous conversation.

Exit criteria:

- User can speak to Tanaw.
- Tanaw generates and speaks responses.
- Conversations maintain relevant context.

## Phase 4: Visual Intelligence

Deliverables:

- Camera interface.
- Voice-triggered camera activation.
- Image capture.
- Vision model integration.
- Object identification.
- OCR.

Exit criteria:

- User can request camera analysis through voice.
- Images can be captured and analyzed.
- Results are spoken and displayed.

## Phase 5: Contextual Assistant UI

Deliverables:

- Assistant-style presentation.
- Dynamic component rendering.
- URL input interface.
- Loading and result states.
- Native Android presentation integration.

Exit criteria:

- Voice requests can trigger contextual UI.
- Users can interact with displayed inputs.
- UI actions correctly invoke their respective modules.

## Phase 6: LinkGuard

Deliverables:

- URL input.
- URL validation.
- Threat intelligence integration.
- Domain analysis.
- Risk classification.
- AI explanations.
- Voice responses.

Exit criteria:

- Users can paste URLs.
- LinkGuard performs security checks.
- Risk findings are displayed and spoken.
- Incomplete scans return Unknown.
- Suspicious links are never automatically opened.

## Phase 7: Agent Tool Orchestration

Deliverables:

- Tool registry.
- Input/output schemas.
- Intent routing.
- Tool execution policies.
- Error handling.
- Execution logging.

Exit criteria:

- AI can select appropriate tools.
- Tool arguments are validated.
- Sensitive actions are protected.
- Failed operations are handled safely.

## Phase 8: Optimization and Reliability

Deliverables:

- Battery optimization.
- Background reliability testing.
- Audio interruption handling.
- Network failure handling.
- Security testing.
- Device compatibility testing.

Exit criteria:

- Core functionality works on documented test devices.
- No unintended microphone or camera activation.
- No duplicate tool execution.
- Recoverable failures do not crash the application.

---

# 17. Definition of Done

The initial Tanaw MVP is considered complete when:

1. The system detects the configured wake word under supported conditions.
2. Users can communicate through natural speech.
3. Tanaw generates spoken AI responses.
4. Continuous voice conversations work.
5. Tanaw can activate the camera interface through voice requests.
6. Tanaw can identify objects and analyze images.
7. Tanaw can extract text from images.
8. Voice requests can display contextual assistant interfaces.
9. Users can paste URLs into the LinkGuard interface.
10. LinkGuard can perform security intelligence checks.
11. The system can classify and explain URL risks.
12. Users receive spoken and visual security assessments.
13. Tool execution is permission-aware and validated.
14. Automated tests cover critical functionality.
15. Android background and privacy limitations are documented and tested.

---

# 18. Architectural Principles

The entire implementation must follow these principles:

1. **Native where necessary:** Kotlin handles Android-specific functionality.
2. **Modularity:** Each intelligence module remains independently maintainable.
3. **Tool-based intelligence:** AI interacts through registered tools.
4. **Privacy first:** Minimize audio, image, and conversation retention.
5. **Testability:** Every module should have clear boundaries and test coverage.
6. **Incremental development:** Complete and validate phases before expanding scope.
7. **Human control:** Sensitive operations require appropriate authorization.
8. **Provider independence:** AI and intelligence providers should be replaceable.
9. **Deterministic security:** Security verdicts rely on evidence-based checks, not LLM guesses.
10. **Platform awareness:** Respect Android background execution and permission restrictions.

---

# 19. Final Product Vision

Tanaw is intended to become a personal AI assistant that users can access naturally without constantly navigating through an application.

Users should be able to say:

- "Hey Tanaw, can I ask you something?"
- "Hey Tanaw, ano 'to?"
- "Hey Tanaw, read this for me."
- "Hey Tanaw, check if this link is a scam."
- "Hey Tanaw, explain this error."
- "Hey Tanaw, remind me about this later."

Tanaw should understand the request, identify the information it needs, present the appropriate interface, use its available tools, and provide an understandable response.

**The ultimate goal is to build an assistant that can hear, see, reason, and act within the boundaries of user permissions and device capabilities.**