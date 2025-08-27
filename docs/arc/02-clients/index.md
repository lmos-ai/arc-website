---
title: AI Clients
sidebar_position: 5
---

The Arc Agent framework provides a number of clients to connect to AI services.

However, thanks to Arc Agents simple design, AI Client from other frameworks 
can be easily integrated.

Only the `ChatCompleter` interface needs to be implemented.

```kotlin
interface ChatCompleter {
    suspend fun complete(
        messages: List<ConversationMessage>,
        functions: List<LLMFunction>? = null,
        settings: ChatCompletionSettings? = null
    ): Result<AssistantMessage, AIException>
}
```

Check out the following example on how to set up clients:

- https://github.com/eclipse-lmos/arc/tree/main/arc-runner/src/main/kotlin/server/ChatCompleterProvider.kt