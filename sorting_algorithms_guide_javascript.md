# Tổng hợp Thuật toán Sắp xếp JavaScript - Từ Cơ bản đến Nâng cao

## I. THUẬT TOÁN SẮP XẾP CƠ BẢN

### 1. Bubble Sort (Sắp xếp nổi bọt)

**Ý tưởng**: Giống như bong bóng khí nổi lên mặt nước, phần tử lớn nhất sẽ "nổi" lên cuối mảng qua từng lần so sánh.

**Cách hoạt động**: So sánh từng cặp phần tử liền kề và hoán đổi nếu chúng sai thứ tự.

```javascript
function bubbleSort(arr) {
    const n = arr.length;
    const result = [...arr]; // Tạo bản sao để không thay đổi mảng gốc
    
    for (let i = 0; i < n; i++) {
        // Cờ để kiểm tra xem có hoán đổi nào xảy ra không
        let swapped = false;
        
        for (let j = 0; j < n - i - 1; j++) {
            // So sánh phần tử hiện tại với phần tử kế tiếp
            if (result[j] > result[j + 1]) {
                // Hoán đổi sử dụng destructuring
                [result[j], result[j + 1]] = [result[j + 1], result[j]];
                swapped = true;
            }
        }
        
        // Nếu không có hoán đổi nào, mảng đã được sắp xếp
        if (!swapped) break;
    }
    
    return result;
}

// Ví dụ sử dụng
console.log(bubbleSort([64, 34, 25, 12, 22, 11, 90])); 
// Output: [11, 12, 22, 25, 34, 64, 90]

// Test với mảng đã sắp xếp
console.log(bubbleSort([1, 2, 3, 4, 5])); 
// Output: [1, 2, 3, 4, 5] (chỉ 1 lần lặp nhờ optimization)
```

**Độ phức tạp**: O(n²) - không phù hợp với dữ liệu lớn
**Ưu điểm**: Đơn giản, ổn định (giữ nguyên thứ tự các phần tử bằng nhau)
**Nhược điểm**: Chậm với dữ liệu lớn

### 2. Selection Sort (Sắp xếp chọn)

**Ý tưởng**: Như việc chọn học sinh cao nhất để xếp vào đầu hàng, sau đó chọn cao thứ hai cho vị trí tiếp theo.

**Cách hoạt động**: Tìm phần tử nhỏ nhất trong phần chưa sắp xếp và đặt vào đầu.

```javascript
function selectionSort(arr) {
    const result = [...arr];
    const n = result.length;
    
    for (let i = 0; i < n; i++) {
        // Giả sử phần tử đầu tiên là nhỏ nhất
        let minIndex = i;
        
        for (let j = i + 1; j < n; j++) {
            // Tìm phần tử nhỏ nhất trong phần còn lại
            if (result[j] < result[minIndex]) {
                minIndex = j;
            }
        }
        
        // Hoán đổi phần tử nhỏ nhất với phần tử đầu tiên
        if (minIndex !== i) {
            [result[i], result[minIndex]] = [result[minIndex], result[i]];
        }
    }
    
    return result;
}

// Ví dụ sử dụng
console.log(selectionSort([64, 25, 12, 22, 11])); 
// Output: [11, 12, 22, 25, 64]

// Test với mảng có phần tử trùng lặp
console.log(selectionSort([3, 1, 4, 1, 5, 9, 2, 6])); 
// Output: [1, 1, 2, 3, 4, 5, 6, 9]
```

**Độ phức tạp**: O(n²) - ít hoán đổi hơn Bubble Sort
**Ưu điểm**: Số lần hoán đổi tối thiểu
**Nhược điểm**: Không ổn định, chậm với dữ liệu lớn

### 3. Insertion Sort (Sắp xếp chèn)

**Ý tưởng**: Giống như sắp xếp bài trong tay - lấy từng lá bài và chèn vào vị trí đúng trong phần đã sắp xếp.

**Cách hoạt động**: Xây dựng mảng đã sắp xếp từng phần tử một bằng cách chèn phần tử mới vào vị trí đúng.

```javascript
function insertionSort(arr) {
    const result = [...arr];
    
    for (let i = 1; i < result.length; i++) {
        // Phần tử cần chèn
        const key = result[i];
        let j = i - 1;
        
        // Di chuyển các phần tử lớn hơn key về phía sau
        while (j >= 0 && result[j] > key) {
            result[j + 1] = result[j];
            j--;
        }
        
        // Chèn key vào vị trí đúng
        result[j + 1] = key;
    }
    
    return result;
}

// Ví dụ sử dụng
console.log(insertionSort([5, 2, 4, 6, 1, 3])); 
// Output: [1, 2, 3, 4, 5, 6]

// Test với mảng gần như đã sắp xếp (trường hợp tốt)
console.log(insertionSort([1, 2, 3, 5, 4, 6])); 
// Output: [1, 2, 3, 4, 5, 6] (rất nhanh)
```

**Độ phức tạp**: O(n²) trường hợp xấu, O(n) trường hợp tốt
**Ưu điểm**: Hiệu quả với dữ liệu nhỏ hoặc gần như đã sắp xếp, ổn định
**Nhược điểm**: Kém hiệu quả với dữ liệu lớn

## II. THUẬT TOÁN SẮP XẾP HIỆU QUẢ

### 1. Merge Sort (Sắp xếp trộn)

**Ý tưởng**: Áp dụng chiến lược "chia để trị" - chia mảng thành các phần nhỏ, sắp xếp từng phần rồi trộn lại.

**Cách hoạt động**: Chia mảng làm đôi đến khi chỉ còn 1 phần tử, sau đó trộn các mảng con đã sắp xếp.

```javascript
function mergeSort(arr) {
    // Base case: mảng có 1 hoặc 0 phần tử đã được sắp xếp
    if (arr.length <= 1) {
        return [...arr];
    }
    
    // Chia mảng làm đôi
    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));
    
    // Trộn hai nửa đã sắp xếp
    return merge(left, right);
}

function merge(left, right) {
    const result = [];
    let i = 0, j = 0;
    
    // So sánh và trộn từng phần tử
    while (i < left.length && j < right.length) {
        if (left[i] <= right[j]) {
            result.push(left[i]);
            i++;
        } else {
            result.push(right[j]);
            j++;
        }
    }
    
    // Thêm các phần tử còn lại
    while (i < left.length) {
        result.push(left[i]);
        i++;
    }
    
    while (j < right.length) {
        result.push(right[j]);
        j++;
    }
    
    return result;
}

// Ví dụ sử dụng
console.log(mergeSort([38, 27, 43, 3, 9, 82, 10])); 
// Output: [3, 9, 10, 27, 38, 43, 82]

// Test với mảng lớn
const largeArray = Array.from({length: 1000}, () => Math.floor(Math.random() * 1000));
console.time('Merge Sort');
const sortedLarge = mergeSort(largeArray);
console.timeEnd('Merge Sort');
```

**Độ phức tạp**: O(n log n) - rất ổn định
**Ưu điểm**: Hiệu suất ổn định, ổn định, phù hợp với dữ liệu lớn
**Nhược điểm**: Cần thêm bộ nhớ O(n)

### 2. Quick Sort (Sắp xếp nhanh)

**Ý tưởng**: Chọn một phần tử làm "pivot", sắp xếp sao cho các phần tử nhỏ hơn ở bên trái, lớn hơn ở bên phải.

**Cách hoạt động**: Phân hoạch mảng quanh pivot, sau đó đệ quy sắp xếp các phần.

```javascript
function quickSort(arr, low = 0, high = arr.length - 1) {
    // Tạo bản sao chỉ lần đầu tiên
    if (low === 0 && high === arr.length - 1) {
        arr = [...arr];
    }
    
    if (low < high) {
        // Tìm vị trí phân hoạch
        const pivotIndex = partition(arr, low, high);
        
        // Sắp xếp đệ quy các phần trước và sau pivot
        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
    
    return arr;
}

function partition(arr, low, high) {
    // Chọn phần tử cuối làm pivot
    const pivot = arr[high];
    let i = low - 1; // Chỉ số của phần tử nhỏ hơn
    
    for (let j = low; j < high; j++) {
        // Nếu phần tử hiện tại nhỏ hơn hoặc bằng pivot
        if (arr[j] <= pivot) {
            i++;
            [arr[i], arr[j]] = [arr[j], arr[i]];
        }
    }
    
    // Đặt pivot vào vị trí đúng
    [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]];
    return i + 1;
}

// Phiên bản cải tiến với random pivot
function quickSortOptimized(arr) {
    if (arr.length <= 1) return [...arr];
    
    const result = [...arr];
    return quickSortHelper(result, 0, result.length - 1);
}

function quickSortHelper(arr, low, high) {
    if (low < high) {
        // Random pivot để tránh worst case
        const randomIndex = Math.floor(Math.random() * (high - low + 1)) + low;
        [arr[randomIndex], arr[high]] = [arr[high], arr[randomIndex]];
        
        const pivotIndex = partition(arr, low, high);
        quickSortHelper(arr, low, pivotIndex - 1);
        quickSortHelper(arr, pivotIndex + 1, high);
    }
    return arr;
}

// Ví dụ sử dụng
console.log(quickSort([10, 7, 8, 9, 1, 5])); 
// Output: [1, 5, 7, 8, 9, 10]

console.log(quickSortOptimized([64, 34, 25, 12, 22, 11, 90])); 
// Output: [11, 12, 22, 25, 34, 64, 90]
```

**Độ phức tạp**: O(n log n) trung bình, O(n²) trường hợp xấu
**Ưu điểm**: Nhanh trong thực tế, sắp xếp tại chỗ
**Nhược điểm**: Không ổn định, hiệu suất phụ thuộc vào việc chọn pivot

### 3. Heap Sort (Sắp xếp đống)

**Ý tưởng**: Sử dụng cấu trúc dữ liệu heap (đống) để liên tục lấy ra phần tử lớn nhất.

**Cách hoạt động**: Xây dựng max-heap, sau đó liên tục lấy phần tử lớn nhất ra khỏi heap.

```javascript
function heapSort(arr) {
    const result = [...arr];
    const n = result.length;
    
    // Xây dựng max heap (heapify từ dưới lên)
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapify(result, n, i);
    }
    
    // Lần lượt lấy phần tử từ heap
    for (let i = n - 1; i > 0; i--) {
        // Di chuyển root hiện tại đến cuối
        [result[0], result[i]] = [result[i], result[0]];
        
        // Gọi heapify trên heap đã giảm
        heapify(result, i, 0);
    }
    
    return result;
}

function heapify(arr, n, i) {
    let largest = i; // Khởi tạo largest là root
    const left = 2 * i + 1;
    const right = 2 * i + 2;
    
    // So sánh với con trái
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    // So sánh với con phải
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    // Nếu largest không phải root
    if (largest !== i) {
        [arr[i], arr[largest]] = [arr[largest], arr[i]];
        
        // Đệ quy heapify phần bị ảnh hưởng
        heapify(arr, n, largest);
    }
}

// Ví dụ sử dụng
console.log(heapSort([12, 11, 13, 5, 6, 7])); 
// Output: [5, 6, 7, 11, 12, 13]

// Visualize heap building process
function heapSortWithVisualization(arr) {
    console.log('Original array:', arr);
    const result = [...arr];
    const n = result.length;
    
    console.log('Building max heap...');
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapify(result, n, i);
        console.log(`After heapifying index ${i}:`, [...result]);
    }
    
    console.log('Extracting elements...');
    for (let i = n - 1; i > 0; i--) {
        [result[0], result[i]] = [result[i], result[0]];
        console.log(`Moved ${result[i]} to position ${i}`);
        heapify(result, i, 0);
        console.log(`Heap after extraction:`, [...result]);
    }
    
    return result;
}
```

**Độ phức tạp**: O(n log n) - ổn định
**Ưu điểm**: Hiệu suất ổn định, sắp xếp tại chỗ
**Nhược điểm**: Không ổn định, phức tạp để cài đặt

## III. THUẬT TOÁN SẮP XẾP ĐẶC BIỆT

### 1. Counting Sort (Sắp xếp đếm)

**Ý tưởng**: Đếm số lần xuất hiện của mỗi giá trị thay vì so sánh các phần tử.

**Áp dụng**: Phù hợp khi biết phạm vi giá trị (ví dụ: điểm thi từ 0-100).

```javascript
function countingSort(arr) {
    if (arr.length === 0) return [];
    
    // Tìm giá trị max và min để xác định kích thước mảng đếm
    const maxVal = Math.max(...arr);
    const minVal = Math.min(...arr);
    const range = maxVal - minVal + 1;
    
    // Mảng đếm
    const count = new Array(range).fill(0);
    const output = new Array(arr.length);
    
    // Đếm tần suất xuất hiện
    for (const num of arr) {
        count[num - minVal]++;
    }
    
    // Tính tổng tích lũy
    for (let i = 1; i < range; i++) {
        count[i] += count[i - 1];
    }
    
    // Xây dựng mảng kết quả (từ cuối để đảm bảo stability)
    for (let i = arr.length - 1; i >= 0; i--) {
        output[count[arr[i] - minVal] - 1] = arr[i];
        count[arr[i] - minVal]--;
    }
    
    return output;
}

// Phiên bản đơn giản cho số nguyên dương
function countingSortSimple(arr) {
    if (arr.length === 0) return [];
    
    const max = Math.max(...arr);
    const count = new Array(max + 1).fill(0);
    
    // Đếm tần suất
    for (const num of arr) {
        count[num]++;
    }
    
    // Tái tạo mảng đã sắp xếp
    const result = [];
    for (let i = 0; i <= max; i++) {
        for (let j = 0; j < count[i]; j++) {
            result.push(i);
        }
    }
    
    return result;
}

// Ví dụ sử dụng
console.log(countingSort([4, 2, 2, 8, 3, 3, 1])); 
// Output: [1, 2, 2, 3, 3, 4, 8]

console.log(countingSortSimple([4, 2, 2, 8, 3, 3, 1])); 
// Output: [1, 2, 2, 3, 3, 4, 8]

// Test với điểm thi (0-100)
const scores = [85, 92, 78, 85, 95, 88, 76, 92, 85];
console.log('Sorted scores:', countingSort(scores));
```

**Độ phức tạp**: O(n + k) với k là phạm vi giá trị
**Ưu điểm**: Rất nhanh khi phạm vi giá trị nhỏ, ổn định
**Nhược điểm**: Cần nhiều bộ nhớ nếu phạm vi giá trị lớn

### 2. Radix Sort (Sắp xếp theo cơ số)

**Ý tưởng**: Sắp xếp theo từng chữ số, từ chữ số ít quan trọng nhất đến quan trọng nhất.

**Cách hoạt động**: Sử dụng Counting Sort cho từng chữ số.

```javascript
function radixSort(arr) {
    if (arr.length === 0) return [];
    
    const result = [...arr];
    
    // Tìm số có nhiều chữ số nhất
    const maxNum = Math.max(...result);
    
    // Sắp xếp theo từng chữ số
    for (let exp = 1; Math.floor(maxNum / exp) > 0; exp *= 10) {
        countingSortForRadix(result, exp);
    }
    
    return result;
}

function countingSortForRadix(arr, exp) {
    const n = arr.length;
    const output = new Array(n);
    const count = new Array(10).fill(0);
    
    // Đếm tần suất của các chữ số
    for (const num of arr) {
        const index = Math.floor(num / exp) % 10;
        count[index]++;
    }
    
    // Cập nhật count[i] để chứa vị trí thực tế
    for (let i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }
    
    // Xây dựng mảng kết quả
    for (let i = n - 1; i >= 0; i--) {
        const index = Math.floor(arr[i] / exp) % 10;
        output[count[index] - 1] = arr[i];
        count[index]--;
    }
    
    // Sao chép mảng kết quả vào mảng gốc
    for (let i = 0; i < n; i++) {
        arr[i] = output[i];
    }
}

// Phiên bản với visualization
function radixSortWithSteps(arr) {
    console.log('Original array:', arr);
    const result = [...arr];
    const maxNum = Math.max(...result);
    
    let digitPlace = 1;
    for (let exp = 1; Math.floor(maxNum / exp) > 0; exp *= 10) {
        console.log(`\nSorting by digit ${digitPlace} (${exp}s place):`);
        console.log('Before:', [...result]);
        countingSortForRadix(result, exp);
        console.log('After: ', [...result]);
        digitPlace++;
    }
    
    return result;
}

// Ví dụ sử dụng
console.log(radixSort([170, 45, 75, 90, 2, 802, 24, 66])); 
// Output: [2, 24, 45, 66, 75, 90, 170, 802]

// Test với visualization
radixSortWithSteps([170, 45, 75, 90, 2, 802, 24, 66]);
```

**Độ phức tạp**: O(d × (n + k)) với d là số chữ số
**Ưu điểm**: Hiệu quả với số nguyên, ổn định
**Nhược điểm**: Chỉ áp dụng được với số nguyên và chuỗi

### 3. Bucket Sort (Sắp xếp thùng)

**Ý tưởng**: Phân phối các phần tử vào nhiều "thùng", sắp xếp từng thùng rồi kết hợp lại.

**Áp dụng**: Hiệu quả khi dữ liệu phân bố đều.

```javascript
function bucketSort(arr, bucketCount = arr.length) {
    if (arr.length <= 1) return [...arr];
    
    const maxVal = Math.max(...arr);
    const minVal = Math.min(...arr);
    
    // Tạo các thùng rỗng
    const buckets = Array.from({length: bucketCount}, () => []);
    
    // Phân phối các phần tử vào thùng
    for (const num of arr) {
        // Tính chỉ số thùng
        const bucketIndex = Math.floor(
            ((num - minVal) * (bucketCount - 1)) / (maxVal - minVal)
        );
        buckets[bucketIndex].push(num);
    }
    
    // Sắp xếp từng thùng và kết hợp
    const result = [];
    for (const bucket of buckets) {
        if (bucket.length > 0) {
            // Sử dụng insertion sort cho thùng nhỏ
            bucket.sort((a, b) => a - b);
            result.push(...bucket);
        }
    }
    
    return result;
}

// Phiên bản cho số thực trong khoảng [0, 1)
function bucketSortFloat(arr) {
    if (arr.length <= 1) return [...arr];
    
    const bucketCount = arr.length;
    const buckets = Array.from({length: bucketCount}, () => []);
    
    // Phân phối các phần tử vào thùng
    for (const num of arr) {
        const bucketIndex = Math.floor(num * bucketCount);
        buckets[Math.min(bucketIndex, bucketCount - 1)].push(num);
    }
    
    // Sắp xếp từng thùng và kết hợp
    const result = [];
    for (const bucket of buckets) {
        if (bucket.length > 0) {
            insertionSort(bucket);
            result.push(...bucket);
        }
    }
    
    return result;
}

// Helper function for bucket sort
function insertionSort(arr) {
    for (let i = 1; i < arr.length; i++) {
        const key = arr[i];
        let j = i - 1;
        
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
    return arr;
}

// Ví dụ sử dụng
console.log(bucketSort([42, 32, 33, 52, 37, 47, 51])); 
// Output: [32, 33, 37, 42, 47, 51, 52]

// Test với số thực
const floatArray = [0.897, 0.565, 0.656, 0.1234, 0.665, 0.3434];
console.log('Float array sorted:', bucketSortFloat(floatArray));
```

**Độ phức tạp**: O(n + k) trung bình, O(n²) trường hợp xấu
**Ưu điểm**: Hiệu quả với dữ liệu phân bố đều
**Nhược điểm**: Hiệu suất phụ thuộc vào phân bố dữ liệu

## IV. SO SÁNH VÀ BENCHMARK

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

### Benchmark Function

```javascript
function benchmark() {
    const sizes = [100, 1000, 5000];
    const algorithms = {
        'Bubble Sort': bubbleSort,
        'Selection Sort': selectionSort,
        'Insertion Sort': insertionSort,
        'Merge Sort': mergeSort,
        'Quick Sort': quickSortOptimized,
        'Heap Sort': heapSort,
        'Native JS Sort': (arr) => [...arr].sort((a, b) => a - b)
    };
    
    console.log('Performance Benchmark Results:');
    console.log('=====================================');
    
    sizes.forEach(size => {
        console.log(`\nArray size: ${size}`);
        console.log('-'.repeat(30));
        
        // Tạo mảng random
        const testArray = Array.from({length: size}, () => 
            Math.floor(Math.random() * size)
        );
        
        Object.entries(algorithms).forEach(([name, algorithm]) => {
            // Skip slow algorithms for large arrays
            if (size > 1000 && ['Bubble Sort', 'Selection Sort', 'Insertion Sort'].includes(name)) {
                console.log(`${name.padEnd(20)}: Skipped (too slow for large data)`);
                return;
            }
            
            const testCopy = [...testArray];
            const startTime = performance.now();
            algorithm(testCopy);
            const endTime = performance.now();
            
            console.log(`${name.padEnd(20)}: ${(endTime - startTime).toFixed(2)}ms`);
        });
    });
}

// Chạy benchmark
// benchmark(); // Uncomment để chạy test hiệu suất
```

### Utility Functions và Testing

```javascript
// Hàm kiểm tra tính đúng đắn của thuật toán sắp xếp
function isSorted(arr) {
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) {
            return false;
        }
    }
    return true;
}

// Hàm test tất cả thuật toán
function testAllAlgorithms() {
    const testCases = [
        [],                                    // Mảng rỗng
        [1],                                   // Một phần tử
        [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5],   // Mảng random
        [1, 2, 3, 4, 5],                      // Đã sắp xếp
        [5, 4, 3, 2, 1],                      // Sắp xếp ngược
        [1, 1, 1, 1, 1],                      // Tất cả giống nhau
        [2, 1, 3, 1, 2]                       // Có trùng lặp
    ];
    
    const algorithms = [
        { name: 'Bubble Sort', func: bubbleSort },
        { name: 'Selection Sort', func: selectionSort },
        { name: 'Insertion Sort', func: insertionSort },
        { name: 'Merge Sort', func: mergeSort },
        { name: 'Quick Sort', func: quickSortOptimized },
        { name: 'Heap Sort', func: heapSort },
        { name: 'Counting Sort', func: countingSort },
        { name: 'Radix Sort', func: radixSort },
        { name: 'Bucket Sort', func: bucketSort }
    ];
    
    console.log('Testing All Sorting Algorithms:');
    console.log('==============================');
    
    testCases.forEach((testCase, index) => {
        console.log(`\nTest Case ${index + 1}: [${testCase.join(', ')}]`);
        console.log('-'.repeat(50));
        
        algorithms.forEach(({ name, func }) => {
            try {
                const result = func(testCase);
                const isCorrect = isSorted(result);
                const status = isCorrect ? '✓' : '✗';
                
                console.log(`${status} ${name.padEnd(15)}: [${result.join(', ')}]`);
                
                if (!isCorrect) {
                    console.log(`  ERROR: Result is not sorted correctly!`);
                }
            } catch (error) {
                console.log(`✗ ${name.padEnd(15)}: ERROR - ${error.message}`);
            }
        });
    });
}

// Chạy test
testAllAlgorithms();
```

## V. THUẬT TOÁN HYBRID VÀ TỐI ƯU

### Tim Sort (Hybrid Algorithm - được sử dụng trong Array.sort() của JavaScript)

```javascript
// Simplified version of Tim Sort concept
function timSort(arr) {
    const MIN_MERGE = 32;
    const n = arr.length;
    
    if (n < 2) return [...arr];
    
    const result = [...arr];
    
    // Sắp xếp các run nhỏ bằng insertion sort
    for (let i = 0; i < n; i += MIN_MERGE) {
        const end = Math.min(i + MIN_MERGE - 1, n - 1);
        insertionSortRange(result, i, end);
    }
    
    // Merge các run
    let size = MIN_MERGE;
    while (size < n) {
        for (let start = 0; start < n; start += size * 2) {
            const mid = start + size - 1;
            const end = Math.min(start + size * 2 - 1, n - 1);
            
            if (mid < end) {
                mergeRange(result, start, mid, end);
            }
        }
        size *= 2;
    }
    
    return result;
}

function insertionSortRange(arr, left, right) {
    for (let i = left + 1; i <= right; i++) {
        const key = arr[i];
        let j = i - 1;
        
        while (j >= left && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}

function mergeRange(arr, left, mid, right) {
    const leftArr = arr.slice(left, mid + 1);
    const rightArr = arr.slice(mid + 1, right + 1);
    
    let i = 0, j = 0, k = left;
    
    while (i < leftArr.length && j < rightArr.length) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            i++;
        } else {
            arr[k] = rightArr[j];
            j++;
        }
        k++;
    }
    
    while (i < leftArr.length) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    
    while (j < rightArr.length) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
}

// Ví dụ sử dụng
console.log(timSort([64, 34, 25, 12, 22, 11, 90]));
```

### Intro Sort (Hybrid của Quick Sort, Heap Sort và Insertion Sort)

```javascript
function introSort(arr, maxDepth = null) {
    if (maxDepth === null) {
        maxDepth = Math.floor(Math.log2(arr.length)) * 2;
    }
    
    const result = [...arr];
    introSortHelper(result, 0, result.length - 1, maxDepth);
    return result;
}

function introSortHelper(arr, low, high, maxDepth) {
    const size = high - low + 1;
    
    // Sử dụng insertion sort cho mảng nhỏ
    if (size < 16) {
        insertionSortRange(arr, low, high);
        return;
    }
    
    // Chuyển sang heap sort nếu đệ quy quá sâu
    if (maxDepth === 0) {
        heapSortRange(arr, low, high);
        return;
    }
    
    // Sử dụng quick sort
    const pivotIndex = partition(arr, low, high);
    introSortHelper(arr, low, pivotIndex - 1, maxDepth - 1);
    introSortHelper(arr, pivotIndex + 1, high, maxDepth - 1);
}

function heapSortRange(arr, low, high) {
    // Implementation của heap sort cho một phạm vi
    const tempArr = arr.slice(low, high + 1);
    const sorted = heapSort(tempArr);
    
    for (let i = 0; i < sorted.length; i++) {
        arr[low + i] = sorted[i];
    }
}
```

## VI. HƯỚNG DẪN LỰA CHỌN VÀ THỰC HÀNH

### Khi nào nên sử dụng thuật toán nào?

```javascript
function chooseSortingAlgorithm(data, constraints = {}) {
    const {
        size = data.length,
        memoryLimited = false,
        stabilityRequired = false,
        dataType = 'number',
        isNearlysorted = false,
        range = null
    } = constraints;
    
    console.log('Recommendation based on your constraints:');
    console.log('========================================');
    
    // Dữ liệu nhỏ
    if (size < 50) {
        if (isNearlySort) {
            console.log('✓ Insertion Sort - Perfect for small, nearly sorted data');
            return insertionSort(data);
        } else {
            console.log('✓ Insertion Sort - Simple and efficient for small data');
            return insertionSort(data);
        }
    }
    
    // Counting sort cho số nguyên trong phạm vi nhỏ
    if (dataType === 'integer' && range && range < size) {
        console.log('✓ Counting Sort - Optimal for integers in small range');
        return countingSort(data);
    }
    
    // Memory limited
    if (memoryLimited) {
        if (stabilityRequired) {
            console.log('✓ Merge Sort - Stable but uses O(n) extra space');
            console.log('  Alternative: Consider external sorting for very large data');
            return mergeSort(data);
        } else {
            console.log('✓ Heap Sort - O(1) space, guaranteed O(n log n)');
            return heapSort(data);
        }
    }
    
    // Stability required
    if (stabilityRequired) {
        console.log('✓ Merge Sort - Stable and consistent O(n log n)');
        return mergeSort(data);
    }
    
    // General case - large data
    console.log('✓ Quick Sort (optimized) - Fast average case');
    console.log('  Note: JavaScript\'s native sort() uses Tim Sort');
    return quickSortOptimized(data);
}

// Demo function
function demoRecommendations() {
    console.log('SORTING ALGORITHM RECOMMENDATIONS DEMO');
    console.log('====================================\n');
    
    // Test case 1: Small array
    const smallArray = [5, 2, 8, 1, 9];
    console.log('Small array:', smallArray);
    chooseSortingAlgorithm(smallArray);
    console.log('\n' + '-'.repeat(50) + '\n');
    
    // Test case 2: Integer array with small range
    const integerArray = [3, 1, 4, 1, 5, 9, 2, 6, 5];
    console.log('Integer array (range 1-9):', integerArray);
    chooseSortingAlgorithm(integerArray, { 
        dataType: 'integer', 
        range: 9 
    });
    console.log('\n' + '-'.repeat(50) + '\n');
    
    // Test case 3: Large array, memory limited
    const largeArray = Array.from({length: 1000}, () => Math.floor(Math.random() * 1000));
    console.log('Large array (1000 elements), memory limited');
    chooseSortingAlgorithm(largeArray, { 
        memoryLimited: true,
        stabilityRequired: false 
    });
    console.log('\n' + '-'.repeat(50) + '\n');
    
    // Test case 4: Stability required
    const objectArray = [
        {name: 'Alice', score: 85},
        {name: 'Bob', score: 92},
        {name: 'Charlie', score: 85}
    ];
    console.log('Object array, stability required');
    chooseSortingAlgorithm(objectArray.map(obj => obj.score), { 
        stabilityRequired: true 
    });
}

// Chạy demo
demoRecommendations();
```

### Best Practices và Tips

```javascript
// 1. Always preserve original array
function safeBubbleSort(arr) {
    return bubbleSort([...arr]); // Good practice
}

// 2. Handle edge cases
function robustMergeSort(arr) {
    if (!Array.isArray(arr)) {
        throw new Error('Input must be an array');
    }
    if (arr.length === 0) return [];
    if (arr.some(item => typeof item !== 'number')) {
        throw new Error('All elements must be numbers');
    }
    return mergeSort(arr);
}

// 3. Custom comparison function
function genericMergeSort(arr, compareFunction = (a, b) => a - b) {
    if (arr.length <= 1) return [...arr];
    
    const mid = Math.floor(arr.length / 2);
    const left = genericMergeSort(arr.slice(0, mid), compareFunction);
    const right = genericMergeSort(arr.slice(mid), compareFunction);
    
    return mergeWithComparator(left, right, compareFunction);
}

function mergeWithComparator(left, right, compare) {
    const result = [];
    let i = 0, j = 0;
    
    while (i < left.length && j < right.length) {
        if (compare(left[i], right[j]) <= 0) {
            result.push(left[i]);
            i++;
        } else {
            result.push(right[j]);
            j++;
        }
    }
    
    return result.concat(left.slice(i)).concat(right.slice(j));
}

// Example usage
const people = [
    {name: 'Alice', age: 30},
    {name: 'Bob', age: 25},
    {name: 'Charlie', age: 35}
];

const sortedByAge = genericMergeSort(people, (a, b) => a.age - b.age);
console.log('Sorted by age:', sortedByAge);
```

## VII. BÀI TẬP VÀ THÁCH THỨC

### Câu hỏi Tự kiểm tra
1. Tại sao Quick Sort lại nhanh hơn Merge Sort trong thực tế dù cùng độ phức tạp O(n log n)?
2. Khi nào nên sử dụng thuật toán sắp xếp không so sánh (Counting, Radix, Bucket)?
3. Thuật toán nào phù hợp nhất để sắp xếp danh sách gần như đã được sắp xếp?
4. JavaScript's Array.sort() sử dụng thuật toán gì và tại sao?

### Bài tập Thực hành

```javascript
// Challenge 1: Implement your own Array.prototype.customSort
Array.prototype.customSort = function(compareFunction) {
    // Your implementation here
    return timSort(this);
};

// Challenge 2: Sort array of objects by multiple criteria
function multiCriteriaSort(arr, criteria) {
    return arr.sort((a, b) => {
        for (const criterion of criteria) {
            const { key, direction = 'asc' } = criterion;
            const modifier = direction === 'desc' ? -1 : 1;
            
            if (a[key] < b[key]) return -1 * modifier;
            if (a[key] > b[key]) return 1 * modifier;
        }
        return 0;
    });
}

// Example usage
const students = [
    {name: 'Alice', grade: 85, age: 20},
    {name: 'Bob', grade: 92, age: 19},
    {name: 'Charlie', grade: 85, age: 21}
];

const sorted = multiCriteriaSort(students, [
    {key: 'grade', direction: 'desc'},
    {key: 'age', direction: 'asc'}
]);
console.log('Multi-criteria sorted:', sorted);

// Challenge 3: External Sort for very large datasets
function externalSort(largeArray, memoryLimit = 1000) {
    // Simulate external sorting by chunking data
    const chunks = [];
    
    // Phase 1: Sort chunks that fit in memory
    for (let i = 0; i < largeArray.length; i += memoryLimit) {
        const chunk = largeArray.slice(i, i + memoryLimit);
        chunks.push(mergeSort(chunk));
    }
    
    // Phase 2: Merge sorted chunks
    while (chunks.length > 1) {
        const newChunks = [];
        for (let i = 0; i < chunks.length; i += 2) {
            if (i + 1 < chunks.length) {
                newChunks.push(merge(chunks[i], chunks[i + 1]));
            } else {
                newChunks.push(chunks[i]);
            }
        }
        chunks.splice(0, chunks.length, ...newChunks);
    }
    
    return chunks[0] || [];
}
```

Với hướng dẫn chi tiết này, bạn đã có đầy đủ kiến thức về các thuật toán sắp xếp từ cơ bản đến nâng cao trong JavaScript. Hãy thực hành cài đặt từng thuật toán và áp dụng chúng vào các bài toán thực tế để nắm vững kiến thức!