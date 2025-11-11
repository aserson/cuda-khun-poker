# CFR Kuhn Poker Solver (CUDA)

This repository implements the **Counterfactual Regret Minimization (CFR)** algorithm for the simplified poker game **Kuhn Poker** in **CUDA-parallelized** versions.

The algorithm converges to the **Nash equilibrium**, producing optimal mixed strategies for both players.

---

## 🎯 What is Kuhn Poker?

A classic minimal poker variant used in game theory:

| Rule             | Description                     |
| ---------------- | ------------------------------- |
| Deck             | `J`, `Q`, `K` (3 cards)         |
| Players          | 2                               |
| Cards per player | 1                               |
| Actions          | `c` – check, `b` – bet          |
| Game type        | Zero-sum, imperfect information |

Despite its simplicity, **bluffing is optimal**, making it ideal for CFR benchmarks.

**Theoretical payoffs:**

```
EV(Player 1) ≈ -0.055
EV(Player 2) ≈ +0.055
```

CUDA implementations converge to these values.

---

## 📂 Project Structure

```
/project-root
│
├── CMakeLists.txt
├── src/
│   └── *.cu
└── README.md
```

---

## ✅ Build & Run

### Requirements

* CMake ≥ 3.14
* C++17 compiler (GCC / Clang / MSVC)
* **(Optional)** CUDA Toolkit ≥ 11 for GPU version

### Build (Linux / macOS)

```bash
mkdir build
cd build
cmake ..
cmake --build .
````

### Build (Windows, Visual Studio)

```bash
mkdir build
cd build
cmake .. -G "Visual Studio 17 2022"
cmake --build .
```

### Run

```
./cuda-khun-poker
```

---

## ✅ Output Example

```
Elapsed time: 403.635 ms

Player 1 expected value: -0.0566698
Player 2 expected value: 0.0566698

Player 1 strategies:
Jrr      [0.79, 0.21]
Jrrcb    [1.00, 0.00]
Qrr      [1.00, 0.00]
Qrrcb    [0.45, 0.55]
Krr      [0.39, 0.61]
Krrcb    [0.00, 1.00]

Player 2 strategies:
Jrrc     [0.67, 0.33]
Jrrb     [1.00, 0.00]
Qrrc     [1.00, 0.00]
Qrrb     [0.66, 0.34]
Krrc     [0.00, 1.00]
Krrb     [0.00, 1.00]
```

✅ These match the known Nash equilibrium strategies.

---

## ⚙️ Implementation Details

| Component                   | Description                                           |
| --------------------------- | ----------------------------------------------------- |
| `cfr()` / `cfr_recursive()` | Main CFR recursion                                    |
| `History`                   | Encodes betting sequence                              |
| `InfoSet` / `InfoSetMap`    | Stores strategy, regret, reach probability            |
| `terminal_utils()`          | Computes payoff                                       |
| `next_strategy()`           | Regret-matching update                                |
| CUDA version                | Parallelizes CFR across card combinations and actions |

---

## 🚀 CUDA Version Highlights

* Runs CFR iterations in parallel
* Uses shared memory & atomic operations
* Measurable speedup vs CPU version
* Designed for extending to larger poker games

---

## 📚 References

* H. W. Kuhn (1950) — Original simplified poker game
* Zinkevich et al. (2007) — CFR algorithm
* Brown & Sandholm (2017) — CFR+, Libratus
