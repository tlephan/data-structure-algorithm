# Search Algorithms

## 1. Thuật toán Tìm kiếm Tuyến tính

### 1.1 Linear Search (Tìm kiếm tuần tự)
**Độ phức tạp:** O(n)  
**Ý tưởng:** Duyệt từng phần tử trong mảng cho đến khi tìm thấy giá trị cần tìm.

```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i  # Trả về vị trí
    return -1  # Không tìm thấy

# Ví dụ sử dụng
numbers = [64, 34, 25, 12, 22, 11, 90]
print(f"Tìm 22: {linear_search(numbers, 22)}")  # Kết quả: 4
print(f"Tìm 100: {linear_search(numbers, 100)}")  # Kết quả: -1
```

### 1.2 Linear Search với điều kiện
```python
def find_first_condition(arr, condition):
    for i, value in enumerate(arr):
        if condition(value):
            return {"value": value, "index": i}
    return None

# Ví dụ: Tìm số chẵn đầu tiên
numbers = [1, 3, 5, 8, 9, 12]
result = find_first_condition(numbers, lambda x: x % 2 == 0)
print(result)  # {'value': 8, 'index': 3}

# Tìm số lớn hơn 10
result = find_first_condition(numbers, lambda x: x > 10)
print(result)  # {'value': 12, 'index': 5}
```

### 1.3 Tìm tất cả vị trí xuất hiện
```python
def find_all_occurrences(arr, target):
    positions = []
    for i, value in enumerate(arr):
        if value == target:
            positions.append(i)
    return positions

# Ví dụ
numbers = [1, 3, 5, 3, 8, 3, 9]
print(find_all_occurrences(numbers, 3))  # [1, 3, 5]
```

## 2. Thuật toán Tìm kiếm Nhị phân

### 2.1 Binary Search (Tìm kiếm nhị phân)
**Độ phức tạp:** O(log n)  
**Điều kiện:** Mảng phải được sắp xếp  
**Ý tưởng:** Chia đôi không gian tìm kiếm ở mỗi bước.

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Ví dụ sử dụng
sorted_numbers = [11, 12, 22, 25, 34, 64, 90]
print(f"Tìm 25: {binary_search(sorted_numbers, 25)}")  # Kết quả: 3
print(f"Tìm 50: {binary_search(sorted_numbers, 50)}")  # Kết quả: -1
```

### 2.2 Binary Search Recursive (Đệ quy)
```python
def binary_search_recursive(arr, target, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    
    if left > right:
        return -1
    
    mid = (left + right) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)

# Ví dụ
print(binary_search_recursive([1, 3, 5, 7, 9, 11], 7))  # 3
```

### 2.3 Find Insert Position (Tìm vị trí chèn)
```python
def search_insert_position(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return left  # Vị trí chèn

# Ví dụ
arr = [1, 3, 5, 6]
print(search_insert_position(arr, 5))  # 2
print(search_insert_position(arr, 2))  # 1
print(search_insert_position(arr, 7))  # 4
```

### 2.4 Find First and Last Position
```python
def find_first_last_position(arr, target):
    def find_first():
        left, right = 0, len(arr) - 1
        first_pos = -1
        
        while left <= right:
            mid = (left + right) // 2
            if arr[mid] == target:
                first_pos = mid
                right = mid - 1  # Tiếp tục tìm bên trái
            elif arr[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return first_pos
    
    def find_last():
        left, right = 0, len(arr) - 1
        last_pos = -1
        
        while left <= right:
            mid = (left + right) // 2
            if arr[mid] == target:
                last_pos = mid
                left = mid + 1  # Tiếp tục tìm bên phải
            elif arr[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return last_pos
    
    return [find_first(), find_last()]

# Ví dụ
arr = [5, 7, 7, 8, 8, 8, 10]
print(find_first_last_position(arr, 8))  # [3, 5]
print(find_first_last_position(arr, 6))  # [-1, -1]
```

## 3. Thuật toán Tìm kiếm Đặc biệt

### 3.1 Jump Search (Tìm kiếm nhảy)
**Độ phức tạp:** O(√n)  
**Ý tưởng:** Nhảy với bước cố định, sau đó tìm kiếm tuyến tính trong khối.

```python
import math

def jump_search(arr, target):
    n = len(arr)
    step = int(math.sqrt(n))
    prev = 0
    
    # Tìm khối chứa phần tử
    while arr[min(step, n) - 1] < target:
        prev = step
        step += int(math.sqrt(n))
        if prev >= n:
            return -1
    
    # Tìm kiếm tuyến tính trong khối
    while arr[prev] < target:
        prev += 1
        if prev == min(step, n):
            return -1
    
    if arr[prev] == target:
        return prev
    
    return -1

# Ví dụ sử dụng
arr = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610]
print(jump_search(arr, 55))  # Kết quả: 10
```

### 3.2 Interpolation Search (Tìm kiếm nội suy)
**Độ phức tạp:** O(log log n) cho dữ liệu phân bố đều  
**Ý tưởng:** Ước lượng vị trí dựa trên giá trị cần tìm.

```python
def interpolation_search(arr, target):
    low, high = 0, len(arr) - 1
    
    while (low <= high and target >= arr[low] and target <= arr[high]):
        if low == high:
            return low if arr[low] == target else -1
        
        # Ước lượng vị trí
        pos = low + int(((target - arr[low]) / (arr[high] - arr[low])) * (high - low))
        
        if arr[pos] == target:
            return pos
        elif arr[pos] < target:
            low = pos + 1
        else:
            high = pos - 1
    
    return -1

# Ví dụ sử dụng
uniform_arr = [10, 12, 13, 16, 18, 19, 20, 21, 22, 23, 24, 33, 35, 42, 47]
print(interpolation_search(uniform_arr, 18))  # Kết quả: 4
```

### 3.3 Exponential Search (Tìm kiếm mũ)
**Độ phức tạp:** O(log n)  
**Ý tưởng:** Tìm phạm vi, sau đó dùng binary search.

```python
def exponential_search(arr, target):
    if arr[0] == target:
        return 0
    
    i = 1
    while i < len(arr) and arr[i] <= target:
        i = i * 2
    
    # Binary search trong phạm vi [i//2, min(i, n-1)]
    return binary_search_range(arr, target, i // 2, min(i, len(arr) - 1))

def binary_search_range(arr, target, left, right):
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Ví dụ sử dụng
large_arr = [2, 3, 4, 10, 40, 50, 60, 70, 80, 90, 100]
print(exponential_search(large_arr, 10))  # Kết quả: 3
```

### 3.4 Ternary Search (Tìm kiếm tam phân)
**Độ phức tạp:** O(log₃ n)  
**Ý tưởng:** Chia mảng thành 3 phần.

```python
def ternary_search(arr, target, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    
    if right >= left:
        mid1 = left + (right - left) // 3
        mid2 = right - (right - left) // 3
        
        if arr[mid1] == target:
            return mid1
        if arr[mid2] == target:
            return mid2
        
        if target < arr[mid1]:
            return ternary_search(arr, target, left, mid1 - 1)
        elif target > arr[mid2]:
            return ternary_search(arr, target, mid2 + 1, right)
        else:
            return ternary_search(arr, target, mid1 + 1, mid2 - 1)
    
    return -1

# Ví dụ sử dụng
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(ternary_search(arr, 5))  # Kết quả: 4
```

## 4. Thuật toán Tìm kiếm trong Chuỗi

### 4.1 Naive String Search (Tìm kiếm chuỗi đơn giản)
**Độ phức tạp:** O(nm) với n là độ dài text, m là độ dài pattern

```python
def naive_string_search(text, pattern):
    results = []
    n, m = len(text), len(pattern)
    
    for i in range(n - m + 1):
        match = True
        for j in range(m):
            if text[i + j] != pattern[j]:
                match = False
                break
        if match:
            results.append(i)
    
    return results

# Ví dụ sử dụng
text = "AABAACAADAABAABA"
pattern = "AABA"
print(naive_string_search(text, pattern))  # [0, 9, 12]
```

### 4.2 KMP Algorithm (Knuth-Morris-Pratt)
**Độ phức tạp:** O(n + m)

```python
def build_lps_array(pattern):
    """Xây dựng Longest Proper Prefix Suffix array"""
    m = len(pattern)
    lps = [0] * m
    length = 0
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    
    return lps

def kmp_search(text, pattern):
    n, m = len(text), len(pattern)
    lps = build_lps_array(pattern)
    results = []
    
    i = j = 0  # i cho text, j cho pattern
    
    while i < n:
        if pattern[j] == text[i]:
            i += 1
            j += 1
        
        if j == m:
            results.append(i - j)
            j = lps[j - 1]
        elif i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    
    return results

# Ví dụ sử dụng
print(kmp_search("AABAACAADAABAABA", "AABA"))  # [0, 9, 12]
```

### 4.3 Rabin-Karp Algorithm (Rolling Hash)
**Độ phức tạp:** O(n + m) trung bình, O(nm) worst case

```python
def rabin_karp_search(text, pattern, prime=101):
    n, m = len(text), len(pattern)
    pattern_hash = 0
    text_hash = 0
    h = 1
    results = []
    
    # Tính h = pow(d, m-1) % prime
    for i in range(m - 1):
        h = (h * 256) % prime
    
    # Tính hash cho pattern và cửa sổ đầu tiên của text
    for i in range(m):
        pattern_hash = (256 * pattern_hash + ord(pattern[i])) % prime
        text_hash = (256 * text_hash + ord(text[i])) % prime
    
    # Trượt pattern qua text
    for i in range(n - m + 1):
        # Kiểm tra hash
        if pattern_hash == text_hash:
            # Kiểm tra ký tự thực tế
            if text[i:i + m] == pattern:
                results.append(i)
        
        # Tính hash cho cửa sổ tiếp theo
        if i < n - m:
            text_hash = (256 * (text_hash - ord(text[i]) * h) + ord(text[i + m])) % prime
            if text_hash < 0:
                text_hash += prime
    
    return results

# Ví dụ sử dụng
print(rabin_karp_search("GEEKS FOR GEEKS", "GEEKS"))  # [0, 10]
```

## 5. Thuật toán Tìm kiếm trong Ma trận

### 5.1 Search in 2D Matrix (Ma trận được sắp xếp)
```python
def search_matrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    
    rows, cols = len(matrix), len(matrix[0])
    left, right = 0, rows * cols - 1
    
    while left <= right:
        mid = (left + right) // 2
        mid_value = matrix[mid // cols][mid % cols]
        
        if mid_value == target:
            return True
        elif mid_value < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return False

# Ví dụ sử dụng
matrix = [
    [1,  4,  7,  11],
    [2,  5,  8,  12],
    [3,  6,  9,  16]
]
print(search_matrix(matrix, 5))  # True
print(search_matrix(matrix, 13))  # False
```

### 5.2 Search in Row-wise and Column-wise Sorted Matrix
```python
def search_in_sorted_matrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    
    row, col = 0, len(matrix[0]) - 1
    
    while row < len(matrix) and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            col -= 1
        else:
            row += 1
    
    return False

# Ví dụ sử dụng
sorted_matrix = [
    [1,  4,  7,  11, 15],
    [2,  5,  8,  12, 19],
    [3,  6,  9,  16, 22],
    [10, 13, 14, 17, 24],
    [18, 21, 23, 26, 30]
]
print(search_in_sorted_matrix(sorted_matrix, 5))  # True
print(search_in_sorted_matrix(sorted_matrix, 20))  # False
```

### 5.3 Find Position in Matrix
```python
def find_position_in_matrix(matrix, target):
    """Trả về vị trí (row, col) của target trong matrix"""
    for i in range(len(matrix)):
        for j in range(len(matrix[i])):
            if matrix[i][j] == target:
                return (i, j)
    return (-1, -1)

# Tối ưu cho matrix sắp xếp
def find_position_optimized(matrix, target):
    if not matrix or not matrix[0]:
        return (-1, -1)
    
    row, col = 0, len(matrix[0]) - 1
    
    while row < len(matrix) and col >= 0:
        if matrix[row][col] == target:
            return (row, col)
        elif matrix[row][col] > target:
            col -= 1
        else:
            row += 1
    
    return (-1, -1)

# Ví dụ
matrix = [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
print(find_position_optimized(matrix, 5))  # (1, 1)
```

## 6. Thuật toán Tìm kiếm Nâng cao

### 6.1 Find Peak Element
```python
def find_peak_element(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        
        if nums[mid] > nums[mid + 1]:
            right = mid
        else:
            left = mid + 1
    
    return left

# Ví dụ
print(find_peak_element([1, 2, 3, 1]))  # 2 (peak là 3)
print(find_peak_element([1, 2, 1, 3, 5, 6, 4]))  # 5 (peak là 6)
```

### 6.2 Search in Rotated Sorted Array
```python
def search_rotated_array(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Xác định nửa nào được sắp xếp
        if nums[left] <= nums[mid]:
            # Nửa trái được sắp xếp
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            # Nửa phải được sắp xếp
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1

# Ví dụ: mảng [4,5,6,7,0,1,2] được xoay từ [0,1,2,4,5,6,7]
print(search_rotated_array([4, 5, 6, 7, 0, 1, 2], 0))  # 4
print(search_rotated_array([4, 5, 6, 7, 0, 1, 2], 3))  # -1
```

### 6.3 Find Minimum in Rotated Sorted Array
```python
def find_min_rotated(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid
    
    return nums[left]

# Ví dụ
print(find_min_rotated([3, 4, 5, 1, 2]))  # 1
print(find_min_rotated([4, 5, 6, 7, 0, 1, 2]))  # 0
```

### 6.4 Search for a Range
```python
def search_range(nums, target):
    def find_first():
        left, right = 0, len(nums) - 1
        first_pos = -1
        
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                first_pos = mid
                right = mid - 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return first_pos
    
    def find_last():
        left, right = 0, len(nums) - 1
        last_pos = -1
        
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                last_pos = mid
                left = mid + 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return last_pos
    
    return [find_first(), find_last()]

# Ví dụ
print(search_range([5, 7, 7, 8, 8, 10], 8))  # [3, 4]
print(search_range([5, 7, 7, 8, 8, 10], 6))  # [-1, -1]
```

## 7. Thuật toán Tìm kiếm Đặc biệt

### 7.1 Find Kth Largest Element
```python
import random

def find_kth_largest(nums, k):
    """Quick Select Algorithm - O(n) trung bình"""
    def partition(left, right, pivot_index):
        pivot_value = nums[pivot_index]
        # Di chuyển pivot về cuối
        nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
        
        store_index = left
        for i in range(left, right):
            if nums[i] > pivot_value:  # Sắp xếp giảm dần
                nums[store_index], nums[i] = nums[i], nums[store_index]
                store_index += 1
        
        # Di chuyển pivot về vị trí đúng
        nums[right], nums[store_index] = nums[store_index], nums[right]
        return store_index
    
    def select(left, right, k_smallest):
        if left == right:
            return nums[left]
        
        pivot_index = random.randint(left, right)
        pivot_index = partition(left, right, pivot_index)
        
        if k_smallest == pivot_index:
            return nums[k_smallest]
        elif k_smallest < pivot_index:
            return select(left, pivot_index - 1, k_smallest)
        else:
            return select(pivot_index + 1, right, k_smallest)
    
    return select(0, len(nums) - 1, k - 1)

# Ví dụ
print(find_kth_largest([3, 2, 1, 5, 6, 4], 2))  # 5 (phần tử lớn thứ 2)
```

### 7.2 Find Duplicate Number
```python
def find_duplicate(nums):
    """Floyd's Cycle Detection Algorithm"""
    # Phase 1: Tìm intersection point
    slow = fast = nums[0]
    
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    
    # Phase 2: Tìm entrance của cycle
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    
    return slow

# Ví dụ
print(find_duplicate([1, 3, 4, 2, 2]))  # 2
print(find_duplicate([3, 1, 3, 4, 2]))  # 3
```

### 7.3 Find Missing Number
```python
def find_missing_number(nums):
    """Sử dụng XOR"""
    n = len(nums)
    xor_all = 0
    xor_nums = 0
    
    # XOR tất cả số từ 0 đến n
    for i in range(n + 1):
        xor_all ^= i
    
    # XOR tất cả số trong mảng
    for num in nums:
        xor_nums ^= num
    
    return xor_all ^ xor_nums

# Hoặc sử dụng công thức toán học
def find_missing_number_math(nums):
    n = len(nums)
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)
    return expected_sum - actual_sum

# Ví dụ
print(find_missing_number([3, 0, 1]))  # 2
print(find_missing_number_math([0, 1]))  # 2
```
