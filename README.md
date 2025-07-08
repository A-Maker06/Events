<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Event Manager</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      background-color: #f4f4f4;
    }
    form, .event {
      background-color: #fff;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 20px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    h2 {
      color: #333;
    }
    input, textarea, button {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    button {
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    .btn-group {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      margin-top: 10px;
    }
    .edit-btn {
      background-color: #ffc107;
      color: #000;
      padding: 5px 10px;
      border: none;
      border-radius: 5px;
    }
    .delete-btn {
      background-color: #dc3545;
      color: #fff;
      padding: 5px 10px;
      border: none;
      border-radius: 5px;
    }
    @media (max-width: 600px) {
      body {
        padding: 10px;
      }
    }
  </style>
</head>
<body>

  <h2>Add Event & Rules</h2>

  <form id="eventForm">
    <input type="text" id="eventName" placeholder="Event Name" required>
    <textarea id="eventRules" placeholder="Event Rules" rows="4" required></textarea>
    <button type="submit">Add Event</button>
  </form>

  <div id="eventList"></div>

  <script>
    let editingEvent = null;

    const form = document.getElementById('eventForm');
    const eventList = document.getElementById('eventList');

    form.addEventListener('submit', function (e) {
      e.preventDefault();

      const name = document.getElementById('eventName').value;
      const rules = document.getElementById('eventRules').value;

      if (editingEvent) {
        editingEvent.querySelector('h3').innerText = name;
        editingEvent.querySelector('p').innerHTML = rules.replace(/\n/g, '<br>');
        editingEvent = null;
        form.reset();
        return;
      }

      const eventDiv = document.createElement('div');
      eventDiv.classList.add('event');

      eventDiv.innerHTML = `
        <h3>${name}</h3>
        <p>${rules.replace(/\n/g, '<br>')}</p>
        <div class="btn-group">
          <button type="button" class="edit-btn">Edit</button>
          <button type="button" class="delete-btn">Delete</button>
        </div>
      `;

      eventDiv.querySelector('.edit-btn').addEventListener('click', () => {
        document.getElementById('eventName').value = name;
        document.getElementById('eventRules').value = rules;
        editingEvent = eventDiv;
      });

      eventDiv.querySelector('.delete-btn').addEventListener('click', () => {
        if (confirm('Are you sure to delete this event?')) {
          eventDiv.remove();
        }
      });

      eventList.appendChild(eventDiv);
      form.reset();
    });
  </script>

</body>
</html>