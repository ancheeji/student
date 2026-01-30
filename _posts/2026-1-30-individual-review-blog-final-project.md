---
layout: post
title: "AI Usage Quest - College Board Create Performance Task"
description: "Complete College Board Create PT documentation with code examples, algorithms, and testing"
permalink: /digital-famine/ai/create-pt-blog/
categories: [CSP, Create-PT, AI-Project]
tags: [college-board, create-pt, algorithms, lists, procedures, testing, ai-usage]
author: "[Your Name]"
date: 2026-01-30
---

# AI Usage Quest - College Board Create PT Blog

**Project:** AI Study Buddy - Teaching Responsible AI Usage

---

## Program Purpose and Function

### Purpose
The **AI Usage Quest** is a web application that teaches high school students how to use AI tools responsibly. It collects student preferences on AI tools (ChatGPT, Claude, Gemini, Copilot) across subjects and facilitates discussions about ethical AI usage.

### Inputs
- Subject preferences: 5 subjects × 4 AI tools (radio buttons)
- AI usage status: Yes/No (radio button)
- Free response: Student thoughts on AI policies (text area)
- User authentication: Session cookies

### Outputs
- Bar charts showing AI tool preferences per subject
- Top 3 recent student thoughts
- "Sensational Surveyor" badge award
- Success confirmation message

---

## List Usage - Managing Complexity

### Code with Lists (Clean)
```python
@survey_api.route('/survey', methods=['POST'])
def submit_survey():
    current_user = User.query.filter_by(id=request.cookies.get('user_id')).first()
    if not current_user:
        return jsonify({"error": "Authentication required"}), 401
    
    data = request.get_json()
    
    # LIST INITIALIZATION
    subjects = ['english', 'math', 'science', 'cs', 'history']
    ai_tools = ['ChatGPT', 'Claude', 'Gemini', 'Copilot']
    
    # Validate with iteration
    for subject in subjects:
        if subject not in data or data[subject] not in ai_tools:
            return jsonify({"error": f"Invalid data for {subject}"}), 400
    
    # LIST USAGE - Store responses
    responses = []
    for subject in subjects:
        response_obj = SurveyResponse(
            user_id=current_user.id,
            subject=subject,
            ai_tool=data[subject],
            uses_ai_schoolwork=data.get('useAI', 'No'),
            frq_response=data.get('frq', '')
        )
        responses.append(response_obj)  # Adding to list
    
    # Bulk save
    db.session.bulk_save_objects(responses)
    db.session.commit()
    
    # Aggregate results
    aggregated_data = aggregate_survey_results(subjects)
    
    return jsonify({"success": True, "data": aggregated_data}), 200
```

### Without Lists (Repetitive - 100+ lines)
```python
# BAD APPROACH - Must repeat for each subject
english_response = SurveyResponse(user_id=user_id, subject='english', ai_tool=data['english'])
math_response = SurveyResponse(user_id=user_id, subject='math', ai_tool=data['math'])
science_response = SurveyResponse(user_id=user_id, subject='science', ai_tool=data['science'])
cs_response = SurveyResponse(user_id=user_id, subject='cs', ai_tool=data['cs'])
history_response = SurveyResponse(user_id=user_id, subject='history', ai_tool=data['history'])

db.session.add(english_response)
db.session.add(math_response)
# ... 3 more adds

# Aggregation needs 20+ variables
english_chatgpt = 0
english_claude = 0
# ... 18 more variables!
```

**Lists manage complexity by:**
- Single loop handles all 5 subjects
- Easy to add new subjects
- Bulk operations instead of 5 commits
- No hard-coded variables

---

## Procedures and Algorithms

### Main Procedure with Parameters
```python
def aggregate_survey_results(subjects):
    """
    PURPOSE: Count AI tool preferences for each subject
    PARAMETER: subjects - list of subject names to process
    RETURNS: Dictionary with counts per tool per subject
    
    ALGORITHM: Sequence + Selection + Iteration
    """
    result = {}
    ai_tools = ['ChatGPT', 'Claude', 'Gemini', 'Copilot']
    
    # ITERATION - Process each subject
    for subject in subjects:
        # Initialize counters
        tool_counts = {tool: 0 for tool in ai_tools}
        
        # Query database
        responses = SurveyResponse.query.filter_by(subject=subject).all()
        
        # NESTED ITERATION - Count each response
        for response in responses:
            # SELECTION - Validate tool
            if response.ai_tool in tool_counts:
                tool_counts[response.ai_tool] += 1  # Accumulation
        
        result[subject] = tool_counts
    
    # Add statistics
    result['useAI'] = get_ai_usage_stats()
    result['frqs'] = get_recent_frqs()
    
    return result


def get_recent_frqs():
    """Retrieves top 3 recent FRQ responses"""
    all_responses = SurveyResponse.query\
        .order_by(SurveyResponse.timestamp.desc())\
        .limit(10)\
        .all()
    
    # LIST COMPREHENSION with filtering
    frq_list = [
        {
            'text': response.frq_response,
            'timestamp': response.timestamp.isoformat(),
            'user_id': response.user.username if response.user else 'anonymous'
        }
        for response in all_responses
        if response.frq_response and len(response.frq_response.strip()) > 0
    ]
    
    return frq_list[:3]  # LIST SLICING


def get_ai_usage_stats():
    """Calculates AI usage statistics"""
    all_responses = SurveyResponse.query.all()
    
    # LIST FILTERING
    yes_responses = [r for r in all_responses if r.uses_ai_schoolwork == 'Yes']
    no_responses = [r for r in all_responses if r.uses_ai_schoolwork == 'No']
    
    return {'Yes': len(yes_responses), 'No': len(no_responses)}
```

**Algorithm Components:**
- **Sequence**: Steps execute in order (init → query → count → store)
- **Selection**: if-statement validates AI tool
- **Iteration**: Nested loops process subjects and responses

---

## Testing and Debugging

### Test Case 1: Valid Submission
```python
# Postman Request
POST http://localhost:8887/api/survey
Content-Type: application/json
Cookie: user_id=student_042

{
  "english": "Claude",
  "math": "ChatGPT",
  "science": "Gemini",
  "cs": "Copilot",
  "history": "Claude",
  "useAI": "Yes",
  "frq": "I believe AI should be used as a learning aid, not a replacement."
}

# Expected Response
{
  "success": true,
  "data": {
    "english": {"ChatGPT": 8, "Claude": 15, "Gemini": 5, "Copilot": 3},
    "useAI": {"Yes": 28, "No": 3},
    "frqs": [...]
  }
}
```

### Bug Fix: Empty FRQ Responses

**Original (Buggy):**
```python
def get_recent_frqs():
    all_responses = SurveyResponse.query.limit(10).all()
    
    # BUG: Returns empty FRQs
    frq_list = [
        {'text': response.frq_response, ...}
        for response in all_responses
    ]
    return frq_list[:3]
```

**Problem:** Frontend displayed blank cards for empty responses

**Fixed:**
```python
def get_recent_frqs():
    all_responses = SurveyResponse.query.limit(10).all()
    
    # FIX: Filter out empty responses
    frq_list = [
        {'text': response.frq_response, ...}
        for response in all_responses
        if response.frq_response and len(response.frq_response.strip()) > 0
    ]
    return frq_list[:3]
```

**Result:** Only displays FRQs with actual content

---

## PPR Questions

### 1. Procedure & Selection
**Q: Identify the first if-statement and explain what happens if it evaluates to false.**

**A:** First conditional: `if not current_user:`

**If False** (user exists): Program continues, validates data, processes survey  
**If True** (user not found): Returns 401 error, no database changes

### 2. Procedural Abstraction
**Q: Identify parameters and explain how they manage complexity.**

**A:** Parameter: `subjects` in `aggregate_survey_results(subjects)`

**Manages complexity by:**
- Works with ANY list of subjects (not hard-coded)
- Reusable for different scenarios:
  ```python
  aggregate_survey_results(['english', 'math'])  # Core only
  aggregate_survey_results(['english', 'math', 'science', 'cs', 'history'])  # All
  ```
- No need for separate functions per subject

### 3. Procedure Calls
**Q: Write two different procedure calls.**

**A:**
```python
# Call 1: All subjects (5 iterations)
all_subjects = ['english', 'math', 'science', 'cs', 'history']
full_data = aggregate_survey_results(all_subjects)

# Call 2: Core only (3 iterations)
core_subjects = ['english', 'math', 'science']
core_data = aggregate_survey_results(core_subjects)
```

Different execution: Call 1 does 5 database queries, Call 2 does 3

### 4. Logic Error
**Q: Describe a modification that causes a logic error.**

**A:** Moving `.limit(3)` before filtering:

```python
# BUGGY: Limit BEFORE filter
def get_recent_frqs():
    all_responses = SurveyResponse.query.limit(3).all()  # Gets first 3
    frq_list = [... for r in all_responses if r.frq_response]  # Filters
    return frq_list

# If first 2 are empty, returns only 1 FRQ instead of 3!
```

**Impact:** Frontend expects 3 responses but might get 0-3 depending on empty entries

### 5. List Utilization
**Q: Explain how code uses a list.**

**A:** Lists used for:
- **Adding**: `responses.append(response_obj)`
- **Accessing**: `frq_list[0]` (first FRQ)
- **Traversing**: `for response in all_responses`
- **Slicing**: `frq_list[:3]` (top 3)

Without lists: Would need 50+ separate variables for FRQs

### 6. Algorithm Analysis
**Q: Describe the algorithm in your iteration.**

**A:** `aggregate_survey_results()` algorithm:

**Input:** `['english', 'math']`  
**Database:** English has [Claude, Claude, ChatGPT], Math has [ChatGPT, ChatGPT, Claude]

**Execution:**
```
Iteration 1: subject='english'
  - Initialize: {'ChatGPT': 0, 'Claude': 0, 'Gemini': 0, 'Copilot': 0}
  - Process responses: Claude→1, Claude→2, ChatGPT→1
  - Result: {'ChatGPT': 1, 'Claude': 2, 'Gemini': 0, 'Copilot': 0}

Iteration 2: subject='math'
  - Initialize: {'ChatGPT': 0, 'Claude': 0, 'Gemini': 0, 'Copilot': 0}
  - Process responses: ChatGPT→1, ChatGPT→2, Claude→1
  - Result: {'ChatGPT': 2, 'Claude': 1, 'Gemini': 0, 'Copilot': 0}
```

---

## Database Schema

```sql
CREATE TABLE survey_responses (
    id INTEGER PRIMARY KEY,
    user_id INTEGER NOT NULL,
    subject VARCHAR(50) NOT NULL,
    ai_tool VARCHAR(50) NOT NULL,
    uses_ai_schoolwork VARCHAR(10),
    frq_response TEXT,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

---

## Summary

✅ **Lists** manage complexity (subjects, responses, frq_list)  
✅ **Procedures** with parameters (aggregate_survey_results)  
✅ **Algorithm** uses sequence, selection, iteration  
✅ **Testing** includes bug fixes with before/after code  
✅ **No hard-coded data** - all dynamic database queries