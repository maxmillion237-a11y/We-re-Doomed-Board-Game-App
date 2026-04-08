<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>We're Doomed!</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Oswald:wght@400;600;700&family=Roboto:wght@300;400;700&display=swap');

  :root {
    --orange: #E8890C;
    --dark-orange: #B56A00;
    --brown: #2C1A0E;
    --dark-brown: #1A0E06;
    --cream: #F5E6C8;
    --light-cream: #FFF8EC;
    --red: #C0392B;
    --dark-red: #922B21;
    --green: #27AE60;
    --purple: #8E44AD;
    --blue: #2980B9;
    --teal: #16A085;
    --gold: #F1C40F;
    --shadow: rgba(0,0,0,0.6);
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    font-family: 'Roboto', sans-serif;
    background: var(--dark-brown);
    color: var(--cream);
    min-height: 100vh;
    overflow-x: hidden;
  }

  .screen { display:none; min-height:100vh; }
  .screen.active { display:flex; flex-direction:column; }

  #home-screen {
    align-items: center;
    justify-content: center;
    background: radial-gradient(ellipse at center, #3D1F08 0%, #1A0E06 70%);
    position: relative;
    overflow: hidden;
  }
  #home-screen::before {
    content: '';
    position: absolute;
    inset: 0;
    background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23E8890C' fill-opacity='0.04'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
    opacity: 0.5;
  }
  .home-content { text-align:center; z-index:1; padding: 2rem; }
  .game-logo {
    font-family: 'Bebas Neue', cursive;
    font-size: clamp(4rem, 12vw, 9rem);
    color: var(--orange);
    text-shadow: 6px 6px 0 var(--dark-brown), 0 0 40px rgba(232,137,12,0.4);
    line-height: 0.9;
    letter-spacing: 4px;
  }
  .game-subtitle {
    font-family: 'Oswald', sans-serif;
    font-size: clamp(0.9rem, 2.5vw, 1.3rem);
    color: var(--cream);
    letter-spacing: 6px;
    text-transform: uppercase;
    margin: 0.8rem 0 3rem;
    opacity: 0.8;
  }
  .home-buttons { display:flex; gap:1.5rem; flex-wrap:wrap; justify-content:center; }
  .btn-primary {
    font-family: 'Bebas Neue', cursive;
    font-size: 1.8rem;
    letter-spacing: 3px;
    padding: 1rem 3rem;
    background: var(--orange);
    color: var(--dark-brown);
    border: none;
    cursor: pointer;
    clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
    transition: all 0.2s;
    text-transform: uppercase;
  }
  .btn-primary:hover { background: #F5A030; transform: translateY(-3px); box-shadow: 0 8px 20px rgba(232,137,12,0.4); }
  .btn-secondary {
    font-family: 'Bebas Neue', cursive;
    font-size: 1.8rem;
    letter-spacing: 3px;
    padding: 1rem 3rem;
    background: transparent;
    color: var(--orange);
    border: 3px solid var(--orange);
    cursor: pointer;
    clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
    transition: all 0.2s;
    text-transform: uppercase;
  }
  .btn-secondary:hover { background: rgba(232,137,12,0.15); transform: translateY(-3px); }

  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.85);
    z-index: 1000;
    align-items: center;
    justify-content: center;
    padding: 1rem;
  }
  .modal-overlay.active { display:flex; }
  .modal {
    background: var(--brown);
    border: 3px solid var(--orange);
    padding: 2.5rem;
    max-width: 480px;
    width: 100%;
    position: relative;
    clip-path: polygon(12px 0%, 100% 0%, calc(100% - 12px) 100%, 0% 100%);
  }
  .modal h2 {
    font-family: 'Bebas Neue', cursive;
    font-size: 2.5rem;
    color: var(--orange);
    margin-bottom: 1.5rem;
    letter-spacing: 3px;
  }
  .modal input {
    width: 100%;
    padding: 0.9rem 1.2rem;
    background: var(--dark-brown);
    border: 2px solid var(--dark-orange);
    color: var(--cream);
    font-family: 'Oswald', sans-serif;
    font-size: 1.1rem;
    margin-bottom: 1rem;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 2px;
  }
  .modal input:focus { border-color: var(--orange); }
  .modal input::placeholder { color: rgba(245,230,200,0.3); text-transform: none; letter-spacing: 1px; }
  .modal-buttons { display:flex; gap:1rem; margin-top: 0.5rem; }
  .modal-buttons button { flex:1; }
  .btn-cancel {
    font-family: 'Bebas Neue', cursive;
    font-size: 1.4rem;
    padding: 0.7rem;
    background: transparent;
    color: var(--cream);
    border: 2px solid rgba(245,230,200,0.3);
    cursor: pointer;
    letter-spacing: 2px;
    transition: all 0.2s;
  }
  .btn-cancel:hover { border-color: var(--cream); }
  .room-code-display {
    font-family: 'Bebas Neue', cursive;
    font-size: 3.5rem;
    color: var(--orange);
    letter-spacing: 10px;
    text-align: center;
    background: var(--dark-brown);
    padding: 1rem;
    margin: 1rem 0;
    border: 2px dashed var(--dark-orange);
  }
  .modal-note { font-size: 0.85rem; opacity: 0.6; text-align:center; margin-top: 0.5rem; }

  #lobby-screen {
    background: radial-gradient(ellipse at top, #2C1A0E 0%, #1A0E06 100%);
    padding: 2rem;
    align-items: center;
  }
  .lobby-header { text-align:center; margin-bottom: 2rem; }
  .lobby-header h1 { font-family:'Bebas Neue',cursive; font-size:3rem; color:var(--orange); letter-spacing:4px; }
  .lobby-code-badge {
    display:inline-block;
    background: var(--dark-brown);
    border: 2px solid var(--orange);
    padding: 0.4rem 1.5rem;
    font-family: 'Bebas Neue', cursive;
    font-size: 1.8rem;
    color: var(--orange);
    letter-spacing: 8px;
    margin-top: 0.5rem;
  }
  .lobby-players {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
    gap: 1rem;
    width: 100%;
    max-width: 900px;
    margin-bottom: 2rem;
  }
  .player-slot {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    padding: 1.2rem;
    text-align: center;
    min-height: 120px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    transition: border-color 0.3s;
  }
  .player-slot.filled { border-color: var(--orange); }
  .player-slot.you { border-color: var(--gold); box-shadow: 0 0 15px rgba(241,196,15,0.2); }
  .player-slot .player-name { font-family:'Oswald',sans-serif; font-size:1.2rem; font-weight:600; margin-bottom:0.3rem; }
  .player-slot .player-civ { font-size:0.85rem; color:var(--orange); opacity:0.8; }
  .player-slot .empty-label { opacity:0.3; font-style:italic; }
  .you-badge { font-size:0.7rem; background:var(--gold); color:var(--dark-brown); padding:2px 8px; font-family:'Oswald',sans-serif; font-weight:700; margin-top:0.3rem; }
  .lobby-actions { display:flex; flex-direction:column; align-items:center; gap:1rem; }
  .lobby-status { opacity:0.6; font-family:'Oswald',sans-serif; font-size:0.95rem; letter-spacing:1px; }

  .civ-picker { width:100%; max-width:900px; margin-bottom:2rem; }
  .civ-picker h3 { font-family:'Bebas Neue',cursive; font-size:1.8rem; color:var(--orange); margin-bottom:1rem; text-align:center; letter-spacing:3px; }
  .civ-cards { display:flex; gap:0.8rem; flex-wrap:wrap; justify-content:center; }
  .civ-card {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    padding: 1rem;
    cursor: pointer;
    transition: all 0.2s;
    width: 160px;
    text-align: center;
  }
  .civ-card:hover { border-color: var(--orange); transform: translateY(-4px); }
  .civ-card.selected { border-color: var(--gold); background: rgba(241,196,15,0.1); box-shadow: 0 0 15px rgba(241,196,15,0.3); }
  .civ-card.taken { opacity:0.3; pointer-events:none; }
  .civ-name { font-family:'Bebas Neue',cursive; font-size:1.3rem; color:var(--orange); letter-spacing:2px; margin-bottom:0.4rem; }
  .civ-perk { font-size:0.75rem; color:var(--cream); opacity:0.8; line-height:1.4; }
  .civ-icon { font-size:2rem; margin-bottom:0.4rem; }

  #game-screen {
    background: #0F0804;
    padding: 0;
    min-height: 100vh;
    position: relative;
  }

  .game-topbar {
    background: var(--dark-brown);
    border-bottom: 3px solid var(--orange);
    padding: 0.6rem 1.5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 0.5rem;
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .topbar-logo { font-family:'Bebas Neue',cursive; font-size:1.5rem; color:var(--orange); letter-spacing:3px; }
  .timer-display {
    font-family: 'Bebas Neue', cursive;
    font-size: 2.2rem;
    color: var(--cream);
    letter-spacing: 3px;
    background: var(--dark-brown);
    padding: 0.2rem 1rem;
    border: 2px solid var(--dark-orange);
  }
  .timer-display.urgent { color: var(--red); border-color: var(--red); animation: pulse 1s infinite; }
  @keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:0.5;} }
  .topbar-phase {
    font-family: 'Oswald', sans-serif;
    font-size: 0.9rem;
    letter-spacing: 2px;
    color: var(--orange);
    text-transform: uppercase;
  }

  .game-layout {
    display: grid;
    grid-template-columns: 280px 1fr 280px;
    grid-template-rows: auto 1fr auto;
    gap: 0;
    height: calc(100vh - 60px);
    overflow: hidden;
  }

  @media(max-width:900px) {
    .game-layout { grid-template-columns: 1fr; grid-template-rows: auto; height:auto; overflow:auto; }
  }

  .panel-left {
    background: rgba(26,14,6,0.95);
    border-right: 2px solid rgba(232,137,12,0.2);
    overflow-y: auto;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.6rem;
  }
  .panel-title {
    font-family: 'Bebas Neue', cursive;
    font-size: 1.2rem;
    color: var(--orange);
    letter-spacing: 3px;
    margin-bottom: 0.4rem;
    text-align: center;
    border-bottom: 1px solid rgba(232,137,12,0.3);
    padding-bottom: 0.4rem;
  }

  .player-card {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.2);
    padding: 0.8rem;
    transition: border-color 0.3s;
    position: relative;
  }
  .player-card.active-turn { border-color: var(--gold); box-shadow: 0 0 10px rgba(241,196,15,0.3); }
  .player-card.you-card { border-color: rgba(232,137,12,0.5); }
  .player-card.eliminated { opacity:0.4; }
  .player-card.eliminated::after { content:'ELIMINATED'; position:absolute; inset:0; display:flex; align-items:center; justify-content:center; font-family:'Bebas Neue',cursive; font-size:1.2rem; color:var(--red); background:rgba(0,0,0,0.5); letter-spacing:3px; }
  .pc-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:0.4rem; }
  .pc-name { font-family:'Oswald',sans-serif; font-weight:600; font-size:0.95rem; }
  .pc-civ { font-size:0.7rem; color:var(--orange); font-family:'Oswald',sans-serif; }
  .pc-stats { display:flex; gap:0.5rem; flex-wrap:wrap; }
  .stat-chip {
    display: flex;
    align-items: center;
    gap: 3px;
    background: rgba(0,0,0,0.3);
    padding: 2px 8px;
    font-size: 0.8rem;
    font-family: 'Oswald', sans-serif;
    font-weight: 600;
  }
  .stat-chip .stat-icon { font-size:0.9rem; }
  .stat-chip.res { color: #7EC8E3; }
  .stat-chip.inf { color: var(--gold); }
  .stat-chip.contrib { color: var(--green); }
  .first-player-badge { font-size:0.65rem; background:var(--orange); color:var(--dark-brown); padding:1px 5px; font-family:'Oswald',sans-serif; font-weight:700; }

  .panel-center {
    overflow-y: auto;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .project-box {
    background: linear-gradient(135deg, #1A0E06, #2C1A0E);
    border: 3px solid var(--orange);
    padding: 1.2rem;
    text-align: center;
    position: relative;
  }
  .project-box h3 { font-family:'Bebas Neue',cursive; font-size:1.5rem; color:var(--orange); letter-spacing:4px; margin-bottom:0.8rem; }
  .resource-track {
    display: flex;
    align-items: center;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 0.8rem;
  }
  .track-number {
    font-family: 'Bebas Neue', cursive;
    font-size: 4rem;
    color: var(--cream);
    line-height: 1;
  }
  .track-label { font-family:'Oswald',sans-serif; font-size:0.8rem; opacity:0.7; letter-spacing:2px; text-transform:uppercase; }
  .seats-display {
    background: var(--dark-brown);
    display: inline-block;
    padding: 0.4rem 1.5rem;
    font-family: 'Bebas Neue', cursive;
    font-size: 1.3rem;
    color: var(--gold);
    letter-spacing: 3px;
    margin-top: 0.4rem;
  }
  .progress-bar-wrap { margin: 0.8rem 0; }
  .progress-bar-bg { background: rgba(0,0,0,0.5); height: 12px; border-radius: 0; border: 1px solid rgba(232,137,12,0.3); }
  .progress-bar-fill { height:100%; background: linear-gradient(90deg, var(--dark-orange), var(--orange)); transition: width 0.5s; }

  .phase-box {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    padding: 1rem 1.2rem;
  }
  .phase-box h3 { font-family:'Bebas Neue',cursive; font-size:1.3rem; color:var(--orange); letter-spacing:3px; margin-bottom:0.6rem; }

  .actions-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.6rem;
  }
  .action-btn {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    color: var(--cream);
    padding: 0.8rem;
    cursor: pointer;
    text-align: left;
    transition: all 0.2s;
    font-family: 'Roboto', sans-serif;
    position: relative;
    overflow: hidden;
  }
  .action-btn:hover:not(:disabled) { border-color: var(--orange); background: rgba(232,137,12,0.1); transform:translateY(-2px); }
  .action-btn:disabled { opacity:0.35; cursor:not-allowed; }
  .action-btn.improved { border-color: var(--gold) !important; }
  .action-btn .ab-icon { font-size:1.4rem; display:block; margin-bottom:0.3rem; }
  .action-btn .ab-name { font-family:'Oswald',sans-serif; font-size:0.95rem; font-weight:700; color:var(--orange); letter-spacing:1px; }
  .action-btn .ab-desc { font-size:0.72rem; opacity:0.7; margin-top:0.2rem; line-height:1.3; }
  .action-btn .ab-bonus { font-size:0.7rem; color:var(--gold); margin-top:0.2rem; font-weight:700; }
  .nuke-btn { grid-column: 1/-1; }
  .nuke-btn .action-btn { border-color: rgba(192,57,43,0.5) !important; }
  .nuke-btn .action-btn:hover:not(:disabled) { border-color: var(--red) !important; background: rgba(192,57,43,0.1); }
  .nuke-btn .ab-name { color: var(--red); }

  .contribute-section { }
  .contribute-controls { display:flex; gap:0.6rem; align-items:center; flex-wrap:wrap; margin-top:0.8rem; }
  .contrib-input {
    flex:1; min-width:80px;
    background: var(--dark-brown);
    border: 2px solid var(--dark-orange);
    color: var(--cream);
    padding: 0.6rem;
    font-family: 'Oswald', sans-serif;
    font-size: 1.1rem;
    text-align: center;
    outline: none;
  }
  .contrib-input:focus { border-color: var(--orange); }
  .contrib-info { font-size:0.8rem; opacity:0.6; margin-top:0.4rem; }

  .event-card-display {
    background: var(--dark-brown);
    border: 3px solid;
    padding: 1.5rem;
    text-align: center;
    position: relative;
  }
  .event-card-display.public { border-color: var(--cream); }
  .event-card-display.clandestine { border-color: #555; background: #111; }
  .event-card-display.hidden-event {
    border-color: #333;
    background: #0a0a0a;
    filter: blur(2px);
    pointer-events: none;
  }
  .event-type-badge {
    font-family: 'Oswald', sans-serif;
    font-size: 0.75rem;
    letter-spacing: 3px;
    text-transform: uppercase;
    padding: 3px 12px;
    display: inline-block;
    margin-bottom: 0.8rem;
  }
  .event-type-badge.public { background: var(--cream); color: var(--dark-brown); }
  .event-type-badge.clandestine { background: #333; color: #aaa; }
  .event-title { font-family:'Bebas Neue',cursive; font-size:1.8rem; color:var(--orange); letter-spacing:3px; margin-bottom:0.8rem; }
  .event-text { font-size:0.9rem; line-height:1.6; opacity:0.9; }
  .event-category { font-size:0.75rem; opacity:0.5; margin-top:0.6rem; font-style:italic; }
  .hidden-event-msg { font-family:'Bebas Neue',cursive; font-size:1.5rem; letter-spacing:3px; color:#555; }

  .panel-right {
    background: rgba(26,14,6,0.95);
    border-left: 2px solid rgba(232,137,12,0.2);
    overflow-y: auto;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.8rem;
  }

  .my-stats-box {
    background: var(--dark-brown);
    border: 2px solid var(--orange);
    padding: 1rem;
  }
  .my-stats-box h4 { font-family:'Bebas Neue',cursive; font-size:1rem; color:var(--orange); letter-spacing:3px; margin-bottom:0.6rem; }
  .my-stat-row { display:flex; justify-content:space-between; align-items:center; padding:0.3rem 0; border-bottom:1px solid rgba(255,255,255,0.05); }
  .my-stat-row:last-child { border-bottom:none; }
  .my-stat-label { font-size:0.8rem; opacity:0.7; font-family:'Oswald',sans-serif; }
  .my-stat-value { font-family:'Bebas Neue',cursive; font-size:1.5rem; color:var(--cream); }
  .my-stat-value.res-val { color: #7EC8E3; }
  .my-stat-value.inf-val { color: var(--gold); }
  .my-stat-value.contrib-val { color: var(--green); }

  .my-civ-box {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    padding: 1rem;
  }
  .my-civ-box h4 { font-family:'Bebas Neue',cursive; font-size:1rem; color:var(--orange); letter-spacing:3px; margin-bottom:0.5rem; }
  .my-civ-name { font-family:'Bebas Neue',cursive; font-size:1.4rem; color:var(--cream); letter-spacing:2px; margin-bottom:0.3rem; }
  .my-civ-perks { font-size:0.78rem; line-height:1.6; opacity:0.75; }
  .perk-item { margin-bottom:0.3rem; }
  .perk-item strong { color: var(--gold); }

  .active-effects { }
  .effect-chip {
    background: rgba(142,68,173,0.2);
    border: 1px solid var(--purple);
    padding: 0.4rem 0.6rem;
    font-size: 0.75rem;
    margin-bottom: 0.4rem;
    line-height: 1.4;
  }

  .vote-section {
    background: var(--dark-brown);
    border: 2px solid var(--gold);
    padding: 1rem;
  }
  .vote-section h3 { font-family:'Bebas Neue',cursive; font-size:1.3rem; color:var(--gold); letter-spacing:3px; margin-bottom:0.5rem; }
  .vote-question { font-size:0.85rem; margin-bottom:0.8rem; opacity:0.8; }
  .vote-options { display:flex; gap:0.5rem; flex-wrap:wrap; }
  .vote-option-btn {
    flex:1; min-width:100px;
    background: rgba(0,0,0,0.3);
    border: 2px solid rgba(255,255,255,0.2);
    color: var(--cream);
    padding: 0.6rem 0.4rem;
    cursor: pointer;
    font-family: 'Oswald', sans-serif;
    font-size: 0.85rem;
    transition: all 0.2s;
    text-align: center;
  }
  .vote-option-btn:hover:not(:disabled) { border-color: var(--gold); background: rgba(241,196,15,0.1); }
  .vote-option-btn:disabled { opacity:0.5; cursor:not-allowed; }
  .vote-option-btn.voted { border-color: var(--gold); background: rgba(241,196,15,0.2); }
  .vote-results { font-size:0.8rem; margin-top:0.6rem; opacity:0.7; }

  .target-modal {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.85);
    z-index: 500;
    align-items: center;
    justify-content: center;
  }
  .target-modal.active { display:flex; }
  .target-box {
    background: var(--brown);
    border: 3px solid var(--orange);
    padding: 2rem;
    max-width: 400px;
    width: 90%;
  }
  .target-box h3 { font-family:'Bebas Neue',cursive; font-size:1.8rem; color:var(--orange); letter-spacing:3px; margin-bottom:1rem; }
  .target-options { display:flex; flex-direction:column; gap:0.5rem; }
  .target-option {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    color: var(--cream);
    padding: 0.8rem 1rem;
    cursor: pointer;
    font-family: 'Oswald', sans-serif;
    font-size: 1rem;
    text-align: left;
    transition: all 0.2s;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .target-option:hover { border-color:var(--orange); background:rgba(232,137,12,0.1); }
  .target-cancel { margin-top:0.8rem; width:100%; }

  #end-screen {
    background: radial-gradient(ellipse at center, #1A0E06, #000);
    align-items: center;
    justify-content: center;
    padding: 2rem;
    text-align: center;
  }
  .end-title { font-family:'Bebas Neue',cursive; font-size:clamp(3rem,10vw,7rem); letter-spacing:4px; margin-bottom:1rem; }
  .end-title.doomed { color:var(--red); text-shadow: 0 0 30px rgba(192,57,43,0.5); }
  .end-title.launched { color:var(--orange); text-shadow: 0 0 30px rgba(232,137,12,0.5); }
  .end-results { margin:2rem 0; }
  .end-player-result {
    background: var(--dark-brown);
    border: 2px solid rgba(232,137,12,0.3);
    padding: 1rem 1.5rem;
    margin-bottom: 0.8rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
  }
  .end-player-result.winner { border-color:var(--gold); background:rgba(241,196,15,0.1); }
  .end-player-result.dead { opacity:0.5; }
  .epr-name { font-family:'Oswald',sans-serif; font-size:1.1rem; font-weight:700; }
  .epr-civ { font-size:0.8rem; color:var(--orange); }
  .epr-outcome { font-family:'Bebas Neue',cursive; font-size:1.2rem; letter-spacing:2px; }
  .epr-outcome.winner { color:var(--gold); }
  .epr-outcome.dead { color:var(--red); }
  .epr-stats { font-size:0.8rem; opacity:0.7; }

  .toast-container { position:fixed; top:80px; right:1rem; z-index:2000; display:flex; flex-direction:column; gap:0.5rem; }
  .toast {
    background: var(--brown);
    border-left: 4px solid var(--orange);
    padding: 0.8rem 1.2rem;
    font-family:'Oswald',sans-serif;
    font-size:0.9rem;
    max-width:300px;
    animation: slideIn 0.3s ease, fadeOut 0.5s ease 3.5s forwards;
    box-shadow: 0 4px 15px rgba(0,0,0,0.5);
  }
  .toast.error { border-color:var(--red); }
  .toast.success { border-color:var(--green); }
  .toast.info { border-color:var(--blue); }
  @keyframes slideIn { from{transform:translateX(100%);opacity:0;} to{transform:translateX(0);opacity:1;} }
  @keyframes fadeOut { from{opacity:1;} to{opacity:0;pointer-events:none;} }

  .waiting-overlay {
    position: absolute;
    inset: 0;
    background: rgba(0,0,0,0.7);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 50;
    flex-direction: column;
    gap: 1rem;
  }
  .waiting-overlay.hidden { display:none; }
  .waiting-text { font-family:'Bebas Neue',cursive; font-size:2rem; color:var(--orange); letter-spacing:4px; text-align:center; }
  .waiting-sub { font-size:0.9rem; opacity:0.6; }

  .doom-bar-wrap { width:100%; margin-top:0.5rem; }
  .doom-label { font-family:'Oswald',sans-serif; font-size:0.75rem; letter-spacing:2px; color:var(--red); text-align:right; margin-bottom:3px; }
  .doom-bar-bg { background:rgba(0,0,0,0.5); height:8px; border:1px solid rgba(192,57,43,0.3); }
  .doom-bar-fill { height:100%; background: linear-gradient(90deg, var(--red), #FF6B6B); transition:width 1s linear; }

  .spinner { width:40px; height:40px; border:4px solid rgba(232,137,12,0.2); border-top-color:var(--orange); border-radius:50%; animation:spin 0.8s linear infinite; }
  @keyframes spin { to{transform:rotate(360deg);} }

  .btn-sm {
    font-family:'Bebas Neue',cursive;
    font-size:1.1rem;
    letter-spacing:2px;
    padding:0.5rem 1.2rem;
    background:var(--orange);
    color:var(--dark-brown);
    border:none;
    cursor:pointer;
    transition:all 0.2s;
  }
  .btn-sm:hover { background:#F5A030; }
  .btn-sm.secondary { background:transparent; color:var(--orange); border:2px solid var(--orange); }
  .btn-sm.danger { background:var(--red); color:white; }

  .log-box {
    background: rgba(0,0,0,0.4);
    border: 1px solid rgba(232,137,12,0.15);
    padding: 0.6rem;
    max-height: 140px;
    overflow-y: auto;
    font-size: 0.75rem;
    line-height: 1.6;
    opacity: 0.7;
  }
  .log-entry { border-bottom:1px solid rgba(255,255,255,0.04); padding:2px 0; }
  .log-entry:last-child { border-bottom:none; }

  hr.divider { border:none; border-top:1px solid rgba(232,137,12,0.15); margin:0.5rem 0; }

  .hidden { display:none !important; }

  ::-webkit-scrollbar { width:5px; }
  ::-webkit-scrollbar-track { background:rgba(0,0,0,0.2); }
  ::-webkit-scrollbar-thumb { background:var(--dark-orange); }
</style>
</head>
<body>

<div id="home-screen" class="screen active">
  <div class="home-content">
    <div class="game-logo">WE'RE<br>DOOMED!</div>
    <div class="game-subtitle">★ The Game of Global Panic ★</div>
    <div class="home-buttons">
      <button class="btn-primary" onclick="showCreateModal()">Create Room</button>
      <button class="btn-secondary" onclick="showJoinModal()">Join Room</button>
    </div>
  </div>
</div>

<div id="lobby-screen" class="screen">
  <div class="lobby-header">
    <h1>WE'RE DOOMED!</h1>
    <div>Room Code: <span class="lobby-code-badge" id="lobby-code-display">XXXXX</span></div>
  </div>
  <div class="civ-picker">
    <h3>Choose Your Civilization</h3>
    <div class="civ-cards" id="civ-cards-container"></div>
  </div>
  <div class="lobby-players" id="lobby-players-grid"></div>
  <div class="lobby-actions">
    <div class="lobby-status" id="lobby-status-text">Waiting for players… (4 minimum)</div>
    <button class="btn-primary hidden" id="start-game-btn" onclick="startGame()">Launch The Project</button>
    <button class="btn-secondary" onclick="leaveRoom()">Leave Room</button>
  </div>
</div>

<div id="game-screen" class="screen">
  <div class="game-topbar">
    <div class="topbar-logo">WE'RE DOOMED!</div>
    <div id="timer-display" class="timer-display">15:00</div>
    <div id="topbar-phase" class="topbar-phase">Take Action Phase</div>
  </div>

  <div class="game-layout">
    <div class="panel-left">
      <div class="panel-title">PLAYERS</div>
      <div id="players-list"></div>
      <hr class="divider">
      <div class="panel-title" style="margin-top:0.3rem">ACTIVITY LOG</div>
      <div class="log-box" id="activity-log"></div>
    </div>

    <div class="panel-center" style="position:relative">
      <div class="waiting-overlay hidden" id="waiting-overlay">
        <div class="spinner"></div>
        <div class="waiting-text" id="waiting-text">WAITING...</div>
        <div class="waiting-sub" id="waiting-sub"></div>
      </div>

      <div class="project-box">
        <h3>🚀 THE PROJECT</h3>
        <div class="resource-track">
          <div>
            <div class="track-number" id="project-resources">0</div>
            <div class="track-label">Resources Contributed</div>
          </div>
        </div>
        <div class="progress-bar-wrap">
          <div class="progress-bar-bg">
            <div class="progress-bar-fill" id="progress-fill" style="width:0%"></div>
          </div>
        </div>
        <div class="seats-display" id="seats-display">🚀 0 Seats Built (need 40 to launch)</div>
        <div class="doom-bar-wrap">
          <div class="doom-label">⚠ DOOM</div>
          <div class="doom-bar-bg"><div class="doom-bar-fill" id="doom-bar" style="width:100%"></div></div>
        </div>
      </div>

      <div class="phase-box" id="phase-panel">
        <h3 id="phase-title">TAKE ACTION</h3>
        <div id="phase-content"></div>
      </div>

      <div id="event-area" class="hidden"></div>
      <div id="vote-area" class="hidden"></div>
    </div>

    <div class="panel-right">
      <div class="panel-title">MY CIVILIZATION</div>
      <div class="my-civ-box">
        <div class="my-civ-name" id="my-civ-name">—</div>
        <div class="my-civ-perks" id="my-civ-perks"></div>
      </div>

      <div class="panel-title">MY RESOURCES</div>
      <div class="my-stats-box">
        <div class="my-stat-row">
          <span class="my-stat-label">🌿 Resources</span>
          <span class="my-stat-value res-val" id="my-resources">0</span>
        </div>
        <div class="my-stat-row">
          <span class="my-stat-label">🔺 Influence</span>
          <span class="my-stat-value inf-val" id="my-influence">0</span>
        </div>
        <div class="my-stat-row">
          <span class="my-stat-label">🌿 Contributed</span>
          <span class="my-stat-value contrib-val" id="my-contribution">0</span>
        </div>
        <div class="my-stat-row">
          <span class="my-stat-label">🚀 Seat Status</span>
          <span class="my-stat-value" id="my-seat-status" style="font-size:1rem">TBD</span>
        </div>
      </div>

      <div id="active-effects-area" class="hidden">
        <div class="panel-title">ACTIVE EFFECTS</div>
        <div id="active-effects-list"></div>
      </div>

      <div class="panel-title">CONTRIBUTE TO PROJECT</div>
      <div class="my-stats-box">
        <div style="font-size:0.8rem;opacity:0.7;margin-bottom:0.6rem">Contribute Resources anytime. You must announce your total!</div>
        <div class="contribute-controls">
          <input type="number" class="contrib-input" id="contrib-amount" min="1" placeholder="Amount" />
          <button class="btn-sm" onclick="contributeResources()">CONTRIBUTE</button>
        </div>
        <div class="contrib-info" id="contrib-info"></div>
      </div>
    </div>
  </div>
</div>

<div id="end-screen" class="screen">
  <div id="end-title-display" class="end-title">GAME OVER</div>
  <div id="end-subtitle" style="font-family:'Oswald',sans-serif;font-size:1.1rem;opacity:0.7;margin-bottom:1.5rem;"></div>
  <div class="end-results" id="end-results-list"></div>
  <button class="btn-primary" onclick="goHome()" style="margin-top:1rem">Return to Earth (Home)</button>
</div>

<div class="modal-overlay" id="create-modal">
  <div class="modal">
    <h2>CREATE ROOM</h2>
    <input type="text" id="create-name-input" placeholder="Your name" maxlength="20"/>
    <div id="created-room-code" class="room-code-display hidden"></div>
    <div id="create-status" style="font-size:0.85rem;opacity:0.7;text-align:center;margin-bottom:0.5rem;"></div>
    <div class="modal-buttons">
      <button class="btn-cancel" onclick="closeModals()">Cancel</button>
      <button class="btn-primary" id="create-btn" onclick="createRoom()" style="font-size:1.3rem;padding:0.7rem 1.5rem;">CREATE</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="join-modal">
  <div class="modal">
    <h2>JOIN ROOM</h2>
    <input type="text" id="join-name-input" placeholder="Your name" maxlength="20"/>
    <input type="text" id="join-code-input" placeholder="Room code (e.g. A3X7K)" maxlength="5"/>
    <div id="join-status" style="font-size:0.85rem;opacity:0.7;text-align:center;margin-bottom:0.5rem;"></div>
    <div class="modal-buttons">
      <button class="btn-cancel" onclick="closeModals()">Cancel</button>
      <button class="btn-primary" id="join-btn" onclick="joinRoom()" style="font-size:1.3rem;padding:0.7rem 1.5rem;">JOIN</button>
    </div>
  </div>
</div>

<div class="target-modal" id="target-modal">
  <div class="target-box">
    <h3 id="target-title">CHOOSE TARGET</h3>
    <div class="target-options" id="target-options"></div>
    <button class="btn-sm secondary target-cancel" onclick="closeTargetModal()">Cancel</button>
  </div>
</div>

<div class="toast-container" id="toast-container"></div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-analytics.js";
import {
  getFirestore, doc, collection, setDoc, getDoc, updateDoc,
  onSnapshot, arrayUnion, serverTimestamp, deleteDoc, writeBatch, increment
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyDpWhBOUP6rSWv4N6G1Q7CCTcWvJ8upb4g",
  authDomain: "we-re-doomed-app.firebaseapp.com",
  projectId: "we-re-doomed-app",
  storageBucket: "we-re-doomed-app.firebasestorage.app",
  messagingSenderId: "581413709334",
  appId: "1:581413709334:web:449a23a34d6d823abeb737",
  measurementId: "G-3KYZG2GPYK"
};
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
const db = getFirestore(app);

let myPlayerId = null;
let myPlayerName = null;
let currentRoomCode = null;
let roomUnsubscribe = null;
let playersUnsubscribe = null;
let timerInterval = null;
let pendingAction = null;
let localGameState = null;
let localPlayers = {};
let isHost = false;

const CIVILIZATIONS = {
  Autocracy: {
    icon: '⚔️', color: '#C0392B',
    perk: 'NUKE costs only 5 Resources instead of 8',
    improvedAction: 'nuke',
    perks: [
      { action: 'Produce', desc: 'Gain 2 Resources' },
      { action: 'Indoctrinate', desc: 'Gain 1 Influence' },
      { action: 'Propagandize', desc: 'Spend 1 Resource → steal 1 Influence' },
      { action: 'Invade', desc: 'Spend 1 Influence → steal 2 Resources' },
      { action: 'NUKE ★', desc: 'Spend 5 Resources to eliminate a player' },
    ]
  },
  Corporatocracy: {
    icon: '💼', color: '#27AE60',
    perk: 'PROPAGANDIZE is free (no Resource cost)',
    improvedAction: 'propagandize',
    perks: [
      { action: 'Produce', desc: 'Gain 2 Resources' },
      { action: 'Indoctrinate', desc: 'Gain 1 Influence' },
      { action: 'PROPAGANDIZE ★', desc: 'FREE — steal 1 Influence from any player' },
      { action: 'Invade', desc: 'Spend 1 Influence → steal 2 Resources' },
      { action: 'Nuke', desc: 'Spend 8 Resources to eliminate a player' },
    ]
  },
  Democracy: {
    icon: '🗳️', color: '#2980B9',
    perk: 'INVADE is free (no Influence cost)',
    improvedAction: 'invade',
    perks: [
      { action: 'Produce', desc: 'Gain 2 Resources' },
      { action: 'Indoctrinate', desc: 'Gain 1 Influence' },
      { action: 'Propagandize', desc: 'Spend 1 Resource → steal 1 Influence' },
      { action: 'INVADE ★', desc: 'FREE — steal 2 Resources from any player' },
      { action: 'Nuke', desc: 'Spend 8 Resources to eliminate a player' },
    ]
  },
  Technocracy: {
    icon: '⚙️', color: '#16A085',
    perk: 'PRODUCE gives 3 Resources instead of 2',
    improvedAction: 'produce',
    perks: [
      { action: 'PRODUCE ★', desc: 'Gain 3 Resources' },
      { action: 'Indoctrinate', desc: 'Gain 1 Influence' },
      { action: 'Propagandize', desc: 'Spend 1 Resource → steal 1 Influence' },
      { action: 'Invade', desc: 'Spend 1 Influence → steal 2 Resources' },
      { action: 'Nuke', desc: 'Spend 8 Resources to eliminate a player' },
    ]
  },
  Theocracy: {
    icon: '✝️', color: '#8E44AD',
    perk: 'INDOCTRINATE gives 2 Influence instead of 1',
    improvedAction: 'indoctrinate',
    perks: [
      { action: 'Produce', desc: 'Gain 2 Resources' },
      { action: 'INDOCTRINATE ★', desc: 'Gain 2 Influence' },
      { action: 'Propagandize', desc: 'Spend 1 Resource → steal 1 Influence' },
      { action: 'Invade', desc: 'Spend 1 Influence → steal 2 Resources' },
      { action: 'Nuke', desc: 'Spend 8 Resources to eliminate a player' },
    ]
  }
};

const EVENT_CARDS = [
  { id:1, type:'public', category:'resource', title:'GLOBAL RECESSION', text:'All players lose 2 Resources. Spent resources return to the bag.' },
  { id:2, type:'public', category:'resource', title:'MARKET CRASH', text:'The player with the most Resources loses 3 Resources.' },
  { id:3, type:'public', category:'resource', title:'WEALTH REDISTRIBUTION', text:'The richest player gives 2 Resources to the poorest player.' },
  { id:4, type:'public', category:'resource', title:'FOREIGN AID', text:'The player with the fewest Resources gains 3 Resources.' },
  { id:5, type:'public', category:'resource', title:'TAX THE RICH', text:'Every player with 5 or more Resources loses 2 Resources.' },
  { id:6, type:'public', category:'resource', title:'RESOURCE WINDFALL', text:'All active players gain 1 Resource.' },
  { id:7, type:'public', category:'resource', title:'SUPPLY CHAIN CRISIS', text:'All players lose 1 Resource. This cannot reduce anyone below 0.' },
  { id:8, type:'public', category:'resource', title:'ECONOMIC BOOM', text:'All players gain 2 Resources.' },
  { id:9, type:'public', category:'resource', title:'AUSTERITY MEASURES', text:'All players must immediately contribute 1 Resource to The Project or lose 2 Resources.' },
  { id:10, type:'clandestine', category:'resource', title:'SECRET SUBSIDY', text:'You gain 3 Resources. Tell no one.' },
  { id:11, type:'clandestine', category:'resource', title:'BLACK MARKET', text:'You may steal 2 Resources from any one player right now. No action required.' },
  { id:12, type:'clandestine', category:'resource', title:'EMBEZZLEMENT', text:'Take 1 Resource from every other player. Keep this card secret.' },
  { id:13, type:'clandestine', category:'resource', title:'INSIDER TRADING', text:'Gain Resources equal to the number of active players minus 1.' },
  { id:14, type:'clandestine', category:'resource', title:'OFFSHORE ACCOUNT', text:'Your Resources cannot be stolen by Invade or event cards until the next Contribute phase.' },
  { id:15, type:'public', category:'resource', title:'PRICE CONTROLS', text:'No player may gain more than 1 Resource from any single action until the next Contribute phase.' },
  { id:16, type:'public', category:'resource', title:'INFRASTRUCTURE COLLAPSE', text:'The player with the most Resources loses half their Resources (round down).' },
  { id:17, type:'clandestine', category:'resource', title:'RESOURCE CACHE', text:'Gain 4 Resources. You must contribute at least 2 to The Project this Contribute phase or lose them all.' },
  { id:18, type:'public', category:'resource', title:'RELIEF FUND', text:'All players with 0 Resources gain 3 Resources.' },
  { id:19, type:'public', category:'influence', title:'PROPAGANDA BLITZ', text:'The player with the most Influence loses 2 Influence.' },
  { id:20, type:'public', category:'influence', title:'POPULAR UPRISING', text:'The player with the least Influence gains 2 Influence.' },
  { id:21, type:'public', category:'influence', title:'POLITICAL SCANDAL', text:'All players lose 1 Influence.' },
  { id:22, type:'public', category:'influence', title:'MASS RALLY', text:'All active players gain 1 Influence.' },
  { id:23, type:'public', category:'influence', title:'CELEBRITY ENDORSEMENT', text:'The current first player gains 2 Influence.' },
  { id:24, type:'clandestine', category:'influence', title:'SHADOW LOBBYING', text:'Gain 2 Influence. You may not announce this gain.' },
  { id:25, type:'clandestine', category:'influence', title:'BLACKMAIL', text:'Choose one player. They must give you 1 Influence or lose 2 Resources. You decide their punishment if they refuse.' },
  { id:26, type:'public', category:'influence', title:'INFLUENCE AUDIT', text:'All players must reveal their current Influence total.' },
  { id:27, type:'public', category:'influence', title:'POWER VACUUM', text:'The player with the most Influence loses all but 2 Influence.' },
  { id:28, type:'clandestine', category:'influence', title:'GRASSROOTS CAMPAIGN', text:'Gain 1 Influence for each player who has contributed more to The Project than you.' },
  { id:29, type:'public', category:'influence', title:'MEDIA CIRCUS', text:'Redistribute Influence: the top Influence player gives 1 to the bottom Influence player.' },
  { id:30, type:'clandestine', category:'influence', title:'DEEP STATE ALLIES', text:'Your Influence cannot be stolen or reduced by any means until the start of your next turn.' },
  { id:31, type:'public', category:'influence', title:'NATIONAL HERO', text:'The player who has contributed the most Resources to The Project gains 2 Influence.' },
  { id:32, type:'public', category:'influence', title:'DIPLOMATIC IMMUNITY', text:'Choose one player. That player cannot be targeted by Propagandize or Invade until the next Contribute phase.' },
  { id:33, type:'public', category:'redistribution', title:'PASS THE BUCK', text:'All players pass 1 Resource to the player on their left (clockwise).' },
  { id:34, type:'public', category:'redistribution', title:'REVERSE THE TIDE', text:'All players pass 1 Influence to the player on their right (counter-clockwise).' },
  { id:35, type:'public', category:'redistribution', title:'EQUALIZER', text:'Pool all Resources from all players. Divide them as evenly as possible among all active players (richest gets remainder).' },
  { id:36, type:'public', category:'redistribution', title:'INFLUENCE DRAIN', text:'Pool all Influence from all players. Divide evenly; player with most Resources gets remainder.' },
  { id:37, type:'public', category:'redistribution', title:'ROBIN HOOD', text:'The richest player must give 2 Resources to any player of the drawer\'s choice.' },
  { id:38, type:'public', category:'redistribution', title:'GIVE TO CAESAR', text:'Each player gives 1 Resource to the current first player.' },
  { id:39, type:'public', category:'redistribution', title:'RESOURCE SWAP', text:'The player with the most Resources and the player with the fewest swap their Resource totals.' },
  { id:40, type:'public', category:'redistribution', title:'INFLUENCE SWAP', text:'The player with the most Influence and the player with the fewest swap their Influence totals.' },
  { id:41, type:'clandestine', category:'redistribution', title:'SHELL COMPANY', text:'You may swap your Resource total with any one other player\'s. They are not informed which card was drawn.' },
  { id:42, type:'public', category:'redistribution', title:'TRICKLE DOWN', text:'The player with the most Resources gives 1 Resource to each other active player.' },
  { id:43, type:'public', category:'redistribution', title:'COLLECTIVE BARGAINING', text:'All players vote: if majority agrees, all Resources are pooled and redistributed equally. Ties mean no redistribution.' },
  { id:44, type:'public', category:'redistribution', title:'WEALTH TAX', text:'Every player with 4+ Resources must contribute 1 Resource directly to The Project now.' },
  { id:45, type:'public', category:'redistribution', title:'HUMANITARIAN AID', text:'Each player with 3+ Resources gives 1 Resource to a player with 0 Resources (their choice).' },
  { id:46, type:'clandestine', category:'redistribution', title:'HOSTILE TAKEOVER', text:'Take all Resources from any one player who has 2 or fewer Resources.' },
  { id:47, type:'public', category:'redistribution', title:'GLOBAL COMPACT', text:'All players simultaneously contribute 1 Resource to The Project. Players with 0 are exempt.' },
  { id:48, type:'public', category:'communication', title:'RADIO SILENCE', text:'No player may speak for the duration of the next Take Action phase. Actions must be performed silently. Violations forfeit 1 Influence.' },
  { id:49, type:'public', category:'communication', title:'CENSORSHIP', text:'Only the current first player may speak during the next Take Action phase.' },
  { id:50, type:'public', category:'communication', title:'WHISPER NETWORK', text:'Players may only communicate via whispers to adjacent players for the next full round.' },
  { id:51, type:'public', category:'communication', title:'PROPAGANDA BLACKOUT', text:'No player may make deals or promises for the duration of the next Contribute phase.' },
  { id:52, type:'public', category:'communication', title:'FREE PRESS', text:'All players must truthfully answer one yes/no question asked by any other player. First player chooses question order.' },
  { id:53, type:'clandestine', category:'communication', title:'MOLE IN THE ROOM', text:'You may lie freely about your Resource and Influence totals for the rest of this round only.' },
  { id:54, type:'public', category:'communication', title:'SUMMIT MEETING', text:'All players must state one true thing about their current Resources or Influence before the next action phase begins.' },
  { id:55, type:'public', category:'communication', title:'PRESS CONFERENCE', text:'The first player must announce exactly how much they plan to contribute next Contribute phase. They must honor this if able.' },
  { id:56, type:'public', category:'communication', title:'GOTTA GO FAST', text:'The next Take Action phase has no stalling allowed. All decisions must be made in 5 seconds or the player loses their action.' },
  { id:57, type:'clandestine', category:'communication', title:'DISINFORMATION', text:'You may announce a false Resource total for any one player during this Contribute phase. Make it convincing.' },
  { id:58, type:'public', category:'communication', title:'OPEN BOOKS', text:'This card stays in play. All players must keep their Resources and Influence visible at all times. Discard at game end.' },
  { id:59, type:'public', category:'negotiation', title:'BINDING TREATY', text:'This card stays in play. Any deal made this round is binding — players who break deals lose 3 Influence immediately.' },
  { id:60, type:'public', category:'negotiation', title:'MANDATORY MEDIATION', text:'Every player must make a deal with one other player before the next Take Action phase can begin.' },
  { id:61, type:'public', category:'negotiation', title:'NON-AGGRESSION PACT', text:'No player may Invade or Propagandize against any player who contributed last round. Lasts one Take Action phase.' },
  { id:62, type:'public', category:'negotiation', title:'TRADE EMBARGO', text:'Choose one player. No other player may make any deal involving Resources with that player this round.' },
  { id:63, type:'public', category:'negotiation', title:'MUTUAL ASSURED DESTRUCTION', text:'If any player uses Nuke this round, all other players immediately lose 2 Resources.' },
  { id:64, type:'clandestine', category:'negotiation', title:'SECRET ALLIANCE', text:'Choose one player secretly. If you both board the rocket, you each gain an additional point of Influence for final seat order.' },
  { id:65, type:'public', category:'negotiation', title:'PEACE SUMMIT', text:'No Nuke actions may be taken for the next full round.' },
  { id:66, type:'public', category:'negotiation', title:'HOSTILE NEGOTIATIONS', text:'All deals this round must include a Resource or Influence exchange. Promises alone are not permitted.' },
  { id:67, type:'clandestine', category:'negotiation', title:'DOUBLE AGENT', text:'You may promise the same deal to two different players simultaneously. Only one will be honored — your choice.' },
  { id:68, type:'public', category:'negotiation', title:'CEASEFIRE', text:'This card stays in play until the next event is drawn. No Invade or Propagandize actions are permitted.' },
  { id:69, type:'public', category:'negotiation', title:'OPEN MARKET', text:'Players may freely trade Resources and Influence with each other for the next 60 seconds. All trades are non-binding.' },
  { id:70, type:'public', category:'punishment', title:'THE TAXMAN COMETH', text:'The player with the most Resources must contribute 3 Resources to The Project immediately.' },
  { id:71, type:'public', category:'punishment', title:'POWER CORRUPTS', text:'The player with the most Influence loses 2 Influence.' },
  { id:72, type:'public', category:'punishment', title:'WITCH HUNT', text:'Players vote for one player to lose 2 Resources. Majority wins. First player breaks ties.' },
  { id:73, type:'public', category:'punishment', title:'SCAPEGOAT', text:'Players vote for one player to lose 2 Influence. Majority wins. First player breaks ties.' },
  { id:74, type:'public', category:'punishment', title:'TRIAL BY PEERS', text:'Players vote: one player loses either 2 Resources or 1 Influence. The voted player chooses which.' },
  { id:75, type:'public', category:'punishment', title:'SANCTIONS', text:'Choose any player. They may not take the Produce action during the next Take Action phase.' },
  { id:76, type:'public', category:'punishment', title:'COUP ATTEMPT', text:'The player with the least Influence steals 1 Influence from the player with the most.' },
  { id:77, type:'clandestine', category:'punishment', title:'POLITICAL HIT', text:'Choose one player. They lose 1 Influence. This is anonymous — they do not know who triggered it.' },
  { id:78, type:'clandestine', category:'punishment', title:'CORPORATE SABOTAGE', text:'Choose one player. Their next Produce action gains 1 fewer Resource (minimum 0).' },
  { id:79, type:'public', category:'punishment', title:'AUDIT', text:'The player with the most Resources must honestly state their total. If they lie, they lose all Resources.' },
  { id:80, type:'public', category:'punishment', title:'BETRAYAL', text:'If any player made a deal last round and did not honor it, they lose 3 Resources now.' },
  { id:81, type:'public', category:'punishment', title:'REVOLUTION', text:'All players with more than 5 Resources lose 2 Resources.' },
  { id:82, type:'clandestine', category:'punishment', title:'SABOTEUR', text:'Remove 2 Resources from The Project. Tell no one.' },
  { id:83, type:'public', category:'punishment', title:'RESOURCE SEIZURE', text:'The player with the fewest contributions to The Project loses 2 Resources.' },
  { id:84, type:'public', category:'doom', title:'METEOR SHOWER', text:'The Doom clock accelerates — skip directly to the next Contribute phase.' },
  { id:85, type:'public', category:'doom', title:'TECTONIC COLLAPSE', text:'All players lose 1 Resource AND the Doom clock loses 30 seconds.' },
  { id:86, type:'public', category:'doom', title:'PANDEMIC SURGE', text:'All players must immediately pass their turn. Proceed directly to the Contribute phase.' },
  { id:87, type:'public', category:'doom', title:'SOLAR FLARE', text:'Immediately discard the top 3 event cards. The next event drawn costs the drawer 1 Influence.' },
  { id:88, type:'public', category:'doom', title:'NUCLEAR MELTDOWN', text:'The player with the fewest Resources is eliminated unless another player volunteers to lose 4 Resources in their place.' },
  { id:89, type:'public', category:'doom', title:'COUNTDOWN ACCELERATED', text:'This card stays in play. All future Doom events have double their effect.' },
  { id:90, type:'clandestine', category:'doom', title:'INSIDE JOB', text:'You may choose to add or remove 30 seconds from the Doom clock. No one knows what you chose.' },
  { id:91, type:'public', category:'doom', title:'LAST CHANCE', text:'If The Project has fewer than 20 Resources, all players must contribute at least 1 Resource now or lose 2 Influence.' },
  { id:92, type:'public', category:'doom', title:'GLOBAL CATASTROPHE', text:'All players lose 2 Resources. If The Project still has fewer than 30 Resources after this, skip the next action phase entirely.' },
  { id:93, type:'public', category:'wildcard', title:'ANARCHY', text:'For the next Take Action phase, players may take actions in any order — not clockwise. First come, first served.' },
  { id:94, type:'public', category:'wildcard', title:'SECOND CHANCE', text:'One previously eliminated player may rejoin with 2 Resources and 1 Influence. Players vote on which one. No eliminated players: no effect.' },
  { id:95, type:'public', category:'wildcard', title:'DOUBLE OR NOTHING', text:'The next player to contribute doubles their contribution — but loses all their remaining Resources afterward.' },
  { id:96, type:'clandestine', category:'wildcard', title:'THE WILD CARD', text:'You may immediately take any one action of your choice right now, outside of turn order, for free.' },
  { id:97, type:'public', category:'wildcard', title:'TIME WARP', text:'The current first player takes two actions this Take Action phase instead of one.' },
  { id:98, type:'public', category:'wildcard', title:'DESPERATE MEASURES', text:'Any player may spend all their Influence to immediately contribute that many Resources to The Project.' },
  { id:99, type:'clandestine', category:'wildcard', title:'GUARDIAN ANGEL', text:'You are immune to the next event card that would negatively affect you.' },
  { id:100, type:'public', category:'wildcard', title:'THE FINAL HOUR', text:'This card stays in play. All players gain 1 Influence and 1 Resource at the start of every Contribute phase for the rest of the game.' },
];

function generateRoomCode() {
  const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  let code = '';
  for (let i = 0; i < 5; i++) code += chars[Math.floor(Math.random() * chars.length)];
  return code;
}

function generatePlayerId() {
  return 'p_' + Math.random().toString(36).substr(2, 9) + '_' + Date.now().toString(36);
}

function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function showToast(msg, type = 'info') {
  const tc = document.getElementById('toast-container');
  const t = document.createElement('div');
  t.className = `toast ${type}`;
  t.textContent = msg;
  tc.appendChild(t);
  setTimeout(() => t.remove(), 4500);
}

function seatsFromResources(r) {
  if (r < 40) return 0;
  return Math.min(10, Math.floor((r - 40) / 10) + 1);
}

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

window.showCreateModal = function() {
  document.getElementById('create-modal').classList.add('active');
  document.getElementById('create-name-input').focus();
};
window.showJoinModal = function() {
  document.getElementById('join-modal').classList.add('active');
  document.getElementById('join-name-input').focus();
};
window.closeModals = function() {
  document.querySelectorAll('.modal-overlay').forEach(m => m.classList.remove('active'));
};

function renderCivPicker(takenCivs) {
  const container = document.getElementById('civ-cards-container');
  container.innerHTML = '';
  const myData = localPlayers[myPlayerId];
  const mySelectedCiv = myData ? myData.civilization : null;
  Object.entries(CIVILIZATIONS).forEach(([name, civ]) => {
    const taken = takenCivs.includes(name) && name !== mySelectedCiv;
    const selected = name === mySelectedCiv;
    const card = document.createElement('div');
    card.className = `civ-card ${selected ? 'selected' : ''} ${taken ? 'taken' : ''}`;
    card.innerHTML = `<div class="civ-icon">${civ.icon}</div><div class="civ-name">${name}</div><div class="civ-perk">${civ.perk}</div>`;
    card.onclick = () => selectCivilization(name);
    container.appendChild(card);
  });
}

async function selectCivilization(civName) {
  if (!currentRoomCode || !myPlayerId) return;
  const takenCivs = Object.values(localPlayers)
    .filter(p => p.id !== myPlayerId && p.civilization)
    .map(p => p.civilization);
  if (takenCivs.includes(civName)) { showToast('That civilization is already taken!', 'error'); return; }
  await updateDoc(doc(db, 'rooms', currentRoomCode, 'players', myPlayerId), { civilization: civName });
  showToast(`You chose ${civName}!`, 'success');
}

window.createRoom = async function() {
  const name = document.getElementById('create-name-input').value.trim();
  if (!name) { showToast('Enter your name!', 'error'); return; }
  document.getElementById('create-btn').disabled = true;
  document.getElementById('create-status').textContent = 'Creating room...';
  try {
    myPlayerId = generatePlayerId();
    myPlayerName = name;
    const code = generateRoomCode();
    const shuffledDeck = shuffle(EVENT_CARDS.map(c => c.id));
    await setDoc(doc(db, 'rooms', code), {
      hostId: myPlayerId,
      status: 'lobby',
      phase: 'action',
      round: 0,
      currentPlayerIndex: 0,
      firstPlayerIndex: 0,
      projectResources: 0,
      eventDeck: shuffledDeck,
      currentEvent: null,
      currentEventDrawerId: null,
      activeEffects: [],
      contributionVotes: {},
      voteData: null,
      contributeReady: {},
      gameLog: [],
      timerStart: null,
      timerDuration: 900,
      createdAt: serverTimestamp()
    });
    await setDoc(doc(db, 'rooms', code, 'players', myPlayerId), {
      id: myPlayerId,
      name,
      civilization: null,
      resources: 0,
      influence: 0,
      contribution: 0,
      isEliminated: false,
      isReady: false,
      joinedAt: serverTimestamp(),
      turnOrder: 0
    });
    currentRoomCode = code;
    isHost = true;
    const codeDisplay = document.getElementById('created-room-code');
    codeDisplay.textContent = code;
    codeDisplay.classList.remove('hidden');
    document.getElementById('create-status').textContent = 'Share this code with your friends!';
    document.getElementById('create-btn').textContent = 'GO TO LOBBY';
    document.getElementById('create-btn').onclick = () => { closeModals(); enterLobby(); };
    document.getElementById('create-btn').disabled = false;
  } catch(e) {
    showToast('Error creating room: ' + e.message, 'error');
    document.getElementById('create-btn').disabled = false;
    document.getElementById('create-status').textContent = '';
  }
};

window.joinRoom = async function() {
  const name = document.getElementById('join-name-input').value.trim();
  const code = document.getElementById('join-code-input').value.trim().toUpperCase();
  if (!name) { showToast('Enter your name!', 'error'); return; }
  if (code.length !== 5) { showToast('Enter a valid 5-character room code!', 'error'); return; }
  document.getElementById('join-btn').disabled = true;
  document.getElementById('join-status').textContent = 'Finding room...';
  try {
    const roomSnap = await getDoc(doc(db, 'rooms', code));
    if (!roomSnap.exists()) { showToast('Room not found!', 'error'); document.getElementById('join-btn').disabled = false; return; }
    const roomData = roomSnap.data();
    if (roomData.status !== 'lobby') { showToast('Game already started!', 'error'); document.getElementById('join-btn').disabled = false; return; }
    myPlayerId = generatePlayerId();
    myPlayerName = name;
    currentRoomCode = code;
    isHost = false;
    const playerCount = Object.keys(localPlayers).length;
    await setDoc(doc(db, 'rooms', code, 'players', myPlayerId), {
      id: myPlayerId,
      name,
      civilization: null,
      resources: 0,
      influence: 0,
      contribution: 0,
      isEliminated: false,
      isReady: false,
      joinedAt: serverTimestamp(),
      turnOrder: playerCount
    });
    closeModals();
    enterLobby();
  } catch(e) {
    showToast('Error joining room: ' + e.message, 'error');
    document.getElementById('join-btn').disabled = false;
    document.getElementById('join-status').textContent = '';
  }
};

function enterLobby() {
  showScreen('lobby-screen');
  document.getElementById('lobby-code-display').textContent = currentRoomCode;
  subscribeToRoom();
}

window.leaveRoom = async function() {
  if (roomUnsubscribe) roomUnsubscribe();
  if (playersUnsubscribe) playersUnsubscribe();
  if (timerInterval) clearInterval(timerInterval);
  if (currentRoomCode && myPlayerId) {
    await deleteDoc(doc(db, 'rooms', currentRoomCode, 'players', myPlayerId));
  }
  currentRoomCode = null; myPlayerId = null; localPlayers = {}; localGameState = null;
  showScreen('home-screen');
};

function subscribeToRoom() {
  if (roomUnsubscribe) roomUnsubscribe();
  if (playersUnsubscribe) playersUnsubscribe();

  roomUnsubscribe = onSnapshot(doc(db, 'rooms', currentRoomCode), snap => {
    if (!snap.exists()) { showToast('Room closed.', 'error'); goHome(); return; }
    localGameState = snap.data();
    if (localGameState.status === 'playing') {
      showScreen('game-screen');
      renderGameScreen();
    } else if (localGameState.status === 'ended') {
      showEndScreen();
    } else {
      renderLobby();
    }
  });

  playersUnsubscribe = onSnapshot(collection(db, 'rooms', currentRoomCode, 'players'), snap => {
    localPlayers = {};
    snap.docs.forEach(d => { localPlayers[d.id] = d.data(); });
    if (localGameState && localGameState.status === 'lobby') renderLobby();
    else if (localGameState && localGameState.status === 'playing') renderGameScreen();
  });
}

function renderLobby() {
  const players = Object.values(localPlayers).sort((a,b) => (a.joinedAt?.seconds||0) - (b.joinedAt?.seconds||0));
  const count = players.length;
  const takenCivs = players.filter(p => p.civilization).map(p => p.civilization);
  renderCivPicker(takenCivs);

  const grid = document.getElementById('lobby-players-grid');
  grid.innerHTML = '';
  const maxSlots = Math.max(8, count + 1);
  for (let i = 0; i < Math.min(maxSlots, 10); i++) {
    const slot = document.createElement('div');
    const p = players[i];
    if (p) {
      const isMe = p.id === myPlayerId;
      slot.className = `player-slot filled ${isMe ? 'you' : ''}`;
      slot.innerHTML = `<div class="player-name">${p.name}</div><div class="player-civ">${p.civilization || 'Choosing...'}</div>${isMe ? '<div class="you-badge">YOU</div>' : ''}`;
    } else {
      slot.className = 'player-slot';
      slot.innerHTML = '<div class="empty-label">Waiting...</div>';
    }
    grid.appendChild(slot);
  }

  const allChosen = players.every(p => p.civilization);
  const canStart = count >= 4 && allChosen && isHost;
  const startBtn = document.getElementById('start-game-btn');
  startBtn.classList.toggle('hidden', !canStart);

  let statusMsg = '';
  if (count < 4) statusMsg = `Waiting for players… (${count}/4 minimum, need ${4 - count} more)`;
  else if (!allChosen) statusMsg = 'Waiting for all players to choose a civilization…';
  else if (!isHost) statusMsg = 'Waiting for host to start the game…';
  else statusMsg = `${count} players ready. You may start!`;
  document.getElementById('lobby-status-text').textContent = statusMsg;
}

window.startGame = async function() {
  const players = Object.values(localPlayers);
  if (players.length < 4) { showToast('Need at least 4 players!', 'error'); return; }
  if (!players.every(p => p.civilization)) { showToast('All players must choose a civilization!', 'error'); return; }

  const ordered = players.sort((a,b) => (a.joinedAt?.seconds||0) - (b.joinedAt?.seconds||0));
  const batch = writeBatch(db);
  ordered.forEach((p, i) => {
    batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
      turnOrder: i, resources: 0, influence: 0, contribution: 0, isEliminated: false
    });
  });
  batch.update(doc(db, 'rooms', currentRoomCode), {
    status: 'playing',
    phase: 'action',
    round: 1,
    currentPlayerIndex: 0,
    firstPlayerIndex: 0,
    contributeReady: {},
    timerStart: serverTimestamp(),
    timerDuration: 900,
    gameLog: ['🚀 The Project has begun! The world has 15 minutes.']
  });
  await batch.commit();
};

function renderGameScreen() {
  if (!localGameState || !localPlayers) return;
  const gs = localGameState;
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const me = localPlayers[myPlayerId];
  if (!me) return;

  renderTimer(gs);

  const pr = gs.projectResources || 0;
  document.getElementById('project-resources').textContent = pr;
  const seats = seatsFromResources(pr);
  document.getElementById('seats-display').textContent = seats > 0
    ? `🚀 ${seats} Seat${seats !== 1 ? 's' : ''} Built`
    : `🚀 No Seats Yet (need ${40 - pr} more Resources to launch)`;
  const pct = Math.min(100, (pr / 130) * 100);
  document.getElementById('progress-fill').style.width = pct + '%';

  const pl = document.getElementById('players-list');
  pl.innerHTML = '';
  players.forEach((p, i) => {
    const isActive = i === gs.currentPlayerIndex && gs.phase === 'action' && !p.isEliminated;
    const isFirst = i === gs.firstPlayerIndex;
    const isMe = p.id === myPlayerId;
    const card = document.createElement('div');
    card.className = `player-card ${isActive ? 'active-turn' : ''} ${isMe ? 'you-card' : ''} ${p.isEliminated ? 'eliminated' : ''}`;
    const civ = CIVILIZATIONS[p.civilization] || {};
    card.innerHTML = `
      <div class="pc-header">
        <div>
          <div class="pc-name">${p.name}${isMe?' (You)':''}</div>
          <div class="pc-civ">${civ.icon||''} ${p.civilization||'?'}</div>
        </div>
        <div>${isFirst ? '<span class="first-player-badge">1ST</span>' : ''}</div>
      </div>
      <div class="pc-stats">
        <div class="stat-chip res"><span class="stat-icon">🌿</span>${p.resources}</div>
        <div class="stat-chip inf"><span class="stat-icon">🔺</span>${p.influence}</div>
        <div class="stat-chip contrib"><span class="stat-icon">🚀</span>${p.contribution}</div>
      </div>`;
    pl.appendChild(card);
  });

  document.getElementById('my-resources').textContent = me.resources || 0;
  document.getElementById('my-influence').textContent = me.influence || 0;
  document.getElementById('my-contribution').textContent = me.contribution || 0;

  const myCiv = CIVILIZATIONS[me.civilization] || {};
  document.getElementById('my-civ-name').textContent = `${myCiv.icon||''} ${me.civilization||'Unknown'}`;
  const perksHtml = (myCiv.perks || []).map(p =>
    `<div class="perk-item"><strong>${p.action}:</strong> ${p.desc}</div>`
  ).join('');
  document.getElementById('my-civ-perks').innerHTML = perksHtml;

  const seatEl = document.getElementById('my-seat-status');
  seatEl.textContent = me.isEliminated ? '❌ ELIMINATED' : 'Determined at end';
  seatEl.style.color = me.isEliminated ? 'var(--red)' : 'var(--cream)';

  renderPhase(gs, players, me);

  const effects = gs.activeEffects || [];
  const effectsArea = document.getElementById('active-effects-area');
  if (effects.length > 0) {
    effectsArea.classList.remove('hidden');
    document.getElementById('active-effects-list').innerHTML = effects.map(e =>
      `<div class="effect-chip">📌 ${e.title}: ${e.text}</div>`
    ).join('');
  } else {
    effectsArea.classList.add('hidden');
  }

  const log = document.getElementById('activity-log');
  const entries = (gs.gameLog || []).slice(-20).reverse();
  log.innerHTML = entries.map(e => `<div class="log-entry">${e}</div>`).join('');

  document.getElementById('topbar-phase').textContent = phaseLabel(gs.phase);
}

function phaseLabel(phase) {
  const labels = { action:'Take Action Phase', contribute:'Contribute Phase', event:'Event Phase', tiebreak:'Contribution Tiebreak', ended:'Game Over' };
  return labels[phase] || phase;
}

function renderTimer(gs) {
  if (timerInterval) clearInterval(timerInterval);
  if (!gs.timerStart) return;
  function tick() {
    const elapsed = (Date.now() / 1000) - gs.timerStart.seconds;
    const remaining = Math.max(0, gs.timerDuration - elapsed);
    const mins = Math.floor(remaining / 60);
    const secs = Math.floor(remaining % 60);
    const el = document.getElementById('timer-display');
    el.textContent = `${String(mins).padStart(2,'0')}:${String(secs).padStart(2,'0')}`;
    el.classList.toggle('urgent', remaining <= 120);
    const doomEl = document.getElementById('doom-bar');
    doomEl.style.width = ((remaining / gs.timerDuration) * 100) + '%';
    if (remaining <= 0 && isHost && gs.status === 'playing') {
      clearInterval(timerInterval);
      endGame();
    }
  }
  tick();
  timerInterval = setInterval(tick, 1000);
}

function renderPhase(gs, players, me) {
  const phaseTitle = document.getElementById('phase-title');
  const phaseContent = document.getElementById('phase-content');
  const eventArea = document.getElementById('event-area');
  const voteArea = document.getElementById('vote-area');
  eventArea.classList.add('hidden');
  voteArea.classList.add('hidden');

  const myIdx = players.indexOf(me);
  const isMyTurn = gs.phase === 'action' && gs.currentPlayerIndex === myIdx && !me.isEliminated;

  if (gs.phase === 'action') {
    phaseTitle.textContent = 'TAKE ACTION';
    const currentPlayer = players[gs.currentPlayerIndex];
    if (isMyTurn) {
      phaseContent.innerHTML = '<div style="font-size:0.85rem;opacity:0.7;margin-bottom:0.8rem">It\'s your turn! Choose one action:</div>';
      phaseContent.innerHTML += renderActionsHTML(me);
    } else {
      phaseContent.innerHTML = `<div style="text-align:center;padding:1rem;opacity:0.6;font-family:'Oswald',sans-serif;">Waiting for <strong style="color:var(--orange)">${currentPlayer ? currentPlayer.name : '?'}</strong> to take their action…</div>`;
    }

  } else if (gs.phase === 'contribute') {
    phaseTitle.textContent = 'CONTRIBUTE TO THE PROJECT';
    if (me.isEliminated) {
      phaseContent.innerHTML = '<div style="opacity:0.5;text-align:center;padding:1rem">You are eliminated. Watch and weep.</div>';
    } else {
      const readyMap = gs.contributeReady || {};
      const activePlayers = players.filter(p => !p.isEliminated);
      const readyCount = activePlayers.filter(p => readyMap[p.id]).length;
      const totalCount = activePlayers.length;
      const iAmReady = !!readyMap[myPlayerId];

      if (isHost && readyCount >= totalCount && totalCount > 0) {
        endContributePhase();
      }

      const readyNames = activePlayers
        .filter(p => readyMap[p.id])
        .map(p => p.name)
        .join(', ');

      phaseContent.innerHTML = `
        <div style="font-size:0.85rem;opacity:0.7;margin-bottom:0.8rem">
          Contribute Resources to The Project! Announce your total out loud. Whoever contributes most gets 1st Player Coin + 1 Influence + draws an Event card.
        </div>
        <div style="font-size:0.75rem;opacity:0.5;margin-bottom:0.6rem">Your contribution this round: <strong>${me.contribution}</strong></div>
        <div style="font-size:0.85rem;margin-bottom:0.8rem">Use the Contribute panel on the right ➡️</div>
        <div style="background:rgba(0,0,0,0.3);padding:0.6rem;margin-bottom:0.8rem;font-size:0.8rem;">
          ✅ Ready: <strong style="color:var(--green)">${readyCount}/${totalCount}</strong>
          ${readyNames ? `<div style="opacity:0.6;margin-top:0.3rem">${readyNames}</div>` : ''}
        </div>
        ${!iAmReady
          ? `<button class="btn-sm" onclick="markContributeReady()" style="width:100%">✅ Done Contributing This Round</button>`
          : `<div style="color:var(--green);font-family:'Oswald',sans-serif;font-size:0.95rem;text-align:center;">✅ You're ready — waiting for others…</div>`
        }`;
    }

  } else if (gs.phase === 'event') {
    phaseTitle.textContent = 'EVENT';
    phaseContent.innerHTML = '';
    renderEventCard(gs, me);

  } else if (gs.phase === 'tiebreak') {
    phaseTitle.textContent = 'CONTRIBUTION TIEBREAK';
    phaseContent.innerHTML = '<div style="opacity:0.7;font-size:0.85rem">Multiple players tied for top contribution! Vote for who should receive the bonus.</div>';
    renderVote(gs, me);
  }
}

function renderActionsHTML(me) {
  const civ = me.civilization;
  const civData = CIVILIZATIONS[civ] || {};
  const actions = [
    { key:'produce', icon:'🌿', name:'PRODUCE', desc:'Gain 2 Resources', bonus: civ==='Technocracy' ? 'BONUS: Gain 3 instead of 2' : null },
    { key:'indoctrinate', icon:'🔺', name:'INDOCTRINATE', desc:'Gain 1 Influence', bonus: civ==='Theocracy' ? 'BONUS: Gain 2 instead of 1' : null },
    { key:'propagandize', icon:'📄', name:'PROPAGANDIZE', desc:'Spend 1 Resource → steal 1 Influence from another player', bonus: civ==='Corporatocracy' ? 'BONUS: Free (no cost)' : null, cost: civ==='Corporatocracy' ? 0 : 1, costType:'resource', disabled: civ!=='Corporatocracy' && (me.resources < 1) },
    { key:'invade', icon:'➕', name:'INVADE', desc:'Spend 1 Influence → steal 2 Resources from another player', bonus: civ==='Democracy' ? 'BONUS: Free (no cost)' : null, cost: civ==='Democracy' ? 0 : 1, costType:'influence', disabled: civ!=='Democracy' && (me.influence < 1) },
    { key:'nuke', icon:'☢️', name:'NUKE', desc:`Spend ${civ==='Autocracy'?5:8} Resources to eliminate a player`, bonus: civ==='Autocracy' ? 'BONUS: Costs only 5 Resources' : null, cost: civ==='Autocracy' ? 5 : 8, costType:'resource', disabled: me.resources < (civ==='Autocracy'?5:8), isNuke:true },
  ];

  return `<div class="actions-grid">${actions.map(a => {
    const improved = a.key === civData.improvedAction;
    return `<div class="${a.isNuke ? 'nuke-btn' : ''}">
      <button class="action-btn ${improved ? 'improved' : ''}" onclick="takeAction('${a.key}')" ${a.disabled ? 'disabled' : ''}>
        <span class="ab-icon">${a.icon}</span>
        <div class="ab-name">${a.name}</div>
        <div class="ab-desc">${a.desc}</div>
        ${a.bonus ? `<div class="ab-bonus">⭐ ${a.bonus}</div>` : ''}
      </button>
    </div>`;
  }).join('')}</div>`;
}

window.takeAction = async function(actionKey) {
  const gs = localGameState;
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const me = localPlayers[myPlayerId];
  const myIdx = players.indexOf(me);
  if (gs.currentPlayerIndex !== myIdx || gs.phase !== 'action' || me.isEliminated) return;

  if (actionKey === 'propagandize' || actionKey === 'invade' || actionKey === 'nuke') {
    pendingAction = actionKey;
    showTargetModal(actionKey, players, me);
    return;
  }

  await executeAction(actionKey, null);
};

function showTargetModal(actionKey, players, me) {
  const activePlayers = players.filter(p => !p.isEliminated && p.id !== myPlayerId);
  const modal = document.getElementById('target-modal');
  const title = document.getElementById('target-title');
  const opts = document.getElementById('target-options');
  const labels = { propagandize:'STEAL INFLUENCE FROM', invade:'STEAL RESOURCES FROM', nuke:'NUKE (ELIMINATE)' };
  title.textContent = labels[actionKey] || 'CHOOSE TARGET';
  opts.innerHTML = '';
  activePlayers.forEach(p => {
    const civ = CIVILIZATIONS[p.civilization] || {};
    const btn = document.createElement('button');
    btn.className = 'target-option';
    btn.innerHTML = `<span>${civ.icon||''} ${p.name}</span><span style="opacity:0.6;font-size:0.8rem">${p.civilization} | 🌿${p.resources} 🔺${p.influence}</span>`;
    btn.onclick = () => { closeTargetModal(); executeAction(pendingAction, p.id); };
    opts.appendChild(btn);
  });
  modal.classList.add('active');
}

window.closeTargetModal = function() {
  document.getElementById('target-modal').classList.remove('active');
  pendingAction = null;
};

async function executeAction(actionKey, targetId) {
  const gs = localGameState;
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const me = localPlayers[myPlayerId];
  const civ = me.civilization;
  const target = targetId ? localPlayers[targetId] : null;
  let logMsg = '';
  const batch = writeBatch(db);
  const myRef = doc(db, 'rooms', currentRoomCode, 'players', myPlayerId);
  const roomRef = doc(db, 'rooms', currentRoomCode);

  if (actionKey === 'produce') {
    const gain = civ === 'Technocracy' ? 3 : 2;
    batch.update(myRef, { resources: (me.resources || 0) + gain });
    logMsg = `${me.name} PRODUCED +${gain} Resources`;
  } else if (actionKey === 'indoctrinate') {
    const gain = civ === 'Theocracy' ? 2 : 1;
    batch.update(myRef, { influence: (me.influence || 0) + gain });
    logMsg = `${me.name} INDOCTRINATED +${gain} Influence`;
  } else if (actionKey === 'propagandize') {
    if (!target) return;
    const cost = civ === 'Corporatocracy' ? 0 : 1;
    const tRef = doc(db, 'rooms', currentRoomCode, 'players', targetId);
    batch.update(myRef, { resources: (me.resources||0) - cost, influence: (me.influence||0) + 1 });
    batch.update(tRef, { influence: Math.max(0, (target.influence||0) - 1) });
    logMsg = `${me.name} PROPAGANDIZED — stole 1 Influence from ${target.name}`;
  } else if (actionKey === 'invade') {
    if (!target) return;
    const cost = civ === 'Democracy' ? 0 : 1;
    const steal = Math.min(2, target.resources || 0);
    const tRef = doc(db, 'rooms', currentRoomCode, 'players', targetId);
    batch.update(myRef, { influence: (me.influence||0) - cost, resources: (me.resources||0) + steal });
    batch.update(tRef, { resources: Math.max(0, (target.resources||0) - steal) });
    logMsg = `${me.name} INVADED — stole ${steal} Resources from ${target.name}`;
  } else if (actionKey === 'nuke') {
    if (!target) return;
    const cost = civ === 'Autocracy' ? 5 : 8;
    const tRef = doc(db, 'rooms', currentRoomCode, 'players', targetId);
    batch.update(myRef, { resources: (me.resources||0) - cost });
    batch.update(tRef, { isEliminated: true, resources: 0, influence: 0 });
    logMsg = `☢️ ${me.name} NUKED ${target.name}! They are eliminated!`;
  }

  const activePlayers = players.filter(p => !p.isEliminated);
  const currentActiveIdx = activePlayers.findIndex(p => p.id === myPlayerId);
  const nextActiveIdx = (currentActiveIdx + 1) % activePlayers.length;
  const nextPlayer = activePlayers[nextActiveIdx];
  const nextIdx = players.findIndex(p => p.id === nextPlayer.id);
  const isLastInRound = nextActiveIdx === 0;

  if (isLastInRound) {
    batch.update(roomRef, {
      phase: 'contribute',
      currentPlayerIndex: gs.firstPlayerIndex,
      contributeReady: {},
      gameLog: arrayUnion(logMsg, '🌿 Contribute phase begins!')
    });
  } else {
    batch.update(roomRef, {
      currentPlayerIndex: nextIdx,
      gameLog: arrayUnion(logMsg)
    });
  }

  await batch.commit();
  showToast(logMsg, 'info');
}

// ── Mark Ready for Contribute Phase ──
window.markContributeReady = async function() {
  await updateDoc(doc(db, 'rooms', currentRoomCode), {
    [`contributeReady.${myPlayerId}`]: true
  });
};

window.contributeResources = async function() {
  const me = localPlayers[myPlayerId];
  if (!me || me.isEliminated) { showToast('You are eliminated!', 'error'); return; }
  const amount = parseInt(document.getElementById('contrib-amount').value);
  if (isNaN(amount) || amount < 1) { showToast('Enter a valid amount!', 'error'); return; }
  if (amount > (me.resources || 0)) { showToast('Not enough Resources!', 'error'); return; }
  const batch = writeBatch(db);
  batch.update(doc(db, 'rooms', currentRoomCode, 'players', myPlayerId), {
    resources: (me.resources||0) - amount,
    contribution: (me.contribution||0) + amount
  });
  batch.update(doc(db, 'rooms', currentRoomCode), {
    projectResources: (localGameState.projectResources||0) + amount,
    gameLog: arrayUnion(`${me.name} contributed ${amount} Resources to The Project (total: ${(me.contribution||0)+amount})`)
  });
  await batch.commit();
  document.getElementById('contrib-amount').value = '';
  showToast(`You contributed ${amount} Resources! Shout your total: ${(me.contribution||0)+amount}!`, 'success');
};

window.endContributePhase = async function() {
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const active = players.filter(p => !p.isEliminated);
  const contribs = active.map(p => ({ id:p.id, name:p.name, contribution:p.contribution||0 }));
  const maxC = Math.max(...contribs.map(c => c.contribution));
  const tied = contribs.filter(c => c.contribution === maxC && maxC > 0);

  if (tied.length === 0) {
    await startEventPhase(null);
  } else if (tied.length === 1) {
    await awardTopContributor(tied[0].id);
  } else {
    const voteData = { candidates: tied.map(t => t.id), votes: {}, question: 'Who should receive the top contributor bonus?' };
    await updateDoc(doc(db, 'rooms', currentRoomCode), { phase:'tiebreak', voteData, gameLog: arrayUnion('🗳️ Contribution tiebreak vote!') });
  }
};

async function startEventPhase(winnerId) {
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const batch = writeBatch(db);
  players.forEach(p => {
    batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), { contribution: 0 });
  });
  const drawerId = winnerId || (players.find(p => !p.isEliminated) || players[0]).id;
  batch.update(doc(db, 'rooms', currentRoomCode), {
    phase: 'event',
    contributeReady: {},
    currentEventDrawerId: drawerId,
    gameLog: arrayUnion('📋 No contributions this round. Moving to event phase.')
  });
  await batch.commit();
}

async function awardTopContributor(winnerId) {
  const winner = localPlayers[winnerId];
  const batch = writeBatch(db);
  batch.update(doc(db, 'rooms', currentRoomCode, 'players', winnerId), {
    influence: (winner.influence||0) + 1
  });
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const newFirstIdx = players.findIndex(p => p.id === winnerId);
  players.forEach(p => {
    batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), { contribution: 0 });
  });
  batch.update(doc(db, 'rooms', currentRoomCode), {
    phase: 'event',
    firstPlayerIndex: newFirstIdx,
    currentPlayerIndex: newFirstIdx,
    currentEventDrawerId: winnerId,
    contributeReady: {},
    gameLog: arrayUnion(`🏆 ${winner.name} contributed the most! They gain 1 Influence, the 1st Player Coin, and draw an Event card!`)
  });
  await batch.commit();
}

function renderEventCard(gs, me) {
  const eventArea = document.getElementById('event-area');
  eventArea.classList.remove('hidden');
  const isDrawer = gs.currentEventDrawerId === myPlayerId;
  const currentEvent = gs.currentEvent;

  if (!currentEvent) {
    if (isDrawer) {
      eventArea.innerHTML = `
        <div class="event-card-display public">
          <div class="event-title">DRAW AN EVENT CARD</div>
          <div class="event-text">You contributed the most! Draw an event card.</div>
          <button class="btn-sm" style="margin-top:1rem" onclick="drawEventCard()">DRAW EVENT CARD</button>
        </div>`;
    } else {
      eventArea.innerHTML = `
        <div class="event-card-display public">
          <div class="event-title">EVENT PHASE</div>
          <div class="event-text" style="opacity:0.6">Waiting for ${gs.currentEventDrawerId ? (localPlayers[gs.currentEventDrawerId]||{}).name || 'player' : 'top contributor'} to draw…</div>
        </div>`;
    }
    return;
  }

  const card = EVENT_CARDS.find(c => c.id === currentEvent.id);
  if (!card) return;

  const isClandestine = card.type === 'clandestine';
  const canSee = !isClandestine || isDrawer;

  if (canSee) {
    eventArea.innerHTML = `
      <div class="event-card-display ${isClandestine ? 'clandestine' : 'public'}">
        <div class="event-type-badge ${isClandestine ? 'clandestine' : 'public'}">${isClandestine ? '🤫 CLANDESTINE' : '📢 PUBLIC'}</div>
        <div class="event-title">${card.title}</div>
        <div class="event-text">${card.text}</div>
        <div class="event-category">${card.category.toUpperCase()}</div>
        ${isDrawer || (!isClandestine) ? `<button class="btn-sm" style="margin-top:1rem" onclick="resolveEvent()">Resolve & Continue →</button>` : ''}
      </div>`;
  } else {
    eventArea.innerHTML = `
      <div class="event-card-display clandestine">
        <div class="event-type-badge clandestine">🤫 CLANDESTINE EVENT</div>
        <div class="hidden-event-msg">KNOWN ONLY TO THE DRAWER</div>
        <div class="event-text" style="opacity:0.4;margin-top:0.5rem">A secret event is being resolved…</div>
      </div>`;
  }
}

window.drawEventCard = async function() {
  const gs = localGameState;
  const deck = [...(gs.eventDeck || [])];
  if (deck.length === 0) { showToast('Event deck empty!', 'error'); return; }
  const cardId = deck.shift();
  const card = EVENT_CARDS.find(c => c.id === cardId);
  const updates = {
    eventDeck: deck,
    currentEvent: { id: cardId },
    gameLog: arrayUnion(card.type === 'clandestine' ? `🤫 ${localPlayers[myPlayerId]?.name} drew a Clandestine event card.` : `📢 EVENT: "${card.title}" — ${card.text}`)
  };
  await updateDoc(doc(db, 'rooms', currentRoomCode), updates);
};

window.resolveEvent = async function() {
  const gs = localGameState;
  const card = EVENT_CARDS.find(c => c.id === gs.currentEvent?.id);
  const staysInPlay = card && ['CEASEFIRE','OPEN BOOKS','BINDING TREATY','COUNTDOWN ACCELERATED','THE FINAL HOUR'].includes(card.title);
  const updates = {
    currentEvent: null,
    currentEventDrawerId: null,
    phase: 'action',
    round: (gs.round||1) + 1,
    gameLog: arrayUnion(`✅ Event resolved. Round ${(gs.round||1)+1} begins!`)
  };
  if (staysInPlay) {
    updates.activeEffects = arrayUnion({ id: card.id, title: card.title, text: card.text });
  }
  await applyEventEffect(card, updates);
};

async function applyEventEffect(card, baseUpdates) {
  const players = Object.values(localPlayers).sort((a,b) => a.turnOrder - b.turnOrder);
  const active = players.filter(p => !p.isEliminated);
  const batch = writeBatch(db);

  if (card.title === 'GLOBAL RECESSION' || card.title === 'SUPPLY CHAIN CRISIS') {
    const loss = card.title === 'GLOBAL RECESSION' ? 2 : 1;
    active.forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        resources: Math.max(0, (p.resources||0) - loss)
      });
    });
  } else if (card.title === 'ECONOMIC BOOM' || card.title === 'RESOURCE WINDFALL') {
    const gain = card.title === 'ECONOMIC BOOM' ? 2 : 1;
    active.forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        resources: (p.resources||0) + gain
      });
    });
  } else if (card.title === 'MASS RALLY') {
    active.forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        influence: (p.influence||0) + 1
      });
    });
  } else if (card.title === 'POLITICAL SCANDAL') {
    active.forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        influence: Math.max(0, (p.influence||0) - 1)
      });
    });
  } else if (card.title === 'PASS THE BUCK') {
    const transfers = active.map((p, i) => ({ from: p.id, to: active[(i+1)%active.length].id, amount: Math.min(1, p.resources||0) }));
    active.forEach((p, i) => {
      const give = transfers[i].amount;
      const receive = transfers[(i - 1 + active.length) % active.length].amount;
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        resources: (p.resources||0) - give + receive
      });
    });
  } else if (card.title === 'WEALTH TAX') {
    active.filter(p => (p.resources||0) >= 4).forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        resources: Math.max(0, (p.resources||0) - 1)
      });
      batch.update(doc(db, 'rooms', currentRoomCode), { projectResources: increment(1) });
    });
  } else if (card.title === 'RELIEF FUND') {
    active.filter(p => (p.resources||0) === 0).forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), { resources: 3 });
    });
  } else if (card.title === 'FOREIGN AID') {
    const poorest = active.reduce((a,b) => (a.resources||0) <= (b.resources||0) ? a : b);
    batch.update(doc(db, 'rooms', currentRoomCode, 'players', poorest.id), {
      resources: (poorest.resources||0) + 3
    });
  } else if (card.title === 'GLOBAL CATASTROPHE') {
    active.forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        resources: Math.max(0, (p.resources||0) - 2)
      });
    });
  } else if (card.title === 'POPULAR UPRISING') {
    const poorestInf = active.reduce((a,b) => (a.influence||0) <= (b.influence||0) ? a : b);
    batch.update(doc(db, 'rooms', currentRoomCode, 'players', poorestInf.id), {
      influence: (poorestInf.influence||0) + 2
    });
  } else if (card.title === 'REVOLUTION') {
    active.filter(p => (p.resources||0) > 5).forEach(p => {
      batch.update(doc(db, 'rooms', currentRoomCode, 'players', p.id), {
        resources: Math.max(0, (p.resources||0) - 2)
      });
    });
  }

  batch.update(doc(db, 'rooms', currentRoomCode), baseUpdates);
  await batch.commit();
}

function renderVote(gs, me) {
  const voteArea = document.getElementById('vote-area');
  voteArea.classList.remove('hidden');
  const vd = gs.voteData;
  if (!vd) return;
  const myVote = vd.votes ? vd.votes[myPlayerId] : null;
  const totalVotes = Object.keys(vd.votes || {}).length;
  const activePlayers = Object.values(localPlayers).filter(p => !p.isEliminated);

  voteArea.innerHTML = `
    <div class="vote-section">
      <h3>🗳️ TIEBREAK VOTE</h3>
      <div class="vote-question">${vd.question || 'Vote for the top contributor:'}</div>
      <div class="vote-options">
        ${(vd.candidates||[]).map(cid => {
          const cp = localPlayers[cid];
          return `<button class="vote-option-btn ${myVote===cid?'voted':''}" onclick="castVote('${cid}')" ${myVote?'disabled':''}>
            ${cp ? cp.name : cid}<br><span style="font-size:0.75rem;opacity:0.6">${cp?cp.civilization:''}</span>
          </button>`;
        }).join('')}
      </div>
      <div class="vote-results">${totalVotes}/${activePlayers.length} votes cast${isHost && totalVotes >= activePlayers.length ? ' — <button class="btn-sm" style="font-size:0.85rem;padding:0.3rem 0.8rem" onclick="resolveVote()">Resolve Vote</button>' : ''}</div>
    </div>`;
}

window.castVote = async function(candidateId) {
  const gs = localGameState;
  if (!gs.voteData) return;
  const me = localPlayers[myPlayerId];
  if (!me || me.isEliminated) return;
  await updateDoc(doc(db, 'rooms', currentRoomCode), {
    [`voteData.votes.${myPlayerId}`]: candidateId
  });
};

window.resolveVote = async function() {
  const gs = localGameState;
  const vd = gs.voteData;
  if (!vd) return;
  const tally = {};
  Object.values(vd.votes||{}).forEach(v => { tally[v] = (tally[v]||0) + 1; });
  const winnerId = Object.keys(tally).reduce((a,b) => tally[a] >= tally[b] ? a : b);
  await awardTopContributor(winnerId);
  await updateDoc(doc(db, 'rooms', currentRoomCode), { voteData: null });
};

async function endGame() {
  if (!isHost) return;
  await updateDoc(doc(db, 'rooms', currentRoomCode), {
    status: 'ended',
    phase: 'ended',
    gameLog: arrayUnion('⏱️ Time is up! The rocket launches… or doesn\'t.')
  });
}

function showEndScreen() {
  showScreen('end-screen');
  if (timerInterval) clearInterval(timerInterval);
  const gs = localGameState;
  const pr = gs.projectResources || 0;
  const seats = seatsFromResources(pr);
  const players = Object.values(localPlayers).sort((a,b) => {
    if (a.isEliminated && !b.isEliminated) return 1;
    if (!a.isEliminated && b.isEliminated) return -1;
    if ((b.influence||0) !== (a.influence||0)) return (b.influence||0) - (a.influence||0);
    return (b.contribution||0) - (a.contribution||0);
  });

  const launched = pr >= 40;
  const endTitleEl = document.getElementById('end-title-display');
  const endSubEl = document.getElementById('end-subtitle');

  if (!launched) {
    endTitleEl.textContent = "WE'RE DOOMED!";
    endTitleEl.className = 'end-title doomed';
    endSubEl.textContent = `Only ${pr} Resources contributed. The rocket never launched. Everyone dies.`;
  } else {
    endTitleEl.textContent = 'ROCKET LAUNCHED!';
    endTitleEl.className = 'end-title launched';
    endSubEl.textContent = `${pr} Resources → ${seats} seat${seats!==1?'s':''} built. The ${seats} most influential escape!`;
  }

  const resultsList = document.getElementById('end-results-list');
  resultsList.innerHTML = '';
  players.forEach((p, i) => {
    const onRocket = launched && !p.isEliminated && i < seats;
    const div = document.createElement('div');
    div.className = `end-player-result ${onRocket ? 'winner' : 'dead'}`;
    const civ = CIVILIZATIONS[p.civilization] || {};
    div.innerHTML = `
      <div>
        <div class="epr-name">${civ.icon||''} ${p.name}</div>
        <div class="epr-civ">${p.civilization}</div>
      </div>
      <div class="epr-stats">🌿${p.contribution||0} contributed · 🔺${p.influence||0} influence</div>
      <div class="epr-outcome ${onRocket?'winner':'dead'}">${p.isEliminated ? '☢️ NUKED' : onRocket ? '🚀 ESCAPED!' : '💀 DEAD LOSER'}</div>`;
    resultsList.appendChild(div);
  });
}

window.goHome = function() {
  if (roomUnsubscribe) roomUnsubscribe();
  if (playersUnsubscribe) playersUnsubscribe();
  if (timerInterval) clearInterval(timerInterval);
  currentRoomCode = null; myPlayerId = null; localPlayers = {}; localGameState = null; isHost = false;
  showScreen('home-screen');
};

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') { closeModals(); closeTargetModal(); }
  if (e.key === 'Enter') {
    if (document.getElementById('create-modal').classList.contains('active')) createRoom();
    else if (document.getElementById('join-modal').classList.contains('active')) joinRoom();
  }
});
</script>
</body>
</html>
