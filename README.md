# Secure Multiparty Computation with Beaver Triples (C# Implementation)

This project demonstrates a simple **Secure Multiparty Computation (MPC)** protocol for multiplying secret-shared values using **Beaver triples**, implemented in **C#**.

The system consists of **four parties**:

- **P0** → Party 0 (computing party)  
- **P1** → Party 1 (computing party)  
- **P2 (Helper)** → Provides Beaver triples  
- **P3 (Client)** → Provides inputs and splits them into shares  

---

## 📖 Overview

The goal is to securely compute the product:

1. `z = x * y`  
2. `z2 = z * y_next`  

without revealing `x`, `y`, or `y_next` to any single party.  

This is achieved with **additive secret sharing** and **Beaver triples**:

- Client `P3` splits each input into random shares and sends them to `P0` and `P1`.  
- Helper `P2` generates a random Beaver triple `(a, b, c)` where `c = a*b`.  
- Parties `P0` and `P1` use the shares and the Beaver triple to compute multiplication securely.  

---

## ⚙️ Requirements

- .NET 6.0 or later  
- Works on Windows, Linux, macOS  

---

## 📂 Files

- **`P0.cs`** → Party 0 (computes `z0` shares)  
- **`P1.cs`** → Party 1 (computes `z1` shares)  
- **`P2_Helper.cs`** → Generates Beaver triples and distributes them  
- **`P3_Client.cs`** → Provides inputs, generates shares, and distributes them  

---

## 🔌 Protocol Flow

```mermaid
sequenceDiagram
    participant P3 as P3 (Client)
    participant P0 as P0
    participant P1 as P1
    participant P2 as P2 (Helper)

    P3 ->> P0: Send (x0, y0, y0_next)
    P3 ->> P1: Send (x1, y1, y1_next)

    P2 ->> P0: Send (a0, b0, c0)
    P2 ->> P1: Send (a1, b1, c1)

    P0 ->> P1: Send (d0, e0)
    P1 ->> P0: Send (d1, e1)

    Note over P0,P1: Each computes z0, z1 <br/> Final result: z = z0 + z1
