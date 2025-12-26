# ChatGPT Conversation Extraction for AI Query Logs

## Purpose
Extract best queries from ChatGPT conversation history to populate an AI query log that demonstrates learning progression and academic integrity (Assignment 7 format).

## Step 1: Export Your ChatGPT Data

1. Go to [ChatGPT Settings](https://chat.openai.com/settings)
2. Navigate to **Data Controls** → **Export Data**
3. Click **Export** and wait for email (typically 24 hours)
4. Download `conversations.json` from the link in your email

## Step 2: Extraction Prompt for ChatGPT

Once you have `conversations.json`, use this prompt with ChatGPT (or Claude) to extract relevant queries:

```
I need to create an AI query log for my C programming course (Assignment 7, 7% participation grade).

I've attached my ChatGPT conversation history (conversations.json). Please extract 5-10 of the BEST queries from conversations related to:
- C programming assignments (HW1-HW6, Midterm, Final)
- Debugging C code
- Understanding C concepts (pointers, memory, structs, file I/O)
- Build systems (Makefiles, compilation)

For each query, provide:
1. The exact question I asked
2. Context: What assignment/problem I was working on
3. Outcome: What I learned or how it helped

Format the output as markdown following this template:

### Assignment Name - Date
**Query 1:** "Exact question here"
**Context:** Why I asked this question
**Outcome:** What I learned

Focus on queries that show:
- Learning progression (not just syntax lookups)
- Conceptual breakthroughs
- Debugging with context
- "Why does X work but Y doesn't?" comparisons

Skip:
- Repetitive syntax lookups
- Copy-paste requests
- Queries where I didn't engage with the response
```

## Step 3: Manual Alternative (No Export Available)

If you can't export data, manually review your ChatGPT history:

1. **Open ChatGPT** and navigate to your conversations
2. **Search by keyword:** "C programming", "malloc", "pointer", "segmentation fault", etc.
3. **Filter by date range:** Match your course semester (e.g., Sep-Dec 2025)
4. **Copy representative queries** into the AI-QUERY-LOG.md template

### Quick Extraction Template

For each assignment, ask yourself:
- What was I stuck on? → **Query + Context**
- What did AI explain that clicked? → **Outcome**
- What errors did I debug with AI? → **Query + Context**
- What concepts did I explore beyond requirements? → **Advanced queries**

## Step 4: Format for Submission

### During Semester (Markdown)
Save as `ai-query-log.md` in your course directory:

```markdown
# AI Query Log - C Programming (CSc 50)

## Homework 1: Basic I/O
**Query 1:** "How do I print an integer variable in C using printf?"
**Context:** Needed to display user input from scanf
**Outcome:** Learned %d format specifier, worked correctly

[Continue with 4-9 more queries for HW1]

## Quiz 1: Control Flow
[...]

## Final Project: Horse Race Simulation
[...]
```

### End of Semester (PDF)
Convert to PDF for submission:

```bash
cd ~/code-library/courses/c-intro-50-smc/007-assignment-7/
pandoc ai-query-log.md -o ai-query-log.pdf
```

Or export as PDF from VSCode:
1. Open `ai-query-log.md` in VSCode
2. Right-click → **Markdown: Open Preview**
3. Right-click preview → **Export as PDF**

## Example Query Categories

### Strong Examples (Include These)
- **Conceptual:** "What's the difference between stack and heap memory allocation?"
- **Debugging:** "Why is my linked list causing a segmentation fault when I free() nodes?"
- **Design:** "Should I use an array or linked list to store student records in this assignment?"
- **Breakthrough:** "How do function pointers work in C callbacks?"

### Weak Examples (Skip These)
- **Syntax only:** "How to declare an array in C?"
- **No engagement:** "Write a function to reverse a string" (if you just copied result)
- **Repetitive:** Multiple queries about the same printf format specifier

## Integration with Git Workflow

As you populate the log, commit incrementally:

```bash
cd ~/code-library/courses/c-intro-50-smc/
git add 007-assignment-7/ai-query-log.md
git commit -m "docs: add HW1-HW3 AI queries to participation log"
```

This shows progression throughout the semester rather than a last-minute dump.

## Academic Integrity Note

This log demonstrates **how you used AI as a learning tool**, not that AI completed your work. Your instructor wants to see:

✅ Your thought process before asking  
✅ How you adapted/debugged the response  
✅ Concepts you still didn't understand (critical thinking)  
✅ Learning progression across assignments  

❌ AI writing your code without your understanding  
❌ Copy-paste without modification  
❌ No evidence of debugging or iteration  

## Reference

This format satisfies typical CS course requirements like:
- "Submit 5 queries per assignment showing AI usage"
- "Demonstrate AI as learning aid, not replacement"
- "7% class participation via AI query log"
- "End of semester PDF submission"

See [AI-QUERY-LOG.md](AI-QUERY-LOG.md) for full template and examples.
