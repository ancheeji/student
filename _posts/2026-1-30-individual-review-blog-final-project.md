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
- Tracks user progress with authentication and concept understanding feedback

### Function
- Students log in → answer 10 AI/ethics questions → earn scores → mark concepts as understood
- Authentication required to save progress and track learning

### Inputs
- Login credentials (username, password)
- Answer selections (multiple choice, drag-drop)
- Concept understanding reactions ("Got It" / "Need Help")
- Feedback ratings (stars, category, comments)

### Outputs
- Real-time score with time bonuses
- Live concept understanding counters
- Performance breakdown by subject
- Feedback summary showing aggregate ratings

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
- **Purpose:** Tracks student understanding of AI concepts via "Got It" reactions
- **Usage:** Finds concept by ID, increments counter when clicked

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

### Procedure (File: Frontend JS, Lines 560-620)
```javascript
async function checkAuthenticationAndSetup() {
    // SEQUENCE: Fetch authentication status
    const response = await fetch(`${pythonURI}/api/id`, {
        credentials: 'include'
    });
    
    console.log('Response status:', response.status);
    
    // SELECTION: Check if user is authenticated
    if (response.ok) {
        // SEQUENCE: Parse user data
        const userData = await response.json();
        authenticatedDisplayName = userData.name || userData.uid || 'Player';
        playerName = authenticatedDisplayName;
        
        // SELECTION: Update UI elements
        const welcomeMessage = document.getElementById('welcomeMessage');
        if (welcomeMessage) {
            welcomeMessage.innerHTML = `Time to test your Responsible AI skills, <span style="color: #fbbf24;">${authenticatedDisplayName}</span>!`;
        }
        
        // Show game start screen
        const loginRequired = document.getElementById('loginRequired');
        const gameStart = document.getElementById('gameStart');
        
        if (loginRequired && gameStart) {
            loginRequired.style.display = 'none';
            gameStart.style.display = 'block';
        }
    } else {
        // User NOT authenticated - show login required
        const loginRequired = document.getElementById('loginRequired');
        const gameStart = document.getElementById('gameStart');
        
        if (loginRequired && gameStart) {
            loginRequired.style.display = 'block';
            gameStart.style.display = 'none';
        }
    }
}
```

### Algorithm Steps
1. **SEQUENCE:** Fetch user authentication status from API
2. **SELECTION:** Validate response (is user logged in?)
3. **SELECTION:** If authenticated → parse user data
4. **SEQUENCE:** Set display name from user data
5. **SELECTION:** Update welcome message if element exists
6. **SELECTION:** Show appropriate screen (game start vs login required)
7. **SELECTION:** If not authenticated → show login screen

### Call (Line 703)
```javascript
document.addEventListener('DOMContentLoaded', () => {
    checkAuthenticationAndSetup();  // Check auth on page load
    updatePersistentLeaderboard();   // Load top scores
});
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
    
    // SELECTION: Check if request already pending
    if (el.disabled || _pendingRequests.has(elemID)) {
        return;  // Block duplicate clicks
    }
    
    _pendingRequests.add(elemID);  // Track request
    el.disabled = true;
    
    // Optimistic update
    const currentCount = Number(el.dataset.count) || 0;
    el.textContent = String(currentCount + 1);
    
    fetch(postURL, options)
        .then(response => response.json())
        .then(data => {
            // Update with server value
            if (type === 'haha') el.textContent = String(data.haha);
            else el.textContent = String(data.boohoo);
        })
        .finally(() => {
            el.disabled = false;
            _pendingRequests.delete(elemID);
        });
}
```

### Result
- ✅ Counter increments by exactly 1
- ✅ Duplicate clicks blocked via Set tracking
- ✅ Optimistic UI updates (instant feedback)

---

### Bug #2: Playing Without Login
- **Problem:** Users could access quiz without authentication
- **Expected:** Login required to save scores and progress
- **Actual:** Game accessible to anyone, no progress saved

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
        const userData = await response.json();
        authenticatedDisplayName = userData.name || userData.uid;
        playerName = authenticatedDisplayName;
        
        document.getElementById('loginRequired').style.display = 'none';
        document.getElementById('gameStart').style.display = 'block';
    } else {
        // Not logged in - require login
        document.getElementById('loginRequired').style.display = 'block';
        document.getElementById('gameStart').style.display = 'none';
    }
}
```

### Result
- ✅ Users must log in before playing
- ✅ All progress properly saved to authenticated accounts
- ✅ Display name shown in welcome message

---

## PPR QUESTIONS

### Q1: Selection Statement

**Procedure:** `checkAuthenticationAndSetup()`

**First Conditional:**
```javascript
if (response.ok) {
    // User authenticated - show game
} else {
    // User NOT authenticated - show login
}
```
- **Boolean:** `response.ok` = HTTP request succeeded (status 200-299)
- **If TRUE:** Parse user data → set display name → show game start screen
- **If FALSE:** Show login required message → block game access

---

### Q2: Parameters

**Procedure:** `conceptReactionModal(type, postURL, elemID)`

**Parameters:**
- `type` = "haha" or "boohoo" (which counter to update)
- `postURL` = API endpoint for the reaction
- `elemID` = DOM element ID of the button

**Manages Complexity:**
- **Without:** 20+ separate functions (1 per button per concept)
- **With:** 1 function handles all 20 buttons via parameters
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
- Updates "Got It" counter for concept 1
- Posts to `/api/jokes/like/1`

**Call #2:**
```javascript
conceptReactionModal('boohoo', '/api/jokes/jeer/5', 'boohoo5_modal');
```
- Checks `if (type === 'haha')` → FALSE
- Checks `else if (type === 'boohoo')` → TRUE
- Updates "Need Help" counter for concept 5
- Posts to `/api/jokes/jeer/5`

**Different paths:** Same function, different conditionals executed based on `type` parameter

---

### Q4: Logic Error

**Correct Code:**
```javascript
async function checkAuthenticationAndSetup() {
    const response = await fetch(`${pythonURI}/api/id`, {
        credentials: 'include'  // CRITICAL: Include auth cookies
    });
    
    if (response.ok) {
        const userData = await response.json();
        playerName = userData.name;  // Use authenticated name
        showGameScreen();
    }
}
```

**Buggy Code:**
```javascript
async function checkAuthenticationAndSetup() {
    const response = await fetch(`${pythonURI}/api/id`);
    // Missing credentials: 'include' - cookies not sent!
    
    if (response.ok) {
        const userData = await response.json();
        playerName = userData.name;
        showGameScreen();
    }
}
```

**Scenario:** User is logged in with valid session cookie
- **Buggy:** Fetch without credentials → server doesn't receive cookie → returns 401 → user sees login screen ❌
- **Correct:** Fetch with credentials → server receives cookie → validates session → user sees game ✅

---

### Q5: List Usage

**Accessing:**
```python
def getJoke(id):
    for joke in jokes_data:  # Traverse list
        if joke['id'] == id:
            return joke  # Find & return element
```

**Updating:**
```python
def addJokeHaHa(id):
    for joke in jokes_data:  # Traverse list
        if joke['id'] == id:
            joke['haha'] += 1  # Increment counter (calculation)
            return joke
```

**Why:** 1 list + loop vs 10 variables + 30-line if-elif chain

---

### Q6: Algorithm

**Iteration:**
```javascript
function loadConceptsDataForModal() {
    fetch(getConceptsURLModal)
        .then(response => response.json())
        .then(data => {
            const container = document.getElementById("conceptsResultModal");
            container.innerHTML = '';
            
            // ITERATION: Process each concept
            for (const row of data) {
                const tr = document.createElement("tr");
                
                // Create concept cell
                const concept = document.createElement("td");
                concept.textContent = row.id + ". " + (row.joke || '');
                
                // Create "Got It" button
                const gotItBtn = document.createElement('button');
                gotItBtn.id = 'haha' + row.id + "_modal";
                gotItBtn.textContent = String(row.haha || 0);
                gotItBtn.dataset.count = String(row.haha || 0);
                
                // SELECTION: Setup click handler
                gotItBtn.addEventListener('click', function() {
                    conceptReactionModal('haha', pythonURI + '/api/jokes/like/' + row.id, gotItBtn.id);
                });
                
                // Create "Need Help" button
                const needHelpBtn = document.createElement('button');
                needHelpBtn.id = 'boohoo' + row.id + "_modal";
                needHelpBtn.textContent = String(row.boohoo || 0);
                needHelpBtn.dataset.count = String(row.boohoo || 0);
                
                // SELECTION: Setup click handler
                needHelpBtn.addEventListener('click', function() {
                    conceptReactionModal('boohoo', pythonURI + '/api/jokes/jeer/' + row.id, needHelpBtn.id);
                });
                
                // Add cells to row
                const gotItCell = document.createElement("td");
                gotItCell.appendChild(gotItBtn);
                const needHelpCell = document.createElement("td");
                needHelpCell.appendChild(needHelpBtn);
                
                tr.appendChild(concept);
                tr.appendChild(gotItCell);
                tr.appendChild(needHelpCell);
                container.appendChild(tr);
            }
        });
}
```

**Process Each Element:**
1. **ITERATION:** Loop through all concepts in data array
2. Create table row for this concept
3. Create concept text cell with ID and description
4. **SELECTION:** Create "Got It" button with counter value
5. **SELECTION:** Attach click handler that calls `conceptReactionModal` with 'haha'
6. **SELECTION:** Create "Need Help" button with counter value
7. **SELECTION:** Attach click handler that calls `conceptReactionModal` with 'boohoo'
8. Append all cells to row, add row to container
9. Process repeats for all 10 concepts