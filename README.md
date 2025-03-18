# README.md
# 🌟 Project Name

## 📌 Description
A brief overview of the project and what it does.

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
