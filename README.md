# rediaz860.github.io
Our In-House Draft for the Wilson-Gray YMCA
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🏀 Basketball Draft System</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        .container {
            max-width: 1600px;
            margin: 0 auto;
        }
        h1 {
            text-align: center;
            color: white;
            font-size: 3em;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        .main-layout {
            display: grid;
            grid-template-columns: 1fr 3fr;
            gap: 20px;
        }
        .players-section {
            background: white;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            height: fit-content;
            position: sticky;
            top: 20px;
        }
        .players-section h2 {
            color: #667eea;
            margin-bottom: 15px;
            font-size: 1.5em;
            border-bottom: 2px solid #667eea;
            padding-bottom: 10px;
        }
        .players-list {
            list-style: none;
            max-height: 800px;
            overflow-y: auto;
        }
        .player-item {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            padding: 12px;
            margin: 8px 0;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            border-left: 4px solid #667eea;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: 500;
        }
        .player-item:hover {
            transform: translateX(5px);
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .player-item.selected {
            background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
            border-left: 4px solid #20c997;
            color: white;
        }
        .player-item.used {
            opacity: 0.4;
            cursor: not-allowed;
            background: #e9ecef;
            border-left: 4px solid #999;
        }
        .coaches-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 20px;
        }
        .coach-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            border-top: 4px solid #667eea;
        }
        .coach-card h3 {
            color: #667eea;
            margin-bottom: 15px;
            font-size: 1.4em;
        }
        .coach-roster {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            min-height: 250px;
            margin-bottom: 15px;
            border: 2px dashed #667eea;
        }
        .roster-item {
            background: white;
            padding: 12px;
            margin: 8px 0;
            border-radius: 6px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-left: 3px solid #667eea;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .remove-btn {
            background: #dc3545;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.85em;
            transition: all 0.2s;
            font-weight: bold;
        }
        .remove-btn:hover {
            background: #c82333;
            transform: scale(1.05);
        }
        .counter {
            background: #667eea;
            color: white;
            padding: 8px 12px;
            border-radius: 20px;
            font-size: 0.95em;
            font-weight: bold;
            text-align: center;
            margin-bottom: 10px;
        }
        .counter.full {
            background: #28a745;
        }
        .add-btn {
            background: #667eea;
            color: white;
            border: none;
            padding: 12px;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
            font-weight: bold;
            transition: all 0.2s;
            font-size: 1em;
        }
        .add-btn:hover:not(:disabled) {
            background: #764ba2;
            transform: scale(1.02);
        }
        .add-btn:disabled {
            background: #ccc;
            cursor: not-allowed;
            opacity: 0.6;
        }
        .empty-roster {
            color: #999;
            text-align: center;
            padding: 40px 20px;
            font-style: italic;
        }
        .scrollbar::-webkit-scrollbar {
            width: 8px;
        }
        .scrollbar::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        .scrollbar::-webkit-scrollbar-thumb {
            background: #667eea;
            border-radius: 10px;
        }
        .scrollbar::-webkit-scrollbar-thumb:hover {
            background: #764ba2;
        }
        @media (max-width: 1024px) {
            .main-layout {
                grid-template-columns: 1fr;
            }
            .players-section {
                position: static;
                max-height: 300px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🏀 Basketball Draft System</h1>
        
        <div class="main-layout">
            <!-- Player Pool -->
            <div class="players-section">
                <h2>📋 Available Players</h2>
                <ul class="players-list scrollbar" id="playerPool"></ul>
            </div>
            
            <!-- Coach Rosters -->
            <div>
                <div class="coaches-grid" id="coachesGrid"></div>
            </div>
        </div>
    </div>

    <script>
        // All 38 Players
        const playersPool = [
            'Jahmari Bennefield',
            'Jahsaih Taylor',
            'Chaise Walcott',
            'Divontae Williams',
            'Sermir Brunson',
            'Lomir Otto',
            'Ryan Smalling',
            'Khaliq Waite',
            'Kayvon',
            'Anthony Maclean',
            'Duane Davis',
            'Bry\'Aire Hales',
            'Tremaine Jackson',
            'Trevor Davis',
            'Eliezel Torres',
            'Jemere Ormsby',
            'Jasiri Maribei',
            'Jashon Ford',
            'Calvin Chaney',
            'Diamond Buckly',
            'Ahzy Champion',
            'Alon Gibson',
            'Jamair Copeland',
            'Jaquan Reynolds',
            'Able Ceasar',
            'Gavin Ashby',
            'Isaac Burgos',
            'JJ Haughton',
            'Keneilia Morgan',
            'Ffrino',
            'Isaac Dennis',
            'Za\'Kye Cameron',
            'Norberto Antonett',
            'Kendal Hurst',
            'Eriel Rivera',
            'Joaniel Nunez',
            'Darius Lyn',
            'Nathaniel Jones'
        ];

        const coaches = [
            { id: 1, name: 'Coach Diamond' },
            { id: 2, name: 'Coach Rich' },
            { id: 3, name: 'Coach George' },
            { id: 4, name: 'Coach Jonas' },
            { id: 5, name: 'Coach Brennen' },
            { id: 6, name: 'Coach Six' }
        ];

        const MAX_PLAYERS = 7;
        let coachRosters = {};
        let selectedPlayer = null;
        let assignedPlayers = new Set();

        // Initialize
        function init() {
            coaches.forEach(coach => {
                coachRosters[coach.id] = [];
            });
            renderPlayerPool();
            renderCoaches();
        }

        function renderPlayerPool() {
            const playerPool = document.getElementById('playerPool');
            playerPool.innerHTML = '';
            
            playersPool.forEach(player => {
                const li = document.createElement('li');
                li.className = 'player-item';
                
                if (assignedPlayers.has(player)) {
                    li.classList.add('used');
                }
                if (selectedPlayer === player) {
                    li.classList.add('selected');
                }
                
                li.innerHTML = `
                    <span>${player}</span>
                    <span style="font-size: 1.1em;">${selectedPlayer === player ? '✓' : ''}</span>
                `;
                
                li.onclick = () => {
                    if (!assignedPlayers.has(player)) {
                        togglePlayerSelection(player);
                    }
                };
                playerPool.appendChild(li);
            });
        }

        function togglePlayerSelection(player) {
            if (selectedPlayer === player) {
                selectedPlayer = null;
            } else {
                selectedPlayer = player;
            }
            renderPlayerPool();
        }

        function renderCoaches() {
            const coachesGrid = document.getElementById('coachesGrid');
            coachesGrid.innerHTML = '';
            
            coaches.forEach(coach => {
                const card = document.createElement('div');
                card.className = 'coach-card';
                
                const roster = coachRosters[coach.id];
                const isFull = roster.length >= MAX_PLAYERS;
                
                let rosterHTML = `
                    <h3>${coach.name}</h3>
                    <div class="counter ${isFull ? 'full' : ''}">${roster.length}/${MAX_PLAYERS} Players</div>
                    <div class="coach-roster" id="roster-${coach.id}">
                `;
                
                if (roster.length === 0) {
                    rosterHTML += '<div class="empty-roster">No players selected yet</div>';
                } else {
                    roster.forEach(player => {
                        rosterHTML += `
                            <div class="roster-item">
                                <span>${player}</span>
                                <button class="remove-btn" onclick="removePlayerFromCoach(${coach.id}, '${player.replace(/'/g, "\\'")}')">Remove</button>
                            </div>
                        `;
                    });
                }
                
                rosterHTML += `
                    </div>
                    <button class="add-btn" ${isFull || !selectedPlayer ? 'disabled' : ''} onclick="addSelectedPlayerToCoach(${coach.id})">
                        ${isFull ? '✓ Team Full' : selectedPlayer ? '➕ Add ' + selectedPlayer : 'Select a Player'}
                    </button>
                `;
                
                card.innerHTML = rosterHTML;
                coachesGrid.appendChild(card);
            });
        }

        function addSelectedPlayerToCoach(coachId) {
            if (!selectedPlayer) {
                alert('Please select a player first!');
                return;
            }
            
            if (coachRosters[coachId].length >= MAX_PLAYERS) {
                alert('This coach already has 7 players!');
                return;
            }
            
            if (assignedPlayers.has(selectedPlayer)) {
                alert('This player is already assigned!');
                return;
            }
            
            coachRosters[coachId].push(selectedPlayer);
            assignedPlayers.add(selectedPlayer);
            selectedPlayer = null;
            
            renderPlayerPool();
            renderCoaches();
        }

        function removePlayerFromCoach(coachId, player) {
            coachRosters[coachId] = coachRosters[coachId].filter(p => p !== player);
            assignedPlayers.delete(player);
            renderPlayerPool();
            renderCoaches();
        }

        // Start the app
        init();
    </script>
</body>
</html>
