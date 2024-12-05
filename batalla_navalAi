import random

class Ship:
    def __init__(self, name, size):
        self.name = name
        self.size = size
        self.positions = []
        self.hits = 0

    def place_ship(self, start_row, start_col, direction, board):
        if direction == 'H':
            if start_col + self.size > 10:
                return False
            for i in range(self.size):
                if board[start_row][start_col + i] != ' ':
                    return False
            for i in range(self.size):
                board[start_row][start_col + i] = self.name[0]
                self.positions.append((start_row, start_col + i))
        elif direction == 'V':
            if start_row + self.size > 10:
                return False
            for i in range(self.size):
                if board[start_row + i][start_col] != ' ':
                    return False
            for i in range(self.size):
                board[start_row + i][start_col] = self.name[0]
                self.positions.append((start_row + i, start_col))
        return True

    def hit(self):
        self.hits += 1
        return self.hits == self.size

class Destroyer(Ship):
    def __init__(self):
        super().__init__("Destructor", 2)

class Submarine(Ship):
    def __init__(self):
        super().__init__("Submarino", 3)

class Battleship(Ship):
    def __init__(self):
        super().__init__("Acorazado", 4)

class Player:
    def __init__(self, name, is_ai=False):
        self.name = name
        self.board = [[' ' for _ in range(10)] for _ in range(10)]
        self.ships = []
        self.hits = [[' ' for _ in range(10)] for _ in range(10)]
        self.is_ai = is_ai

    def place_ships(self):
        ships = [Destroyer(), Submarine(), Battleship()]
        for ship in ships:
            while True:
                if self.is_ai:
                    start_row = random.randint(0, 9)
                    start_col = random.randint(0, 9)
                    direction = random.choice(['H', 'V'])
                else:
                    print(f"{self.name}, coloca tu {ship.name} de tamaño {ship.size}.")
                    start_row = int(input("Fila inicial: "))
                    start_col = int(input("Columna inicial: "))
                    direction = input("Dirección (H para horizontal, V para vertical): ").upper()

                if ship.place_ship(start_row, start_col, direction, self.board):
                    self.ships.append(ship)
                    if not self.is_ai:
                        self.print_board(self.board)
                    break
                elif not self.is_ai:
                    print("Posición no válida. Inténtalo de nuevo.")

    def print_board(self, board):
        for row in board:
            print(" ".join(row))
        print()

    def attack(self, opponent):
        if self.is_ai:
            while True:
                row = random.randint(0, 9)
                col = random.randint(0, 9)
                if opponent.board[row][col] not in ['A', 'T']:
                    break
        else:
            while True:
                print(f"{self.name}, elige una posición para atacar.")
                row = int(input("Fila: "))
                col = int(input("Columna: "))
                if 0 <= row < 10 and 0 <= col < 10 and opponent.board[row][col] not in ['A', 'T']:
                    break
                else:
                    print("Posición no válida o ya atacada. Intenta de nuevo.")

        if opponent.board[row][col] == ' ':
            print(f"{self.name} atacó en ({row}, {col}) y fue Agua!")
            self.hits[row][col] = 'A'
            opponent.board[row][col] = 'A'
        else:
            print(f"{self.name} atacó en ({row}, {col}) y fue Impacto!")
            self.hits[row][col] = 'T'
            for ship in opponent.ships:
                if (row, col) in ship.positions:
                    if ship.hit():
                        print(f"¡Hundido! {self.name} hundió el {ship.name}.")
                    break
            opponent.board[row][col] = 'T'

    def all_ships_sunk(self):
        return all(ship.hits == ship.size for ship in self.ships)

class BattleshipGame:
    def __init__(self):
        self.player1 = Player("Jugador 1")
        self.player2 = Player("Jugador 2", is_ai=True)

    def play(self):
        print("Bienvenido al juego de Batalla Naval!")
        print("Jugador 1 coloca sus barcos.")
        self.player1.place_ships()
        print("El Jugador 2 (computadora) está colocando sus barcos automáticamente.")
        self.player2.place_ships()

        current_player = self.player1
        opponent = self.player2

        while True:
            current_player.attack(opponent)
            if opponent.all_ships_sunk():
                print(f"¡{current_player.name} ha ganado el juego!")
                break
            current_player, opponent = opponent, current_player

# Crear una instancia del juego y jugar
game = BattleshipGame()
game.play()
