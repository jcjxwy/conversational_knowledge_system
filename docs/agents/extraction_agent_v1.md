Extraction Agent Contract — v1

1. 1. Purpose

The Extraction Agent is responsible for transforming a raw dialogue session into structured knowledge that conforms strictly to a provided template schema.
The agent behaves as a pure extraction and transformation component:
It does not generate new knowledge
It does not store data
It does not modify templates
Its sole responsibility is to extract structured information from dialogue.

2. Inputs

The agent receives two inputs: raw dialogue data and a template definition.

2.1 Raw Dialogue Input
{
  "topic": "machine_learning_notes",
  "session_id": "session_001",
  "messages": [
    { "role": "user", "content": "..." },
    { "role": "assistant", "content": "..." }
  ]
}

Rules:
Messages are ordered chronologically
Content may include irrelevant or conversational text
The agent must infer relevant knowledge solely from message content

2.2 Template Definition
{
  "version": 1,
  "template_type": "dynamic_map",
  "description": "Key ML concepts discussed in dialogue",
  "entry_schema": {
    "summary": "string",
    "examples": "string"
  }
}

Notes:
template_type = dynamic_map means keys are decided dynamically by the agent
entry_schema defines the required fields for each extracted entry
The agent must strictly adhere to the template

3. Output Schema (Strict)

The agent must output valid JSON only, with no surrounding text, comments, or formatting.
{
  "knowledge_points": {
    "<dynamic_key>": {
      "summary": "string",
      "examples": "string"
    }
  }
}

Output Rules

<dynamic_key> must be a concise, normalized concept name
Values must exactly match the entry_schema
No additional fields are allowed
The root key must be knowledge_points

4. Behavioral Rules

The agent must:
Extract only information explicitly present in the dialogue
Identify distinct knowledge points relevant to the topic
Merge repeated mentions into a single entry
Use concise, neutral, factual language

The agent must not:
Invent or hallucinate new concepts
Add explanatory text outside the defined schema
Reference the prompt, instructions, or itself
Include raw dialogue content in the output

5. Missing Information & Edge Cases

If a required field cannot be inferred, use an empty string ("")
If no relevant knowledge points are found, return:
{
  "knowledge_points": {}
}
This output is considered valid.

6. Determinism & Validation
The agent output must be deterministic given the same inputs
All outputs must be validated against the template schema before storage
Invalid outputs should be rejected and retried with stricter instructions

7. Versioning

This contract applies to template_version = 1
Breaking changes require a new contract version (e.g. ExtractionAgent_v2)
Previously processed data must remain immutable
Schema migrations must be handled by separate migration agents

8. Non-Goals

The Extraction Agent is not responsible for:
Template creation or modification
Data storage or persistence
PDF export or visualization
Ranking or scoring importance
Embedding generation

9. Summary

This contract ensures the Extraction Agent behaves as a controlled, auditable system component, enabling reliable downstream processing such as storage, migration, and document generation.