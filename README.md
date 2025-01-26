# Snake game
This is a simple Snake Game built with Python and Tkinter. The game features a classic snake that moves around a grid, eats food to grow, and ends when it collides with the walls or itself. It includes a start/restart button, random food spawning, and smooth movement controls using keyboard input. Here I will explain how to make the snake game in Python with Tkinter

**Getting Started**

1. **Set Up the Game Window:**
   
   - Start by importing the necessary libraries and defining the game window size.
   ```bash
   import tkinter as tk
   import random
    ```
   - Define the grid size and tile size for the game board. These constants determine how large the game will be.
     ```bash
     ROWS = 20
     COLS = 20
     TILE_SIZE = 25
     WINDOW_WIDTH = TILE_SIZE * COLS
     WINDOW_HEIGHT = TILE_SIZE * ROWS
     SNAKE_COLOR = "green"
     FOOD_COLOR = "red"
     BG_COLOR = "black"
     ```
     
    - Define movement directions for the snake:
    ```bash
    DIRECTIONS = {
    "Up": (0, -1),
    "Down": (0, 1),
    "Left": (-1, 0),
    "Right": (1, 0),}
    ```

2. **Create the Main Game Class:**

   - Create a SnakeGame class to handle the game logic.
   ```bash
   class SnakeGame:
    def __init__(self, master):
        self.master = master
        self.master.title("Snake Game")
        self.master.resizable(False, False)
   ```
   
   - Create a canvas aka the game board for drawing the game elements:
   ```bash
   self.canvas = tk.Canvas(master, width=WINDOW_WIDTH, height=WINDOW_HEIGHT, bg=BG_COLOR) self.canvas.pack()
    ```
    
    - Add a start button to begin the game:
    ```bash
    self.start_button = tk.Button(master, text="Start Game", command=self.start_game, font=("Arial", 14)) self.start_button.pack(pady=10)
    ```
    
    - Next we are going to make the snake's start position and food"
    ```bash
        self.snake = [(5, 5), (5, 6), (5, 7)]  # Initial snake position
        self.food = self.spawn_food()
        self.direction = "Up"
        self.running = False  # Start paused
    ```
    
    - Binding the keyboard controls to the snake:
    ```bash
    self.update_snake()
    self.master.bind("<KeyPress>", self.change_direction)
    ```

3. **How to generate the food:**
   - Define the method to generate the random food spots:
     ```bash
     def spawn_food(self):
     """Generates a random position for the food."""
     while True:
     food_position = (random.randint(0, COLS - 1), random.randint(0, ROWS - 1))
     if food_position not in self.snake:
     return food_position
     ```
     
4. **Draw the snake and food:**
   - Define method to update the game display:
    ```bash
    def update_snake(self):
    """Draws the snake and food on the canvas."""
    self.canvas.delete("all")

    # Draw food
    x, y = self.food
    self.canvas.create_rectangle(
    x * TILE_SIZE, y * TILE_SIZE, (x + 1) * TILE_SIZE, (y + 1) * TILE_SIZE,
    fill=FOOD_COLOR, outline="white"
    )

    # Draw snake
    for x, y in self.snake:
    self.canvas.create_rectangle(
    x * TILE_SIZE, y * TILE_SIZE, (x + 1) * TILE_SIZE, (y + 1) * TILE_SIZE,
    fill=SNAKE_COLOR, outline="black"
    )
    ```

5. **Keyboard commands:**
   - Define the method to change the snake's direction:
   ```bash
   def change_direction(self, event):
   """Changes the snake's direction based on user input."""
   if event.keysym in DIRECTIONS and self.running:
   new_direction = event.keysym
   # Prevent reversing direction
   if (DIRECTIONS[new_direction][0] + DIRECTIONS[self.direction][0] == 0 and
   DIRECTIONS[new_direction][1] + DIRECTIONS[self.direction][1] == 0):
   return
   self.direction = new_direction
    ```
6. **Moving of the snake:**
   - Define how to move the snake based on the direction:
   ```bash
   def move_snake(self):
    """Moves the snake and handles game logic."""
    if not self.running:
        return

    head_x, head_y = self.snake[0]
    dx, dy = DIRECTIONS[self.direction]
    new_head = (head_x + dx, head_y + dy)

    # Collision with walls
    if new_head[0] < 0 or new_head[0] >= COLS or new_head[1] < 0 or new_head[1] >= ROWS:
        self.game_over()
        return

    # Collision with itself
    if new_head in self.snake:
        self.game_over()
        return

    # Move snake
    self.snake.insert(0, new_head)

    # Eating food
    if new_head == self.food:
        self.food = self.spawn_food()  # Generate new food
    else:
        self.snake.pop()  # Remove the last part of the snake

    self.update_snake()
    self.master.after(100, self.move_snake)  # Control speed of snake

    ```
8. **Game over**
   - Stops the game when the snake dies
   ```bash
   def game_over(self):
   """Ends the game and shows restart button."""
   self.running = False
   self.canvas.create_text(
   WINDOW_WIDTH // 2, WINDOW_HEIGHT // 2,
   text="GAME OVER", font=("Arial", 24), fill="white"
   )
   self.start_button.config(text="Restart Game", command=self.restart_game)
   self.start_button.pack(pady=10)
   ```
9. **Restarts the game**
   ```bash
   def restart_game(self):
   """Resets the game state and starts a new game."""
   self.snake = [(5, 5), (5, 6), (5, 7)]
   self.food = self.spawn_food()
   self.direction = "Up"
   self.running = True
   self.start_button.pack_forget()
   self.update_snake()
   self.move_snake()
   ```
10. **Starts the game**
    ```bash
    if __name__ == "__main__":
    root = tk.Tk()
    game = SnakeGame(root)
    root.mainloop()
    ```
