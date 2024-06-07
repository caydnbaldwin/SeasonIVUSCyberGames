# Flag Checker
Level: Beginner

Description:
```
We found this cryptic Python script that validates the user's flag, but we're having trouble understanding the code. Can you find the correct flag that passes through the program?

[pyrev.py]
```

## Writeup
Reverse Engineering challenge. `pyrev.py` starts with a predefined sequence `phoneSteak`. It then takes a user input as `libraryDiscussion` and converts it to its ASCII value as `confusedSheep` and preserves the length of the ASCII values in `mintFarm`. It then preserves the length of the ASCII values of `phoneSteak` as `trustBreed`. It defines `seaTent` as 6, `callCover` as 17, `foxEmbox` as (248 // `trustBreed`) % `trustBreed`, `outfitStrike` as 10, `brushCopy` as (341 // `trustBreed`) % 17, and `injectPush` as (1240 + 28 // `trustBreed`) % `trustBreed`. and initializes two empty lists, `makeupRoof` and `tinRoyalty`. Then it begins if `trustBreed` equals `mintFarm`, subtract 27 from each ASCII value in `confusedSheep` and store it in the `makeupRoof` list. Then XOR each value in `makeupRoof` with 15 and store the result in `tinRoyalty`. Then it swaps the values in `tinRoyalty` at positions `seaTemt` and `injectPush`. Then swaps values in `tinRoyalty` at positions `outfitStrike` and `foxEmbox`. The swaps values in `tinRoyalty` at positions `callCover` and `brushCopy`. Then splits `tinRoyalty` into two halves, `lineMoon` and `puddingCommission`. Then it combines `lineMoon` with the reversed `puddingCommission`. Then it checks if `tinRoyalty` matches the `phoneSteak` list, which prints `Correct!! :)` if true and `Incorrect flag :(` if false.

To reverse engineer this script, `tinRoyalty` needs to match `phoneSteak`. Begin by splitting `phoneSteak` into `lineMoon` and `puddingCommission` and combining them. Then undo all of the swaps to get to the original `tinRoyalty`. Begin with `callCover` and `brushCopy`, then `outfitStrike` and `foxEmbox`, then `seaTent` and `injectPush`.Then revert the XOR operation with 15 to each element of `tinRoyalty`. Then revert the subtraction by adding 27 to each element. Finally, convert the ASCII to characters.

```
phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88]

# Step 1: Split phoneSteak into lineMoon and puddingCommission
mid = len(phoneSteak) // 2
lineMoon = phoneSteak[:mid]
puddingCommission = phoneSteak[mid:][::-1]

# Combine them
tinRoyalty = lineMoon + puddingCommission

# Constants
seaTent = 6
callCover = 17
foxEmbox = (248 // len(phoneSteak)) % len(phoneSteak)
outfitStrike = 10
brushCopy = (341 // len(phoneSteak)) % 17
injectPush = (1240 + 28 // len(phoneSteak)) % len(phoneSteak)

# Step 2: Undo the swaps
tinRoyalty[callCover], tinRoyalty[brushCopy] = tinRoyalty[brushCopy], tinRoyalty[callCover]
tinRoyalty[outfitStrike], tinRoyalty[foxEmbox] = tinRoyalty[foxEmbox], tinRoyalty[outfitStrike]
tinRoyalty[seaTent], tinRoyalty[injectPush] = tinRoyalty[injectPush], tinRoyalty[seaTent]

# Step 3: Revert XOR with 15
tinRoyalty = [value ^ 15 for value in tinRoyalty]

# Step 4: Revert the subtraction of 27
confusedSheep = [value + 27 for value in tinRoyalty]

# Step 5: Convert ASCII values back to characters to get the flag
flag = ''.join(chr(value) for value in confusedSheep)
print(flag)
```

**Flag** - `SIVBGR{pyth0n_r3v3rs1ng_pr0}`
