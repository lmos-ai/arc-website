---
title: Agent Description Format
description: Standardized agent metadata format.
---

# Agent Description Format

## Problem statement

A standardized format is essential for describing the capabilities and metadata of intelligent agents. This specification should offer an appropriate level of abstraction to ensure interoperability across a wide range of agent platforms and domains.

Agent metadata is essential for several reasons:
* **Discovery:** It allows agents to find each other based on capabilities, making it easier to assemble multi-agent systems for complex tasks.
* **Interoperability:** By specifying input/output formats and API endpoints, metadata ensures that agents can communicate effectively.
* **Version information:** Version information aids in managing updates and ensuring compatibility between different agent versions.
* **Security:** Metadata about authentication and encryption capabilities helps maintain a secure multi-agent environment.
* **Autonomy:** With comprehensive metadata, agents can make informed decisions about which other agents to interact with, enhancing the system's autonomy.

## Web of Things

[W3C Web of Things (WoT)](https://www.w3.org/WoT/) offers standardized mechanisms for agent discovery and description through its [Thing Description (TD)](https://www.w3.org/TR/wot-thing-description11/) format. This allows agents to express their capabilities and services in a consistent, machine-readable way. TDs are usually encoded in JSON format that supports JSON-LD.

A Thing Description typically contains:
- Metadata about the Thing
- A longer explanation of what the Thing does or represents.
- Interaction affordances (Properties, Actions, and Events)
- Data JSON schemas for machine-understandability
- Information about the version of the Thing
- Security definitions
- Web links to related Things or resources

Interaction affordances define how you can interact with the Thing, which may include:
- **Properties:** The state or attributes of the Thing, which can be read.
- **Actions:** Functions that can be invoked on the Thing .
- **Events:** Notifications emitted by the Thing when certain conditions are met.

The TD allows mapping of these interactions to various transport protocols (HTTP, MQTT, CoAP, etc.). This makes it 
possible to abstract away the underlying technical details of the protocol. For more details see [Agent 
Communication](/docs/multi_agent_system/agent_communication).

### Thing Description example

This example illustrates how a Weather Agent can be modeled using a Thing Description, with HTTP as the primary communication protocol, although alternative protocols may also be utilized. The Agent metadata describes that the agent uses the gpt-4o model from Azure and integrates with OpenWeatherMap API to provide weather information. The agent supports both text and voice interactions in English and German, adheres to GDPR compliance, and uses data anonymization. It offers a single action, "getWeather," which takes a natural language question and interaction mode as input and returns weather information in natural language. The service is secured using basic authentication and is accessed via a POST request to a specified endpoint, but other security schemes, such as OAuth2 tokens, can also be used.

```json
{
    "@context": [
        "https://www.w3.org/2022/wot/td/v1.1",
        {
            "htv": "http://www.w3.org/2011/http#",
            "lmos": "https://eclipse.dev/lmos/protocol/v1",
        },
        "https://schema.org/"
    ],
    "id": "urn:uuid:6f1d3a7a-1f97-4e6b-b45f-f3c2e1c84c77",
    "title": "WeatherAgent",
    "@type": "lmos:Agent",
    "lmos:metadata": {
        "lmos:vendor": {
            "lmos:name": "WeatherAI Inc.",
            "lmos:url": "https://weatherai.example.com"
        },
        "lmos:model": {
            "lmos:name": "gpt-4o",
            "lmos:provider": "Azure"
        },
        "lmos:serviceIntegration": {
            "lmos:weatherAPI": "OpenWeatherMap",
            "lmos:apiVersion": "v2.5",
            "lmos:apiDocumentation": "https://openweathermap.org/api"
        },
        "lmos:dataPrivacy": {
            "lmos:dataRetentionPeriod": "30 days",
            "lmos:anonymizationMethod": "HASHING"
        },
        "lmos:interaction": {
            "lmos:supportedLanguages": ["en_US", "de_DE"],
            "lmos:interactionMode": ["text", "voice"]
        },
        "lmos:compliance": {
            "lmos:regulatoryCompliance": "GDPR"
        }
    },
    "securityDefinitions": {
        "basic_sc": {
            "scheme": "basic",
            "in": "header"
        }
    },
    "security": "basic_sc",
    "actions": {
        "getWeather": {
            "description": "Fetches weather information based on user input.",
            "safe": true, //  Used to signal that there is no internal state changed when invoking the action. 
            "idempotent": false, // Informs whether the Action can be called repeatedly with the same result.
            "synchronous": true,
            "input": {
               "type": "object",
                "properties": {
                    "question": {
                        "type": "string"
                    },
                    "interactionMode": {
                        "type": "string",
                        "enum": ["text", "voice"]
                    }
                },
                "required": ["question","interactionMode"]
            },
            "output": {
                "type": "string",
                "description": "Natural language output providing weather information."
            },            
            "forms": [
                {
                    "op": "invokeaction",
                    "href": "https://weatherai.example.com/weather",
                    "contentType": "application/json",
                    "htv:methodName":"POST"
                }
            ]
        }
    }
}
```

### Kotlin-wot example

Here's a simple example of how you can interact with the Weather Agent from a Kotlin application using [kotlin-wot](https://github.com/RobWin/kotlin-wot) from Eclipse LMOS.

```javascript
suspend fun fetchWeather(question: String, interactionMode: String) {
    try {
        // Initialize the protocols which should be supported
        val servient = Servient(
            clientFactories = listOf(HttpProtocolClientFactory())
        )

        // Initialize the WoT client
        val wot = Wot.create(servient)

        // Request the Thing Description from the Weather Agent
        val td: ThingDescription = wot.requestThingDescription("http://weatheragent.example.com/td")

        // Consume the Thing Description
        val thing = wot.consume(td)

        // Prepare input parameters for the action
        val inputParams = mapOf(
            "question" to question,
            "interactionMode" to interactionMode
        )

        // Invoke the getWeather action
        val weatherResponse = thing.invokeAction("getWeather", inputParams.toInteractionInputValue())
        val weatherData = weatherResponse.value();

        println("Weather data: $weatherData")
    } catch (err: Exception) {
        println("Error fetching weather: ${err.message}")
    }
}
```

### Node-wot example

Here's a simple example of how you can interact with the Weather Agent from a Node.js application using [node-wot](https://github.com/eclipse-thingweb/node-wot) from [Eclipse ThingWeb](https://thingweb.io/).

```javascript
const WoT = require('@node-wot/core');
const HttpClientFactory = require('@node-wot/binding-http');

async function fetchWeather(question, interactionMode) {
    try {
        // Initialize the protocols which should be supported
        const servient = new Servient();
        servient.addClientFactory(new HttpClientFactory());

        // Request the Thing Description from the Weather Agent
        const td = await WoT.requestThingDescription("http://weatheragent.example.com/td");
        
        // Consume the Thing Description
        const thing = await WoT.consume(td);

        // Prepare input parameters for the action
        const inputParams = {
            question: question,
            interactionMode: interactionMode
        };

        // Invoke the getWeather action
        const weatherResponse = await thing.invokeAction("getWeather", inputParams);
        const weatherData = await weatherResponse.value();
    } catch (err) {
        console.error("Error fetching weather:", err);
    }
}

// Example usage
const question = 'What is the weather in berlin?'; 
const interactionMode = 'text';
fetchWeather(question, interactionMode);
```

## Semantic Web

Semantic Web technologies can significantly enhance Thing Descriptions (TDs) and provide valuable benefits for Multi-Agent Systems (MAS).

1. **Enhanced Interoperability:**
   Semantic Web technologies allow TDs to use standardized vocabularies and ontologies, enabling different systems to understand and interpret the descriptions consistently.

2. **Rich Semantic Annotations:**
   TDs can be enriched with semantic annotations, providing more detailed and machine-readable information about a Thing's capabilities, relationships, and context.

3. **Reasoning Capabilities:**
   Semantic Web technologies enable logical reasoning over TDs, allowing agents to infer additional information and make more intelligent decisions about how to interact with Things.

4. **Linked Data:**
   By using Semantic Web principles, TDs can easily link to other relevant resources, creating a web of interconnected knowledge about Things and their environments.

5. **Extensibility:**
   The Semantic Web's flexible nature allows for easy extension of TDs with domain-specific vocabularies and ontologies.
