---
layout: post
title: "AI Study Buddy - Prompt Engineering Challenge"
description: "College Board CPT: Interactive quiz teaching responsible AI usage through prompt engineering"
permalink: /skill-b-submodule-3/
categories: [CPT, Backend, JavaScript]
tags: [flask, api, procedures, algorithms, lists]
author: "Michelle Ji"
date: 2026-2-09
---

## PROGRAM PURPOSE AND FUNCTION

### Purpose
- Interactive game teaching students prompt engineering and responsible AI usage through quizzes

### Function
- Students answer 10 AI/ethics questions → earn points → compete on leaderboard → earn badges

### Inputs
- Login credentials
- Answer selections (multiple choice, drag-drop)
- Feedback ratings (stars, category, comments)

### Outputs
- Real-time score with time bonuses
- Live leaderboard (top 10)
- Badge notifications
- Performance breakdown by subject

---

## LIST INITIALIZATION & USAGE

### List Init (File: `hacks/jokes.py`, Lines 10-65)
```python
jokes_data = [
    {"id": 1, "joke": "Prompt Engineering: ...", "haha": 0, "boohoo": 0},
    {"id": 2, "joke": "Large Language Models: ...", "haha": 0, "boohoo": 0},
    # ... 8 more (10 total AI concepts)
]
```

### List Use (Lines 93-99)
```python
def addJokeHaHa(id):
    for joke in jokes_data:  # Iterate through list
        if joke['id'] == id:
            joke['haha'] += 1  # Update element
            return joke
```
- **Purpose:** Tracks student understanding of AI concepts
- **Usage:** Finds concept by ID, increments "Got It" counter

---

## MANAGING COMPLEXITY

### With List (Good)
```python
jokes_data = [concept1, concept2, ...]  # 1 list

def addJokeHaHa(id):
    for joke in jokes_data:  # 3-line loop
        if joke['id'] == id:
            joke['haha'] += 1
```

### Without List (Bad)
```python
concept1 = {...}  # 10 separate variables
concept2 = {...}

def addJokeHaHa(id):
    if id == 1: concept1['haha'] += 1  # 30-line if-elif chain
    elif id == 2: concept2['haha'] += 1
    # ... 8 more
```

### Why List is Better
- 1 data structure vs 10 variables
- 3-line loop vs 30-line if-elif
- Adding concept #11 = append to list vs modifying 5+ functions

---

## PROCEDURE & ALGORITHM

### Procedure (File: Frontend JS, Lines 400-464)
```javascript
async function updatePersistentLeaderboard() {
    // SEQUENCE: Fetch data
    const response = await fetch(`${API_URL}/leaderboard`);
    
    // SELECTION: Check success
    if (!response.ok) return;
    
    const data = await response.json();
    const tbody = document.getElementById('persistentLeaderboardBody');
    
    // ITERATION: Build 10 rows
    for (let i = 0; i < 10; i++) {
        const entry = data.leaderboard[i];
        
        // SELECTION: Medal for top 3
        if (i === 0) rankHtml = `🥇 1`;
        else if (i === 1) rankHtml = `🥈 2`;
        else if (i === 2) rankHtml = `🥉 3`;
        else rankHtml = `${i + 1}`;
        
        // SELECTION: Data or placeholder
        if (entry) {
            tr.innerHTML = `${rankHtml} | ${entry.playerName} | ${entry.score}`;
        } else {
            tr.innerHTML = `${rankHtml} | - | -`;
        }
        tbody.appendChild(tr);
    }
}
```

### Algorithm Steps
1. **SEQUENCE:** Fetch leaderboard from API
2. **SELECTION:** Validate response
3. **ITERATION:** Loop 10 times
4. **SELECTION:** Assign medals (🥇🥈🥉) for top 3
5. **SELECTION:** Display data if exists, else "-"

### Call (Line 385)
```javascript
updatePersistentLeaderboard();  // Display leaderboard
setInterval(updatePersistentLeaderboard, 30000);  // Refresh every 30s
```

---

## TESTING & DEBUGGING

### Bug #1: Duplicate Incrementing (Check for Understanding)
- **Problem:** Spam-clicking "Got It" buttons sent multiple API requests
- **Input:** User clicks button 5 times rapidly
- **Expected:** Counter increases by 1
- **Actual:** Counter increases by 5

### Debug Process
```javascript
console.log('Button clicked:', elemID, 'Disabled:', el.disabled);
// Output showed: Button clicked 5 times before disabled = true
```

### Before (Buggy)
```javascript
function conceptReactionModal(type, postURL, elemID) {
    const el = document.getElementById(elemID);
    el.disabled = true;  // Too late - already clicked 5 times!
    fetch(postURL);
}
```

### After (Fixed)
```javascript
const _pendingRequests = new Set();

function conceptReactionModal(type, postURL, elemID) {
    const el = document.getElementById(elemID);
    
    if (el.disabled || _pendingRequests.has(elemID)) {
        return;  // Block duplicate clicks
    }
    
    _pendingRequests.add(elemID);
    el.disabled = true;
    fetch(postURL);
}
```

### Result
- ✅ Counter increments by exactly 1
- ✅ Duplicate clicks blocked

---

### Bug #2: Playing Without Login
- **Problem:** Users could access quiz without authentication
- **Expected:** Login required to save scores
- **Actual:** Game accessible to anyone, scores not saved

### Before (Buggy)
```javascript
// No authentication check
document.getElementById('gameStart').style.display = 'block';
```

### After (Fixed)
```javascript
async function checkAuthenticationAndSetup() {
    const response = await fetch(`${pythonURI}/api/id`, {
        credentials: 'include'
    });
    
    if (response.ok) {
        // Logged in - show game
        document.getElementById('gameStart').style.display = 'block';
    } else {
        // Not logged in - require login
        document.getElementById('loginRequired').style.display = 'block';
    }
}
```

### Result
- ✅ Users must log in before playing
- ✅ All scores properly saved to accounts

---

## PPR QUESTIONS

### Q1: Selection Statement

**Procedure:** `updatePersistentLeaderboard()`

**First Conditional:**
```javascript
if (!response.ok) return;
```
- **Boolean:** `!response.ok` = HTTP request failed
- **If FALSE:** Continue → parse JSON → update leaderboard
- **If TRUE:** Exit early → log error → keep old data

---

### Q2: Parameters

**Procedure:** `conceptReactionModal(type, postURL, elemID)`

**Parameters:**
- `type` = "haha" or "boohoo"
- `postURL` = API endpoint
- `elemID` = button ID

**Manages Complexity:**
- **Without:** 20+ separate functions (1 per button)
- **With:** 1 function handles all buttons via parameters
- **Example:**
```javascript
conceptReactionModal('haha', '/api/jokes/like/1', 'haha1_modal');
conceptReactionModal('boohoo', '/api/jokes/jeer/5', 'boohoo5_modal');
```

---

### Q3: Different Calls

**Call #1:**
```javascript
conceptReactionModal('haha', '/api/jokes/like/1', 'haha1_modal');
```
- Checks `if (type === 'haha')` → TRUE
- Updates `data.haha` count

**Call #2:**
```javascript
conceptReactionModal('boohoo', '/api/jokes/jeer/5', 'boohoo5_modal');
```
- Checks `if (type === 'haha')` → FALSE
- Checks `else if (type === 'boohoo')` → TRUE
- Updates `data.boohoo` count

**Different paths:** Same function, different conditionals executed

---

### Q4: Logic Error

**Correct Code:**
```python
entry.create()  # Save score FIRST
top_10_user_ids = [e.user_id for e in LeaderboardEntry.get_top_scores(10)]
if g.current_user.id in top_10_user_ids:  # THEN check top 10
    award_badge()
```

**Buggy Code:**
```python
top_10_user_ids = [...]  # Check top 10 FIRST (before saving)
if g.current_user.id in top_10_user_ids:
    award_badge()
entry.create()  # Save score AFTER (too late!)
```

**Scenario:** User scores 120 (new #1)
- **Buggy:** Check top 10 → not there yet → no badge → save score ❌
- **Correct:** Save score → check top 10 → found → badge awarded ✅

---

### Q5: List Usage

**Accessing:**
```python
def getJoke(id):
    for joke in jokes_data:  # Traverse
        if joke['id'] == id: return joke  # Find & return
```

**Updating:**
```python
def addJokeHaHa(id):
    for joke in jokes_data:  # Traverse
        if joke['id'] == id:
            joke['haha'] += 1  # Increment (calculation)
```

**Why:** 1 list + loop vs 10 variables + 30-line if-elif

---

### Q6: Algorithm

**Iteration:**
```javascript
for (let i = 0; i < 10; i++) {
    const entry = data.leaderboard[i];  // Get element
    
    if (i === 0) rank = "🥇 1";  // Top 3 get medals
    else if (i === 1) rank = "🥈 2";
    else if (i === 2) rank = "🥉 3";
    else rank = `${i + 1}`;
    
    if (entry) display(entry.name, entry.score);  // Data or placeholder
    else display("-", "-");
}
```

**Process Each Element:**
1. Get element at index i
2. Assign rank (medals for top 3)
3. Display data if exists, else "-"
4. Always creates 10 rows
