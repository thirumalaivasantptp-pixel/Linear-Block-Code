# Experimental verification of Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
PC & Google Colab
# Theory
A Linear Block Code is an error-correcting code in which k data bits are encoded into n-bit codewords using linear combinations, so that errors during transmission can be detected and corrected.Each block of k bits is converted into an n-bit codeword using a generator matrix. The codewords follow linear properties (sum of any two codewords is also a valid codeword).
# Program
```
import numpy as np

pb = []

# Number of parity bits (n-k)
col = int(input("Enter the number of parity bits : "))

# Number of message bits (k)
row = int(input("Enter the number of message bits : "))

# -----------------------------
# Construct Parity Matrix P
# -----------------------------
print("\nEnter the Parity Matrix P row-wise:")

for i in range(row):
    p = list(map(int, input(f"Row {i+1}: ").split()))

    # Check correct column size
    if len(p) != col:
        print(f"Error: Enter exactly {col} elements")
        exit()

    pb.append(p)

P = np.array(pb, dtype=int)

# -----------------------------
# Generator Matrix G = [P | I]
# -----------------------------
I_k = np.eye(row, dtype=int)

G = np.hstack((P, I_k))

print("\nGenerator Matrix G:")
print(G)

# -----------------------------
# Generate all message vectors
# -----------------------------
k = row

messages = np.array([
    [int(x) for x in format(i, f'0{k}b')]
    for i in range(2**k)
])

# Generate codewords
codewords = np.mod(np.dot(messages, G), 2)

print("\nMessage      Codeword      Weight")

weights = []

for i in range(len(messages)):
    wt = np.sum(codewords[i])
    weights.append(wt)

    print(messages[i], "   ", codewords[i], "   ", wt)

# -----------------------------
# Minimum Hamming Distance
# -----------------------------
non_zero_weights = [w for w in weights if w != 0]

d_min = min(non_zero_weights)

print("\nMinimum Hamming Distance =", d_min)

# -----------------------------
# Parity Check Matrix H=[I | Pᵀ]
# -----------------------------
I_r = np.eye(col, dtype=int)

H = np.hstack((I_r, P.T))

print("\nParity Check Matrix H:")
print(H)

# Transpose of H
H_T = H.T

# -----------------------------
# Receive codeword
# -----------------------------
rc = list(map(int, input("\nEnter received codeword: ").split()))

# Length check
if len(rc) != G.shape[1]:
    print("Error: Invalid codeword length")
    exit()

r = np.array(rc)

# Syndrome
S = np.mod(np.dot(r, H_T), 2)

print("Syndrome =", S)

# -----------------------------
# Error Detection & Correction
# -----------------------------
error = np.zeros(G.shape[1], dtype=int)

for i in range(G.shape[1]):

    if np.array_equal(H_T[i], S):
        error[i] = 1
        break

print("Error Vector =", error)

# Corrected codeword
corrected = np.mod(r + error, 2)

print("Corrected Codeword =", corrected)
```
# Output

<img width="433" height="716" alt="image" src="https://github.com/user-attachments/assets/a1711717-6b78-45c9-869e-ee83f83fee04" />
<img width="433" height="216" alt="image" src="https://github.com/user-attachments/assets/3593ab0e-2ec7-4a38-b295-6d8aa183352d" />

# Results
Using linear block codes, errors in transmitted data can be efficiently detected and corrected, improving the reliability of communication systems without significantly increasing the data size.
