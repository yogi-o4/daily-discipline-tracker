<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Daily Discipline Tracker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: #f0f8ff;
      color: #333;
    }
    h1 {
      text-align: center;
    }
    .task {
      display: flex;
      align-items: center;
      margin: 10px 0;
    }
    input[type="checkbox"] {
      margin-right: 10px;
    }
    #submitBtn {
      display: block;
      margin: 20px auto;
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    #status, #privileges {
      text-align: center;
      margin-top: 20px;
    }
    @media (max-width: 600px) {
      body {
        padding: 10px;
      }
    }
  </style>
</head>
<body>
  <h1>Daily Discipline Tracker</h1>
  <div class="task-list">
    <div class="task"><input type="checkbox" id="brush" /> Brush Teeth</div>
    <div class="task"><input type="checkbox" id="bath" /> Bath</div>
    <div class="task"><input type="checkbox" id="sheets" /> Fold Sheets</div>
    <div class="task"><input type="checkbox" id="water" /> Drank 3 Ltr Water</div>
  </div>
  <button id="submitBtn">Submit Today's Tasks</button>
  <div id="status"></div>
  <div id="privileges"></div>

  <script>
    const submitBtn = document.getElementById('submitBtn');
    const statusDiv = document.getElementById('status');
    const privilegesDiv = document.getElementById('privileges');

    function areAllTasksChecked() {
      return document.getElementById('brush').checked &&
             document.getElementById('bath').checked &&
             document.getElementById('sheets').checked &&
             document.getElementById('water').checked;
    }

    function resetCheckboxes() {
      document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.checked = false);
    }

    function updateStatus(days) {
      statusDiv.textContent = `You've completed your tasks ${days} day(s) in a row.`;
    }

    function getTodayKey() {
      const today = new Date();
      return today.toISOString().split('T')[0];
    }

    function getStoredProgress() {
      return JSON.parse(localStorage.getItem('dailyProgress') || '{}');
    }

    function storeTodayCompletion() {
      const progress = getStoredProgress();
      const todayKey = getTodayKey();
      progress[todayKey] = true;
      localStorage.setItem('dailyProgress', JSON.stringify(progress));
    }

    function getStreak() {
      const progress = getStoredProgress();
      let streak = 0;
      for (let i = 0; i < 7; i++) {
        const date = new Date();
        date.setDate(date.getDate() - i);
        const key = date.toISOString().split('T')[0];
        if (progress[key]) {
          streak++;
        } else {
          break;
        }
      }
      return streak;
    }

    submitBtn.addEventListener('click', () => {
      if (areAllTasksChecked()) {
        storeTodayCompletion();
        const streak = getStreak();
        updateStatus(streak);
        window.location.href = 'boobie.html';
        resetCheckboxes();
      } else {
        alert("Please complete all tasks before submitting.");
      }
    });

    // Load current streak on page load
    window.addEventListener('load', () => {
      const streak = getStreak();
      updateStatus(streak);
    });
  </script>
</body>
</html>
