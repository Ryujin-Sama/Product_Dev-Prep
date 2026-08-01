
Create an credit account in Claude and add 5$ and configure API key to the local,
Create a simple py file to echo using Claude's Haiku model (cheap and less tokens)
*Py file will be committed in git.*

There are 3 roles - 
1 - System Role
2 - User Role
3 - Assistant Role 

Basically each message or prompt which is associated with any conversation has to be associated with a role

Here user is the end point who asks questions and stuffs, and assistant is the AI tool, IN short whatever we type on the AI tools using GUI it's considered as a User role
System role is something we ask the AI tool to act as 


### Chat vs Agents

#### Standard Chat 
User sends a message - Claude responds - Done

#### Agentic Loop
User sends a message - Claude reasons and act - Code executes tool - Claude gets info and reasons again - goes back to step 1

This loop repeats until complete

An agent acts, observes and decides autonomously. 

#### The Agentic loop lifecycle

-> Sends Request (Calls API with messages array + tools)  
-> Inspect stop_reason(check if tool_use or end_turn) 
-> Execute (Run local tool if tool_use)
-> Append (add results to history loop to step 1)
if end_turn in step 2 the Exit : Return the final answer.

an agent will pause it's execution for a stop_reason and the stop_reason is of two type - tool_use and end_turn, on end_turn it return the answer and on tool_use it will continue the execution


#### Why the messages array matters

The Claude API is 100% Stateless and every API call must include the complete conversation history 
*Note - The Architect is responsible for state
Missing tools = Hallucinations
Mismatched IDs = Broken loops*

#### How the message array grows across iterations

State 1 -> user msg
State 2 -> user msg -> (Assistant: tool_use)
State 3 -> user msg -> (Assistant: tool_use) -> user: tool_result
State 4 -> user msg -> (Assistant: tool_use) -> user: tool_result -> (Assistant: end_turn)

*Rule 1 -> Append BOTH the tool call AND the tool result*
*Rule 2 -> The tool_use_id MUST perfectly match between them*


#### The two stop_reason values you need to know

1. tool_use -> Status : Claude wants to act, Content: Contains tool blocks, Action: Execute tool, append loop again, Rule: Never return this to the user.
2. end_turn -> Status : Claude has finished, Content: Contains text blocks, Actions: Break Loop, extract text, Rule: This is your final answer.

#### Why the model drives tool selection

##### Model-Driven (Agentic)

- Claude reads the full conversation context 
- Decides which tool to call next based on reasoning
- Can call tools in any order, skip unnecessary ones.
- Intelligence lives in the model

##### Pre-Configured (Non -Agentic)

- Hardcoded if/then configured logic
- Rigid, fixed sequence of tool execution
- Fragile when novel situations or requirements change
- Intelligence lives in the code.


### CHEAT SHEET for this Note

1. ***end_turn*** is the only valid loop exit
2. ***tool_use*** means execute and continue
3. Always append assistant first, then user
4. ***tool_use_id*** must prefectly match
5. The API is stateless, send full history





