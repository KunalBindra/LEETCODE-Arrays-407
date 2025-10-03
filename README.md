# LEETCODE-Arrays-407
---

### 🔑 Idea of the algorithm:

* We process cells from the **boundary inward**, starting with the **lowest boundary height**.
* At each step, we check neighbors. If a neighbor is lower, water is trapped.
* We always keep track of the **maximum boundary height so far** using the priority queue.

---

### Example Input

Let’s dry run with a small case:

```
heightMap = [
  [1, 4, 3],
  [3, 2, 5],
  [4, 3, 1]
]
```

---

### Step 1: Initialization

* `water = 0`
* `pq = min-heap by height`
* Insert all boundary cells into the heap:

```
Boundary cells pushed:
(1,0,0), (3,0,2), (4,0,1),
(3,1,0), (5,1,2),
(4,2,0), (1,2,2), (3,2,1)
```

Heap (min-heap by height):

```
[(1,0,0), (1,2,2), (3,0,2), (3,1,0), (3,2,1), (4,0,1), (4,2,0), (5,1,2)]
```

---

### Step 2: Process PQ

#### Pop `(1,0,0)`

* Neighbors:

  * `(1,0)` → already boundary
  * `(0,1)` → already boundary
  * `(1,0,-1)` → out of bounds
  * `(-1,0,0)` → out of bounds
    ✅ Nothing added.

---

#### Pop `(1,2,2)`

* Neighbors:

  * `(1,2,1)` → height = 3

    * trapped = max(1 - 3, 0) = 0
    * push `(3,2,1)` but already visited.
      ✅ Nothing added.

---

#### Pop `(3,0,2)`

* Neighbors:

  * `(1,0,2)` → height = 5, no water
  * `(0,1,2)` → boundary
    ✅ Nothing new.

---

#### Pop `(3,1,0)`

* Neighbors:

  * `(1,1,0)` → height = 2

    * trapped = max(3 - 2, 0) = 1 ✅
    * water = 1
    * push `(3,1,1)`
      ✅ 1 unit water stored.

---

#### Pop `(3,2,1)`

* Neighbors:

  * `(1,2,1)` → already visited
  * `(2,1,1)` → height = 2

    * trapped = max(3 - 2, 0) = 1 ✅
    * water = 2
    * push `(3,1,1)` but duplicate check avoids.
      ✅ +1 water.

---

#### Pop `(4,0,1)`

* Neighbors:

  * `(1,0,1)` → height = 2

    * trapped = max(4 - 2, 0) = 2 ✅
    * water = 4
    * push `(4,1,1)`
      ✅ +2 water.

---

#### Pop `(4,2,0)`

* Neighbors:

  * `(2,1,0)` → height = 3

    * trapped = max(4 - 3, 0) = 1 ✅
    * water = 5
    * push `(4,1,0)`
      ✅ +1 water.

---

#### Pop `(5,1,2)`

* Neighbors:

  * `(1,1,2)` → height = 2

    * trapped = max(5 - 2, 0) = 3 ✅
    * water = 8
    * push `(5,1,1)`
      ✅ +3 water.

---

### Final Answer

`water = 8`

---

### 🔁 General Dry Run Flow:

1. Add all boundaries to PQ (visited = true).
2. Pop smallest boundary cell.
3. For each neighbor:

   * If **not visited**:

     * If neighbor height < current height → water trapped.
     * Push `(max(height, neighbor_height))` into PQ.
4. Repeat until PQ empty.

---

✅ So in this example, trapped water = **8 units**.
