import pygame
import os
from PIL import Image
import io

# Initialize Pygame
pygame.init()

# Screen settings
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Fight Scene with Image Overlay")

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Character properties
player1_pos = [100, HEIGHT // 2]
player2_pos = [600, HEIGHT // 2]
player_size = 50
speed = 5

# Create a placeholder image (since no image was uploaded)
def create_placeholder_image():
    img = Image.new('RGB', (40, 40), color='green')  # Green square as placeholder
    img_byte_arr = io.BytesIO()
    img.save(img_byte_arr, format='PNG')
    img_byte_arr.seek(0)
    return img_byte_arr

# Load and resize image
def load_image():
    try:
        # If you upload an image, replace this with actual image loading
        placeholder = create_placeholder_image()
        image = pygame.image.load(placeholder)
        image = pygame.transform.scale(image, (40, 40))  # Resize to fit character
        return image
    except Exception as e:
        print(f"Error loading image: {e}")
        return None

# Load image for player 2
player2_image = load_image()

# Game loop
running = True
clock = pygame.time.Clock()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Move players toward each other
    if player1_pos[0] < player2_pos[0] - player_size:
        player1_pos[0] += speed
        player2_pos[0] -= speed
    else:
        # Simulate "fight" by stopping movement
        speed = 0

    # Clear screen
    screen.fill(WHITE)

    # Draw player 1 (red circle)
    pygame.draw.circle(screen, RED, (int(player1_pos[0]), int(player1_pos[1])), player_size // 2)

    # Draw player 2 (blue circle with image)
    pygame.draw.circle(screen, BLUE, (int(player2_pos[0]), int(player2_pos[1])), player_size // 2)
    if player2_image:
        screen.blit(player2_image, (int(player2_pos[0] - 20), int(player2_pos[1] - 20)))

    # Update display
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
