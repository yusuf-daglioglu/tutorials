# ALGORITHM SEARCH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 arama algoritmaları

### 📌📌 İkili Arama Algoritması (⟷ Binary Search Algorithm)

- This algorithm design to work in a sorted array only.
- It based on `parçala fethet (⟷ divide and conquer)`.
- Initially it starts to search from the center of array.
- Lets say we search `S` and the middle element of the array is `M`. This algorithm does:
  - If `S=M` then return index of `M` (element founded)
  - If `S<M` then return `binarySearch(0, index-of-M, S)`
  - If `S>M` then return `binarySearch(index-of-M, array-max-index, S)`

### 📌📌 doğrusal arama Algoritması (⟷ linear search Algorithm ⟷ sıralı arama ⟷ Sequential Search)

- On this algorithm it does not matter if the array is sorted or not.
- It compares each element one by one.

## 📌 Comparison table

| name   | best | average | worst | worst space |
|--------|------|---------|-------|-------------|
| binary | 1    | log n   | log n | 1           |
| lineer | 1    | n       | n     | 1           |
