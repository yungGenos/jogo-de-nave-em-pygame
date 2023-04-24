import pygame
import random

pygame.init()

# Carregar a imagem de fundo
background_image = pygame.image.load('img/sg.jpg')

# Configurar a janela do jogo
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))

# Carregar as imagens das naves
player_image = pygame.image.load('img/skskave-removebg-preview.png')
enemy_image = pygame.image.load('img/sexouiui-removebg-preview.png')
bullet_image = pygame.image.load('img/iro_lases-removebg-preview.png')

# Definir a classe para a nave inimiga
class Enemy:
    def __init__(self):
        self.image = enemy_image
        self.width = 50
        self.height = 50
        self.x = random.randint(0, screen_width - self.width)
        self.y = -self.height
        self.speed = 5

    def update(self):
        self.y += self.speed
        player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
        enemy_rect = pygame.Rect(self.x, self.y, self.width, self.height)
        if player_rect.colliderect(enemy_rect):
            print('Colisão')
            global collision_count
            collision_count += 1
            if collision_count >= 3:
                global game_running
                game_running = False
        if self.y > screen_height:
            self.x = random.randint(0, screen_width - self.width)
            self.y = -self.height

    def draw(self):
        screen.blit(self.image, (self.x, self.y))

# Definir a classe para o projétil
class Bullet:
    def __init__(self):
        self.image = bullet_image
        self.width = 10
        self.height = 20
        self.x = player_x + player_width / 2 - self.width / 2
        self.y = player_y - self.height
        self.speed = 10

    def update(self):
        self.y -= self.speed

    def draw(self):
        screen.blit(self.image, (self.x, self.y))

    def off_screen(self):
        return self.y > screen_height

    def get_rect(self):
        return pygame.Rect(self.x, self.y, self.width, self.height)

# Criar uma instância da nave do jogador
player_width = 50
player_height = 50
player_x = screen_width / 2 - player_width / 2
player_y = screen_height / 2 - player_height / 2

# Criar uma lista para as naves inimigas e os projéteis
enemies = []
bullets = []

# Definir o temporizador para adicionar naves inimigas
enemy_timer = 0
ENEMY_DELAY = 550  # segundos e 60 fps

# Contador de colisões
collision_count = 0



# Loop principal do jogo
game_running = True
while game_running:
    # Verificar eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                    vbullets.append(B)


    # Limpar a tela
    screen.blit(background_image, (0, 0))

    # Desenhar a nave do jogador
    screen.blit(player_image, (player_x, player_y))

    # Atualizar a posição da nave do jogador
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= 5
    if keys[pygame.K_RIGHT]:
        player_x += 5
    if keys[pygame.K_UP]:
        player_y -= 5
    if keys[pygame.K_DOWN]:
        player_y += 5

    # Atualizar as naves inimigas
    for enemy in enemies:
        enemy.update()
        enemy.draw()

    # Adicionar uma nova nave inimiga a cada 2 segundos
    enemy_timer += 1
    if enemy_timer >= ENEMY_DELAY:
        enemies.append(Enemy())
        enemy_timer = 0

    # Atualizar a tela
    pygame.display.update()

pygame.quit()
