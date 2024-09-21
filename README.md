import random

# Slot machine setup
symbols = ['A', 'B', 'C', 'D', 'Jackpot']  
frequencies = [0.4, 0.3, 0.2, 0.1, 0.05] 
payouts = {'A': 5, 'B': 10, 'C': 20, 'D': 50, 'Jackpot': 1000}  # Payouts for each symbol

# Create reels
def create_reels():
    reels = []
    for _ in range(3):  
        reel = random.choices(symbols, frequencies, k=5)  # Each reel has 5 positions
        reels.append(reel)
    return reels

# Display the current reels setup
def display_reels(reels):
    for i, reel in enumerate(reels, 1):
        print(f"Reel {i}: {reel}")

# Spin the reels and create a 3x3 grid
def spin_reels(reels):
    grid = [[random.choice(reel) for reel in reels] for _ in range(3)]
    return grid

# Check for a win in the middle row
def check_win(grid):
    middle_row = grid[1]  # Only check the middle row
    if all(symbol == middle_row[0] for symbol in middle_row):
        symbol = middle_row[0]
        payout = payouts[symbol]
        return payout, symbol
    return 0, None

# Display the grid for each spin
def display_grid(grid):
    print("Spin outcome:")
    for row in grid:
        print(row)

# Simulate multiple spins and track winnings
def simulate_spins(n_spins):
    reels = create_reels()  # Initialize the reels
    total_winnings = 0
    jackpot_wins = 0
    regular_wins = 0

    print("\nInitial Reel Setup:")
    display_reels(reels)

    print(f"\nSimulating {n_spins} spins...\n")

    for spin_number in range(1, n_spins + 1):
        grid = spin_reels(reels)
        print(f"Spin {spin_number}:")
        display_grid(grid)
        payout, symbol = check_win(grid)
        if payout > 0:
            total_winnings += payout
            if symbol == 'Jackpot':
                jackpot_wins += 1
            else:
                regular_wins += 1
            print(f"  Win! Matched: {symbol}, Payout: {payout}\n")
        else:
            print("  No win.\n")
    
    return total_winnings, regular_wins, jackpot_wins

# Calculate RTP (Return to Player)
def calculate_rtp(total_winnings, n_spins, bet_per_spin=1):
    total_bet = n_spins * bet_per_spin
    rtp = (total_winnings / total_bet) * 100  # RTP as a percentage
    return rtp

# Main function to run the simulation
def run_slot_machine_simulation():
    n_spins = int(input("Enter the number of spins to simulate: "))
    total_winnings, regular_wins, jackpot_wins = simulate_spins(n_spins)

    print("\n=== Simulation Results ===")
    print(f"Total spins: {n_spins}")
    print(f"Total winnings: {total_winnings}")
    print(f"Regular wins: {regular_wins}")
    print(f"Jackpot wins: {jackpot_wins}")

    rtp = calculate_rtp(total_winnings, n_spins)
    print(f"Return to Player (RTP): {rtp:.2f}%")

# Run the simulation
run_slot_machine_simulation()
