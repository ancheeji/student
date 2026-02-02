---
layout: post
title: "AI Study Buddy Progress"
description: "Quick reflection on accomplishments and goals for next week"
permalink: /teamreview-individual-submodule-3/
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

### Backend
- Developed Flask REST API (`/questions`, `/scores`, `/feedback`)
- Created SQLAlchemy models for data persistence
- Built "Check for Understanding" tracking system
- Integrated badge awards with user authentication

### Technical Skills
- Used lists for question banks and game state
- Implemented async/await for API calls
- Created reusable procedures like `loadQuestionsFromBackend()`
- Built algorithms with sequence, selection, and iteration

---

## Goals for Next Week

1. **Bug Fixes**: Fix badge display timing and modal z-index issues
2. **New Content**: Add 5 more drag-and-drop questions
3. **UX Improvements**: Add loading spinners and better error messages
4. **Testing**: Write Postman test cases for all API endpoints

---

## How I'll Improve

### Better Code Organization
```javascript
// ❌ Before: Too long
async function endGame() {
    // 50 lines of mixed logic
}

// ✅ After: Split into focused procedures
async function endGame() {
    const results = calculateFinalResults();
    await saveGameData(results);
    updateResultsDisplay(results);
}
```

### Improved Error Handling
```javascript
// ❌ Before
catch (error) {
    alert('Error!');
}

// ✅ After
catch (error) {
    showErrorMessage('Unable to connect. Check your internet.');
}
```

### Add Documentation
- Write comments explaining complex algorithms
- Document all API endpoints
- Add JSDoc to procedures

---

## Reflection

**Wins**: Successfully built full-stack app meeting all CPT requirements 

**Challenges**: Async timing, CORS configuration, CSS positioning

**Key Lesson**: Plan data flow before coding—prevents bugs later 

**Next Priority**: Fix bugs, add content, improve error handling 