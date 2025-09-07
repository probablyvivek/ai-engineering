# AI Agents: A Practical Guide

Look, AI agents sound fancy. But honestly? Most of them are just chatbots with superpowers. Let me show you what really works.

## The Foundation: Your AI Assistant Gets Tools

![The Augmented LLM](https://github.com/probablyvivek/ai-engineering/blob/main/agents/The%20augmented%20LLM.png?raw=true)

Think of this like giving your AI three basic abilities:

- **Retrieval**: "Hey, look this up for me"
- **Tools**: "Go do this thing in another app"  
- **Memory**: "Remember what we talked about"

That's it. Seriously.

```python
# This is what 80% of "AI agents" actually do
def smart_assistant(question):
    # Maybe look something up
    facts = search_database(question)
    
    # Maybe use an API
    api_result = call_external_service(question)
    
    # Maybe remember context
    history = get_conversation_history()
    
    # Combine everything and respond
    response = llm_call(question, facts, api_result, history)
    return response
```

Before you build anything complex, get this working. Most problems? They stop here.

## When You Actually Need Multiple Steps

### Pattern 1: Do This, Then That (Prompt Chaining)

![The Prompt Chaining Workflow](https://github.com/probablyvivek/ai-engineering/blob/main/agents/The%20prompt%20chaining%20workflow.png?raw=true)

Perfect for tasks like: Write email → Check if it sounds professional → Send it

```python
def write_and_send_email(request):
    # Step 1: Write the email
    draft = llm_call("Write email about: " + request)
    
    # Step 2: Check if it's good (the "gate")
    if not sounds_professional(draft):
        return "Email draft failed quality check"
    
    # Step 3: Send it
    result = send_email(draft)
    return result
```

**Use this when**: Your task has clear, predictable steps that always happen in the same order.

### Pattern 2: Sort and Route

![The Routing Workflow](https://github.com/probablyvivek/ai-engineering/blob/main/agents/The%20routing%20workflow.png?raw=true)

Like a really smart receptionist. Different questions go to different specialists.

```python
def handle_customer_question(question):
    # Figure out what kind of question this is
    category = classify_question(question)
    
    # Send to the right specialist
    if category == "billing":
        return billing_expert(question)
    elif category == "technical":
        return tech_support(question)
    else:
        return general_helper(question)
```

**Use this when**: You get very different types of inputs that need totally different responses.

### Pattern 3: Do Multiple Things at Once

![The Parallelization Workflow](https://github.com/probablyvivek/ai-engineering/blob/main/agents/The%20parallelization%20workflow.png?raw=true)

Two flavors:
- **Split the work**: "Check the code for bugs" + "Check the code for style" simultaneously
- **Get multiple opinions**: Ask 3 different AIs the same question, pick the best answer

**Use this when**: You can break work into independent pieces or want multiple perspectives.


### Pattern 4: The Critic and Creator Duo

![The Evaluator-Optimizer Workflow](https://github.com/probablyvivek/ai-engineering/blob/main/agents/The%20evaluator-optimizer%20workflow.png?raw=true)

One AI writes something. Another AI critiques it. First AI improves it. Repeat until it's good enough.

```python
def write_with_feedback(task):
    attempts = 0
    
    while attempts < 5:  # Don't loop forever
        draft = writer_ai(task)
        critique = critic_ai(draft)
        
        if critique.says_its_good:
            return draft
        else:
            task += f"Please improve: {critique.feedback}"
            attempts += 1
    
    return "Couldn't get it right after 5 tries"
```

**Real example**: Writing marketing copy where one AI writes, another checks if it follows brand guidelines.

### Pattern 5: The Manager and Workers

![The Orchestrator-Workers Workflow](https://github.com/probablyvivek/ai-engineering/blob/main/agents/The%20orchestrator-workers%20workflow.png?raw=true)

Manager AI breaks down complex work and assigns pieces to specialist AIs. Then combines all the results.

**Use this when**: You have a complex task that needs multiple types of expertise, but you can't predict upfront exactly what work needs doing.

## True Agents (The Autonomous Ones)

### Basic Agent: Acts on Its Own

![Autonomous Agent](https://github.com/probablyvivek/ai-engineering/blob/main/agents/Autonomous%20agent.png?raw=true)

Now we're talking about AI that actually makes decisions and takes actions without asking permission for every step.

```python
def autonomous_agent(goal):
    steps_taken = 0
    
    while not goal_achieved() and steps_taken < 20:
        # AI decides what to do next
        action = ai_choose_action(goal, current_situation)
        
        # Actually do it
        result = execute_action(action)
        
        # Learn from what happened
        update_understanding(result)
        
        # Maybe ask human for help
        if totally_stuck():
            human_guidance = ask_for_help()
            incorporate_guidance(human_guidance)
        
        steps_taken += 1
    
    return final_result
```

**Warning**: Only use this when:
- The AI can't break anything important
- You have clear ways to measure success/failure
- Human can intervene when needed

### The Ultimate: Coding Agent

![High-Level Flow of a Coding Agent](https://github.com/probablyvivek/ai-engineering/blob/main/agents/High-level%20flow%20of%20a%20coding%20agent.png?raw=true)

This is the most complex pattern, but it works because programming has built-in feedback loops (tests pass or fail).

**Phase 1**: "What exactly do you want me to build?"
**Phase 2**: Write code, run tests, fix issues, repeat until tests pass
**Phase 3**: "Done! Here's what I built."

Works because:
- Clear success criteria (tests pass)
- Immediate feedback (code runs or doesn't)
- Can't cause real damage (just code changes)

## The Reality Check: Start Small

Here's what actually happens in practice:

1. **80% of problems**: Solved with basic augmented LLM
2. **15% of problems**: Need simple workflows (chaining, routing)
3. **4% of problems**: Need intermediate patterns
4. **1% of problems**: Actually need autonomous agents

Don't build a coding agent when you just need to format some data.

## What Makes Agents Actually Work

### Feedback is Everything
The best agents get clear, immediate feedback:
- Code either compiles or doesn't
- Tests pass or fail
- APIs return success/error codes
- Users say "yes, that's right" or "no, try again"

Vague feedback = agent that wanders around confused.

### Design Your Tools Well
If your AI needs to use a calculator, don't make it type "please calculate 2+2" in natural language. Give it a `calculate(2+2)` function that returns `4`.

### Humans Stay in the Loop
Even "autonomous" agents need escape hatches. Always include:
- Maximum number of attempts
- "Ask for help" triggers
- Easy ways for humans to course-correct

## The Framework Question

LangGraph, AutoGen, CrewAI, whatever - they're tools, not magic. Many successful agents are just 50 lines of Python calling OpenAI's API directly.

Start simple. Add frameworks if they genuinely help. But understand what's actually happening underneath.

## Bottom Line

Building effective AI agents isn't about the fanciest architecture. It's about:

1. **Understanding your actual problem**
2. **Starting with the simplest solution that could work**
3. **Adding complexity only when you've proven you need it**
4. **Making sure you can measure success objectively**

Most "agents" are just chatbots with APIs. And honestly? That solves most problems just fine.

Start there. Get it working. Then maybe, maybe consider something fancier.