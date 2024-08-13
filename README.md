import pygame
import math
import time

# pygame setup
pygame.init()
font = pygame.font.Font(None, 36)
screen = pygame.display.set_mode((1280, 720))
clock = pygame.time.Clock()
running = True
dt = 0

# Initialize player positions
player1_pos = pygame.Vector2(150, 200)  # White player
player2_pos = pygame.Vector2(1150, 700)  # Red player

# Initialize scores
white_score = 0
red_score = 0

# Initialize timers
white_player_start_time = time.time()

# Define walls as rectangles
walls = [
    pygame.Rect(100, 100, 300, 10),   # Horizontal wall
    pygame.Rect(100, 100, 300, 10),   # Horizontal wall
    pygame.Rect(100, 100, 300, 10),   # Horizontal wall
    pygame.Rect(100, 100, 10, 200),   # Vertical wall
    pygame.Rect(200, 200, 200, 10),   # Horizontal wall
    pygame.Rect(400, 100, 10, 110),   # Vertical wall
    pygame.Rect(100, 300, 110, 10),   # Horizontal wall
    pygame.Rect(200, 300, 10, 100),   # Vertical wall
    pygame.Rect(300, 300, 10, 100),   # Vertical wall
    pygame.Rect(300, 400, 200, 10),   # Horizontal wall
    pygame.Rect(500, 200, 10, 210),   # Vertical wall
    pygame.Rect(500, 200, 150, 10),   # Horizontal wall
    pygame.Rect(650, 100, 10, 110),   # Vertical wall
    pygame.Rect(550, 300, 200, 10),   # Horizontal wall
    pygame.Rect(650, 300, 10, 150),   # Vertical wall
    pygame.Rect(600, 450, 110, 10),   # Horizontal wall
    pygame.Rect(800, 100, 10, 260),   # Vertical wall
    pygame.Rect(700, 350, 110, 10),   # Horizontal wall
    pygame.Rect(850, 200, 220, 10),   # Horizontal wall
    pygame.Rect(950, 300, 10, 110),   # Vertical wall
    pygame.Rect(850, 400, 110, 10),   # Horizontal wall
    pygame.Rect(950, 400, 10, 210),   # Vertical wall
    pygame.Rect(850, 600, 110, 10),   # Horizontal wall
    pygame.Rect(1050, 100, 10, 210),  # Vertical wall
    pygame.Rect(1050, 100, 170, 10),  # Horizontal wall
    pygame.Rect(1150, 200, 70, 10),   # Horizontal wall
    pygame.Rect(1150, 200, 10, 100),  # Vertical wall
    pygame.Rect(1050, 300, 170, 10),  # Horizontal wall
    pygame.Rect(1050, 400, 170, 10),  # Horizontal wall
    pygame.Rect(1150, 500, 70, 10),   # Horizontal wall
    pygame.Rect(1150, 400, 10, 100),  # Vertical wall
    pygame.Rect(1050, 500, 10, 110),  # Vertical wall
    pygame.Rect(1050, 600, 170, 10),  # Horizontal wall
]

while running:
    # poll for events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # fill the screen with a color to wipe away anything from last frame
    screen.fill("black")

    # Drawing the walls
    for wall in walls:
        pygame.draw.rect(screen, "gray", wall)

    # Drawing the players as circles
    pygame.draw.circle(screen, "white", player1_pos, 20)
    pygame.draw.circle(screen, "red", player2_pos, 20)

    # Moving player 1 (controlled by WASD keys)
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w]:
        player1_pos.y -= 300 * dt
    if keys[pygame.K_s]:
        player1_pos.y += 300 * dt
    if keys[pygame.K_a]:
        player1_pos.x -= 300 * dt
    if keys[pygame.K_d]:
        player1_pos.x += 300 * dt

    # Moving player 2 (controlled by Arrow keys)
    if keys[pygame.K_UP]:
        player2_pos.y -= 300 * dt
    if keys[pygame.K_DOWN]:
        player2_pos.y += 300 * dt
    if keys[pygame.K_LEFT]:
        player2_pos.x -= 300 * dt
    if keys[pygame.K_RIGHT]:
        player2_pos.x += 300 * dt

    # Implementing the bounding box for player 1
    if player1_pos.x - 20 < 0:
        player1_pos.x = 20
    if player1_pos.x + 20 > screen.get_width():
        player1_pos.x = screen.get_width() - 20
    if player1_pos.y - 20 < 0:
        player1_pos.y = 20
    if player1_pos.y + 20 > screen.get_height():
        player1_pos.y = screen.get_height() - 20

    # Implementing the bounding box for player 2
    if player2_pos.x - 20 < 0:
        player2_pos.x = 20
    if player2_pos.x + 20 > screen.get_width():
        player2_pos.x = screen.get_width() - 20
    if player2_pos.y - 20 < 0:
        player2_pos.y = 20
    if player2_pos.y + 20 > screen.get_height():
        player2_pos.y = screen.get_height() - 20

    # Collision detection with walls for player 1
    player1_rect = pygame.Rect(player1_pos.x - 20, player1_pos.y - 20, 40, 40)
    for wall in walls:
        if player1_rect.colliderect(wall):
            # Basic collision response: move back to the previous position
            if keys[pygame.K_w]:
                player1_pos.y += 300 * dt
            if keys[pygame.K_s]:
                player1_pos.y -= 300 * dt
            if keys[pygame.K_a]:
                player1_pos.x += 300 * dt
            if keys[pygame.K_d]:
                player1_pos.x -= 300 * dt

    # Collision detection with walls for player 2
    player2_rect = pygame.Rect(player2_pos.x - 20, player2_pos.y - 20, 40, 40)
    for wall in walls:
        if player2_rect.colliderect(wall):
            # Basic collision response: move back to the previous position
            if keys[pygame.K_UP]:
                player2_pos.y += 300 * dt
            if keys[pygame.K_DOWN]:
                player2_pos.y -= 300 * dt
            if keys[pygame.K_LEFT]:
                player2_pos.x += 300 * dt
            if keys[pygame.K_RIGHT]:
                player2_pos.x -= 300 * dt

    # Check if players are touching
    distance = math.sqrt((player1_pos.x - player2_pos.x) ** 2 + (player1_pos.y - player2_pos.y) ** 2)
    if distance <= 40:  # 40 is the sum of the radii of both players
        red_score += 1
        player1_pos = pygame.Vector2(150, 200)  # White player
        player2_pos = pygame.Vector2(1150, 700)  # Red player
        white_player_start_time = time.time()  # Reset white player's survival time

    # Check if the white player has survived for 30 seconds
    if time.time() - white_player_start_time >= 10:
        white_score += 1
        white_player_start_time = time.time()  # Reset timer
    survival_time = int(time.time() - white_player_start_time)
    timer_text = font.render(f"White Player Survival Time: {survival_time}s", True, "white")
    screen.blit(timer_text, (10, 50))
    score_text = font.render(f"White Player: {white_score}  Red Player: {red_score}", True, "white")
    screen.blit(score_text, (10, 10))
    # Check for game end condition
    if white_score >= 10 or red_score >= 10:
        running = False

    # flip() the display to put your work on screen
    pygame.display.flip()

    # limits FPS to 60
    dt = clock.tick(60) / 1000

pygame.quit()
