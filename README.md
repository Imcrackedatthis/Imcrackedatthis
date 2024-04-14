import pygame

# Define some colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

# Define screen size
WIDTH = 800
HEIGHT = 600

# Initialize Pygame
pygame.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Simple Mario")

# Define player class
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((32, 32))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.x = 64
        self.rect.y = HEIGHT - 64
        self.x_vel = 0
        self.y_vel = 0
        self.is_jumping = False

    def update(self):
        self.x_vel = 0  # Reset horizontal velocity each frame
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            self.x_vel = -5
        if keys[pygame.K_RIGHT]:
            self.x_vel = 5
        if keys[pygame.K_SPACE] and not self.is_jumping:
            self.is_jumping = True
            self.y_vel = -10

        # Handle jumping physics (simplified)
        if self.is_jumping:
            self.y_vel += 0.5  # Simulate gravity
            if self.y_vel > 0:
                self.is_jumping = False

        # Update position based on velocity
        self.rect.x += self.x_vel
        self.rect.y += self.y_vel

        # Check for collisions (replace with actual collision detection)
        if self.rect.bottom >= HEIGHT:
            self.rect.bottom = HEIGHT
            self.y_vel = 0

# Define game loop
running = True
clock = pygame.time.Clock()
player = Player()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Update game objects
    player.update()

    # Draw game elements
    screen.fill(BLACK)
    screen.blit(player.image, player.rect)
    pygame.display.flip()

    # Set frame rate
    clock.tick(60)

# Quit Pygame
pygame.quit()
