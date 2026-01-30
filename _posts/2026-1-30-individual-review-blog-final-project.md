---
layout: post
title: "AI Study Buddy - Prompt Engineering Challenge"
description: "College Board CPT: Interactive quiz teaching responsible AI usage through prompt engineering"
permalink: /skill-b-submodule-3/
categories: [CPT, Backend, JavaScript]
tags: [flask, api, procedures, algorithms, lists]
author: "Your Name"
date: 2025-01-30
---

## Program Purpose & Function

**Purpose**: Educational platform teaching students to craft effective AI prompts through interactive quizzes with performance tracking and concept understanding.

**Inputs**: Player name, answer selections, time remaining, feedback ratings, concept understanding clicks  
**Outputs**: Score calculations, performance analytics, badge awards, concept tracking data

---

## Backend: Data Abstraction & Procedures

### List Usage - Question Bank Management

```python
# FILE: model/prompt_questions.py
from __init__ import db

class PromptQuestion(db.Model):
    __tablename__ = 'prompt_questions'
    
    id = db.Column(db.Integer, primary_key=True)
    subject = db.Column(db.String(50), nullable=False)
    scenario = db.Column(db.Text, nullable=False)
    question_data = db.Column(db.JSON, nullable=False)  # Stores options list
    correct_answer = db.Column(db.Integer, nullable=False)
    explanation = db.Column(db.Text)
    
    @staticmethod
    def get_questions_by_subject(subject_filter=None):
        """LIST: Returns filtered list of questions"""
        if subject_filter:
            questions = PromptQuestion.query.filter_by(subject=subject_filter).all()
        else:
            questions = PromptQuestion.query.all()
        
        return [q.to_dict() for q in questions]  # List comprehension
    
    def to_dict(self):
        return {
            'id': self.id,
            'subject': self.subject,
            'scenario': self.scenario,
            'options': self.question_data.get('options', []),
            'correctAnswer': self.correct_answer,
            'explanation': self.explanation
        }
```

**Why lists matter**: Without this list, we'd need separate queries for each question type. The list enables dynamic filtering, shuffling, and flexible question selection across subjects.

---

### Procedure with Algorithm (Sequence, Selection, Iteration)

```python
# FILE: prompt_game_api.py

def calculate_performance_metrics(subject_scores, subject_counts):
    """
    PARAMETERS:
        - subject_scores: dict of correct answers per subject
        - subject_counts: dict of total questions attempted
    RETURNS: Performance analysis with weak area recommendations
    """
    
    # INITIALIZATION (List)
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
            performance[subject] = {
                'correct': correct,
                'total': total,
                'percentage': round(percentage, 2)
            }
            
            # NESTED SELECTION: Identify weak areas
            if percentage < 50:
                weak_areas.append({
                    'subject': subject,
                    'percentage': percentage,
                    'recommendation': f"Review {subject} strategies"
                })
        else:
            performance[subject] = {'correct': 0, 'total': 0, 'percentage': 0}
    
    # SEQUENCE: Sort by lowest percentage
    weak_areas.sort(key=lambda x: x['percentage'])
    
    return {'performance': performance, 'weak_areas': weak_areas}
```

---

## Frontend: JavaScript Procedure Calls

### Procedure 1: Load Questions from Backend

```javascript
// FILE: frontend submodule_3 HTML (embedded JavaScript)

let allQuestions = [];  // LIST INITIALIZATION
let gameQuestions = [];
let currentQuestionIndex = 0;

// PROCEDURE with no parameters
async function loadQuestionsFromBackend() {
    try {
        const response = await fetch(`${API_URL}/questions`);
        
        // SELECTION: Validate response
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();  // OUTPUT
        
        // SELECTION: Check if data exists
        if (data.success && data.questions) {
            allQuestions = data.questions;  // LIST ASSIGNMENT
            return true;
        } else {
            throw new Error('Invalid response format');
        }
    } catch (error) {
        console.error('Error loading questions:', error);
        alert('Failed to load questions from server.');
        return false;
    }
}

// PROCEDURE CALL in game start
async function startGame() {
    playerName = document.getElementById('playerName').value.trim();
    
    // SELECTION: Validate input
    if (!playerName) {
        alert('Please enter your name!');
        return;
    }

    // SELECTION: Check if questions loaded
    if (allQuestions.length === 0) {
        const loaded = await loadQuestionsFromBackend();  // PROCEDURE CALL
        if (!loaded) return;
    }

    gameQuestions = shuffleArray(allQuestions);  // PROCEDURE CALL with parameter
    loadQuestion();  // PROCEDURE CALL
}
```

### Procedure 2: Shuffle and Display Questions

```javascript
// PROCEDURE with PARAMETER (list)
function shuffleArray(array) {
    const newArray = [...array];  // Create copy of list
    
    // ITERATION: Fisher-Yates shuffle algorithm
    for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        
        // SEQUENCE: Swap elements
        [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
    }
    
    return newArray;  // OUTPUT: Shuffled list
}

// PROCEDURE: Display current question
function loadQuestion() {
    const question = gameQuestions[currentQuestionIndex];  // LIST ACCESS
    selectedAnswer = null;
    questionStartTime = Date.now();

    // Update display
    document.getElementById('currentQuestion').textContent = currentQuestionIndex + 1;
    document.getElementById('scenario').textContent = question.scenario;

    // PROCEDURE CALL with parameter (list)
    const shuffledOptions = shuffleArray(question.options);
    
    // ITERATION: Create HTML for each option in list
    const optionsHtml = shuffledOptions.map((option, index) => `
        <div class="prompt-option" onclick="selectAnswer(${index})">
            <div class="prompt-option-label">Option ${String.fromCharCode(65 + index)}</div>
            <div class="prompt-text">${option.text}</div>
        </div>
    `).join('');

    document.getElementById('promptOptions').innerHTML = optionsHtml;
    window.currentOptions = shuffledOptions;  // Store for validation
    
    startTimer();  // PROCEDURE CALL
}
```

### Procedure 3: Calculate Score with Time Bonus

```javascript
// PROCEDURE with PARAMETERS
function checkAnswer(optionIndex) {
    clearInterval(timerInterval);

    const question = gameQuestions[currentQuestionIndex];  // LIST ACCESS
    const selectedOption = optionIndex >= 0 ? window.currentOptions[optionIndex] : null;
    const isCorrect = selectedOption && selectedOption.isCorrect;

    let pointsEarned = 0;
    
    // SELECTION: Calculate points if correct
    if (isCorrect) {
        // SEQUENCE: Base points + time bonus
        const timeBonus = Math.floor(timeLeft / 3);
        pointsEarned = 100 + timeBonus;
        score += pointsEarned;

        subjectScores[question.subject]++;  // Update subject tracking
    }
    
    subjectCounts[question.subject]++;  // Increment total

    // Update display
    document.getElementById('currentScore').textContent = score;

    // PROCEDURE CALL with parameter
    displayFeedback(isCorrect, pointsEarned, selectedOption);
}

// PROCEDURE: Display feedback to user
function displayFeedback(correct, points, option) {
    const feedback = document.getElementById('feedback');
    
    // SELECTION: Show different messages based on correctness
    if (correct) {
        feedback.className = 'feedback correct show';
        feedback.innerHTML = `
            <strong>✓ Correct! (+${points} points)</strong><br>
            ${option.explanation}
        `;
    } else {
        feedback.className = 'feedback incorrect show';
        feedback.innerHTML = `
            <strong>✗ Incorrect</strong><br>
            ${option ? option.explanation : 'Time\'s up!'}
        `;
    }

    document.getElementById('nextBtn').style.display = 'inline-block';
}
```

### Procedure 4: Load AI Concepts (Check for Understanding)

```javascript
// PROCEDURE: Fetch and display concepts list
async function loadConceptsData() {
    const conceptsContainer = document.getElementById("conceptsResultModal");
    
    try {
        const response = await fetch(`${pythonURI}/api/jokes/`, fetchOptions);
        
        // SELECTION: Validate response
        if (response.status !== 200) {
            displayError('Failed to load concepts');
            return;
        }
        
        const data = await response.json();  // OUTPUT: List of concepts
        conceptsContainer.innerHTML = '';

        // ITERATION: Process each concept in list
        for (const row of data) {
            const tr = document.createElement("tr");

            // Create concept text cell
            const concept = document.createElement("td");
            concept.innerHTML = row.id + ". " + row.joke;

            // Create "Got It" button with click handler
            const gotIt = document.createElement("td");
            const gotItBtn = document.createElement('button');
            gotItBtn.id = 'haha' + row.id;
            gotItBtn.innerHTML = row.haha;  // Display count
            gotItBtn.onclick = function () {
                // PROCEDURE CALL with parameters
                conceptReaction('haha', `${pythonURI}/api/jokes/like/${row.id}`, gotItBtn.id);
            };
            gotIt.appendChild(gotItBtn);

            // Create "Need Help" button
            const needHelp = document.createElement("td");
            const needHelpBtn = document.createElement('button');
            needHelpBtn.id = 'boohoo' + row.id;
            needHelpBtn.innerHTML = row.boohoo;
            needHelpBtn.onclick = function () {
                conceptReaction('boohoo', `${pythonURI}/api/jokes/jeer/${row.id}`, needHelpBtn.id);
            };
            needHelp.appendChild(needHelpBtn);

            tr.appendChild(concept);
            tr.appendChild(gotIt);
            tr.appendChild(needHelp);
            conceptsContainer.appendChild(tr);
        }
    } catch (err) {
        displayError(err);
    }
}

// PROCEDURE with PARAMETERS: Update concept reaction count
function conceptReaction(type, postURL, elemID) {
    const options = {
        ...fetchOptions,
        method: 'PUT',
    };

    fetch(postURL, options)
        .then(response => response.json())
        .then(data => {
            // SELECTION: Update correct counter based on type
            if (type === 'haha') {
                document.getElementById(elemID).innerHTML = data.haha;
            } else if (type === 'boohoo') {
                document.getElementById(elemID).innerHTML = data.boohoo;
            }
        })
        .catch(err => console.error(err));
}
```

---

## Testing & Debugging

### Test Case: Different Procedure Calls

```javascript
// CALL 1: Perfect score - all correct answers
checkAnswer(0);  // Correct option selected
// Executes: isCorrect = true, calculates full points + time bonus

// CALL 2: Incorrect answer
checkAnswer(2);  // Wrong option selected  
// Executes: isCorrect = false, no points added, shows explanation

// CALL 3: Timeout (no selection)
checkAnswer(-1);  // Time expired
// Executes: isCorrect = false, shows "Time's up!" message
```

### Bug Fixed: Question Loading Race Condition

**Original Error**:
```javascript
// ❌ Bug: Tried to access empty list
function startGame() {
    loadQuestionsFromBackend();  // Async but not awaited!
    gameQuestions = shuffleArray(allQuestions);  // allQuestions is empty!
}
```

**Problem**: `allQuestions` was still empty when `shuffleArray()` tried to use it, causing no questions to display.

**Fix**:
```javascript
// ✅ Fixed: Proper async/await sequencing
async function startGame() {
    if (allQuestions.length === 0) {
        const loaded = await loadQuestionsFromBackend();  // Wait for data
        if (!loaded) return;  // Exit if loading failed
    }
    gameQuestions = shuffleArray(allQuestions);  // Now has data
}
```

---

## PPR Questions

**Procedure**: `loadQuestionsFromBackend()` - fetches question data  
**Call**: `await loadQuestionsFromBackend()` in `startGame()`  
**List Init**: `let allQuestions = []` - stores all available questions  
**List Use**: `gameQuestions[currentQuestionIndex]` - accesses current question

**Boolean Expression**: `if (data.success && data.questions)` checks both API success flag AND list existence  
**If False**: Throws error, preventing game from starting with invalid data

**Parameters Manage Complexity**: `shuffleArray(array)` works with any list - questions, options, or items. Single procedure handles all randomization needs instead of separate shuffle functions.

---

## Conclusion

This program demonstrates procedural abstraction through reusable API calls, manages complexity with lists for question banks and options, and implements algorithms with proper sequencing (score calculation), selection (conditionals for correctness), and iteration (processing concept lists).