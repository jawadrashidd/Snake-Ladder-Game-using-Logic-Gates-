# Digital Snake and Ladder Game

A two-player Snake and Ladder game built only from logic ICs and simulated in Proteus. The board is a 4 × 4 grid of LEDs (positions 0–15). A 555-clocked counter acts as the dice, adders and comparators work out each move, D flip-flops remember both players' positions, and 4-to-16 decoders light up the board. The first player to reach position 15 wins.

Course: EEE 4308 — Digital Electronics Lab, Islamic University of Technology (IUT)

---

## How the Game Works

1. **Choose the player.** A switch selects whose turn it is: 0 for Player 1, 1 for Player 2.
2. **Roll the dice.** While the dice button is held, a fast counter cycles through 1–3. Releasing the button freezes the count, and the result is shown on a 7-segment display.
3. **Compute the move.** The dice value is added to the current position. If the new position is a snake's head, the player drops by the snake's length. If it is a ladder's bottom, the player climbs by the ladder's length. A ladder wins if both apply.
4. **Store the position.** The new position is written into the active player's register only. The other player's position does not change.
5. **Update the board.** Decoders move the player's LED to the new block.
6. **Detect the winner.** When a player reaches `1111` (15), that player's win indicator turns on.
7. **Reset.** A reset button returns both players to position 0 without power-cycling.

---

## Board

Each of the 16 blocks has four LEDs:

| LED | Shows |
|---|---|
| Red | Player 1 |
| Blue | Player 2 |
| Green | Snake |
| Yellow | Ladder |

Snake and ladder LEDs stay lit. Player LEDs move as the game runs, and both can be on in the same block.

---

## Modules

| Module | Inputs | Outputs | How it works |
|---|---|---|---|
| Dice | Dice button | 2-bit value (1–3) | A 555 clock drives a counter while the button is held, and a BCD-to-7-segment decoder shows the result |
| Player Selector | Selector switch | Active player (0 = P1, 1 = P2) | Steers the write to the correct player register |
| Snake and Ladder Logic | Current position (4-bit), dice (2-bit), snake head and length, ladder bottom and length | Next position (4-bit) | Adders form the new position, comparators check for a snake or ladder, and the matching length is subtracted or added |
| Memory | Active player, next position, dice-pressed signal | P1 and P2 positions (4-bit each) | Two 4-bit D flip-flop registers with write-enable logic, clocked by a separate high-frequency clock |
| Reset | Reset button | Reset signal | Clears both position registers to 0 |
| Position Decoders | 4-bit positions | One LED per decoder | Six 4-to-16 decoders: P1, P2, snake head, snake tail, ladder bottom, ladder top |
| Winning | P1 and P2 positions | Win indicators | Compares each position with `1111` |

---

## Components

From the Proteus bill of materials (140 parts in total):

| IC | Qty | Function |
|---|---|---|
| 555 | 2 | Timer, used as the dice clock and the memory clock |
| 7476 | 2 | Dual JK flip-flop |
| 4013 | 6 | Dual D flip-flop |
| 7447 | 1 | BCD to 7-segment decoder |
| 7483 | 6 | 4-bit binary adder |
| 7485 | 3 | 4-bit magnitude comparator |
| 74HC386 | 3 | Quad XOR gate |
| 74154 | 6 | 4-to-16 line decoder |
| 74LS122 | 2 | Retriggerable monostable multivibrator |
| 7408 | 18 | Quad 2-input AND gate |
| 4073 | 1 | Triple 3-input AND gate |
| 7432 | 2 | Quad 2-input OR gate |
| 7404 | 4 | Hex inverter |
| AND gate | 2 | Single AND gate |

| Other parts | Qty | Value |
|---|---|---|
| LEDs | 66 | 17 red, 16 blue, 16 green, 17 yellow |
| Capacitors | 6 | 2 × 0.01 µF, 2 × 21.5 µF, 2 × 183 µF |
| Resistors | 6 | 15 kΩ, 2 × 100 kΩ, 1 kΩ, 2 × 39 kΩ |
| Momentary SPDT switches | 2 | — |
| 5 V supplies | 2 | — |

---

## Running

1. Open the project file in Proteus.
2. Press Reset to put both players at position 0.
3. Select the player, then press and release the dice button.
4. Switch players and repeat until one player reaches block 15.
