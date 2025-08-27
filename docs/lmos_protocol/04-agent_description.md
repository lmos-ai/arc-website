---
title: Agent Description Format
description: Standardized agent metadata format.
sidebar_position: 3
---

## Problem statement

A standardized format is essential for describing the capabilities and metadata of intelligent agents. This specification should offer an appropriate level of abstraction to ensure interoperability across a wide range of agent platforms and domains.

Agent metadata is essential for several reasons:
* **Discovery:** It allows agents to find each other based on capabilities, making it easier to assemble multi-agent systems for complex tasks.
* **Interoperability:** By specifying input/output formats and API endpoints, metadata ensures that agents can communicate effectively.
* **Version information:** Version information aids in managing updates and ensuring compatibility between different agent versions.
* **Security:** Metadata about authentication and encryption capabilities helps maintain a secure multi-agent environment.
* **Autonomy:** With comprehensive metadata, agents can make informed decisions about which other agents to interact with, enhancing the system's autonomy.

## Solution

In the context of the LMOS protocol an agent is a specialized software program that uses the capabilities of a machine learning model to perform specific tasks. An agent typically has a clear goal or objective it aims to achieve, which is dependent on the task it's designed to perform. The goal drives how the agent interacts with data, models, or APIs to accomplish its task.

LMOS agents can have a state (properties), can perform actions and emit events. State represents the agent’s current configuration or context. Actions define the tasks the agent can perform. Events allow the agent to proactively notify users or other agents.

The LMOS Agent Description Format builds on top of the [Thing Description (TD)](https://www.w3.org/TR/wot-thing-description11/) format from the Web of Things (WoT) specification and extends it by specifying an additional schema tailored to the needs of intelligent  agents. This allows agents to express their capabilities and services in a consistent, machine-readable way. TDs are usually encoded in JSON format that supports JSON-LD.

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

The format is structured similarly to the Thing Description but includes LMOS-specific properties to describe the agent’s capabilities and metadata. These additional properties are defined within the "lmos" namespace, ensuring they are clearly differentiated from standard WoT terms.

Here is the table and description for an **Agent** in the context of the **LMOS Protocol vocabulary**:

| **Vocabulary Term** | **Value** | **Assignment** | **Type** |
|---------------------|-----------------|----------------|----------|
| **@context**         | https://eclipse.dev/lmos/protocol/v1 | Mandatory | URI |
| **@type**            | Agent  | Mandatory | string


Example:

```json
{
   "@context": [
      "https://www.w3.org/2022/wot/td/v1.1",
      {
         "lmos": "https://eclipse.dev/lmos/protocol/v1",
      }
   ],
   "@type": "lmos:Agent",
   "id": "urn:uuid:6f1d3a7a-1f97-4e6b-b45f-f3c2e1c84c77",
   "title": "Agent Name",
   "lmos:metadata": {
        "lmos:vendor": {
            "lmos:name": "Deutsche Telekom AG",
            "lmos:url": "https://telekom.de"
        }
   }
}
```

Full example:

```json
{
   "@context": [
      "https://www.w3.org/2022/wot/td/v1.1",
      {
         "lmos": "https://eclipse.dev/lmos/protocol/v1",
      }
   ],
   "id": "urn:uuid:6f1d3a7a-1f97-4e6b-b45f-f3c2e1c84c77",
   "title": "WeatherAgent",
   "@type": "lmos:Agent",
   "links": [{
      "rel": "service-doc",
      "href": "https://weatherai.example.com/manual.pdf",
      "type": "application/pdf",
      "hreflang": "en"
   }],
   "lmos:metadata": {
        "lmos:vendor": {
            "lmos:name": "Deutsche Telekom AG",
            "lmos:url": "https://telekom.de"
        }
   }
    "securityDefinitions": {
        "basic_sc": {
            "scheme": "basic",
            "in": "header"
        }
    },
    "security": "basic_sc",
    "properties": {
        "modelConfiguration": {
            "description": "Current configuration of the underlying LLM, including version, temperature, and maximum tokens.",
            "type": "object",
            "readOnly": true,
            "properties": {
                "modelName": {
                    "type": "string",
                    "description": "Name of the model in use, e.g., gpt-4o."
                },
                "temperature": {
                    "type": "number",
                    "description": "Temperature setting for controlling response randomness.",
                    "minimum": 0,
                    "maximum": 1
                },
                "maxTokens": {
                    "type": "integer",
                    "description": "Maximum number of tokens the model is allowed to generate."
                }
            },
            "forms": [
                {
                    "op": "readproperty",
                    "href": "https://weatherai.example.com/things/urn:uuid:6f1d3a7a-1f97-4e6b-b45f-f3c2e1c84c77/properties/modelConfiguration",
                    "contentType": "application/json"
                }
            ]
        }
    },
    "actions": {
        "getWeather": {
            "description": "Fetches weather information based on user input.",
            "safe": true,
            "idempotent": false,
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
                    "href": "https://weatherai.example.com/things/urn:uuid:6f1d3a7a-1f97-4e6b-b45f-f3c2e1c84c77/actions/getWeather",
                    "contentType": "application/json",
                    "htv:methodName":"POST"
                }
            ]
        }
    },
    "events": {
        "userFeedbackReceived": {
            "description": "Emitted when a user provides feedback on the service, with a rating from 1 to 5.",
            "data": {
                "type": "object",
                "properties": {
                    "rating": {
                        "type": "integer",
                        "description": "User rating, where 1 is the lowest and 5 is the highest.",
                        "minimum": 1,
                        "maximum": 5
                    },
                    "comment": {
                        "type": "string",
                        "description": "Optional user comment providing additional feedback."
                    }
                },
                "required": ["rating"]
            },
            "forms": [
                {
                    "op": "subscribeevent",
                    "href": "https://weatherai.example.com/things/urn:uuid:6f1d3a7a-1f97-4e6b-b45f-f3c2e1c84c77/events/userFeedbackReceived",
                    "contentType": "application/json"
                }
            ]
        }
    }
}
```

## Agent Communication

To ensure interaction between agents, the following communication patterns must be supported:

1. **Request-Reply**  
   - Agents must be able to send requests and receive a single response, providing the requested information or acknowledgment.

2. **One Request-Multiple Responses**  
   - Agents must be able to handle requests that return multiple responses over time, accommodating ongoing updates or multiple pieces of information.

3. **Event-Driven/Notifications**  
   - Agents must support the ability to send notifications without expecting a reply, allowing for status updates.

4. **Publish-Subscribe**  
   - A mechanism must be in place to allow agents to publish messages or events and for other agents to subscribe and receive those messages.

5. **Request-Stream / Response-Stream**  
   - Continuous or real-time data streams must be supported, allowing agents to receive voice streams and response with voice streams.
