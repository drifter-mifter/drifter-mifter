import pygame
import random

# Inicjalizacja Pygame
pygame.init()

# Ustawienia okna gry
szerokosc = 800
wysokosc = 600
okno_gry = pygame.display.set_mode((szerokosc, wysokosc))
pygame.display.set_caption("Gra z przeszkodami")

# Kolory
bialy = (255, 255, 255)
czerwony = (255, 0, 0)

# Gracz
gracz_x = szerokosc // 2
gracz_y = wysokosc - 50
gracz_szerokosc = 40
gracz_wysokosc = 40
predkosc_gracza = 5

# Przeszkody
przeszkody = []
przeszkod_szerokosc = 50
przeszkod_wysokosc = 50
predkosc_przeszkod = 5

# Nagrody
nagrody = []
nagroda_szerokosc = 30
nagroda_wysokosc = 30
predkosc_nagrod = 3

# Wynik
wynik = 0

# Funkcja do tworzenia nowych przeszkód
def stworz_przeszkode():
    x = random.randint(0, szerokosc - przeszkod_szerokosc)
    y = 0
    przeszkody.append([x, y])

# Funkcja do tworzenia nowych nagród
def stworz_nagrode():
    x = random.randint(0, szerokosc - nagroda_szerokosc)
    y = 0
    nagrody.append([x, y])

# Główna pętla gry
gra_dziala = True
while gra_dziala:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            gra_dziala = False

    # Poruszanie się gracza
    klawisze = pygame.key.get_pressed()
    if klawisze[pygame.K_LEFT] and gracz_x > 0:
        gracz_x -= predkosc_gracza
    if klawisze[pygame.K_RIGHT] and gracz_x < szerokosc - gracz_szerokosc:
        gracz_x += predkosc_gracza

    # Tworzenie nowych przeszkód
    if random.randint(0, 100) < 2:
        stworz_przeszkode()

    # Poruszanie się przeszkód
    for przeszkoda in przeszkody:
        przeszkoda[1] += predkosc_przeszkod

    # Tworzenie nowych nagród
    if random.randint(0, 100) < 1:
        stworz_nagrode()

    # Poruszanie się nagród
    for nagroda in nagrody:
        nagroda[1] += predkosc_nagrod

    # Kolizje z przeszkodami
    for przeszkoda in przeszkody:
        if gracz_x < przeszkoda[0] + przeszkod_szerokosc and gracz_x + gracz_szerokosc > przeszkoda[0] and gracz_y < przeszkoda[1] + przeszkod_wysokosc and gracz_y + gracz_wysokosc > przeszkoda[1]:
            gra_dziala = False

    # Kolizje z nagrodami
    for nagroda in nagrody:
        if gracz_x < nagroda[0] + nagroda_szerokosc and gracz_x + gracz_szerokosc > nagroda[0] and gracz_y < nagroda[1] + nagroda_wysokosc and gracz_y + gracz_wysokosc > nagroda[1]:
            nagrody.remove(nagroda)
            wynik += 1

    # Usuwanie przeszkód, które wypadły poza ekran
    przeszkody = [przeszkoda for przeszkoda in przeszkody if przeszkoda[1] < wysokosc]

    # Usuwanie nagród, które wypadły poza ekran
    nagrody = [nagroda for nagroda in nagrody if nagroda[1] < wysokosc]

    # Czyszczenie ekranu
    okno_gry.fill(bialy)

    # Rysowanie gracza
    pygame.draw.rect(okno_gry, czerwony, (gracz_x, gracz_y, gracz_szerokosc, gracz_wysokosc))

    # Rysowanie przeszkód
    for przeszkoda in przeszkody:
        pygame.draw.rect(okno_gry, czerwony, (przeszkoda[0], przeszkoda[1], przeszkod_szerokosc, przeszkod_wysokosc))

    # Rysowanie nagród
    for nagroda in nagrody:
        pygame.draw.rect(okno_gry, czerwony, (nagroda[0], nagroda[1], nagroda_szerokosc, nagroda_wysokosc))

    # Wyświetlanie wyniku
    font = pygame.font.Font(None, 36)
    text = font.render(f"Wynik: {wynik}", True, czerwony)
    okno_gry.blit(text, (10, 10))

    # Aktualizacja ekranu
    pygame.display.update()

# Zakończenie gry
pygame.quit()





