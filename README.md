# SlotMachineGame-Python
This is a simple text-based slot machine game implemented in Python. The game allows players to deposit money, 
place bets on multiple lines, spin the slot machine, and check for winnings.

## Features
- Deposit money to start playing.
- Place bets on up to 3 lines.
- Spin the slot machine.
- Check for winnings based on the symbols and lines bet on.
- Display the current balance.
- Quit the game at any time.

## How to Play
01. Deposit Money: Enter the amount of money you want to deposit to start playing.
02. Place Bets: Choose the number of lines to bet on (1-3) and the amount to bet per line.
03. Spin the Slot Machine: The slot machine will spin and display the outcome.
04. Check Winnings: If you win, the winnings will be added to your balance.
05. Continue or Quit: You can continue playing as long as you have a balance or quit the game.

## Game Rules
- You can bet on up to 3 lines.
- Each line bet must be between $1 and $100.
- The slot machine has 4 symbols: A, B, C, and D.
- Each symbol has a specific value and count
  - A: Count = 2, Value = 5
  - B: Count = 4, Value = 4
  - C: Count = 6, Value = 3
  - D: Count = 8, Value = 2
- If all symbols in a line match, you win the bet amount multiplied by the symbol's value.

## Code Explanation
### Constants and Symbol Configurations
```MAX_LINES = 3
MAX_BET = 100
MIN_BET = 1

ROWS = 3
COLS = 3

symbol_count = {
    "A": 2,
    "B": 4,
    "C": 6,
    "D": 8
}

symbol_value = {
    "A": 5,
    "B": 4,
    "C": 3,
    "D": 2
}
```
These constants and dictionaries define the maximum lines, bet range, and the slot machine's symbol configuration.

## Main Functions
### check_winnings
```def check_winnings(columns, lines, bet, values):
    winnings = 0
    winning_lines = []
    for line in range(lines):
        symbol = columns[0][line]
        for column in columns:
            symbol_to_check = column[line]
            if symbol != symbol_to_check:
                break
        else:
            winnings += values[symbol] * bet
            winning_lines.append(line + 1)

    return winnings, winning_lines
```
This function checks for winning lines and calculates the total winnings.

### get_slot_machine_spin
```def get_slot_machine_spin(rows, cols, symbols):
    all_symbol = []
    for symbol, symbol_count in symbols.items():
        for _ in range(symbol_count):
            all_symbol.append(symbol)

    columns = []
    for _ in range(cols):
        column = []
        current_symbol = all_symbol[:]
        for _ in range(rows):
            value = random.choice(all_symbol)
            current_symbol.remove(value)
            column.append(value)

        columns.append(column)

    return columns
```
This function generates a random spin for the slot machine.

### print_slot_machine
```def print_slot_machine(columns):
    for row in range(len(columns[0])):
        for i, column in enumerate(columns):
            if i != len(columns) - 1:
                print(column[row], end=" | ")
            else:
                print(column[row], end="")

        print()
```
This function prints the slot machine's outcome.

### deposit
```def deposit():
    while True:
        amount = input("Enter amount you want to enter: $")
        if amount.isdigit():
            amount = int(amount)
            if amount > 0:
                break
            else:
                print("Amount must be greater than zero")
        else:
            print("Please enter a valid amount")

    return amount
```
This function handles depositing money into the game.

### get_number_of_lines
```def get_number_of_lines():
    while True:
        lines = input(f"Enter the lines to bet on (1-{MAX_LINES}):")
        if lines.isdigit():
            lines = int(lines)
            if 1 <= lines <= MAX_LINES:
                break
            else:
                print(f"You should enter a number between 1 to {MAX_LINES}")
        else:
            print("Please enter a valid number")
    
    return lines
```
This function prompts the player to enter the number of lines to bet on.

### get_bet
```def get_bet():
    while True:
        amount = input("What would you like to bet? $")
        if amount.isdigit():
            amount = int(amount)
            if MIN_BET <= amount <= MAX_BET:
                break
            else:
                print(f"Please enter a bet between ${MIN_BET} and ${MAX_BET}")
        else:
            print("Enter a valid amount")

    return amount
```
This function prompts the player to enter the bet amount.

### spin
```def spin(balance):
    lines = get_number_of_lines()
    while True:
        bet = get_bet()
        total_bet = bet * lines
        
        if total_bet > balance:
            print(f"You do not have enough to bet that amount, your current balance is ${balance}")
        else:
            break

    print(f"You are betting ${bet} on {lines} lines. Total bet is ${total_bet}")

    slots = get_slot_machine_spin(ROWS, COLS, symbol_count)
    print_slot_machine(slots)
    winnings, winning_lines = check_winnings(slots, lines, bet, symbol_value)
    print(f"You won ${winnings}.")
    print(f"You won on lines:", *winning_lines)

    return winnings - total_bet
```
This function handles the main game logic for spinning the slot machine and calculating the outcome.

### main
```def main():
    balance = deposit()

    while True:
        print(f"Current balance is ${balance}")
        answer = input("Press enter to play (q to quit).")
        if answer == "q":
            break
        balance += spin(balance)
    
    print(f"You left with ${balance}")
```
This function runs the game, allowing the player to deposit money, play, and quit.

## Setup and Running the Game
01. Ensure you have Python installed on your machine.
02. Clone the repository or download the script.
03. Open a terminal or command prompt and navigate to the script's directory.
04. Run the script using the command: ```python slot_machine.py```

## Contributing
Contributions are welcome! Please fork the repository and submit a pull request for any improvements or bug fixes.
