---
layout: post
title: "AI Study Buddy - Prompt Engineering Challenge"
description: "College Board CPT: Interactive quiz teaching responsible AI usage through prompt engineering"
permalink: /skill-b-submodule-3/
categories: [CPT, Backend, JavaScript]
tags: [flask, api, procedures, algorithms, lists]
author: "Michelle Ji"
date: 2025-01-30
---

## Program Purpose & Function

**Purpose**: Educational platform teaching students to craft effective AI prompts through interactive quizzes with performance tracking and concept understanding.

**Inputs**: Player name, answer selections, time remaining, concept understanding clicks  
**Outputs**: Score calculations, performance analytics, badge awards, concept tracking data

---

## Data Abstraction - List Usage

```python
# FILE: model/prompt_questions.py
@staticmethod
def get_questions_by_subject(subject_filter=None):
    """LIST: Returns filtered list of questions"""
    if subject_filter:
        questions = PromptQuestion.query.filter_by(subject=subject_filter).all()
    else:
        questions = PromptQuestion.query.all()
    
    return [q.to_dict() for q in questions]  # List comprehension
```

**Why lists matter**:
- Uses **selection** (if statement) to filter by subject or return all
- **List comprehension** converts database objects to dictionaries
- Without lists: separate queries needed for each question type
- Manages complexity through single filterable structure

---

## Algorithm Implementation (Sequence, Selection, Iteration)

```python
# FILE: prompt_game_api.py
def calculate_performance_metrics(subject_scores, subject_counts):
    subjects = ['math', 'science', 'history', 'cs']
    performance = {}
    weak_areas = []  # List to track subjects needing improvement
    
    # ITERATION: Process each subject
    for subject in subjects:
        total = subject_counts.get(subject, 0)
        correct = subject_scores.get(subject, 0)
        
        # SELECTION: Check if attempted (Boolean: total > 0)
        if total > 0:
            # SEQUENCE: Calculate percentage
            percentage = (correct / total) * 100
            performance[subject] = {'correct': correct, 'total': total, 'percentage': round(percentage, 2)}
            
            # NESTED SELECTION: Identify weak areas
            if percentage < 50:
                weak_areas.append({'subject': subject, 'percentage': percentage})
    
    # SEQUENCE: Sort by lowest percentage
    weak_areas.sort(key=lambda x: x['percentage'])
    return {'performance': performance, 'weak_areas': weak_areas}
```

**Algorithm breakdown**:
- **Iteration**: Loops through subject list
- **Selection**: Outer if checks if attempted (prevents division by zero), nested if finds weak areas (<50%)
- **Sequence**: Calculate → store → check threshold → append → sort
- `weak_areas` list grows dynamically (0-4 items), eliminates hardcoded variables

---

## Procedural Abstraction - Frontend

### Procedure 1: Load Questions

```javascript
let allQuestions = [];  // LIST INITIALIZATION

async function loadQuestionsFromBackend() {
    const response = await fetch(`${API_URL}/questions`);
    if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
    
    const data = await response.json();
    if (data.success && data.questions) {
        allQuestions = data.questions;  // LIST ASSIGNMENT
        return true;
    }
    throw new Error('Invalid response');
}
```

**Explanation**:
- Reusable procedure fetching from API
- **Selection**: Validates HTTP response and data structure
- Prevents race condition where game starts with empty list

---

### Procedure 2: Shuffle Algorithm

```javascript
function shuffleArray(array) {
    const newArray = [...array];
    // ITERATION: Fisher-Yates shuffle
    for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [newArray[i], newArray[j]] = [newArray[j], newArray[i]];  // SEQUENCE: Swap
    }
    return newArray;
}
```

**Explanation**:
- **Iteration**: For loop processes list backwards
- **Sequence**: Swap operation randomizes elements
- **Procedural abstraction**: Works with any list via parameter (questions, options, items)

---

### Procedure 3: Score Calculation

```javascript
function checkAnswer(optionIndex) {
    const question = gameQuestions[currentQuestionIndex];  // LIST ACCESS
    const selectedOption = optionIndex >= 0 ? window.currentOptions[optionIndex] : null;
    const isCorrect = selectedOption && selectedOption.isCorrect;

    // SELECTION: Calculate points if correct
    if (isCorrect) {
        const timeBonus = Math.floor(timeLeft / 3);  // SEQUENCE
        score += 100 + timeBonus;
        subjectScores[question.subject]++;
    }
    
    displayFeedback(isCorrect, selectedOption);  // PROCEDURE CALL
}
```

**Explanation**:
- **List access**: Gets current question and option
- **Selection**: If correct, calculates score
- **Sequence**: Base points + time bonus (every 3 seconds = 1 point)
- Calls separate `displayFeedback()` procedure (modularity)

---

### Procedure 4: Concept Tracking

```javascript
async function loadConceptsData() {
    const response = await fetch(`${pythonURI}/api/jokes/`);
    const data = await response.json();  // List of concepts
    
    // ITERATION: Process each concept
    for (const row of data) {
        const gotItBtn = document.createElement('button');
        gotItBtn.innerHTML = row.haha;
        gotItBtn.onclick = () => conceptReaction('haha', `/api/jokes/like/${row.id}`, gotItBtn.id);
        // ... create row
    }
}

function conceptReaction(type, postURL, elemID) {
    fetch(postURL, {method: 'PUT'})
        .then(response => response.json())
        .then(data => {
            // SELECTION: Update correct counter
            if (type === 'haha') {
                document.getElementById(elemID).innerHTML = data.haha;
            } else if (type === 'boohoo') {
                document.getElementById(elemID).innerHTML = data.boohoo;
            }
        });
}
```

**Explanation**:
- **Iteration**: For loop creates row for each concept
- **Procedural abstraction**: `type` parameter determines which counter updates
- **Selection**: If/else chooses between "haha" or "boohoo"
- Eliminates duplicate code

---

## Testing & Debugging

### Different Procedure Calls

```javascript
checkAnswer(0);   // CALL 1: Correct - executes scoring logic
checkAnswer(2);   // CALL 2: Incorrect - skips scoring, shows feedback
checkAnswer(-1);  // CALL 3: Timeout - handles edge case
```

### Bug Fixed: Race Condition

```javascript
// ❌ Bug: shuffleArray() ran before data loaded
function startGame() {
    loadQuestionsFromBackend();  // Not awaited
    gameQuestions = shuffleArray(allQuestions);  // Empty!
}

// ✅ Fix: Proper async/await sequencing
async function startGame() {
    if (allQuestions.length === 0) {
        await loadQuestionsFromBackend();  // Wait for data
    }
    gameQuestions = shuffleArray(allQuestions);  // Now has data
}
```

**Explanation**:
- **Logic error**: Function executed before API completed
- **Fix**: Added `await` to pause until loading finishes
- Demonstrates proper **sequencing**: load → validate → use

---

## PPR Questions

**Procedure**: `loadQuestionsFromBackend()`  
**Call**: `await loadQuestionsFromBackend()` in `startGame()`  
**List Init**: `let allQuestions = []`  
**List Use**: `gameQuestions[currentQuestionIndex]`

**Boolean**: `if (data.success && data.questions)` - checks API success AND list existence  
**If False**: Throws error, prevents game from starting with invalid data

**Parameters**: `shuffleArray(array)` works with any list - single procedure handles all randomization

---

## Conclusion

This program demonstrates procedural abstraction through reusable API calls, manages complexity with lists for question banks, and implements algorithms with sequencing, selection, and iteration.