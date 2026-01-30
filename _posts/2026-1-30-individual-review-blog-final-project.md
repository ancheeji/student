---
layout: post
title: "AI Study Buddy - Prompt Engineering Challenge"
description: "College Board CPT: Interactive quiz teaching responsible AI usage through prompt engineering"
permalink: /prompt-challenge-cpt/
categories: [CPT, Backend, JavaScript]
tags: [flask, api, procedures, algorithms, lists]
author: "Your Name"
date: 2025-01-30
---

## Program Purpose & Function

**Purpose**: Educational platform teaching students to craft effective AI prompts through interactive quizzes.

**Inputs**: Player name, answer selections, feedback ratings, concept understanding clicks  
**Outputs**: Score calculations, leaderboard rankings, badge awards, performance analytics

---

## Backend: Data Abstraction & Procedures

### List Usage - Leaderboard Management

```python
# FILE: model/prompt_scores.py
from __init__ import db
from sqlalchemy import desc

class PromptScore(db.Model):
    __tablename__ = 'prompt_scores'
    
    id = db.Column(db.Integer, primary_key=True)
    player_name = db.Column(db.String(100), nullable=False)
    score = db.Column(db.Integer, nullable=False)
    correct_answers = db.Column(db.Integer, nullable=False)
    math_score = db.Column(db.Integer, default=0)
    science_score = db.Column(db.Integer, default=0)
    history_score = db.Column(db.Integer, default=0)
    cs_score = db.Column(db.Integer, default=0)
    timestamp = db.Column(db.DateTime, nullable=False)
    
    @staticmethod
    def get_leaderboard(limit=10):
        """LIST: Returns ordered list of top scores"""
        scores = PromptScore.query.order_by(desc(PromptScore.score)).limit(limit).all()
        return [score.read() for score in scores]  # List comprehension
```

**Why lists matter**: Without this list, we'd need 10 separate variables and database queries. The list enables dynamic sorting, iteration, and flexible limit parameters.

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

### Procedure 1: Submit Score to Backend

```javascript
// FILE: frontend submodule_3 HTML (embedded JavaScript)

async function endGame() {
    showScreen('resultsScreen');
    
    // Calculate totals using iteration
    const correctCount = subjectScores.math + subjectScores.science + 
                        subjectScores.history + subjectScores.cs;
    
    // PROCEDURE CALL: Send data to backend API
    await submitScoreToBackend(playerName, score, correctCount, subjectScores);
    
    // Load updated leaderboard
    await updatePersistentLeaderboard();
}

// PROCEDURE with PARAMETERS
async function submitScoreToBackend(name, totalScore, correct, subjects) {
    try {
        const response = await fetch(`${API_URL}/scores`, {
            method: 'POST',
            credentials: 'include',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                playerName: name,           // INPUT
                score: totalScore,          // INPUT
                correctAnswers: correct,    // INPUT
                subjectScores: subjects,    // INPUT (dict)
                timestamp: new Date().toISOString()
            })
        });

        const result = await response.json();  // OUTPUT
        
        // SELECTION: Check if badge awarded
        if (result.badge_awarded === true) {
            handleBadgeAward(result);  // PROCEDURE CALL
        }
        
        return result;
    } catch (error) {
        console.error('Score submission failed:', error);
        return null;
    }
}
```

### Procedure 2: Load and Display Leaderboard

```javascript
// PROCEDURE: Fetch leaderboard (no parameters)
async function updatePersistentLeaderboard() {
    try {
        const response = await fetch(`${API_URL}/leaderboard`);
        
        // SELECTION: Validate response
        if (!response.ok) return;

        const data = await response.json();
        
        // SELECTION: Check if data exists
        if (data.success && data.leaderboard) {
            const tbody = document.getElementById('persistentLeaderboardBody');
            tbody.innerHTML = '';

            // ITERATION: Loop through list (0-9 for top 10)
            for (let i = 0; i < 10; i++) {
                const entry = data.leaderboard[i];  // LIST ACCESS
                const tr = document.createElement('tr');

                // NESTED SELECTION: Determine rank styling
                let rankHtml = '';
                if (i === 0) {
                    rankHtml = `<td class="rank-cell rank-1"><span class="medal">🥇</span> 1</td>`;
                } else if (i === 1) {
                    rankHtml = `<td class="rank-cell rank-2"><span class="medal">🥈</span> 2</td>`;
                } else if (i === 2) {
                    rankHtml = `<td class="rank-cell rank-3"><span class="medal">🥉</span> 3</td>`;
                } else {
                    rankHtml = `<td class="rank-cell">${i + 1}</td>`;
                }

                // SELECTION: Check if entry exists at this position
                if (entry) {
                    tr.innerHTML = `
                        ${rankHtml}
                        <td class="name-cell">${entry.playerName || '-'}</td>
                        <td class="score-cell">${entry.score || '-'}</td>
                    `;
                } else {
                    tr.innerHTML = `${rankHtml}<td>-</td><td>-</td>`;
                }
                
                tbody.appendChild(tr);
            }
        }
    } catch (error) {
        console.error('Leaderboard update failed:', error);
    }
}
```

### Procedure 3: Load AI Concepts (Check for Understanding)

```javascript
// PROCEDURE with PARAMETER
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

// PROCEDURE: Update concept reaction count
function conceptReaction(type, postURL, elemID) {
    const options = {
        ...fetchOptions,
        method: 'PUT',  // Update existing data
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
// CALL 1: High score submission
await submitScoreToBackend("Alice", 850, 10, {
    math: 3, science: 3, history: 2, cs: 2
});
// Executes: Success path, badge check triggers

// CALL 2: Low score submission  
await submitScoreToBackend("Bob", 250, 4, {
    math: 1, science: 0, history: 2, cs: 1
});
// Executes: Success path, different badge logic, shows weak areas
```

### Bug Fixed: Leaderboard Race Condition

**Original Error**:
```javascript
// ❌ Bug: Called before data loaded
async function endGame() {
    updatePersistentLeaderboard();  // Async but not awaited!
    loadLeaderboard();  // Uses stale data
}
```

**Fix**:
```javascript
// ✅ Fixed: Proper async/await sequencing
async function endGame() {
    await submitScoreToBackend(...);
    await updatePersistentLeaderboard();  // Wait for backend
    loadLeaderboard();  // Now has fresh data
}
```

---

## PPR Questions

**Procedure**: `submitScoreToBackend(name, totalScore, correct, subjects)`  
**Call**: `await submitScoreToBackend(playerName, score, correctCount, subjectScores)`  
**List Init**: `const tbody = document.getElementById('persistentLeaderboardBody')`  
**List Use**: `for (let i = 0; i < 10; i++) { const entry = data.leaderboard[i]; }`

**Boolean Expression**: `if (data.success && data.leaderboard)` checks both success flag AND list existence  
**If False**: Function returns early, preventing errors from accessing undefined data

---

## Conclusion

This program demonstrates procedural abstraction through reusable API calls, manages complexity with lists for leaderboard data, and implements algorithms with proper sequencing, selection (conditionals), and iteration (loops).