# Tổng hợp Thuật toán Sắp xếp - Từ Cơ bản đến Nâng cao

## I. THUẬT TOÁN SẮP XẾP CƠ BẢN

### 1. Bubble Sort (Sắp xếp nổi bọt)

**Ý tưởng**: Giống như bong bóng khí nổi lên mặt nước, phần tử lớn nhất sẽ "nổi" lên cuối mảng qua từng lần so sánh.

**Cách hoạt động**: So sánh từng cặp phần tử liền kề và hoán đổi nếu chúng sai thứ tự.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        # Cờ để kiểm tra xem có hoán đổi nào xảy ra không
        swapped = False
        for j in range(0, n - i - 1):
            # So sánh phần tử hiện tại với phần tử kế tiếp
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        # Nếu không có hoán đổi nào, mảng đã được sắp xếp
        if not swapped:
            break
    return arr

# Ví dụ: [64, 34, 25, 12, 22, 11, 90] → [11, 12, 22, 25, 34, 64, 90]
```

**Độ phức tạp**: O(n²) - không phù hợp với dữ liệu lớn
**Ưu điểm**: Đơn giản, ổn định (giữ nguyên thứ tự các phần tử bằng nhau)
**Nhược điểm**: Chậm với dữ liệu lớn

### 2. Selection Sort (Sắp xếp chọn)

**Ý tưởng**: Như việc chọn học sinh cao nhất để xếp vào đầu hàng, sau đó chọn cao thứ hai cho vị trí tiếp theo.

**Cách hoạt động**: Tìm phần tử nhỏ nhất trong phần chưa sắp xếp và đặt vào đầu.

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        # Giả sử phần tử đầu tiên là nhỏ nhất
        min_idx = i
        for j in range(i + 1, n):
            # Tìm phần tử nhỏ nhất trong phần còn lại
            if arr[j] < arr[min_idx]:
                min_idx = j
        # Hoán đổi phần tử nhỏ nhất với phần tử đầu tiên
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr

# Ví dụ: [64, 25, 12, 22, 11] → [11, 12, 22, 25, 64]
```

**Độ phức tạp**: O(n²) - ít hoán đổi hơn Bubble Sort
**Ưu điểm**: Số lần hoán đổi tối thiểu
**Nhược điểm**: Không ổn định, chậm với dữ liệu lớn

### 3. Insertion Sort (Sắp xếp chèn)

**Ý tưởng**: Giống như sắp xếp bài trong tay - lấy từng lá bài và chèn vào vị trí đúng trong phần đã sắp xếp.

**Cách hoạt động**: Xây dựng mảng đã sắp xếp từng phần tử một bằng cách chèn phần tử mới vào vị trí đúng.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        # Phần tử cần chèn
        key = arr[i]
        j = i - 1
        
        # Di chuyển các phần tử lớn hơn key về phía sau
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        
        # Chèn key vào vị trí đúng
        arr[j + 1] = key
    return arr

# Ví dụ: [5, 2, 4, 6, 1, 3] → [1, 2, 3, 4, 5, 6]
```

**Độ phức tạp**: O(n²) trường hợp xấu, O(n) trường hợp tốt
**Ưu điểm**: Hiệu quả với dữ liệu nhỏ hoặc gần như đã sắp xếp, ổn định
**Nhược điểm**: Kém hiệu quả với dữ liệu lớn

## II. THUẬT TOÁN SẮP XẾP HIỆU QUẢ

### 1. Merge Sort (Sắp xếp trộn)

**Ý tưởng**: Áp dụng chiến lược "chia để trị" - chia mảng thành các phần nhỏ, sắp xếp từng phần rồi trộn lại.

**Cách hoạt động**: Chia mảng làm đôi đến khi chỉ còn 1 phần tử, sau đó trộn các mảng con đã sắp xếp.

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    # Chia mảng làm đôi
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    # Trộn hai nửa đã sắp xếp
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    # So sánh và trộn từng phần tử
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    # Thêm các phần tử còn lại
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# Ví dụ: [38, 27, 43, 3, 9, 82, 10] → [3, 9, 10, 27, 38, 43, 82]
```

**Độ phức tạp**: O(n log n) - rất ổn định
**Ưu điểm**: Hiệu suất ổn định, ổn định, phù hợp với dữ liệu lớn
**Nhược điểm**: Cần thêm bộ nhớ O(n)

### 2. Quick Sort (Sắp xếp nhanh)

**Ý tưởng**: Chọn một phần tử làm "pivot", sắp xếp sao cho các phần tử nhỏ hơn ở bên trái, lớn hơn ở bên phải.

**Cách hoạt động**: Phân hoạch mảng quanh pivot, sau đó đệ quy sắp xếp các phần.

```python
def quick_sort(arr, low=0, high=None):
    if high is None:
        high = len(arr) - 1
    
    if low < high:
        # Tìm vị trí phân hoạch
        pi = partition(arr, low, high)
        
        # Sắp xếp đệ quy các phần trước và sau pivot
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
    
    return arr

def partition(arr, low, high):
    # Chọn phần tử cuối làm pivot
    pivot = arr[high]
    i = low - 1  # Chỉ số của phần tử nhỏ hơn
    
    for j in range(low, high):
        # Nếu phần tử hiện tại nhỏ hơn hoặc bằng pivot
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    # Đặt pivot vào vị trí đúng
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

# Ví dụ: [10, 7, 8, 9, 1, 5] → [1, 5, 7, 8, 9, 10]
```

**Độ phức tạp**: O(n log n) trung bình, O(n²) trường hợp xấu
**Ưu điểm**: Nhanh trong thực tế, sắp xếp tại chỗ
**Nhược điểm**: Không ổn định, hiệu suất phụ thuộc vào việc chọn pivot

### 3. Heap Sort (Sắp xếp đống)

**Ý tưởng**: Sử dụng cấu trúc dữ liệu heap (đống) để liên tục lấy ra phần tử lớn nhất.

**Cách hoạt động**: Xây dựng max-heap, sau đó liên tục lấy phần tử lớn nhất ra khỏi heap.

```python
def heap_sort(arr):
    n = len(arr)
    
    # Xây dựng max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    
    # Lần lượt lấy phần tử từ heap
    for i in range(n - 1, 0, -1):
        # Di chuyển root hiện tại đến cuối
        arr[0], arr[i] = arr[i], arr[0]
        
        # Gọi heapify trên heap đã giảm
        heapify(arr, i, 0)
    
    return arr

def heapify(arr, n, i):
    largest = i  # Khởi tạo largest là root
    left = 2 * i + 1
    right = 2 * i + 2
    
    # So sánh với con trái
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    # So sánh với con phải
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    # Nếu largest không phải root
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        # Đệ quy heapify phần bị ảnh hưởng
        heapify(arr, n, largest)

# Ví dụ: [12, 11, 13, 5, 6, 7] → [5, 6, 7, 11, 12, 13]
```

**Độ phức tạp**: O(n log n) - ổn định
**Ưu điểm**: Hiệu suất ổn định, sắp xếp tại chỗ
**Nhược điểm**: Không ổn định, phức tạp để cài đặt

## III. THUẬT TOÁN SẮP XẾP ĐẶC BIỆT

### 1. Counting Sort (Sắp xếp đếm)

**Ý tưởng**: Đếm số lần xuất hiện của mỗi giá trị thay vì so sánh các phần tử.

**Áp dụng**: Phù hợp khi biết phạm vi giá trị (ví dụ: điểm thi từ 0-100).

```python
def counting_sort(arr):
    if not arr:
        return arr
    
    # Tìm giá trị max để xác định kích thước mảng đếm
    max_val = max(arr)
    min_val = min(arr)
    range_val = max_val - min_val + 1
    
    # Mảng đếm
    count = [0] * range_val
    output = [0] * len(arr)
    
    # Đếm tần suất xuất hiện
    for num in arr:
        count[num - min_val] += 1
    
    # Tính tổng tích lũy
    for i in range(1, range_val):
        count[i] += count[i - 1]
    
    # Xây dựng mảng kết quả
    for i in range(len(arr) - 1, -1, -1):
        output[count[arr[i] - min_val] - 1] = arr[i]
        count[arr[i] - min_val] -= 1
    
    return output

# Ví dụ: [4, 2, 2, 8, 3, 3, 1] → [1, 2, 2, 3, 3, 4, 8]
```

**Độ phức tạp**: O(n + k) với k là phạm vi giá trị
**Ưu điểm**: Rất nhanh khi phạm vi giá trị nhỏ, ổn định
**Nhược điểm**: Cần nhiều bộ nhớ nếu phạm vi giá trị lớn

### 2. Radix Sort (Sắp xếp theo cơ số)

**Ý tưởng**: Sắp xếp theo từng chữ số, từ chữ số ít quan trọng nhất đến quan trọng nhất.

**Cách hoạt động**: Sử dụng Counting Sort cho từng chữ số.

```python
def radix_sort(arr):
    if not arr:
        return arr
    
    # Tìm số có nhiều chữ số nhất
    max_num = max(arr)
    
    # Sắp xếp theo từng chữ số
    exp = 1
    while max_num // exp > 0:
        counting_sort_for_radix(arr, exp)
        exp *= 10
    
    return arr

def counting_sort_for_radix(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10
    
    # Đếm tần suất của các chữ số
    for num in arr:
        index = (num // exp) % 10
        count[index] += 1
    
    # Cập nhật count[i] để chứa vị trí thực tế
    for i in range(1, 10):
        count[i] += count[i - 1]
    
    # Xây dựng mảng kết quả
    for i in range(n - 1, -1, -1):
        index = (arr[i] // exp) % 10
        output[count[index] - 1] = arr[i]
        count[index] -= 1
    
    # Sao chép mảng kết quả vào mảng gốc
    for i in range(n):
        arr[i] = output[i]

# Ví dụ: [170, 45, 75, 90, 2, 802, 24, 66] → [2, 24, 45, 66, 75, 90, 170, 802]
```

**Độ phức tạp**: O(d × (n + k)) với d là số chữ số
**Ưu điểm**: Hiệu quả với số nguyên, ổn định
**Nhược điểm**: Chỉ áp dụng được với số nguyên và chuỗi

### 3. Bucket Sort (Sắp xếp thùng)

**Ý tưởng**: Phân phối các phần tử vào nhiều "thùng", sắp xếp từng thùng rồi kết hợp lại.

**Áp dụng**: Hiệu quả khi dữ liệu phân bố đều.

```python
def bucket_sort(arr):
    if len(arr) <= 1:
        return arr
    
    # Xác định số thùng
    bucket_count = len(arr)
    max_val = max(arr)
    min_val = min(arr)
    
    # Tạo các thùng rỗng
    buckets = [[] for _ in range(bucket_count)]
    
    # Phân phối các phần tử vào thùng
    for num in arr:
        # Tính chỉ số thùng
        bucket_index = int((num - min_val) * (bucket_count - 1) / (max_val - min_val))
        buckets[bucket_index].append(num)
    
    # Sắp xếp từng thùng và kết hợp
    result = []
    for bucket in buckets:
        if bucket:
            # Sử dụng insertion sort cho thùng nhỏ
            insertion_sort(bucket)
            result.extend(bucket)
    
    return result

# Ví dụ với số thực: [0.897, 0.565, 0.656, 0.1234, 0.665, 0.3434]
```

**Độ phức tạp**: O(n + k) trung bình, O(n²) trường hợp xấu
**Ưu điểm**: Hiệu quả với dữ liệu phân bố đều
**Nhược điểm**: Hiệu suất phụ thuộc vào phân bố dữ liệu

## IV. SO SÁNH VÀ LỰA CHỌN THUẬT TOÁN

### Bảng So sánh Hiệu suất

| Thuật toán | Tốt nhất | Trung bình | Xấu nhất | Bộ nhớ | Ổn định |
|------------|----------|------------|----------|---------|---------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Có |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | Không |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Có |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Có |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | Không |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | Không |
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | O(k) | Có |
| Radix Sort | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | Có |

### Hướng dẫn Lựa chọn

**Dữ liệu nhỏ (< 50 phần tử)**: Insertion Sort - đơn giản và hiệu quả
**Dữ liệu trung bình**: Quick Sort - nhanh trong thực tế
**Cần ổn định**: Merge Sort - hiệu suất ổn định và bảo toàn thứ tự
**Bộ nhớ hạn chế**: Heap Sort - sắp xếp tại chỗ với O(n log n)
**Dữ liệu số nguyên nhỏ**: Counting Sort - cực nhanh
**Dữ liệu số nguyên lớn**: Radix Sort - hiệu quả với số có nhiều chữ số

## V. TIPS VÀ THỰC HÀNH

### Câu hỏi Tự kiểm tra
1. Tại sao Quick Sort lại nhanh hơn Merge Sort trong thực tế dù cùng độ phức tạp O(n log n)?
2. Khi nào nên sử dụng thuật toán sắp xếp không so sánh (Counting, Radix, Bucket)?
3. Thuật toán nào phù hợp nhất để sắp xếp danh sách gần như đã được sắp xếp?

### Bài tập Thực hành
- Cài đặt các thuật toán cơ bản bằng tay
- So sánh thời gian chạy thực tế với các bộ dữ liệu khác nhau
- Tối ưu hóa Quick Sort bằng cách chọn pivot thông minh
- Kết hợp nhiều thuật toán (hybrid sorting) cho hiệu quả tối ưu