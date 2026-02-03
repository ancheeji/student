---
layout: post
title: "Weekly Review: AI Study Buddy Progress"
description: "Quick reflection on accomplishments and goals for next week"
permalink: /weekly-review-jan30/
categories: [Review, Progress]
tags: [reflection, goals, development]
author: "Your Name"
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

3. **Clean Up Formatting**: Remove leaderboard display after quiz completion to focus on personal performance metrics, and make the formatting less bulky 

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