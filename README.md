# Snake Game

A classic Snake Game implemented in Java using Swing, featuring core gameplay, score tracking, and power-ups to enhance the gaming experience. This project provides a clear and straightforward example of game development in Java, focusing on a robust game loop, collision detection, and event handling.

## Table of Contents

  - [About the Project](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#about-the-project)
  - [Features](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#features)
  - [Project Structure](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#project-structure)
  - [Development Details](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#development-details)
      - [Core Components](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#core-components)
      - [Game Loop and Logic](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#game-loop-and-logic)
      - [Event Handling](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#event-handling)
      - [Game Mechanics](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#game-mechanics)
  - [Getting Started](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#getting-started)
      - [Prerequisites](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#prerequisites)
      - [Installation](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#installation)
  - [How to Play](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#how-to-play)
  - [Contributing](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#contributing)
  - [License](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#license)
  - [Contact](https://github.com/subha0319/SnakeGame?tab=readme-ov-file#contact)

## About the Project

This project is a recreation of the popular arcade game "Snake." The player controls a snake on a bordered plane, attempting to eat food items while avoiding collisions with its own body or the game boundaries. Each food item consumed makes the snake grow longer, increasing the challenge. The game focuses on providing a fun and engaging experience with traditional gameplay mechanics and introduces simple power-ups for variety.

## Features

  * **Classic Snake Gameplay**: Navigate the snake to eat food and grow.
  * **Score Tracking**: Keep track of your high score during gameplay.
  * **Power-ups**: Discover and utilize power-ups that appear periodically, providing bonus points.
  * **Collision Detection**: Implements checks for collisions with walls and the snake's own body.
  * **Game Over State**: Displays a "Game Over" screen with the final score.

## Project Structure

The project follows a standard Java Swing application structure, managed by NetBeans (as indicated by `nbproject` files).

```
SnakeGame/
├── build.xml                       # Ant build script for the project
├── manifest.mf                     # Manifest file for JAR packaging
├── nbproject/                      # NetBeans project configuration files
│   ├── build-impl.xml              # Core Ant build implementation by NetBeans
│   ├── genfiles.properties         # Generated file tracking for NetBeans
│   ├── private/
│   │   └── private.properties      # User-specific NetBeans properties
│   └── project.properties          # Project-specific NetBeans properties
│   └── project.xml                 # NetBeans project metadata
└── src/                            # Source code directory
    ├── GameFrame.java              # Main JFrame for the game window
    ├── GamePanel.java              # JPanel containing the core game logic and rendering
    └── SnakeGame.java              # Main class with the entry point (`main` method)
```

## Development Details

This section delves into the architectural and implementation choices made in the Snake Game.

### Core Components

The game's functionality is primarily encapsulated within three key Java classes:

  * **`SnakeGame.java`**: This is the application's entry point. Its `main` method simply instantiates `GameFrame`, which then sets up the game window. This separation ensures the `main` method remains clean and focused solely on application initialization.
  * **`GameFrame.java`**: Extends `javax.swing.JFrame` and serves as the main window for the game.
      * It initializes a `GamePanel` and adds it to itself.
      * Sets up basic window properties like title ("Snake Game"), default close operation, resizability, and centers the window on the screen.
      * The `pack()` method is used to size the frame so that all its contents are at or above their preferred sizes, ensuring the `GamePanel` is displayed correctly.
  * **`GamePanel.java`**: This is the most crucial class, extending `javax.swing.JPanel` and implementing `ActionListener`. It manages the entire game state, rendering, and logic.
      * **Constants**: Defines important game parameters such as `SCREEN_WIDTH`, `SCREEN_HEIGHT`, `UNIT_SIZE` (size of snake segments and food), `GAME_UNITS` (total possible game units on screen), and `DELAY` (game speed in milliseconds).
      * **Game State Variables**: `x` and `y` arrays store the coordinates of each snake body part. Other variables track `bodyParts`, `applesEaten`, `appleX`, `appleY`, `direction`, `running` state, `timer`, `random` (for generating positions), `powerUpActive`, `powerUpX`, and `powerUpY`.
      * **Constructor (`GamePanel()`):** Initializes the `Random` object, sets the panel's preferred size and background color, enables focus, and registers a `MyKeyAdapter` for input. It then calls `startGame()`.

### Game Loop and Logic

The game loop is driven by a `javax.swing.Timer` in the `GamePanel` class:

  * **`startGame()`**: Initializes the game by creating the first apple, setting `running` to `true`, and starting the `Timer` with the defined `DELAY`. The `GamePanel` itself acts as the `ActionListener` for the timer.
  * **`actionPerformed(ActionEvent e)`**: This method is called repeatedly by the `Timer`.
      * If `running` is `true`, it orchestrates the main game logic: `move()`, `checkApple()`, `checkPowerUp()`, and `checkCollisions()`.
      * Finally, `repaint()` is called to trigger the `paintComponent` method and redraw the game state.

### Rendering (`draw` and `paintComponent`)

  * **`paintComponent(Graphics g)`**: Overridden from `JPanel`, this method is responsible for all drawing operations. It first calls `super.paintComponent(g)` and then delegates to the `draw(g)` method.
  * **`draw(Graphics g)`**:
      * If the game is `running`, it draws the apple (red circle), power-up (blue circle, if `powerUpActive`), and the snake (green head, darker green body segments).
      * The current score is rendered in the top center of the screen.
      * If the game is not running, it calls `gameOver(g)`.
  * **`gameOver(Graphics g)`**: Displays the final score and a "Game Over" message in the center of the screen when the game ends.

### Event Handling

Input is handled by an inner class `MyKeyAdapter`:

  * **`MyKeyAdapter extends KeyAdapter`**: This inner class extends `KeyAdapter` to simplify handling keyboard events.
  * **`keyPressed(KeyEvent e)`**: Detects arrow key presses (`VK_LEFT`, `VK_RIGHT`, `VK_UP`, `VK_DOWN`) and updates the snake's `direction` accordingly, preventing immediate reversal (e.g., cannot turn directly left if moving right).

### Game Mechanics

  * **`newApple()`**: Randomly generates new coordinates for the food item within the game boundaries, ensuring it aligns with the `UNIT_SIZE` grid.
  * **`newPowerUp()`**: Similar to `newApple()`, it generates coordinates for a power-up and sets `powerUpActive` to `true`. Power-ups appear every 5 apples eaten.
  * **`move()`**: Updates the `x` and `y` coordinates of the snake's body parts. Each segment takes the position of the one in front of it, and the head's position is updated based on the current `direction`.
  * **`checkApple()`**: Checks if the snake's head has collided with the apple. If so, `bodyParts` and `applesEaten` are incremented, a new apple is generated, and a power-up is created if `applesEaten` is a multiple of 5.
  * **`checkPowerUp()`**: Checks for collision with a power-up. If a power-up is collected, `powerUpActive` is set to `false`, and `applesEaten` increases by 2.
  * **`checkCollisions()`**:
      * Detects if the snake's head (`x[0], y[0]`) collides with any of its body parts.
      * Checks for collisions with the screen boundaries.
      * If any collision occurs, `running` is set to `false`, and the game `timer` is stopped.

## Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

This project is built with Java. You will need:

  * Java Development Kit (JDK) 18 or higher (as specified in `project.properties`)
  * Apache Ant (version 1.8.0 or higher)
  * Optionally, a Java IDE like NetBeans (recommended for easy setup as the project includes NetBeans-specific configuration files).

### Installation

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/subha0319/SnakeGame.git
    ```

2.  **Navigate to the project directory:**

    ```bash
    cd SnakeGame
    ```

3.  **Build the project using Ant:**

    ```bash
    ant jar
    ```

    This command will compile the Java source files and create an executable JAR file named `SnakeGame.jar` in the `dist` directory.

4.  **Run the game:**

    ```bash
    java -jar dist/SnakeGame.jar
    ```

    Alternatively, if you are using NetBeans:

      * Open NetBeans.
      * Go to `File > Open Project...` and select the `SnakeGame` folder.
      * Once the project is loaded, you can run it directly from the IDE (e.g., by clicking the "Run Project" button or `Shift + F6`).

## How to Play

  * Use the **arrow keys** (Up, Down, Left, Right) to control the direction of the snake.
  * Eat the red food items to grow the snake and increase your score.
  * Collect blue power-ups for bonus points.
  * Avoid colliding with the snake's own body or the game boundaries.
  * The game ends upon collision, displaying your final score.

## Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star\! Thanks again\!

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

## License

Distributed under the MIT License.

## Contact

Subhashini - [https://github.com/subha0319] - [subhashini0319@gmail.com]

Project Link: [https://github.com/subha0319/SnakeGame.git](https://github.com/subha0319/SnakeGame.git)
