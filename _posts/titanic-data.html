
---
layout: base
title: Titanic Survival Predictor
description: Predict passenger survival probability using a logistic regression model trained on Titanic data.
permalink: /titanic/
courses: { csp: {week: 1} }
type: collab
---


{% extends "layouts/base.html" %}

{% block title %}Titanic Survival Predictor{% endblock %}

{% block content %}
<div id="titanic-app">
  <div class="t-card">
    <h1 class="t-title">Titanic Survival Predictor</h1>
    <p class="t-sub">Enter passenger details to estimate survival probability</p>

    <div class="t-grid">
      <div class="t-field">
        <label class="t-label">Name</label>
        <input class="t-input" id="name" type="text" value="John Doe" />
      </div>

      <div class="t-field">
        <label class="t-label">Passenger Class</label>
        <select class="t-input" id="pclass">
          <option value="1">1st Class</option>
          <option value="2">2nd Class</option>
          <option value="3" selected>3rd Class</option>
        </select>
      </div>

      <div class="t-field">
        <label class="t-label">Sex</label>
        <select class="t-input" id="sex">
          <option value="male" selected>Male</option>
          <option value="female">Female</option>
        </select>
      </div>

      <div class="t-field">
        <label class="t-label">Age</label>
        <input class="t-input" id="age" type="number" min="0" max="100" value="30" />
      </div>

      <div class="t-field">
        <label class="t-label">Siblings / Spouses Aboard</label>
        <input class="t-input" id="sibsp" type="number" min="0" value="0" />
      </div>

      <div class="t-field">
        <label class="t-label">Parents / Children Aboard</label>
        <input class="t-input" id="parch" type="number" min="0" value="0" />
      </div>

      <div class="t-field">
        <label class="t-label">Fare Paid ($)</label>
        <input class="t-input" id="fare" type="number" min="0" step="0.01" value="14.50" />
      </div>

      <div class="t-field">
        <label class="t-label">Port of Embarkation</label>
        <select class="t-input" id="embarked">
          <option value="C">Cherbourg (C)</option>
          <option value="Q">Queenstown (Q)</option>
          <option value="S" selected>Southampton (S)</option>
        </select>
      </div>

      <div class="t-field">
        <label class="t-label">Traveling Alone</label>
        <select class="t-input" id="alone">
          <option value="true" selected>Yes</option>
          <option value="false">No</option>
        </select>
      </div>
    </div>

    <button class="t-btn" id="predict-btn" onclick="predictSurvival()">
      Predict Survival
    </button>

    <div id="t-error" class="t-error" style="display:none;"></div>

    <div id="t-result" class="t-result" style="display:none;">
      <div id="t-verdict" class="t-verdict"></div>
      <div class="t-bars">
        <div class="t-bar-row">
          <div class="t-bar-meta">
            <span class="t-bar-label">Survival</span>
            <span class="t-bar-pct" id="survive-pct">0%</span>
          </div>
          <div class="t-track">
            <div class="t-fill survive-fill" id="survive-bar"></div>
          </div>
        </div>
        <div class="t-bar-row">
          <div class="t-bar-meta">
            <span class="t-bar-label">Death</span>
            <span class="t-bar-pct" id="die-pct">0%</span>
          </div>
          <div class="t-track">
            <div class="t-fill die-fill" id="die-bar"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  #titanic-app {
    min-height: 80vh;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding: 48px 16px;
    font-family: 'Georgia', serif;
    background: #f7f6f2;
  }

  .t-card {
    background: #fff;
    border: 1px solid #d8d5cc;
    border-radius: 4px;
    padding: 40px 44px;
    width: 100%;
    max-width: 640px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.05);
  }

  .t-title {
    font-size: 26px;
    font-weight: 700;
    color: #111;
    margin: 0 0 6px;
    letter-spacing: -0.5px;
  }

  .t-sub {
    font-size: 14px;
    color: #777;
    margin: 0 0 30px;
  }

  .t-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 18px 24px;
    margin-bottom: 26px;
  }

  .t-field {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .t-label {
    font-size: 11px;
    font-weight: 700;
    color: #555;
    text-transform: uppercase;
    letter-spacing: 0.6px;
    font-family: sans-serif;
  }

  .t-input {
    padding: 9px 11px;
    border: 1px solid #ccc;
    border-radius: 3px;
    font-size: 14px;
    font-family: 'Georgia', serif;
    background: #fafaf8;
    color: #111;
    outline: none;
    transition: border-color 0.15s;
  }

  .t-input:focus {
    border-color: #888;
    background: #fff;
  }

  .t-btn {
    width: 100%;
    padding: 13px;
    background: #1a1a1a;
    color: #fff;
    border: none;
    border-radius: 3px;
    font-size: 15px;
    font-family: 'Georgia', serif;
    cursor: pointer;
    letter-spacing: 0.3px;
    transition: background 0.15s;
  }

  .t-btn:hover { background: #333; }
  .t-btn:disabled { background: #999; cursor: not-allowed; }

  .t-error {
    margin-top: 14px;
    color: #c0392b;
    font-size: 13px;
    font-family: sans-serif;
  }

  .t-result {
    margin-top: 26px;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 22px 26px;
    transition: border-color 0.3s;
  }

  .t-verdict {
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 18px;
  }

  .t-bars { display: flex; flex-direction: column; gap: 12px; }

  .t-bar-row {}

  .t-bar-meta {
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
  }

  .t-bar-label {
    font-size: 13px;
    color: #555;
    font-family: sans-serif;
  }

  .t-bar-pct {
    font-size: 13px;
    font-weight: 700;
    font-family: sans-serif;
    color: #222;
  }

  .t-track {
    height: 10px;
    background: #eee;
    border-radius: 5px;
    overflow: hidden;
  }

  .t-fill {
    height: 100%;
    border-radius: 5px;
    width: 0%;
    transition: width 0.5s ease;
  }

  .survive-fill { background: #16a34a; }
  .die-fill { background: #dc2626; }

  @media (max-width: 520px) {
    .t-card { padding: 28px 20px; }
    .t-grid { grid-template-columns: 1fr; }
  }
</style>

<script>
  async function predictSurvival() {
    const btn = document.getElementById('predict-btn');
    const errorEl = document.getElementById('t-error');
    const resultEl = document.getElementById('t-result');

    errorEl.style.display = 'none';
    resultEl.style.display = 'none';
    btn.disabled = true;
    btn.textContent = 'Predicting...';

    const passenger = {
      name:     [document.getElementById('name').value || 'Passenger'],
      pclass:   [parseInt(document.getElementById('pclass').value)],
      sex:      [document.getElementById('sex').value],
      age:      [parseFloat(document.getElementById('age').value)],
      sibsp:    [parseInt(document.getElementById('sibsp').value)],
      parch:    [parseInt(document.getElementById('parch').value)],
      fare:     [parseFloat(document.getElementById('fare').value)],
      embarked: [document.getElementById('embarked').value],
      alone:    [document.getElementById('alone').value === 'true'],
    };

    try {
      const res = await fetch('/api/titanic/predict', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(passenger),
      });

      if (!res.ok) throw new Error(`Server error: ${res.status}`);
      const data = await res.json();

      const survivePct = Math.round(data.survive * 100);
      const diePct = Math.round(data.die * 100);
      const survived = survivePct >= 50;

      document.getElementById('survive-pct').textContent = survivePct + '%';
      document.getElementById('die-pct').textContent = diePct + '%';
      document.getElementById('survive-bar').style.width = survivePct + '%';
      document.getElementById('die-bar').style.width = diePct + '%';

      const verdict = document.getElementById('t-verdict');
      verdict.textContent = survived ? '✓ Likely Survived' : '✗ Likely Did Not Survive';
      verdict.style.color = survived ? '#16a34a' : '#dc2626';

      resultEl.style.borderColor = survived ? '#16a34a' : '#dc2626';
      resultEl.style.display = 'block';

    } catch (e) {
      errorEl.textContent = 'Error: ' + e.message;
      errorEl.style.display = 'block';
    } finally {
      btn.disabled = false;
      btn.textContent = 'Predict Survival';
    }
  }
</script>
{% endblock %}