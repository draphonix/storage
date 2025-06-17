# Memory Generation Template for Mem0 MCP OpenMemory

[[LLM: You are the Memory Curator persona. Your role is to detect memory-worthy conversations, guide template selection, extract information, and generate properly formatted memories for Mem0 MCP OpenMemory. Monitor conversations for trigger patterns and proactively offer memory creation when appropriate.]]

## Memory Detection Triggers

[[LLM: Watch for these conversation patterns and automatically offer memory creation:

EXPLICIT REQUESTS:
- "save this conversation as a memory"
- "document this solution" 
- "add this to our knowledge base"
- "create a memory for this"
- "this should be remembered"

IMPLICIT SIGNALS:
- Problem resolution: "fixed it", "working now", "solution found"
- Learning moments: "learned that", "discovered", "insight"
- Process completion: "steps are", "workflow is", "procedure"
- Integration success: "connected", "API working", "integration complete"

When detected, say: "I notice this conversation contains valuable knowledge. Would you like me to create a structured memory document using the [TEMPLATE_TYPE] template?"]]

## Template Selection Logic

[[LLM: Analyze conversation content and select appropriate template:

BUG_FIX: Keywords: bug, error, issue, problem, fix, resolved, troubleshooting
INTEGRATION: Keywords: API, integrate, connect, authentication, oauth, webhook  
LEARNING: Keywords: learned, discovered, insight, tip, best practice
PROCESS: Keywords: process, workflow, steps, procedure, how-to
INFRASTRUCTURE: Keywords: deploy, server, infrastructure, configuration, environment

Announce your selection: "Based on this conversation about [TOPIC], I'll use the [TEMPLATE_TYPE] template."]]

## Information Gathering

[[LLM: Before generating memory, collect required information. Ask for missing details:
- Project name: "What project is this for?"
- Author: "Who should be credited as the author?"
- Technologies: "What technologies should I tag this with?"
- Module/feature: "Is this for a specific module or feature?"

If information is available in conversation, extract automatically.]]

## Memory Template Generation

[[LLM: Generate memory using the appropriate template below. Replace all {{placeholders}} with extracted information. Follow the exact format for Mem0 compatibility.]]

^^CONDITION: bug_fix_template^^

---
title: "Fixed: {{problem_summary}}"
author: "{{author_name}}"
creation_date: "{{current_date}}"
last_updated_date: "{{current_date}}"
status: "Active"
project_name: "{{project_name}}"
module_or_feature: "{{module_name}}"
technology_stack: "{{tech_stack_comma_separated}}"
tags: "problem-type:bug, {{tech_tags}}, {{area_tags}}"
related_memory_ids: ""
---

## Problem Summary
{{clear_problem_description}}

## Failed Attempts
<<REPEAT: failed_attempt>>
### Attempt {{attempt_number}}:
**Approach:** {{approach_description}}
**Outcome & Analysis:** {{why_it_failed}}
<</REPEAT>>

## Successful Solution
**Description:** {{solution_description}}

**Implementation Steps:**
<<REPEAT: solution_step>>
{{step_number}}. {{step_description}}
<</REPEAT>>

^^CONDITION: has_code^^
**Code Snippets:**
```{{language}}
{{code_example}}
```
^^/CONDITION: has_code^^

**Verification:** {{how_to_verify_fix}}

## Key Takeaway
{{primary_learning_one_sentence}}

^^/CONDITION: bug_fix_template^^

^^CONDITION: integration_template^^

---
title: "Integration: {{service_name}} via {{auth_method}}"
author: "{{author_name}}"
creation_date: "{{current_date}}"
last_updated_date: "{{current_date}}"
status: "Active"
project_name: "{{project_name}}"
technology_stack: "{{tech_stack_comma_separated}}"
tags: "problem-type:integration, {{service_tags}}, {{tech_tags}}"
related_memory_ids: ""
---

## Integration Overview
**Service/API:** {{service_description}}
**Purpose:** {{integration_purpose}}
**Authentication:** {{auth_method_details}}

## Problem Summary
{{integration_context_and_requirements}}

## Implementation Process
{{challenges_and_resolution_process}}

## Successful Solution
**Configuration:**
{{required_settings_and_env_vars}}

**Implementation Steps:**
<<REPEAT: implementation_step>>
{{step_number}}. {{step_description}}
<</REPEAT>>

^^CONDITION: has_code^^
**Code Examples:**
```{{language}}
{{integration_code}}
```
^^/CONDITION: has_code^^

## Key Takeaways
**Primary Learning:** {{main_integration_insight}}
**Best Practices:** {{recommendations_for_similar_integrations}}

^^/CONDITION: integration_template^^

^^CONDITION: learning_template^^

---
title: "Learning: {{key_insight}}"
author: "{{author_name}}"
creation_date: "{{current_date}}"
last_updated_date: "{{current_date}}"
status: "Active"
project_name: "{{project_name}}"
technology_stack: "{{relevant_technologies}}"
tags: "solution-type:learning, {{concept_tags}}, {{tech_tags}}"
related_memory_ids: ""
---

## Discovery Context
{{situation_where_learning_occurred}}

## Key Learning
{{main_insight_or_discovery}}

## Practical Applications
**How to Apply:**
{{when_and_how_to_use_this_knowledge}}

**Examples:**
<<REPEAT: example>>
{{example_description}}
<</REPEAT>>

^^CONDITION: has_code^^
```{{language}}
{{code_example}}
```
^^/CONDITION: has_code^^

## Related Concepts
{{connected_ideas_and_principles}}

## Future Applications
{{how_knowledge_applies_to_upcoming_work}}

^^/CONDITION: learning_template^^

^^CONDITION: process_template^^

---
title: "Process: {{process_name}}"
author: "{{author_name}}"
creation_date: "{{current_date}}"
last_updated_date: "{{current_date}}"
status: "Active"
project_name: "{{project_name}}"
technology_stack: "{{tools_and_technologies}}"
tags: "category:process, {{workflow_tags}}, {{tool_tags}}"
related_memory_ids: ""
---

## Process Overview
{{what_process_accomplishes_and_when_to_use}}

## Prerequisites
<<REPEAT: prerequisite>>
- {{prerequisite_description}}
<</REPEAT>>

## Step-by-Step Process
<<REPEAT: process_step>>
{{step_number}}. {{detailed_step_description}}
<</REPEAT>>

## Tools & Resources
**Required Tools:** {{list_of_necessary_tools}}
**Documentation:** {{relevant_documentation_links}}

## Common Pitfalls
{{issues_to_avoid_and_prevention}}

## Success Criteria
{{how_to_verify_process_completion}}

^^/CONDITION: process_template^^

^^CONDITION: infrastructure_template^^

---
title: "Infrastructure: {{infrastructure_component}}"
author: "{{author_name}}"
creation_date: "{{current_date}}"
last_updated_date: "{{current_date}}"
status: "Active"
project_name: "{{project_name}}"
technology_stack: "{{infrastructure_technologies}}"
tags: "category:infrastructure, {{deployment_tags}}, {{tech_tags}}"
related_memory_ids: ""
---

## Infrastructure Overview
{{infrastructure_purpose_and_scope}}

## Configuration Details
{{infrastructure_configuration_specifics}}

## Implementation Steps
<<REPEAT: infrastructure_step>>
{{step_number}}. {{implementation_step_description}}
<</REPEAT>>

## Monitoring & Validation
{{monitoring_and_validation_procedures}}

## Troubleshooting
{{common_issues_and_solutions}}

## Key Takeaways
**Infrastructure Insights:** {{key_infrastructure_learnings}}
**Best Practices:** {{infrastructure_best_practices}}

^^/CONDITION: infrastructure_template^^

## Memory Presentation and Saving

[[LLM: After generating the memory, present it with these instructions:

"Here's your structured memory document, formatted for Mem0 MCP OpenMemory:

[DISPLAY THE COMPLETE FORMATTED MEMORY]

**To save this memory to Mem0:**
1. Copy the entire formatted text above (including the --- markers)
2. In your MCP-compatible client (Cursor, Claude Desktop, etc.), use:
   `add_memories("paste the copied text here")`
3. The memory will be saved with proper metadata and tags for easy retrieval

**Memory Summary:**
- Type: [TEMPLATE_TYPE]
- Project: [PROJECT_NAME] 
- Technologies: [TECH_LIST]
- Tags: [TAG_LIST]

Would you like me to make any adjustments before you save it?"]]

## Quality Validation

[[LLM: Before presenting the final memory, verify:
- Title is descriptive and under 80 characters
- All mandatory fields are populated
- Tags follow established conventions (category:value format)
- Content sections are comprehensive and actionable
- Key takeaway is clearly stated
- Format is compatible with Mem0 MCP OpenMemory structure

If any issues are found, correct them before presentation.]]
