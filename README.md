# Basketball Draft System

## Overview
This system allows users to manage their basketball teams by dragging and dropping players into their respective coach rosters.

## Features
- Drag-and-drop functionality for players.

## Drag-and-Drop Implementation
To implement the drag-and-drop functionality:

1. **HTML Structure**: Ensure that the players are listed in a panel and that the coach roster sections are defined.

2. **JavaScript Event Listeners**:
   - Add `dragstart` event listeners to players in the left panel.
   - Add `dragover` event listeners to the coach roster sections to allow dropping.
   - Add `drop` event listeners to handle the player drop action.
   - Add `dragend` event listeners to reset the styles after dragging.

3. **CSS for Visual Feedback**:
   - Change opacity of the draggable player during dragging.
   - Highlight the drop zones when dragging over them.

### Example Implementation
```javascript
const players = document.querySelectorAll('.player');
const coachRosters = document.querySelectorAll('.coach-roster');

// Drag Start
players.forEach(player => {
  player.addEventListener('dragstart', (e) => {
    e.dataTransfer.setData('text/plain', player.id);
    player.style.opacity = '0.5';
  });

  player.addEventListener('dragend', () => {
    player.style.opacity = '';
  });
});

// Drag Over
coachRosters.forEach(roster => {
  roster.addEventListener('dragover', (e) => {
    e.preventDefault(); // Allow drop
    roster.classList.add('highlight'); // Highlight drop zone
  });

  roster.addEventListener('dragleave', () => {
    roster.classList.remove('highlight');
  });

  // Drop
  roster.addEventListener('drop', (e) => {
    e.preventDefault();
    const playerId = e.dataTransfer.getData('text/plain');
    const player = document.getElementById(playerId);
    roster.appendChild(player);
    roster.classList.remove('highlight');
  });
});
```

## Conclusion
With the above implementation, users can easily drag players into their desired rosters, enhancing their experience with the Basketball Draft System.