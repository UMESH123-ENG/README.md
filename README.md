# README.md
# 🌟 minees

## 📌 Description
Minesweeper Game Description:
Overview: This is a simple implementation of the classic Minesweeper game using HTML, CSS, and JavaScript. The objective of the game is to clear a grid of cells without triggering any mines. The game provides a grid of cells, some of which contain mines. When you click on a cell, it reveals either a number (indicating how many mines are in neighboring cells) or a mine (which ends the game). The goal is to uncover all non-mine cells while avoiding the mines.

How to Play:
Grid Layout:

The game is played on a 10x10 grid with a total of 100 cells.
20 mines are randomly placed within the grid, and you don't know where they are initially.
Cell Reveal:

Left-clicking on a cell will either:
Reveal a number (indicating how many mines are in the adjacent cells).
If the cell is empty (no mines adjacent), it will reveal surrounding empty cells automatically.
If the cell contains a mine, the game ends with a "Game Over" alert.
Flags:

Right-clicking on a cell will flag it as a possible mine. This is useful to mark where you think a mine might be, which helps avoid accidentally revealing it later.
The flagged cells will have a yellow background, signaling that they are suspected to contain mines.
You can unflag a cell by right-clicking again.
Winning Condition:

The game is won when all the cells that do not contain mines are revealed.
When this happens, a "You Win!" alert is displayed.
Losing Condition:

The game is lost if you click on a cell containing a mine, triggering the "Game Over" alert.
Game Features:
Dynamic Grid: The grid size is fixed at 10x10 (100 cells). You can modify this later for a larger or smaller grid.
Random Mine Placement: Mines are randomly placed on the grid at the start of the game.
Auto-reveal: Empty cells (those with no neighboring mines) automatically trigger the revealing of surrounding cells, mimicking the real Minesweeper behavior.
Cell Flags: Right-click to flag suspected mine locations.
Simple Alerts: Alerts show up when you either win or lose the game.
Game Logic:
Mine Placement: Randomly assigns mines to the grid.
Number Assignment: After placing the mines, each non-mine cell is assigned a number that indicates how many mines are in the adjacent cells.
Revealing Cells: When a cell is clicked, it is revealed. If it contains a number, it will display the number. If it's empty (surrounded by no mines), adjacent cells are recursively revealed.
Win Condition: When all non-mine cells are revealed, the game checks for a win.
Flagging: Right-clicking a cell flags it as potentially containing a mine, which helps avoid accidental clicks.
Technologies Used:
HTML: Used to create the structure of the game, including the grid and buttons (if any).
CSS: Styles the game layout, grid cells, and their states (e.g., revealed cells, flagged cells).
JavaScript: Handles the game logic, including:
Initializing the grid.
Placing mines and calculating neighboring mine counts.
Handling user interactions like revealing cells and flagging them.
How It Works Internally:
Grid Initialization: The grid is dynamically created using JavaScript, with each cell being represented as an object that tracks whether it's a mine, whether it's revealed, and whether it's flagged.

Cell Click Events: Clicking a cell triggers the logic that reveals the cell. If the cell is empty, it triggers a recursive reveal of neighboring cells. If it contains a number, it simply displays that number.

Game State Management: The game maintains the state of whether it’s over (won or lost) and updates the board visually in response to player actions.

Flagging Mechanism: Right-clicking flags cells that the player suspects may contain mines. These flagged cells are visually marked with a yellow color to distinguish them from revealed cells.

Conclusion:
This version of Minesweeper is a great starting point for learning how to build interactive games with HTML, CSS, and JavaScript. It focuses on core gameplay mechanics like revealing cells, placing mines, and tracking flags. You can further enhance this game by adding features such as:

A timer to track how long the player takes.
A reset button to start a new game without refreshing the page.
Difficulty levels (e.g., easy, medium, hard) by adjusting grid size and mine count.
By understanding this basic version, you can expand and customize it into your own Minesweeper variant!

## 🎨 Demo Preview (HTML & CSS)
Here is a simple **HTML & CSS** snippet from the project:

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minesweeper Game</title>
  <style>
    /* Reset styles */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f1f1f1;
    }

    .game-container {
      text-align: center;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(10, 30px);
      grid-template-rows: repeat(10, 30px);
      gap: 5px;
      margin: 0 auto;
    }

    .cell {
      width: 30px;
      height: 30px;
      background-color: #ddd;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 14px;
      cursor: pointer;
      border: 1px solid #bbb;
    }

    .cell.revealed {
      background-color: #f9f9f9;
    }

    .cell.mine {
      background-color: red;
    }

    .cell.flag {
      background-color: yellow;
    }

    .cell.number {
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div class="game-container">
    <h1>Minesweeper</h1>
    <div class="grid">
      <!-- Grid cells will be dynamically generated here -->
    </div>
  </div>

  <script>
    // Configuration of the game grid
    const gridSize = 10; // 10x10 grid
    const mineCount = 20; // Number of mines

    // Elements
    const gridContainer = document.querySelector('.grid');

    // Game state
    let grid = [];
    let revealedCells = 0;
    let gameOver = false;

    // Initialize the game
    function initGame() {
      grid = createEmptyGrid(gridSize);
      placeMines(grid, mineCount);
      updateNumbers(grid);
      renderGrid(grid);
    }

    // Create a grid with empty cells
    function createEmptyGrid(size) {
      const grid = [];
      for (let row = 0; row < size; row++) {
        const newRow = [];
        for (let col = 0; col < size; col++) {
          newRow.push({
            isMine: false,
            revealed: false,
            flagged: false,
            neighboringMines: 0,
          });
        }
        grid.push(newRow);
      }
      return grid;
    }

    // Place mines randomly
    function placeMines(grid, count) {
      let minesPlaced = 0;
      while (minesPlaced < count) {
        const row = Math.floor(Math.random() * gridSize);
        const col = Math.floor(Math.random() * gridSize);

        if (!grid[row][col].isMine) {
          grid[row][col].isMine = true;
          minesPlaced++;
        }
      }
    }

    // Update the numbers (how many mines around each cell)
    function updateNumbers(grid) {
      for (let row = 0; row < gridSize; row++) {
        for (let col = 0; col < gridSize; col++) {
          if (grid[row][col].isMine) continue;

          let mineCount = 0;
          for (let r = -1; r <= 1; r++) {
            for (let c = -1; c <= 1; c++) {
              const newRow = row + r;
              const newCol = col + c;
              if (newRow >= 0 && newRow < gridSize && newCol >= 0 && newCol < gridSize) {
                if (grid[newRow][newCol].isMine) {
                  mineCount++;
                }
              }
            }
          }
          grid[row][col].neighboringMines = mineCount;
        }
      }
    }

    // Render the grid in the HTML
    function renderGrid(grid) {
      gridContainer.innerHTML = ''; // Clear the grid

      for (let row = 0; row < gridSize; row++) {
        for (let col = 0; col < gridSize; col++) {
          const cell = document.createElement('div');
          cell.classList.add('cell');
          cell.dataset.row = row;
          cell.dataset.col = col;

          // Add click event listener for each cell
          cell.addEventListener('click', () => handleCellClick(row, col));

          // Add right-click flag functionality
          cell.addEventListener('contextmenu', (e) => {
            e.preventDefault();
            handleCellFlag(row, col);
          });

          gridContainer.appendChild(cell);
        }
      }
    }

    // Handle left-click on a cell
    function handleCellClick(row, col) {
      if (gameOver || grid[row][col].revealed || grid[row][col].flagged) return;

      const cell = grid[row][col];
      const cellElement = document.querySelector(`[data-row="${row}"][data-col="${col}"]`);

      // If it's a mine, game over
      if (cell.isMine) {
        cellElement.classList.add('mine');
        alert('Game Over! You hit a mine.');
        gameOver = true;
        return;
      }

      revealCell(row, col);
    }

    // Handle right-click flag on a cell
    function handleCellFlag(row, col) {
      if (gameOver || grid[row][col].revealed) return;

      const cell = grid[row][col];
      const cellElement = document.querySelector(`[data-row="${row}"][data-col="${col}"]`);

      if (cell.flagged) {
        cell.flagged = false;
        cellElement.classList.remove('flag');
      } else {
        cell.flagged = true;
        cellElement.classList.add('flag');
      }
    }

    // Reveal a cell and its neighbors if it's empty
    function revealCell(row, col) {
      const cell = grid[row][col];
      const cellElement = document.querySelector(`[data-row="${row}"][data-col="${col}"]`);

      if (cell.revealed) return;

      cell.revealed = true;
      revealedCells++;
      cellElement.classList.add('revealed');

      if (cell.neighboringMines > 0) {
        cellElement.classList.add('number');
        cellElement.textContent = cell.neighboringMines;
      } else {
        // Recursively reveal neighboring cells
        for (let r = -1; r <= 1; r++) {
          for (let c = -1; c <= 1; c++) {
            const newRow = row + r;
            const newCol = col + c;
            if (newRow >= 0 && newRow < gridSize && newCol >= 0 && newCol < gridSize) {
              revealCell(newRow, newCol);
            }
          }
        }
      }

      // Check if the game is won
      if (revealedCells === gridSize * gridSize - mineCount) {
        alert('You Win!');
        gameOver = true;
      }
    }

    // Start a new game
    initGame();
  </script>
</body>
</html>
