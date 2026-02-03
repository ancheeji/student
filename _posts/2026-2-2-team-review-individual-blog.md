---
layout: post
title: "Weekly Review: AI Study Buddy Progress"
description: "Individual contribution and goals for next week"
permalink: /weekly-review-jan30/
categories: [Review, Progress]
tags: [reflection, goals, development]
author: "Michelle Ji"
date: 2025-01-30
--- 
## What I've Accomplished

### Frontend
- Built interactive quiz with 10 prompt engineering questions
- Implemented drag-and-drop ordering exercises
- Created timer system with visual warnings
- Added badge notification popups with animations
- Integrated user authentication flow

### Backend
- Developed Flask REST API (`/questions`, `/scores`, `/feedback`)
- Created SQLAlchemy models for data persistence
- Built "Check for Understanding" tracking system with concept reactions
- Integrated badge awards with user authentication

### Technical Skills
- Used lists for question banks and game state
- Implemented async/await for API calls
- Created reusable procedures like `loadQuestionsFromBackend()` and `conceptReaction()`
- Built algorithms with sequence, selection, and iteration

---

## Goals for Next Week

1. **Authentication Integration**: Remove manual name entry—automatically use logged-in user's info. Redirect to login page if not authenticated.

2. **Fix Concept Tracking**: Debug the joke/concept increment system (`haha`/`boohoo` counters not updating correctly after clicks)

3. **Clean Up Results Screen**: Remove leaderboard display after quiz completion to focus on personal performance metrics

4. **Badge Display Fix**: Resolve timing issue where badges don't appear immediately after being awarded

5. **Error Handling**: Add loading spinners during API calls and improve error messages

---

## How I'll Improve

### Authentication Check
```javascript
// ✅ New: Verify user is logged in before starting
async function startGame() {
    const user = await checkAuthentication();
    
    if (!user) {
        window.location.href = '/login';
        return;
    }
    
    playerName = user.username;  // Auto-populate from session
    gameQuestions = shuffleArray(allQuestions);
    loadQuestion();
}
```

### Fix Concept Reaction Updates
```javascript
// Current issue: Counters don't refresh properly
function conceptReaction(type, postURL, elemID) {
    const options = {
        ...fetchOptions,
        method: 'PUT',
    };

    fetch(postURL, options)
        .then(response => response.json())
        .then(data => {
            // ✅ Add immediate visual feedback
            const button = document.getElementById(elemID);
            button.innerHTML = data[type];  // Update count immediately
            button.style.animation = 'pulse 0.3s';  // Visual confirmation
        })
        .catch(err => console.error('Update failed:', err));
}
```

### Simplified Results Display
```javascript
// ✅ Remove leaderboard section from results screen
async function endGame() {
    showScreen('resultsScreen');
    const results = calculateFinalResults();
    await saveGameData(results);
    updateResultsDisplay(results);
    // Removed: loadLeaderboard() call
}
```

### Better Error Messages
```javascript
// ❌ Before
catch (error) {
    alert('Error!');
}

// ✅ After
catch (error) {
    if (error.message.includes('network')) {
        showErrorMessage('Unable to connect. Check your internet.');
    } else if (error.status === 401) {
        showErrorMessage('Please log in to continue.');
        setTimeout(() => window.location.href = '/login', 2000);
    }
}
```

---

## Reflection

**Wins**: Successfully built full-stack app with authentication, real-time concept tracking, and badge system 

**Challenges**: Async timing for badge awards, concept counter refresh, authentication flow 

**Key Lesson**: Always check user authentication state before allowing access to features 

**Next Priority**: Integrate auth check, fix concept tracking, streamline results display 






##IGNORE THIS PART##
## My Logged-In User View

**Username**: [Your Username]  
**Role**: Developer - Submodule 3 (Prompt Engineering Challenge)  
**Authentication**: Integrated with Flask session management and badge system

---

## UI Walkthrough

**Start Screen**: Player enters name → Start Challenge button initiates quiz

**Game Screen**: 30-second timer with visual warnings → 10 questions across 4 subjects → Drag-and-drop exercises → Real-time score with time bonus

**Results Screen**: Final score + subject breakdown → Badge popups → Feedback modal → Concept tracking ("Got It" vs "Need Help")

---

## My Superpower: Interactive Learning Through Gamification

- Real-time scoring with time pressure creates urgency
- Visual feedback (animations, color changes) reinforces learning
- Badge system motivates completion
- Concept tracking identifies areas needing help

**Impact**: Makes learning AI concepts engaging instead of reading static content

---

## My Code

### API Endpoints
```python
# FILE: prompt_game_api.py
class QuestionsAPI(Resource):
    def get(self):
        questions = PromptQuestion.query.all()
        return jsonify({'success': True, 'questions': [q.to_dict() for q in questions]})

class ScoresAPI(Resource):
    def post(self):
        data = request.get_json()
        new_score = PromptScore(
            player_name=data['playerName'],
            score=data['score'],
            timestamp=datetime.utcnow()
        )
        new_score.create()
        return jsonify({'success': True})
```

**Deployment**: Flask server at `http://127.0.0.1:8887`  
**Transactional Data**: Scores saved to SQLite with timestamps  
**Bulk Data**: Question bank stored in JSON format

---

### Frontend to Backend Correlation

**Frontend**:
```javascript
async function loadQuestionsFromBackend() {
    const response = await fetch(`${API_URL}/questions`);
    const data = await response.json();
    allQuestions = data.questions;
}
```

**Backend**:
```python
return jsonify({'success': True, 'questions': [...]})
```

**Flow**: Frontend fetches → Backend queries database → Returns JSON → Frontend stores in list → Shuffles for gameplay

---

### Concept Tracking

**Frontend**:
```javascript
function conceptReaction(type, postURL, elemID) {
    fetch(postURL, {method: 'PUT'})
        .then(response => response.json())
        .then(data => {
            document.getElementById(elemID).innerHTML = data[type];
        });
}
```

**Backend**:
```python
def put(self, id):
    addJokeHaHa(id)  # Increment counter
    return jsonify(getJoke(id))
```

**Flow**: User clicks "Got It" → PUT request → Backend increments → Returns updated count → Frontend displays

---

## My Process

**Brainstorm**: Sketched quiz flow, decided on 4 subjects with drag-and-drop variety

**Iteration**: V1 (basic multiple choice) → V2 (timer + scoring) → V3 (drag-and-drop) → V4 (badges + concept tracking)

**Peer Review**: Teammate found async bug with questions not loading, suggested pulse animation for concept clicks

**Polish**: Added smooth transitions, specific error messages, badge notification animations

---

## Happy Moments! 

- Finally got drag-and-drop working after 3 hours
- Timer turning red at 10 seconds looked perfect
- Classmate said "This is actually fun!" during testing
- Fixed async race condition bug on first try

---

## Goals for Next Week

1. **Authentication**: Remove manual name entry, auto-populate from logged-in user, redirect to login if not authenticated

2. **Fix Concept Tracking**: Debug `haha`/`boohoo` counters not updating after clicks

3. **Clean Up Formatting**: Remove leaderboard from results screen

---

## Reflection

**Wins**: Built interactive quiz with authentication, concept tracking, badges 

**Challenges**: Async timing, counter refresh, authentication flow 

**Key Lesson**: Always check auth state before feature access 

**Next Priority**: Auth integration, fix tracking, streamline display 