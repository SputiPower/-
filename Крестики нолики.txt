# ───
# Консольная игра "Крестики-нолики" 3x3
# Автор: SputiPower 
# Описание: Игра на двоих игроков. Управление через ввод координат.
# ───

def print_board(board):
    """Игровое поле."""
    print("\n    0   1   2 ")
    print("  ┌───┬───┬───┐")
    for i, row in enumerate(board):
        row_str = f"{i} │ " + " │ ".join(row) + " │"
        print(row_str)
        if i < 2:
            print("  ├───┼───┼───┤")
        else:
            print("  └───┴───┴───┘")
    print()


def check_winner(board):
    """Проверяет наличие победителя или ничьей."""
    # Горизонтали, вертикали, диагонали
    lines = board + list(map(list, zip(*board)))  # строки и столбцы
    lines.append([board[i][i] for i in range(3)])  # главная диагональ
    lines.append([board[i][2 - i] for i in range(3)])  # побочная диагональ

    for line in lines:
        if line == ["X"] * 3:
            return "X"
        if line == ["O"] * 3:
            return "O"

    # Ничья — если нет пустых клеток
    if all(cell != " " for row in board for cell in row):
        return "Draw"

    return None  # Игра продолжается


def get_player_move(player_symbol, board):
    """Запрашивает координаты хода у игрока и проверяет их корректность."""
    while True:
        move = input(f"Ход игрока '{player_symbol}' (веди строку и столбец через пробел): ")

        try:
            x_str, y_str = move.strip().split()
            x, y = int(x_str), int(y_str)

            if not (0 <= x <= 2 and 0 <= y <= 2):
                print("Ошибка леее: координаты должны быть от 0 до 2.")
                continue

            if board[x][y] != " ":
                print("Ошибка уцы: клетка уже занята.")
                continue

            return x, y

        except ValueError:
            print("Ошибка: ведиДа две цифры через пробел (например: 1 2).")


def show_welcome_message():
    """Выводит приветственное сообщение и правила."""
    print("Добро пожаловать в игру Крестики-нолики!")
    print("Размер поля: 3 x 3")
    print("Игрок 1 — X")
    print("Игрок 2 — O")
    print("Чтобы сделать ход, введите координаты строки и столбца через пробел.")
    print("Пример: 0 2 (означает: 1-я строка, 3-й столбец)")
    print("-" * 40)


def main():
    """Основная логика игры."""
    board = [[" " for _ in range(3)] for _ in range(3)]  # пустое поле
    current_player = "X"

    show_welcome_message()
    print_board(board)

    while True:
        x, y = get_player_move(current_player, board)
        board[x][y] = current_player
        print_board(board)

        result = check_winner(board)
        if result == "Draw":
            print("Игра окончена: ничья!")
            break
        elif result:
            print(f"Победа! Игрок '{result}' выиграл!")
            break

        # Смена игрока
        current_player = "O" if current_player == "X" else "X"


# Точка входа
if __name__ == "__main__":
    main()
