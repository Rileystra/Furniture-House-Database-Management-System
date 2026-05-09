# 📦 Logistics Optimisation Algorithm – Python

A constraint-based **Python optimisation algorithm** that solves the van loading problem — maximising delivery efficiency by intelligently packing items based on weight distribution, stacking rules and retrieval order.

---

## 📌 What It Does

Manual van loading is error-prone and inefficient. This algorithm automates the decision-making by:

- Evaluating which items can be stacked based on weight and fragility constraints
- Distributing load evenly across the van to maintain safe weight balance
- Ordering items so the last delivery is loaded last (LIFO — Last In, First Out)
- Flagging constraint violations before a route begins

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3 |
| Algorithm | Constraint-based optimisation |
| Data Format | JSON / CSV input |
| Testing | Python unittest |

---

## 🧠 How the Algorithm Works

The problem is modelled as a **constraint satisfaction problem (CSP)**:

```
Input: List of items (weight, dimensions, fragility, delivery stop)
         └── Constraints: max weight, stack rules, balance threshold

Algorithm:
  1. Sort items by delivery stop (reverse order — last stop loads first)
  2. Apply stacking rules (fragile items on top, heavy items on base)
  3. Check cumulative weight against van capacity
  4. Validate left/right balance distribution
  5. Output: ordered loading plan + constraint report

Output: Optimised loading sequence + any violations flagged
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- No external libraries required (pure Python)

### Installation

```bash
git clone https://github.com/YOUR-USERNAME/logistics-optimisation.git
cd logistics-optimisation
```

### Usage

**1. Define your items in `items.json`:**
```json
[
  { "id": "PKG001", "weight_kg": 12, "fragile": false, "stop": 3 },
  { "id": "PKG002", "weight_kg": 4,  "fragile": true,  "stop": 1 },
  { "id": "PKG003", "weight_kg": 8,  "fragile": false, "stop": 2 }
]
```

**2. Run the optimiser:**
```bash
python optimise.py --input items.json --capacity 500
```

**3. View the loading plan:**
```
Loading Plan (load in this order):
  1. PKG001 — 12kg — Stop 3  [BASE]
  2. PKG003 — 8kg  — Stop 2  [MID]
  3. PKG002 — 4kg  — Stop 1  [TOP — fragile]

Total weight: 24kg / 500kg capacity
Balance: OK
Constraint violations: None
```

---

## 📂 Project Structure

```
logistics-optimisation/
│
├── optimise.py         # Main algorithm entry point
├── constraints.py      # Constraint definitions and validation logic
├── loader.py           # Item sorting and stacking logic
├── items.json          # Sample input data
├── tests/
│   └── test_optimise.py  # Unit tests for constraint validation
└── README.md
```

---

## ✅ Constraints Modelled

| Constraint | Rule |
|---|---|
| Max capacity | Total weight must not exceed van limit |
| Stack safety | Fragile items always loaded on top |
| Weight balance | Left/right distribution within ±15% threshold |
| Retrieval order | Stop 1 items loaded last (LIFO) |
| Base stability | Heaviest items always form the base layer |

---

## 🧪 Running Tests

```bash
python -m unittest tests/test_optimise.py -v
```

Tests cover constraint validation, edge cases (overloaded van, all-fragile loads, single-item input) and correct LIFO ordering.

---

## 📈 Future Improvements

- [ ] Multi-van routing — assign items across a fleet optimally
- [ ] GUI visualisation of the loading plan
- [ ] Integration with Google Maps API for route-aware loading
- [ ] Genetic algorithm approach for larger item sets

---

## 💡 Why This Project

This was built to explore how software can solve real operational problems that businesses deal with every day. The goal was not just to write code, but to model a problem correctly from first principles — defining the constraints before writing a single line of solution logic.

---

## 👤 Author

**Uriel Djantou Fanja**
📧 urieldjantou@gmail.com
🔗 [GitHub](https://github.com/YOUR-USERNAME)

---

## 📄 Licence

MIT — free to use, modify and distribute.


<img width="412" height="227" alt="08_query_staff_manager" src="https://github.com/user-attachments/assets/4e58c057-dc92-4455-b609-ca35206ec070" /><img width="519" height="579" alt="03_furniture_table" src="https://github.com/user-attachments/assets/42a31b44-5dab-41ea-ab37-9ed286b69140" />
<img width="524" height="182" alt="09_query_furniture_by_branch" src="https://github.com/user-attachments/assets/bdf84916-9580-4b31-885f-7fc8b11dc973" />
<img width="519" height="336" alt="05_rentals_table" src="https://github.com/user-attachments/assets/e441eb0c-594c-4b09-b6dc-259d0e8d25ad" />
<img width="686" height="374" alt="04_members_table" src="https://github.com/user-attachments/assets/5e495ecd-be9a-42a9-97ef-d42675784fb0" />
<img width="787" height="789" alt="06_staff_table" src="https://github.com/user-attachments/assets/88c186d0-ed9f-4782-8142-f49c4a818b7f" />
<img width="528" height="602" alt="12_query_decode" src="https://github.com/user-attachments/assets/45ad63af-9537-4d4d-b25a-f1aca0992ef9" />
<img width="628" height="399" alt="11_query_rank" src="https://github.com/user-attachments/assets/4aa1d7e5-790f-4a16-b612-a8a7825c3d1b" />
<img width="524" height="317" alt="01_relational_model" src="https://github.com/user-attachments/assets/436030b7-7286-445e-96a9-f3d1c65d6c19" />
<img width="623" height="273" alt="13_query_decode_results" src="https://github.com/user-attachments/assets/8e41436a-f29b-491e-b353-88436e77e51a" />
<img width="673" height="514" alt="02_branch_table" src="https://github.com/user-attachments/assets/86a6b371-738d-43ea-ac48-1b10bfbd5d96" />
<img width="641" height="563" alt="14_query_rental_price_results" src="https://github.com/user-attachments/assets/1601f114-adb1-4f49-bc06-4d09ea08716f" />



![07_query_rental_price](https://github.com/user-attachments/assets/68bb2b76-fca8-4d97-97de-68a4b3cabf91)
