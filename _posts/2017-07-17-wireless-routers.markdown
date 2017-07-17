---
layout: post
title:  "Wireless Routers"
date:   2017-07-17 09:55:03 +0800
categories: interview
---
## Description

Alice just bought a very big house with N rooms and N-1 doors, which each door connects two rooms. Besides, each room have at least one door and at most 3 doors(of course Alice can go to every room in this house).

However, a modern person cannot live without Wifi, so Alice wants to put M wireless routers in several rooms. As the routers are cheap, only adjacent rooms (which have a door connect to this room) can receive it, and each router works independently.

Since M routers may cannot cover every room, Alice tells you the satisfaction point S[i] she could have if room i have Wifi. Please help Alice to calculate the maximum satisfaction point she can get in total.

## Input

The first line is two integers N(2 <= N <= 1000), M(1 <= M <= N, M <= 100) Then are N lines, each shows the satisfaction S[i](1 <= S[i] <= 10) point of room i. Then are N - 1 lines, each contains two integers x, y, which represents a door is between room x and y.

## Output

Just output the maximum point of satisfaction.

## Limits

- Memory limit per test: 256 megabytes
- Time limit per test: The faster the better

## Compile & Environment

**C++**

```
g++ Main.cc -o Main -fno-asm -Wall -lm --static -std=c++0x -DONLINE_JUDGE
```

**Java**

J2SE 8

Maximum stack size is 50m

## Sample Test

**Input**

```
5 1
1 2 3 4 5
2 1
3 2
4 2
5 3
```

**Output**
```
10
```
