# Monster Slayer - Vue 3 + Vite

Monster Slayer is a simple turn-based game built with **Vue 3** using **Vite**.  
The player fights a monster by attacking, using special attacks, healing, or surrendering. The game keeps a battle log and shows health bars for both player and monster.

## Features

- Turn-based combat system
- Player and monster health bars with animations
- Special attack every 3 rounds
- Battle log tracking each action
- Game over screen with win, lose, or draw
- Fully modular Vue 3 components

## Project setup

```bash
# Install dependencies
npm install

# Run the dev server
npm run dev
```

Open http://localhost:5173
to see the game in your browser.

## File Structure

- `src/components/HealthBar.vue` - Player/Monster health bars
- `src/components/Controls.vue` - Action buttons
- `src/components/BattleLog.vue` - Battle log list
- `src/components/GameOver.vue` - Game over screen
- `src/App.vue` - Main app logic and template
- `index.html` - Mount point for Vue app
- `styles.css` - Global styles
