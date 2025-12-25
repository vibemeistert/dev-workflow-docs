# AI Query Log - Learning Documentation

## Purpose

Track AI coding assistant interactions to demonstrate learning progression and problem-solving approaches. This practice serves multiple purposes:

1. **Academic Compliance** - Required documentation for courses mandating AI usage transparency
2. **Learning Reflection** - Capture conceptual breakthroughs and recurring patterns
3. **Portfolio Evidence** - Demonstrate thoughtful AI collaboration vs blind copy-paste
4. **Knowledge Gaps** - Identify topics requiring deeper study

## Format Template

Save queries per assignment/project in a single cumulative document (PDF or Markdown).

```
Project/Assignment Name - Date

Query 1: [Your exact question to AI]
Context: [Why you asked - stuck on syntax? Conceptual confusion? Exploring alternatives?]
Outcome: [Did the answer work? What did you learn?]

Query 2: [Your exact question to AI]
Context: [...]
Outcome: [...]

[5-10 representative queries per project]
```

## Example: C Programming Course

### Homework 1: Hello World Variants

**Query 1:** "How do I print an integer variable in C using printf?"  
**Context:** Needed to display user input from scanf  
**Outcome:** Learned %d format specifier, worked correctly

**Query 2:** "What's the difference between %f and %.2f in printf?"  
**Context:** Wanted to control decimal places in float output  
**Outcome:** Understood precision control, used %.2f for currency formatting

**Query 3:** "Why is my printf output not starting on a new line?"  
**Context:** Output was concatenating on same line  
**Outcome:** Added \n escape sequence, learned about stdout buffering

**Query 4:** "How can I print a string variable using printf in C?"  
**Context:** Transitioning from int to char arrays  
**Outcome:** Used %s format specifier, discovered difference between char and char*

**Query 5:** "Give me an example of using printf with multiple variables (int and float)."  
**Context:** Needed to display mixed data types in single statement  
**Outcome:** Learned format string order matches variable order

### Quiz 1: Control Flow

**Query 1:** "How do I implement a do-while loop in C?"  
**Context:** Needed menu that executes at least once  
**Outcome:** Learned do-while guarantees one execution vs while

**Query 2:** "What's the difference between break and continue in loops?"  
**Context:** Confused about loop control keywords  
**Outcome:** break exits loop entirely, continue skips to next iteration

[...]

### Final Project: Horse Race Simulation

**Query 1:** "How do I read a CSV file line by line in C?"  
**Context:** Needed to import horse race data from external file  
**Outcome:** Used fgets() with strtok() for parsing, handled newline characters

**Query 2:** "What's the best way to store multiple horse records in C?"  
**Context:** Choosing between arrays and linked lists  
**Outcome:** Used struct array for fixed-size data, learned about dynamic allocation for future

[...]

## Best Practices

### What to Log
- Conceptual questions showing learning progression
- Debugging queries with context (error messages, attempted solutions)
- Architecture/design questions for larger projects
- "Why does X work but Y doesn't?" comparisons
- Queries that led to breakthroughs or "aha!" moments

### What to Skip
- Repetitive syntax lookups (one example is enough)
- Copy-paste requests without understanding
- Queries where you didn't read/apply the response

### Storage Strategy

**During Semester:**
```bash
~/code-library/courses/c-intro-50-csc/ai-query-log.md
```

Update after each assignment while context is fresh.

**End of Semester:**
Export to PDF for submission:
```bash
pandoc ai-query-log.md -o ai-query-log.pdf
```

## Academic Integrity Note

This log demonstrates **how you used AI as a learning tool**, not that AI completed your work. Include:
- Your thought process before asking
- How you adapted/debugged the response
- Concepts you still didn't understand (showing critical thinking)

## Integration with Git Workflow

Commit query log updates alongside code commits:

```bash
git add projects/hw1/ ai-query-log.md
git commit -m "feat(hw1): complete case-area calculator, log AI debugging queries"
```

This creates a timeline showing learning progression throughout the course.

## Advanced: Queries for Unused AI

If you completed assignments without AI but still need to demonstrate engagement:

**Advanced Topics for C Course:**
- "Explain how linked lists manage dynamic memory in C"
- "What are the trade-offs between arrays and linked lists for student records?"
- "How do function pointers work in C and when would I use them?"
- "Explain the difference between stack and heap memory allocation"
- "How does malloc() interact with the operating system?"
- "What are some common memory leak patterns in C and how to detect them?"
- "Explain how multi-threading works in C using pthreads"

These show you're exploring beyond course requirements.

## Reference

This format satisfies typical CS course requirements like:
- "Submit 5 queries per assignment showing AI usage"
- "Demonstrate AI as learning aid, not replacement"
- "7% class participation via AI query log"
- "End of semester PDF submission"

Adapt the template to match your instructor's specific requirements.
