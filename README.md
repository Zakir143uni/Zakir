# Zakir
import pygame
import random
import time

# Initialize pygame
pygame.init()

# Screen dimensions
WIDTH, HEIGHT = 600, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('Simple Car Game')

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Load car image
car_width = 50
car = pygame.Surface((car_width, 60))
car.fill(GREEN)

# Obstacle settings
obstacle_width = 50
obstacle_height = 60
obstacle_speed = 5

# Fonts
font = pygame.font.SysFont('Arial', 30)
small_font = pygame.font.SysFont('Arial', 20)

# Clock
clock = pygame.time.Clock()

# Car position
x = WIDTH // 2 - car_width // 2
y = HEIGHT - 80

# Game over flag
game_over = False

def draw_obstacle(obstacle_x, obstacle_y):
    pygame.draw.rect(screen, RED, (obstacle_x, obstacle_y, obstacle_width, obstacle_height))

def game_loop():
    global x, game_over
    x_change = 0

    # Obstacle position and speed
    obstacle_x = random.randint(0, WIDTH - obstacle_width)
    obstacle_y = -obstacle_height

    while not game_over:
        screen.fill(WHITE)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -5
                elif event.key == pygame.K_RIGHT:
                    x_change = 5
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    x_change = 0

        x += x_change

        # Prevent the car from going off-screen
        if x < 0:
            x = 0
        elif x > WIDTH - car_width:
            x = WIDTH - car_width

        # Move the obstacle down
        obstacle_y += obstacle_speed
        if obstacle_y > HEIGHT:
            obstacle_y = -obstacle_height
            obstacle_x = random.randint(0, WIDTH - obstacle_width)

        # Check for collisions
        if y < obstacle_y + obstacle_height:
            if x + car_width > obstacle_x and x < obstacle_x + obstacle_width:
                game_over = True

        # Draw car
        screen.blit(car, (x, y))

        # Draw obstacle
        draw_obstacle(obstacle_x, obstacle_y)

        # Update screen
        pygame.display.update()

        # Control frame rate
        clock.tick(60)

    # Display game over text
    game_over_text = font.render("Game Over", True, BLACK)
    screen.blit(game_over_text, (WIDTH // 2 - game_over_text.get_width() // 2, HEIGHT // 2 - game_over_text.get_height() // 2))
    pygame.display.update()

    # Wait for a few seconds before closing
    time.sleep(2)
    pygame.quit()

# Start the game loop
game_loop()
