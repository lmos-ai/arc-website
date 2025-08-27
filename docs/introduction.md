---
title: What is LMOS?
sidebar_position: 1
---

# What is LMOS?

## Our vision of an Internet of Agents

Eclipse LMOS is paving the way for an **Internet of Agents (IoA)** — an internet-scale multi-agent system where AI agents and tools from different organizations can be easily **published, discovered, and interconnected**, regardless of the technologies they are built on. This vision builds upon the evolution of the **Social Web** and the **Internet of Things (IoT)**, extending its principles to web-based multi-agent systems.

## LMOS Protocol

To achieve this vision, Eclipse LMOS incorporates **well-established W3C protocols and ideas** from the Internet of Things (IoT), the Semantic Web, and Social Web. This adaptation is formalized in the [LMOS Protocol](/lmos/docs/lmos_protocol/introduction). The LMOS Protocol is designed to establish a distributed, open ecosystem that eliminates vendor lock-in, ensuring that users are not bound to major corporations or central authorities.

The **LMOS Protocol** draws inspiration from existing decentralized technologies. For example, **Matter/Thread** enables smart home devices from different manufacturers to connect and work together seamlessly through a **mesh network**. In this network, devices communicate directly with one another, ensuring reliability and resilience without relying on a central hub. If one device fails, the network automatically reroutes the communication through other devices, providing a flexible and robust solution for smart homes.

Similarly, the [ActivityPub Protocol](https://activitypub.rocks/), which is a federated social media networking protocol, allows users to interact across different social media networks. It enables interoperability, ensuring that users are not locked into a single platform or provider, fostering an open and connected ecosystem. The analogy to social media networks is especially relevant, because agents should also be able to interact, share information, and collaborate in a decentralized manner. Just as humans can engage across various social media networks, agents should be able to discover and work with others in a flexible, open ecosystem. 

The **LMOS Protocol** shares these goals of decentralization and flexibility. It's designed for "big world" use cases. It enables tools to be discovered and connected to agents, agents to collaborate within multi-agent systems, and agents to be integrated into existing applications — **regardless of the underlying technologies**. This creates an ecosystem where diverse AI capabilities can easily interact and be applied in various scenarios, ensuring that the system is not restricted by specific providers or technologies.

![Internet of Agents](/img/internet_of_agents_intro-light.png#light-mode-only)
![Internet of Agents](/img/internet_of_agents_intro-dark.png#dark-mode-only)

import Standards from './lmos_protocol/_standards.mdx';

<Standards  />

## LMOS Platform

In addition, Eclipse LMOS provides an open-source, vendor-neutral [cloud platform](/lmos/docs/lmos_platform/overview) to build and run enterprise-ready multi-agent systems on cloud or on-prem. It also serves as the reference implementation of the LMOS protocol, providing a standardized platform for multi-agent systems.
The key features of the LMOS platform are:

- **Simplified AI Agent Development:**  
   LMOS provides a framework called [Agent ReaCtor (ARC)](/lmos/docs/arc/index) to abstract the intricacies of large language models (LLMs), memory management, and tool integration. Just as an operating system manages applications and hardware, the LMOS framework acts as a virtual "OS" for AI agents, allowing developers to focus on creating intelligent, adaptable agents without getting bogged down by infrastructure complexity.

- **Open, Interoperable Architecture:**  
   Built on the principles of openness and interoperability, LMOS supports seamless integration of AI agents across diverse platforms and networks. By embracing open standards, LMOS ensures that agents from various platforms can communicate, share knowledge, and collaborate, fostering innovation across industries. You can leverage LMOS’s multi-agent capabilities with frameworks like Agent ReaCtor (ARC), Langchain4j, LlamaIndex or LangChain, seamlessly integrating them into the LMOS platform without being tied to a specific development language or framework. 

- **Agent Lifecycle Management:**  
   LMOS enables real-world deployment strategies, such as canary releases and advanced routing techniques based on Natural Language Understanding (NLU). These features allow enterprises to introduce new agent features incrementally, ensuring stability and reducing risk during updates.

- **Cloud-Native Scalability:**  
   Leveraging Kubernetes, LMOS ensures cloud-native scalability, allowing AI agents to grow alongside your business needs. Whether managing a small number of agents or deploying hundreds across multiple channels (e.g., web, mobile, IVR), LMOS dynamically scales to meet the demands of your enterprise while maintaining high performance.

- **Collaborative Agent Ecosystem:**  
   LMOS is designed to enable AI agents to collaborate efficiently, sharing tools, memory, and knowledge much like applications in a traditional operating system. This collaborative approach enhances adaptability, allowing agents to handle complex queries and deliver more accurate, comprehensive solutions.

- **Intelligent Task Management:**  
   LMOS utilizes both language model-based and vector-based approaches for intelligent task routing. Its integrated Runtime and Router ensure that tasks are dynamically assigned to the most suitable AI agents based on their registered capabilities, delivering fast, accurate query resolution and optimal resource utilization.

## Our principles

Our principles — **Openness, Interoperability, Security and Transparency** — guide the development of this platform. We believe that systems built on these principles foster collaboration and innovation.

**Openness:** Embrace open standards and protocols to foster collaboration, innovation, and accessibility within the intelligent agent ecosystem, over closed proprietary systems that hinder collaboration and innovation.

**Interoperability:** Ensure seamless communication and interaction between diverse intelligent agents, allowing them to work across platforms and ecosystems, over isolated systems or lock-ins that restrict interoperability and limit user choice.

**Transparency and Accountability:** Prioritize transparency in agent design, operation, and decision-making processes, promoting user trust and accountability for ethical behavior, over opaque systems that conceal operations and lack accountability.

**Data Privacy and Security:** Uphold robust security measures and privacy standards to protect sensitive data, ensuring user confidentiality and trustworthiness of intelligent agents, over insecure systems that compromise user privacy and expose data to unauthorized access.

## What's the name's origin?

The name **LMOS** stands for **Language Model Operating System**. Just as an operating system abstracts hardware and manages software applications, LMOS abstracts reduces the complexity to develop AI Agents by providing APIs and libraries to interact with infrastructure and tools to manage the lifecycle of AI Agents. 

![Operating System Analogy](/img/os-analogy-light.png#light-mode-only)
![Operating System Analogy](/img/os-analogy-dark.png#dark-mode-only)
