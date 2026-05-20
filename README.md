# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
Google colab
# Program
```
import numpy as np

pb = []

col = int(input("Enter the Parity bits : "))
row = int(input("Enter the Message bits : "))

# Parity matrix
for i in range(row):
    pb.append(list(map(int, input(f"Enter the row values : {i+1} (Separated by space) : ").split())))

p_mat = np.array(pb)

# Generator Matrix G = [P | I]
g_mat = np.hstack((p_mat, np.eye(row, dtype=int)))

n, k = g_mat.T.shape

# Possible message bits
m = np.array([[int((i >> (k-j-1)) & 1) for j in range(k)] for i in range(2**k)])

# Codewords
c = np.mod(m @ g_mat, 2)

# Hamming weights
h_mat = np.sum(c, axis=1).reshape(-1, 1)
d_min = np.min(h_mat[1:])

# Parity Check Matrix H = [Pᵀ | I]
hp = np.hstack((p_mat.T, np.eye(n-k, dtype=int)))
ht = hp.T

print('**********')
print('The Generator Matrix is: ')
for r in g_mat:
    print(*r)

print('**********')
print('Message Bits  Codeword   Hamming Weight')

code_word = np.hstack((m, c, h_mat))

for r in code_word:
    print(" ".join(map(str, r[:k])), '\t',
          " ".join(map(str, r[k:n+k])), '\t',
          r[-1])

print('**********')
print(f'Minimum Hamming distance : {d_min}')

print('**********')
print('Parity Check Matrix')
for r in hp:
    print(*r)

print('**********')
print('Parity Check Matrix Transpose')
for r in ht:
    print(*r)

# Received codeword
rc = np.array([list(map(int, input("Enter the error codeword : ").split()))])

# Syndrome Calculation
e = np.mod(rc @ ht, 2)

print('**********')
print("Syndeome of given received codeword is :", *e[0])

print('**********')
print('Syndrome Matrix')

I = np.eye(n, dtype=int)

for i in range(n):
    print(" ".join(map(str, ht[i])) + '\t' +
          " ".join(map(str, I[i])))

# Find error position
err = np.zeros(n, dtype=int)

for i in range(n):
    if np.array_equal(e[0], ht[i]):
        err = I[i]

print("The error postion is :", *err)

# Correct codeword
print("The correct codeword is :", *(err + rc[0]))
```
# Calculation

# Output Waveform

<img width="632" height="1072" alt="image" src="https://github.com/user-attachments/assets/a3ea4889-53d0-431d-afca-b378998ede1b" />


# Results

Hence,the Linear Block Code was executed and tesed successfully.

# Hardware experiment output waveform.
