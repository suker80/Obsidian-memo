```python
n, k = map(int, input().split())

arr = list(map(int, input().split()))
answer = 0


def put_flour():
    min_val = min(arr)
    for i in range(n):
        if arr[i] == min_val:
            arr[i] += 1


def rotate(matrix):
    temp_list = [[0] * len(matrix) for _ in range(len(matrix[0]))]

    for i in range(len(matrix[0])):
        for j in range(len(matrix)):
            temp_list[i][j] = matrix[len(matrix) - j - 1][i]
    return temp_list


# 1 2 2 3
def roll():
    global arr
    roll_list = [[arr[1], arr[0]], arr[2:]]
    if n == 4:
        arr = roll_list
        return
    idx = 2
    while True:
        temp_list = []
        for i in range(len(roll_list) - 1):
            temp_list.append(roll_list[i])
        temp_list.append(roll_list[-1][:idx])
        temp_list = rotate(temp_list)
        temp_list.append(roll_list[-1][idx:])

        idx = len(temp_list[0])

        last_len = len(temp_list[-1])

        roll_list = temp_list
        if last_len - idx < len(temp_list):
            break

    arr = roll_list


def flatten():
    global arr
    temp_list = [[0] * len(_) for _ in arr]

    for i in range(len(arr)):
        for j in range(len(arr[i])):
            if j + 1 < len(arr[i]):
                a = arr[i][j]
                b = arr[i][j + 1]
                d = abs(a - b) // 5
                if a < b:
                    temp_list[i][j] += d
                    temp_list[i][j + 1] -= d
                elif a > b:
                    temp_list[i][j] -= d
                    temp_list[i][j + 1] += d

            if i + 1 < len(arr):
                a = arr[i][j]
                b = arr[i + 1][j]
                d = abs(a - b) // 5

                if a < b:
                    temp_list[i][j] += d
                    temp_list[i + 1][j] -= d
                elif a > b:
                    temp_list[i][j] -= d
                    temp_list[i + 1][j] += d

    for i in range(len(arr)):
        for j in range(len(arr[i])):
            arr[i][j] += temp_list[i][j]

    temp_list = []
    for j in range(len(arr[-1])):
        for i in range(len(arr) - 1, -1, -1):
            if len(arr[i]) > j:
                temp_list.append(arr[i][j])
    arr = temp_list


def fold():
    global arr
    temp_n = n
    temp_list = [list(reversed(arr[:n // 2])), arr[n // 2:]]
    temp_n //= 2
    temp_list2 = []
    for i in range(len(temp_list) - 1, -1, - 1):
        temp_list2.append(list(reversed(temp_list[i][:temp_n // 2])))
    for i in range(len(temp_list)):
        temp_list2.append(temp_list[i][temp_n // 2:])
    arr = temp_list2


count = 0
while True:
    count += 1
    put_flour()
    roll()
    flatten()
    fold()
    flatten()

    if max(arr) - min(arr) <= k:
        break
print(count)
```