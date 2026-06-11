<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>World Cup 2026 — Pool Tracker</title>
<style>
:root { color-scheme: light; }
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: #f7f8fa; color: #1a1a2e; line-height: 1.5; padding: 20px; }
.container { max-width: 1200px; margin: 0 auto; }

header {
  background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%);
  color: white; padding: 24px 28px; border-radius: 12px; margin-bottom: 16px;
  display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 16px;
}
header h1 { font-size: 22px; font-weight: 700; }
header .subtitle { font-size: 13px; opacity: 0.85; margin-top: 4px; }
.score-display { display: flex; gap: 32px; align-items: center; }
.score-block { text-align: center; }
.score-block .label { font-size: 11px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.75; }
.score-block .value { font-size: 36px; font-weight: 700; line-height: 1; margin-top: 4px; }
.score-block .sub { font-size: 12px; opacity: 0.75; margin-top: 2px; }

/* Tab bar */
.tab-bar {
  display: flex; gap: 2px; padding: 6px; background: white; border-radius: 12px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04); margin-bottom: 20px; overflow-x: auto;
  scrollbar-width: thin;
}
.tab {
  padding: 8px 14px; font-size: 13px; font-weight: 500; color: #6b7280; background: transparent;
  border: none; border-radius: 6px; cursor: pointer; white-space: nowrap; transition: all 0.15s;
}
.tab:hover { background: #f3f4f6; color: #1a1a2e; }
.tab.active { background: #3b82f6; color: white; font-weight: 600; }
.tab.summary-tab { font-weight: 700; }
.tab.me-tab.active { background: #fbbf24; color: #78350f; }

section {
  background: white; border-radius: 12px; padding: 22px; margin-bottom: 20px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04);
}
section h2 { font-size: 17px; margin-bottom: 8px; color: #1a1a2e; }
section .hint { font-size: 12px; color: #6b7280; margin-bottom: 14px; }

/* Schedule list */
.schedule-list { display: flex; flex-direction: column; gap: 6px; }
.schedule-date-group { margin-top: 12px; }
.schedule-date-group:first-child { margin-top: 0; }
.schedule-date-header { font-size: 12px; font-weight: 700; color: #6b7280; text-transform: uppercase; letter-spacing: 0.5px; padding: 4px 0 6px; border-bottom: 1px solid #e5e7eb; margin-bottom: 6px; }
.match-card {
  display: grid; grid-template-columns: minmax(90px, auto) 1fr auto; gap: 12px; align-items: center;
  padding: 10px 14px; background: #f9fafb; border-left: 4px solid #d1d5db; border-radius: 6px; font-size: 14px;
}
.match-card.group-A { border-left-color: #ef4444; }
.match-card.group-B { border-left-color: #06b6d4; }
.match-card.group-C { border-left-color: #84cc16; }
.match-card.group-D { border-left-color: #f97316; }
.match-card.group-E { border-left-color: #f59e0b; }
.match-card.group-F { border-left-color: #14b8a6; }
.match-card.group-G { border-left-color: #8b5cf6; }
.match-card.group-H { border-left-color: #ec4899; }
.match-card.group-I { border-left-color: #3b82f6; }
.match-card.group-J { border-left-color: #6366f1; }
.match-card.group-K { border-left-color: #d946ef; }
.match-card.group-L { border-left-color: #10b981; }
.match-card.double { background: #fef3c7; border-left-color: #f59e0b; }
.match-time { font-weight: 600; font-size: 13px; color: #1a1a2e; font-variant-numeric: tabular-nums; }
.local-time { font-weight: 400; color: #6b7280; }
.match-teams { font-size: 14px; }
.match-teams .mine { font-weight: 700; color: #1e3a8a; }
.match-teams .opp { color: #4b5563; }
.match-venue { font-size: 11px; color: #6b7280; margin-top: 2px; }
.team-owners { font-size: 11px; color: #9ca3af; font-weight: 400; }
.match-meta { display: flex; gap: 6px; align-items: center; }
.group-tag { font-size: 10px; font-weight: 700; padding: 2px 6px; background: white; border: 1px solid #d1d5db; border-radius: 4px; color: #4b5563; }
.tv-tag { font-size: 10px; color: #9ca3af; }
.double-badge { font-size: 10px; font-weight: 700; padding: 2px 6px; background: #f59e0b; color: white; border-radius: 4px; }

/* Standings */
table.standings { width: 100%; border-collapse: collapse; }
table.standings th, table.standings td { padding: 10px 12px; text-align: left; border-bottom: 1px solid #e5e7eb; font-size: 14px; }
table.standings th { font-weight: 600; color: #6b7280; font-size: 11px; text-transform: uppercase; letter-spacing: 0.5px; }
table.standings tr.me { background: #fffbeb; font-weight: 700; }
table.standings .rank-cell { width: 50px; font-weight: 700; }
table.standings .points-cell { text-align: right; font-weight: 700; width: 80px; font-variant-numeric: tabular-nums; }
table.standings .picks-cell { font-size: 12px; color: #6b7280; }

/* Summary tab specifics */
.tournament-info { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 12px; }
.info-card { padding: 12px; background: #f3f4f6; border-radius: 8px; }
.info-card .label { font-size: 10px; color: #6b7280; text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; }
.info-card .value { font-size: 16px; font-weight: 600; margin-top: 4px; color: #1a1a2e; }
.info-card .value-detail { font-size: 12px; color: #6b7280; margin-top: 2px; }

.key-dates { display: flex; flex-direction: column; gap: 8px; }
.key-date {
  display: grid; grid-template-columns: 110px 1fr auto; gap: 12px; align-items: center;
  padding: 10px 14px; background: #f9fafb; border-left: 4px solid #3b82f6; border-radius: 6px; font-size: 14px;
}
.key-date.milestone { border-left-color: #f59e0b; background: #fffbeb; }
.key-date.final { border-left-color: #fbbf24; background: #fef3c7; }
.key-date-date { font-weight: 700; font-size: 13px; color: #1a1a2e; }
.key-date-event { font-weight: 500; }
.key-date-detail { font-size: 12px; color: #6b7280; margin-top: 2px; }
.key-date-note { font-size: 11px; color: #6b7280; }

/* Player roster summary on player tab */
.player-roster {
  display: flex; flex-wrap: wrap; gap: 8px; margin-top: 4px;
}
.roster-pill {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 5px 10px; background: #f3f4f6; border-radius: 20px; font-size: 12px;
}
.roster-pill .pill-mult { font-size: 10px; padding: 1px 5px; border-radius: 8px; font-weight: 700; }
.roster-pill.tier-1 { background: #fffbeb; } .roster-pill.tier-1 .pill-mult { background: #fbbf24; color: #78350f; }
.roster-pill.tier-2 { background: #eff6ff; } .roster-pill.tier-2 .pill-mult { background: #60a5fa; color: #1e3a8a; }
.roster-pill.tier-3 { background: #fff7ed; } .roster-pill.tier-3 .pill-mult { background: #fb923c; color: #7c2d12; }
.roster-pill.tier-4 { background: #fef2f2; } .roster-pill.tier-4 .pill-mult { background: #f87171; color: #7f1d1d; }

.empty-state { padding: 32px; text-align: center; color: #9ca3af; font-size: 13px; font-style: italic; }

/* Today's matches */
.today-matches { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 14px; }
.today-match-card {
  background: #f9fafb; border-radius: 12px; padding: 18px; border-left: 5px solid #d1d5db;
  display: flex; flex-direction: column; gap: 10px;
}
.today-match-card.group-A { border-left-color: #ef4444; }
.today-match-card.group-B { border-left-color: #06b6d4; }
.today-match-card.group-C { border-left-color: #84cc16; }
.today-match-card.group-D { border-left-color: #f97316; }
.today-match-card.group-E { border-left-color: #f59e0b; }
.today-match-card.group-F { border-left-color: #14b8a6; }
.today-match-card.group-G { border-left-color: #8b5cf6; }
.today-match-card.group-H { border-left-color: #ec4899; }
.today-match-card.group-I { border-left-color: #3b82f6; }
.today-match-card.group-J { border-left-color: #6366f1; }
.today-match-card.group-K { border-left-color: #d946ef; }
.today-match-card.group-L { border-left-color: #10b981; }
.today-match-time { font-size: 13px; font-weight: 700; color: #6b7280; text-transform: uppercase; letter-spacing: 0.5px; }
.today-match-teams { display: flex; align-items: center; justify-content: space-between; gap: 8px; }
.today-team { display: flex; flex-direction: column; align-items: center; gap: 6px; flex: 1; min-width: 0; }
.today-flag { font-size: 48px; line-height: 1; }
.today-team-name { font-size: 15px; font-weight: 700; text-align: center; color: #1a1a2e; }
.today-team-name .owner-tag { display: block; font-size: 11px; font-weight: 400; color: #9ca3af; margin-top: 2px; }
.today-team.mine .today-team-name { color: #1e3a8a; }
.today-team.mine .today-flag { filter: drop-shadow(0 0 0 transparent); }
.today-vs { font-size: 13px; font-weight: 700; color: #9ca3af; flex-shrink: 0; }
.today-match-venue { font-size: 12px; color: #6b7280; text-align: center; }
.today-match-meta { display: flex; gap: 6px; align-items: center; justify-content: center; }
</style>
</head>
<body>
<div class="container">

  <header>
    <div>
      <h1>🏆 World Cup 2026 — Pool Tracker</h1>
      <div class="subtitle" id="headerSubtitle">Tournament Summary · Jun 11 – Jul 19, 2026</div>
    </div>
    <div class="score-display" id="scoreDisplay" style="display:none;">
      <div class="score-block">
        <div class="label">Total</div>
        <div class="value" id="playerTotal">0</div>
        <div class="sub">points</div>
      </div>
      <div class="score-block">
        <div class="label">Pool Rank</div>
        <div class="value" id="playerRank">—</div>
        <div class="sub">of 16</div>
      </div>
    </div>
  </header>

  <div class="tab-bar" id="tabBar"></div>

  <div id="tabContent"></div>

</div>

<script>
// === DATA ===
const ALL_MATCHES = [{"date": "2026-06-11", "day": "Thu Jun 11", "time": "3:00 PM", "home": "Mexico", "away": "South Africa", "venue": "Estadio Azteca, Mexico City", "group": "A", "tv": "Fox"}, {"date": "2026-06-11", "day": "Thu Jun 11", "time": "10:00 PM", "home": "South Korea", "away": "Czech Republic", "venue": "Estadio Akron, Guadalajara", "group": "A", "tv": "FS1"}, {"date": "2026-06-18", "day": "Thu Jun 18", "time": "12:00 PM", "home": "Czech Republic", "away": "South Africa", "venue": "Mercedes-Benz Stadium, Atlanta", "group": "A", "tv": "Fox"}, {"date": "2026-06-18", "day": "Thu Jun 18", "time": "9:00 PM", "home": "Mexico", "away": "South Korea", "venue": "Estadio Akron, Guadalajara", "group": "A", "tv": "Fox"}, {"date": "2026-06-24", "day": "Wed Jun 24", "time": "9:00 PM", "home": "Czech Republic", "away": "Mexico", "venue": "Estadio Azteca, Mexico City", "group": "A", "tv": "Fox"}, {"date": "2026-06-24", "day": "Wed Jun 24", "time": "9:00 PM", "home": "South Africa", "away": "South Korea", "venue": "Estadio BBVA, Monterrey", "group": "A", "tv": "FS1"}, {"date": "2026-06-12", "day": "Fri Jun 12", "time": "3:00 PM", "home": "Canada", "away": "Bosnia and Herzegovina", "venue": "BMO Field, Toronto", "group": "B", "tv": "Fox"}, {"date": "2026-06-13", "day": "Sat Jun 13", "time": "3:00 PM", "home": "Qatar", "away": "Switzerland", "venue": "Levi's Stadium, Santa Clara CA", "group": "B", "tv": "Fox"}, {"date": "2026-06-18", "day": "Thu Jun 18", "time": "3:00 PM", "home": "Switzerland", "away": "Bosnia and Herzegovina", "venue": "SoFi Stadium, Inglewood CA", "group": "B", "tv": "Fox"}, {"date": "2026-06-18", "day": "Thu Jun 18", "time": "6:00 PM", "home": "Canada", "away": "Qatar", "venue": "BC Place, Vancouver", "group": "B", "tv": "FS1"}, {"date": "2026-06-24", "day": "Wed Jun 24", "time": "3:00 PM", "home": "Switzerland", "away": "Canada", "venue": "BC Place, Vancouver", "group": "B", "tv": "Fox"}, {"date": "2026-06-24", "day": "Wed Jun 24", "time": "3:00 PM", "home": "Bosnia and Herzegovina", "away": "Qatar", "venue": "Lumen Field, Seattle", "group": "B", "tv": "FS1"}, {"date": "2026-06-13", "day": "Sat Jun 13", "time": "6:00 PM", "home": "Brazil", "away": "Morocco", "venue": "MetLife Stadium, East Rutherford NJ", "group": "C"}, {"date": "2026-06-13", "day": "Sat Jun 13", "time": "9:00 PM", "home": "Haiti", "away": "Scotland", "venue": "Gillette Stadium, Foxborough MA", "group": "C"}, {"date": "2026-06-19", "day": "Fri Jun 19", "time": "6:00 PM", "home": "Scotland", "away": "Morocco", "venue": "Gillette Stadium, Foxborough MA", "group": "C"}, {"date": "2026-06-19", "day": "Fri Jun 19", "time": "8:30 PM", "home": "Brazil", "away": "Haiti", "venue": "Lincoln Financial Field, Philadelphia", "group": "C"}, {"date": "2026-06-24", "day": "Wed Jun 24", "time": "6:00 PM", "home": "Scotland", "away": "Brazil", "venue": "Hard Rock Stadium, Miami", "group": "C"}, {"date": "2026-06-24", "day": "Wed Jun 24", "time": "6:00 PM", "home": "Morocco", "away": "Haiti", "venue": "Mercedes-Benz Stadium, Atlanta", "group": "C"}, {"date": "2026-06-12", "day": "Fri Jun 12", "time": "9:00 PM", "home": "United States", "away": "Paraguay", "venue": "SoFi Stadium, Inglewood CA", "group": "D", "tv": "Fox"}, {"date": "2026-06-13", "day": "Sat Jun 13", "time": "12:00 AM", "home": "Australia", "away": "Turkey", "venue": "BC Place, Vancouver", "group": "D", "tv": "FS1"}, {"date": "2026-06-19", "day": "Fri Jun 19", "time": "3:00 PM", "home": "United States", "away": "Australia", "venue": "Lumen Field, Seattle", "group": "D", "tv": "Fox"}, {"date": "2026-06-19", "day": "Fri Jun 19", "time": "11:00 PM", "home": "Turkey", "away": "Paraguay", "venue": "Levi's Stadium, Santa Clara CA", "group": "D", "tv": "FS1"}, {"date": "2026-06-25", "day": "Thu Jun 25", "time": "10:00 PM", "home": "Turkey", "away": "United States", "venue": "SoFi Stadium, Inglewood CA", "group": "D", "tv": "Fox"}, {"date": "2026-06-25", "day": "Thu Jun 25", "time": "10:00 PM", "home": "Paraguay", "away": "Australia", "venue": "Levi's Stadium, Santa Clara CA", "group": "D", "tv": "FS1"}, {"date": "2026-06-14", "day": "Sun Jun 14", "time": "1:00 PM", "home": "Germany", "away": "Curacao", "venue": "NRG Stadium, Houston", "group": "E", "tv": "Fox"}, {"date": "2026-06-14", "day": "Sun Jun 14", "time": "7:00 PM", "home": "Ivory Coast", "away": "Ecuador", "venue": "Lincoln Financial Field, Philadelphia", "group": "E", "tv": "FS1"}, {"date": "2026-06-20", "day": "Sat Jun 20", "time": "4:00 PM", "home": "Germany", "away": "Ivory Coast", "venue": "BMO Field, Toronto", "group": "E", "tv": "Fox"}, {"date": "2026-06-20", "day": "Sat Jun 20", "time": "8:00 PM", "home": "Ecuador", "away": "Curacao", "venue": "Arrowhead Stadium, Kansas City MO", "group": "E", "tv": "FS1"}, {"date": "2026-06-25", "day": "Thu Jun 25", "time": "4:00 PM", "home": "Curacao", "away": "Ivory Coast", "venue": "Lincoln Financial Field, Philadelphia", "group": "E", "tv": "FS1"}, {"date": "2026-06-25", "day": "Thu Jun 25", "time": "4:00 PM", "home": "Ecuador", "away": "Germany", "venue": "MetLife Stadium, East Rutherford NJ", "group": "E", "tv": "Fox"}, {"date": "2026-06-14", "day": "Sun Jun 14", "time": "4:00 PM", "home": "Netherlands", "away": "Japan", "venue": "AT&T Stadium, Arlington TX", "group": "F"}, {"date": "2026-06-14", "day": "Sun Jun 14", "time": "10:00 PM", "home": "Sweden", "away": "Tunisia", "venue": "Estadio BBVA, Monterrey", "group": "F"}, {"date": "2026-06-20", "day": "Sat Jun 20", "time": "1:00 PM", "home": "Netherlands", "away": "Sweden", "venue": "NRG Stadium, Houston", "group": "F"}, {"date": "2026-06-20", "day": "Sat Jun 20", "time": "10:00 PM", "home": "Tunisia", "away": "Japan", "venue": "Estadio Akron, Guadalajara", "group": "F"}, {"date": "2026-06-25", "day": "Thu Jun 25", "time": "7:00 PM", "home": "Japan", "away": "Sweden", "venue": "AT&T Stadium, Arlington TX", "group": "F"}, {"date": "2026-06-25", "day": "Thu Jun 25", "time": "7:00 PM", "home": "Tunisia", "away": "Netherlands", "venue": "Arrowhead Stadium, Kansas City MO", "group": "F"}, {"date": "2026-06-15", "day": "Mon Jun 15", "time": "3:00 PM", "home": "Belgium", "away": "Egypt", "venue": "Lumen Field, Seattle", "group": "G", "tv": "Fox"}, {"date": "2026-06-15", "day": "Mon Jun 15", "time": "9:00 PM", "home": "Iran", "away": "New Zealand", "venue": "SoFi Stadium, Inglewood CA", "group": "G", "tv": "FS1"}, {"date": "2026-06-21", "day": "Sun Jun 21", "time": "3:00 PM", "home": "Belgium", "away": "Iran", "venue": "SoFi Stadium, Inglewood CA", "group": "G", "tv": "FS1"}, {"date": "2026-06-21", "day": "Sun Jun 21", "time": "9:00 PM", "home": "New Zealand", "away": "Egypt", "venue": "BC Place, Vancouver", "group": "G", "tv": "FS1"}, {"date": "2026-06-26", "day": "Fri Jun 26", "time": "11:00 PM", "home": "Egypt", "away": "Iran", "venue": "Lumen Field, Seattle", "group": "G", "tv": "FS1"}, {"date": "2026-06-26", "day": "Fri Jun 26", "time": "11:00 PM", "home": "New Zealand", "away": "Belgium", "venue": "BC Place, Vancouver", "group": "G", "tv": "Fox"}, {"date": "2026-06-15", "day": "Mon Jun 15", "time": "12:00 PM", "home": "Spain", "away": "Cape Verde", "venue": "Mercedes-Benz Stadium, Atlanta", "group": "H", "tv": "Fox"}, {"date": "2026-06-15", "day": "Mon Jun 15", "time": "6:00 PM", "home": "Saudi Arabia", "away": "Uruguay", "venue": "Hard Rock Stadium, Miami", "group": "H", "tv": "FS1"}, {"date": "2026-06-21", "day": "Sun Jun 21", "time": "12:00 PM", "home": "Spain", "away": "Saudi Arabia", "venue": "Mercedes-Benz Stadium, Atlanta", "group": "H", "tv": "Fox"}, {"date": "2026-06-21", "day": "Sun Jun 21", "time": "6:00 PM", "home": "Uruguay", "away": "Cape Verde", "venue": "Hard Rock Stadium, Miami", "group": "H", "tv": "FS1"}, {"date": "2026-06-26", "day": "Fri Jun 26", "time": "8:00 PM", "home": "Cape Verde", "away": "Saudi Arabia", "venue": "NRG Stadium, Houston", "group": "H", "tv": "FS1"}, {"date": "2026-06-26", "day": "Fri Jun 26", "time": "8:00 PM", "home": "Uruguay", "away": "Spain", "venue": "Estadio Akron, Guadalajara", "group": "H", "tv": "Fox"}, {"date": "2026-06-16", "day": "Tue Jun 16", "time": "3:00 PM", "home": "France", "away": "Senegal", "venue": "MetLife Stadium, East Rutherford NJ", "group": "I"}, {"date": "2026-06-16", "day": "Tue Jun 16", "time": "6:00 PM", "home": "Iraq", "away": "Norway", "venue": "Gillette Stadium, Foxborough MA", "group": "I"}, {"date": "2026-06-22", "day": "Mon Jun 22", "time": "5:00 PM", "home": "France", "away": "Iraq", "venue": "Lincoln Financial Field, Philadelphia", "group": "I"}, {"date": "2026-06-22", "day": "Mon Jun 22", "time": "8:00 PM", "home": "Norway", "away": "Senegal", "venue": "MetLife Stadium, East Rutherford NJ", "group": "I"}, {"date": "2026-06-26", "day": "Fri Jun 26", "time": "3:00 PM", "home": "Norway", "away": "France", "venue": "Gillette Stadium, Foxborough MA", "group": "I"}, {"date": "2026-06-26", "day": "Fri Jun 26", "time": "3:00 PM", "home": "Senegal", "away": "Iraq", "venue": "Lincoln Financial Field, Philadelphia", "group": "I"}, {"date": "2026-06-16", "day": "Tue Jun 16", "time": "9:00 PM", "home": "Argentina", "away": "Algeria", "venue": "Arrowhead Stadium, Kansas City MO", "group": "J", "tv": "Fox"}, {"date": "2026-06-17", "day": "Wed Jun 17", "time": "12:00 AM", "home": "Austria", "away": "Jordan", "venue": "Levi's Stadium, Santa Clara CA", "group": "J", "tv": "FS1"}, {"date": "2026-06-22", "day": "Mon Jun 22", "time": "1:00 PM", "home": "Argentina", "away": "Austria", "venue": "AT&T Stadium, Arlington TX", "group": "J", "tv": "Fox"}, {"date": "2026-06-22", "day": "Mon Jun 22", "time": "11:00 PM", "home": "Jordan", "away": "Algeria", "venue": "Levi's Stadium, Santa Clara CA", "group": "J", "tv": "FS1"}, {"date": "2026-06-27", "day": "Sat Jun 27", "time": "10:00 PM", "home": "Jordan", "away": "Argentina", "venue": "AT&T Stadium, Arlington TX", "group": "J", "tv": "Fox"}, {"date": "2026-06-27", "day": "Sat Jun 27", "time": "10:00 PM", "home": "Algeria", "away": "Austria", "venue": "Arrowhead Stadium, Kansas City MO", "group": "J", "tv": "FS1"}, {"date": "2026-06-17", "day": "Wed Jun 17", "time": "1:00 PM", "home": "Portugal", "away": "DR Congo", "venue": "NRG Stadium, Houston", "group": "K"}, {"date": "2026-06-17", "day": "Wed Jun 17", "time": "10:00 PM", "home": "Uzbekistan", "away": "Colombia", "venue": "Estadio Azteca, Mexico City", "group": "K"}, {"date": "2026-06-23", "day": "Tue Jun 23", "time": "1:00 PM", "home": "Portugal", "away": "Uzbekistan", "venue": "NRG Stadium, Houston", "group": "K"}, {"date": "2026-06-23", "day": "Tue Jun 23", "time": "10:00 PM", "home": "Colombia", "away": "DR Congo", "venue": "Estadio Akron, Guadalajara", "group": "K"}, {"date": "2026-06-27", "day": "Sat Jun 27", "time": "7:30 PM", "home": "Colombia", "away": "Portugal", "venue": "Hard Rock Stadium, Miami", "group": "K"}, {"date": "2026-06-27", "day": "Sat Jun 27", "time": "7:30 PM", "home": "DR Congo", "away": "Uzbekistan", "venue": "Mercedes-Benz Stadium, Atlanta", "group": "K"}, {"date": "2026-06-17", "day": "Wed Jun 17", "time": "4:00 PM", "home": "England", "away": "Croatia", "venue": "AT&T Stadium, Arlington TX", "group": "L"}, {"date": "2026-06-17", "day": "Wed Jun 17", "time": "7:00 PM", "home": "Ghana", "away": "Panama", "venue": "BMO Field, Toronto", "group": "L"}, {"date": "2026-06-23", "day": "Tue Jun 23", "time": "4:00 PM", "home": "England", "away": "Ghana", "venue": "Gillette Stadium, Foxborough MA", "group": "L"}, {"date": "2026-06-23", "day": "Tue Jun 23", "time": "7:00 PM", "home": "Panama", "away": "Croatia", "venue": "BMO Field, Toronto", "group": "L"}, {"date": "2026-06-27", "day": "Sat Jun 27", "time": "4:00 PM", "home": "Panama", "away": "England", "venue": "MetLife Stadium, East Rutherford NJ", "group": "L"}, {"date": "2026-06-27", "day": "Sat Jun 27", "time": "4:00 PM", "home": "Croatia", "away": "Ghana", "venue": "Lincoln Financial Field, Philadelphia", "group": "L"}];
const TEAMS_MULT = {"Spain": 1, "Argentina": 1, "France": 1, "England": 1, "Brazil": 1, "Portugal": 1, "Netherlands": 1, "Belgium": 1, "Germany": 1, "Croatia": 1, "Morocco": 2, "Colombia": 2, "United States": 2, "Mexico": 2, "Uruguay": 2, "Switzerland": 2, "Japan": 2, "Senegal": 2, "Iran": 2, "Turkey": 2, "South Korea": 2, "Ecuador": 2, "Austria": 2, "Australia": 2, "Canada": 2, "Norway": 2, "Panama": 2, "Egypt": 3, "Algeria": 3, "Scotland": 3, "Sweden": 3, "Paraguay": 3, "Czech Republic": 3, "Ivory Coast": 3, "Tunisia": 3, "DR Congo": 3, "Uzbekistan": 3, "Qatar": 4, "Iraq": 4, "Saudi Arabia": 4, "South Africa": 4, "Bosnia and Herzegovina": 4, "Jordan": 4, "Cape Verde": 4, "Ghana": 4, "Curacao": 4, "Haiti": 4, "New Zealand": 4};
const TEAM_GROUP = {"Mexico": "A", "South Africa": "A", "South Korea": "A", "Czech Republic": "A", "Canada": "B", "Bosnia and Herzegovina": "B", "Qatar": "B", "Switzerland": "B", "Brazil": "C", "Morocco": "C", "Haiti": "C", "Scotland": "C", "United States": "D", "Paraguay": "D", "Australia": "D", "Turkey": "D", "Germany": "E", "Curacao": "E", "Ivory Coast": "E", "Ecuador": "E", "Netherlands": "F", "Japan": "F", "Sweden": "F", "Tunisia": "F", "Belgium": "G", "Egypt": "G", "Iran": "G", "New Zealand": "G", "Spain": "H", "Cape Verde": "H", "Saudi Arabia": "H", "Uruguay": "H", "France": "I", "Senegal": "I", "Iraq": "I", "Norway": "I", "Argentina": "J", "Algeria": "J", "Austria": "J", "Jordan": "J", "Portugal": "K", "DR Congo": "K", "Uzbekistan": "K", "Colombia": "K", "England": "L", "Croatia": "L", "Ghana": "L", "Panama": "L"};
const POOL = [{"key": "duke", "handle": "Duke \\ The Newsman", "picks": ["France", "Mexico", "Ivory Coast", "Czech Republic", "Ghana", "Jordan"]}, {"key": "jason", "handle": "Jason \\ Floyd Pepper", "picks": ["Brazil", "Switzerland", "Sweden", "Croatia", "Scotland", "Iraq"]}, {"key": "kit", "handle": "Kit \\ Wayne", "picks": ["Spain", "Portugal", "Ivory Coast", "Austria", "South Korea", "Jordan"]}, {"key": "evan", "handle": "Evan \\ Crazy Harry", "picks": ["France", "Portugal", "Egypt", "Canada", "Australia", "Curacao"]}, {"key": "ed", "handle": "Ed \\ Pops", "picks": ["Japan", "Mexico", "Paraguay", "Algeria", "Iran", "Iraq"]}, {"key": "shane", "handle": "Shane \\ Wanda", "picks": ["Argentina", "Germany", "Senegal", "Cape Verde", "DR Congo", "Haiti"]}, {"key": "miller", "handle": "Miller \\ Bobo", "picks": ["Germany", "Netherlands", "Croatia", "Scotland", "Panama", "Curacao"]}, {"key": "kate", "handle": "Kate \\ Lew Zealand", "picks": ["Spain", "United States", "Turkey", "Sweden", "DR Congo", "Qatar"]}, {"key": "sabine", "handle": "Sabine \\ The Penguins", "picks": ["England", "Turkey", "Ecuador", "Paraguay", "South Africa", "Haiti"]}, {"key": "julien", "handle": "Julien \\ Link Hogthrob", "picks": ["Uruguay", "United States", "Algeria", "South Korea", "Tunisia", "Qatar"]}, {"key": "drama", "handle": "Drama \\ Robin the Frog", "picks": ["Colombia", "Brazil", "Austria", "Senegal", "Australia", "Panama"]}, {"key": "liz", "handle": "Liz \\ Beauregard", "picks": ["England", "Uruguay", "Bosnia and Herzegovina", "Czech Republic", "Saudi Arabia", "Uzbekistan"]}, {"key": "dp", "handle": "DP \\ Dr. Teeth", "picks": ["Norway", "Switzerland", "Bosnia and Herzegovina", "Canada", "South Africa", "New Zealand"]}, {"key": "dom", "handle": "Dom \\ Sweetums", "picks": ["Colombia", "Japan", "Netherlands", "Belgium", "Saudi Arabia", "Uzbekistan"]}, {"key": "scott", "handle": "Scott \\ Marvin Suggs", "picks": ["Argentina", "Norway", "Egypt", "Ghana", "Tunisia", "Cape Verde"]}, {"key": "pat", "handle": "Pat \\ Uncle Deadly", "picks": ["Morocco", "Morocco", "Ecuador", "Belgium", "Iran", "New Zealand"]}];

const FLAGS = {
  'France':'🇫🇷','Mexico':'🇲🇽','Ivory Coast':'🇨🇮','Czech Republic':'🇨🇿','Ghana':'🇬🇭','New Zealand':'🇳🇿',
  'Brazil':'🇧🇷','Switzerland':'🇨🇭','Sweden':'🇸🇪','Croatia':'🇭🇷','Spain':'🇪🇸','Portugal':'🇵🇹',
  'Austria':'🇦🇹','Egypt':'🇪🇬','Canada':'🇨🇦','Japan':'🇯🇵','Paraguay':'🇵🇾','Algeria':'🇩🇿',
  'Argentina':'🇦🇷','Germany':'🇩🇪','Senegal':'🇸🇳','Cape Verde':'🇨🇻','Netherlands':'🇳🇱','Scotland':'🏴󠁧󠁢󠁳󠁣󠁴󠁿',
  'United States':'🇺🇸','Turkey':'🇹🇷','England':'🏴󠁧󠁢󠁥󠁮󠁧󠁿','Ecuador':'🇪🇨','Uruguay':'🇺🇾','South Korea':'🇰🇷',
  'Colombia':'🇨🇴','Bosnia and Herzegovina':'🇧🇦','Norway':'🇳🇴','Belgium':'🇧🇪','Morocco':'🇲🇦',
  'South Africa':'🇿🇦','Panama':'🇵🇦','Iraq':'🇮🇶','Curacao':'🇨🇼','Iran':'🇮🇷','Saudi Arabia':'🇸🇦',
  'Tunisia':'🇹🇳','DR Congo':'🇨🇩','Uzbekistan':'🇺🇿','Australia':'🇦🇺','Qatar':'🇶🇦','Jordan':'🇯🇴','Haiti':'🇭🇹'
};

// Scoring
const PTS_BY_KEY = {'group':0, 'out_group':0, 'r32':2, 'r16':5, 'qf':9, 'sf':14, 'f':20, 'win':28};

// State
const STORAGE_KEY = 'wc2026_tracker_unified_v1';
function loadState() {
  try { const r = localStorage.getItem(STORAGE_KEY); if (r) return JSON.parse(r); } catch(e){}
  return { teamStatus: {}, activeTab: 'summary' };
}
function saveState() { localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }
let state = loadState();

function statusFor(name) { return state.teamStatus[name] || 'group'; }
function pointsFor(name) {
  const mult = TEAMS_MULT[name];
  if (!mult) return 0;
  return (PTS_BY_KEY[statusFor(name)] || 0) * mult;
}
function playerTotal(player) {
  return player.picks.reduce((sum, t) => sum + pointsFor(t), 0);
}

// === Render tab bar ===
function renderTabBar() {
  const bar = document.getElementById('tabBar');
  const tabs = [
    { key: 'summary', label: '📋 Summary', isSummary: true }
  ];
  POOL.forEach(p => {
    const me = p.key === 'duke';
    tabs.push({ key: p.key, label: p.handle.split(' \\\\ ')[0], isMe: me });
  });
  bar.innerHTML = tabs.map(t => {
    const cls = ['tab'];
    if (t.isSummary) cls.push('summary-tab');
    if (t.isMe) cls.push('me-tab');
    if (state.activeTab === t.key) cls.push('active');
    return `<button class="${cls.join(' ')}" data-tab="${t.key}">${t.label}</button>`;
  }).join('');
  bar.querySelectorAll('.tab').forEach(b => b.addEventListener('click', () => setTab(b.dataset.tab)));
}

function setTab(key) {
  state.activeTab = key;
  saveState();
  renderTabBar();
  renderActive();
}

// === Schedule rendering ===
function renderSchedule(matches, myTeams, teamOwners) {
  teamOwners = teamOwners || {};
  if (matches.length === 0) return '<div class="empty-state">No matches yet.</div>';
  const mySet = new Set(myTeams || []);
  matches = [...matches].sort((a, b) => {
    if (a.date !== b.date) return a.date.localeCompare(b.date);
    const toMin = t => { const [h, m] = t.replace(/ (AM|PM)/, '').split(':').map(Number); const isPM = t.includes('PM'); return ((isPM && h !== 12 ? h + 12 : (!isPM && h === 12 ? 0 : h)) * 60) + m; };
    return toMin(a.time) - toMin(b.time);
  });
  let html = '<div class="schedule-list">';
  let lastDay = '';
  matches.forEach(m => {
    if (m.day !== lastDay) {
      if (lastDay !== '') html += '</div>';
      html += `<div class="schedule-date-group"><div class="schedule-date-header">${m.day}</div>`;
      lastDay = m.day;
    }
    const homeMine = mySet.has(m.home);
    const awayMine = mySet.has(m.away);
    const doubleMine = homeMine && awayMine;
    const homeFlag = FLAGS[m.home] || '';
    const awayFlag = FLAGS[m.away] || '';
    const homeOwners = !homeMine && teamOwners[m.home] ? ` <span class="team-owners">(${teamOwners[m.home].join(', ')})</span>` : '';
    const awayOwners = !awayMine && teamOwners[m.away] ? ` <span class="team-owners">(${teamOwners[m.away].join(', ')})</span>` : '';
    const homeHtml = `<span class="${homeMine?'mine':'opp'}">${homeFlag} ${m.home}</span>${homeOwners}`;
    const awayHtml = `<span class="${awayMine?'mine':'opp'}">${awayFlag} ${m.away}</span>${awayOwners}`;
    html += `
      <div class="match-card group-${m.group}${doubleMine?' double':''}">
        <div class="match-time">${formatMatchTime(m.date, m.time)}</div>
        <div>
          <div class="match-teams">${homeHtml} <span style="color:#9ca3af;">vs</span> ${awayHtml}</div>
          <div class="match-venue">${m.venue}</div>
        </div>
        <div class="match-meta">
          ${doubleMine ? '<span class="double-badge">BOTH MINE</span>' : ''}
          <span class="group-tag">Group ${m.group}</span>
          ${m.tv ? `<span class="tv-tag">${m.tv}</span>` : ''}
        </div>
      </div>
    `;
  });
  html += '</div></div>';
  return html;
}

// === Standings ===
function renderStandings(highlightKey) {
  const ranked = POOL.map(p => ({...p, total: playerTotal(p)})).sort((a,b) => b.total - a.total);
  const rows = ranked.map((p, i) => {
    const isHighlight = p.key === highlightKey;
    const picksDisplay = p.picks.map(x => `${FLAGS[x]||''} ${x}`).join(' · ');
    return `<tr class="${isHighlight?'me':''}">
      <td class="rank-cell">${i+1}</td>
      <td>${isHighlight?'⭐ ':''}${p.handle}</td>
      <td class="picks-cell">${picksDisplay}</td>
      <td class="points-cell">${p.total}</td>
    </tr>`;
  }).join('');
  return `<table class="standings">
    <thead><tr><th>Rank</th><th>Player</th><th>Picks</th><th class="points-cell">Points</th></tr></thead>
    <tbody>${rows}</tbody>
  </table>`;
}

// === Player tab content ===
function renderPlayerTab(player) {
  const myTeams = player.picks;
  const myMatches = ALL_MATCHES.filter(m => myTeams.includes(m.home) || myTeams.includes(m.away));

  // Build team -> [owner handles] map (excluding current player)
  const teamOwners = {};
  POOL.forEach(p => {
    if (p.key === player.key) return;
    p.picks.forEach(t => {
      if (!teamOwners[t]) teamOwners[t] = [];
            const firstName = p.handle.replace(/ \\.*/, '');
      if (!teamOwners[t].includes(firstName)) teamOwners[t].push(firstName);
    });
  });

  // Roster pills
  const rosterHtml = player.picks.map(t => {
    const mult = TEAMS_MULT[t];
    const flag = FLAGS[t] || '';
    return `<span class="roster-pill tier-${mult}">${flag} ${t} <span class="pill-mult">${mult}x</span></span>`;
  }).join('');

  return `
    <section>
      <h2>${player.handle}'s Roster</h2>
      <div class="player-roster">${rosterHtml}</div>
    </section>
    <section>
      <h2>${player.handle}'s Match Schedule</h2>
      <div class="hint">${myMatches.length} group-stage matches involving ${player.handle}'s teams. Times in ET. Color bar = group. Highlighted = both teams are theirs.</div>
      ${renderSchedule(myMatches, myTeams, teamOwners)}
    </section>
    <section>
      <h2>Pool Standings</h2>
      <div class="hint">All 16 players. ${player.handle} highlighted.</div>
      ${renderStandings(player.key)}
    </section>
  `;
}

// === Time zone helpers ===
// All matches occur during EDT (UTC-4); tournament dates don't cross a DST boundary.
function formatMatchTime(dateStr, timeStr) {
  const m = timeStr.match(/(\d+):(\d+)\s*(AM|PM)/);
  let h = parseInt(m[1], 10);
  const min = parseInt(m[2], 10);
  if (m[3] === 'PM' && h !== 12) h += 12;
  if (m[3] === 'AM' && h === 12) h = 0;

  const [y, mo, d] = dateStr.split('-').map(Number);
  const utcDate = new Date(Date.UTC(y, mo - 1, d, h + 4, min));

  const etStr = utcDate.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit', timeZone: 'America/New_York' });
  const localStr = utcDate.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit' });

  if (localStr === etStr) return `${timeStr} ET`;

  const tzParts = utcDate.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit', timeZoneName: 'short' }).split(' ');
  const tzAbbr = tzParts[tzParts.length - 1];
  return `${localStr} ${tzAbbr}`;
}

// === Today's matches ===
function getTodayDateStr() {
  const d = new Date();
  const y = d.getFullYear();
  const m = String(d.getMonth() + 1).padStart(2, '0');
  const day = String(d.getDate()).padStart(2, '0');
  return `${y}-${m}-${day}`;
}

function buildGlobalTeamOwners() {
  const map = {};
  POOL.forEach(p => {
    const firstName = p.handle.replace(/ \\.*/, '');
    p.picks.forEach(t => {
      if (!map[t]) map[t] = [];
      if (!map[t].includes(firstName)) map[t].push(firstName);
    });
  });
  return map;
}

function renderTodayMatches() {
  const todayStr = getTodayDateStr();
  const todayMatches = ALL_MATCHES.filter(m => m.date === todayStr).sort((a, b) => {
    const toMin = t => { const [h, mi] = t.replace(/ (AM|PM)/, '').split(':').map(Number); const isPM = t.includes('PM'); return ((isPM && h !== 12 ? h + 12 : (!isPM && h === 12 ? 0 : h)) * 60) + mi; };
    return toMin(a.time) - toMin(b.time);
  });

  if (todayMatches.length === 0) {
    return '<div class="empty-state">No matches scheduled for today.</div>';
  }

  const teamOwners = buildGlobalTeamOwners();
  const cards = todayMatches.map(m => {
    const homeFlag = FLAGS[m.home] || '';
    const awayFlag = FLAGS[m.away] || '';
    const homeOwners = teamOwners[m.home] ? `<span class="owner-tag">${teamOwners[m.home].join(', ')}</span>` : '';
    const awayOwners = teamOwners[m.away] ? `<span class="owner-tag">${teamOwners[m.away].join(', ')}</span>` : '';
    return `
      <div class="today-match-card group-${m.group}">
        <div class="today-match-time">${formatMatchTime(m.date, m.time)}</div>
        <div class="today-match-teams">
          <div class="today-team">
            <div class="today-flag">${homeFlag}</div>
            <div class="today-team-name">${m.home}${homeOwners}</div>
          </div>
          <div class="today-vs">VS</div>
          <div class="today-team">
            <div class="today-flag">${awayFlag}</div>
            <div class="today-team-name">${m.away}${awayOwners}</div>
          </div>
        </div>
        <div class="today-match-venue">${m.venue}</div>
        <div class="today-match-meta">
          <span class="group-tag">Group ${m.group}</span>
          ${m.tv ? `<span class="tv-tag">${m.tv}</span>` : ''}
        </div>
      </div>
    `;
  }).join('');

  return `<div class="today-matches">${cards}</div>`;
}

// === Summary tab content ===
function renderSummaryTab() {
  return `
    <section>
      <h2>⚽ Today's Matches</h2>
      ${renderTodayMatches()}
    </section>

    <section>
      <h2>Tournament Overview</h2>
      <div class="tournament-info">
        <div class="info-card">
          <div class="label">Format</div>
          <div class="value">48 teams</div>
          <div class="value-detail">12 groups of 4 → top 2 + 8 best 3rd</div>
        </div>
        <div class="info-card">
          <div class="label">Hosts</div>
          <div class="value">🇺🇸 🇨🇦 🇲🇽</div>
          <div class="value-detail">USA · Canada · Mexico</div>
        </div>
        <div class="info-card">
          <div class="label">Total matches</div>
          <div class="value">104</div>
          <div class="value-detail">72 group · 32 knockout</div>
        </div>
        <div class="info-card">
          <div class="label">Pool</div>
          <div class="value">16 players</div>
          <div class="value-detail">6 picks each, snake draft</div>
        </div>
      </div>
    </section>

    <section>
      <h2>Key Dates</h2>
      <div class="hint">All times Eastern. Watch for the gold milestone markers.</div>
      <div class="key-dates">
        <div class="key-date milestone"><div class="key-date-date">Thu Jun 11</div><div><div class="key-date-event">⚽ Tournament opens</div><div class="key-date-detail">Mexico vs South Africa · 3 PM · Estadio Azteca, Mexico City</div></div><div class="key-date-note">First whistle</div></div>
        <div class="key-date"><div class="key-date-date">Jun 11 – 27</div><div><div class="key-date-event">Group stage</div><div class="key-date-detail">72 matches across 12 groups, ~6 per day</div></div></div>
        <div class="key-date"><div class="key-date-date">Mon Jun 15</div><div><div class="key-date-event">Group of Death opens</div><div class="key-date-detail">Belgium vs Egypt · France's Group I starts Jun 16</div></div></div>
        <div class="key-date"><div class="key-date-date">Sat Jun 27</div><div><div class="key-date-event">Final group games</div><div class="key-date-detail">All Group J, K, L matchday 3 — bracket gets set</div></div></div>
        <div class="key-date milestone"><div class="key-date-date">Sun Jun 28</div><div><div class="key-date-event">🏟️ Round of 32 begins</div><div class="key-date-detail">First knockout round — 8 of 12 third-place teams advance</div></div></div>
        <div class="key-date"><div class="key-date-date">Fri Jul 3</div><div><div class="key-date-event">R32 ends</div><div class="key-date-detail">Field down to 16</div></div></div>
        <div class="key-date milestone"><div class="key-date-date">Sat Jul 4</div><div><div class="key-date-event">🏟️ Round of 16 begins</div><div class="key-date-detail">3 days, 8 matches</div></div></div>
        <div class="key-date"><div class="key-date-date">Thu Jul 9</div><div><div class="key-date-event">Quarterfinals begin</div><div class="key-date-detail">4 matches over 3 days</div></div></div>
        <div class="key-date"><div class="key-date-date">Tue Jul 14</div><div><div class="key-date-event">Semifinals</div><div class="key-date-detail">Final four · 2 matches</div></div></div>
        <div class="key-date"><div class="key-date-date">Sat Jul 18</div><div><div class="key-date-event">3rd-place playoff</div><div class="key-date-detail">Losers of SF · Miami</div></div></div>
        <div class="key-date final"><div class="key-date-date">Sun Jul 19</div><div><div class="key-date-event">🏆 FINAL</div><div class="key-date-detail">MetLife Stadium, East Rutherford NJ · 3 PM</div></div><div class="key-date-note">Champion crowned</div></div>
      </div>
    </section>

    <section>
      <h2>Pool Standings</h2>
      <div class="hint">All 16 players, ranked by current points. Pick a player tab to see their detail.</div>
      ${renderStandings('duke')}
    </section>

    <section>
      <h2>Scoring Reminder</h2>
      <div class="hint">Cumulative points per round reached, then multiplied by team's tier.</div>
      <div class="tournament-info">
        <div class="info-card"><div class="label">Round of 32</div><div class="value">2 pts</div></div>
        <div class="info-card"><div class="label">Round of 16</div><div class="value">5 pts</div></div>
        <div class="info-card"><div class="label">Quarterfinals</div><div class="value">9 pts</div></div>
        <div class="info-card"><div class="label">Semifinals</div><div class="value">14 pts</div></div>
        <div class="info-card"><div class="label">Final</div><div class="value">20 pts</div></div>
        <div class="info-card"><div class="label">Winner</div><div class="value">28 pts</div></div>
      </div>
      <div style="margin-top: 12px; font-size: 12px; color: #6b7280;">
        Multipliers: <strong>1x</strong> Favorites (FIFA top 10) · <strong>2x</strong> Contenders (#11–30) · <strong>3x</strong> Dark Horses (#31–50) · <strong>4x</strong> Flyers (#51+)
      </div>
    </section>
  `;
}

// === Render active tab ===
function renderActive() {
  const content = document.getElementById('tabContent');
  const subtitle = document.getElementById('headerSubtitle');
  const score = document.getElementById('scoreDisplay');

  if (state.activeTab === 'summary') {
    subtitle.textContent = 'Tournament Summary · Jun 11 – Jul 19, 2026';
    score.style.display = 'none';
    content.innerHTML = renderSummaryTab();
  } else {
    const player = POOL.find(p => p.key === state.activeTab);
    if (!player) {
      content.innerHTML = '<div class="empty-state">Player not found.</div>';
      return;
    }
    subtitle.textContent = `Viewing ${player.handle}`;
    score.style.display = 'flex';
    const total = playerTotal(player);
    const ranked = POOL.map(p => ({...p, total: playerTotal(p)})).sort((a,b) => b.total - a.total);
    const rank = ranked.findIndex(p => p.key === player.key) + 1;
    document.getElementById('playerTotal').textContent = total;
    document.getElementById('playerRank').textContent = '#' + rank;
    content.innerHTML = renderPlayerTab(player);
  }
}

// === Init ===
renderTabBar();
renderActive();
</script>
</body>
</html>
