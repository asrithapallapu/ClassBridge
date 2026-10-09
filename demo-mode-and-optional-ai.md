---
name: Demo mode and optional AI
description: ClassBridge's default demo behavior and optional live AI configuration.
---

Keep ClassBridge fully usable without an AI provider credential. Demo Mode is the default, and any live AI provider must remain optional through environment variables; provider absence or failure must not block course upload, learning, quizzes, or mastery tracking.

**Why:** The user explicitly requested a usable app without an API key and said not to block the app or ask for one.

**How to apply:** Preserve the local course-grounded fallback when changing AI, tutoring, topic extraction, or quiz-generation behavior. Do not make a provider credential a setup prerequisite.
