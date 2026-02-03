---
layout: post
title: "Weekly Review: AI Study Buddy Progress"
description: "Individual contribution and goals for next week"
permalink: /weekly-review-jan30/
categories: [Review, Progress]
tags: [reflection, goals, development]
author: "Your Name"
date: 2025-01-30
---

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