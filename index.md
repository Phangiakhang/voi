---
layout: default
title: "Tổng hợp kiến thức 📚"
---

<script defer src="https://cdn.jsdelivr.net/npm/katex@0.15.3/dist/katex.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.15.3/dist/katex.min.css">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.15.3/dist/contrib/auto-render.min.js"
    onload="renderMathInElement(document.body, {
        delimiters: [
            {left: '$$', right: '$$', display: true},
            {left: '$', right: '$', display: false}
        ],
        ignoredTags: ['script', 'noscript', 'style', 'textarea', 'pre']
    });">
</script>

# **📌 MỤC LỤC**
* TOC
{:toc}

---

# **I. NGÔN NGỮ, TỐI ƯU VÀ STL (C++ FOUNDATION)**

### **1. Tối ưu I/O (Fast I/O)**

Code:
```cpp
ios_base::sync_with_stdio(false); 
cin.tie(nullptr); cout.tie(nullptr);
```

> Lưu ý: Tuyệt đối không dùng endl vì nó ép hệ thống phải flush (đẩy dữ liệu ra ngoài) liên tục gây chậm. Hãy dùng \n.

### **2. Pragmas & Compilier**

Code:
```cpp
#pragma warning (disable : 4996)
#pragma GCC optimize("O3,unroll-loops")
#pragma GCC target("avx2,bmi,bmi2,lzcnt,popcnt")
```

### **3. C++ Standard Template Libary (STL)**

*a. `vector` (Mảng động)*

Cơ chế: Lưu trữ dữ liệu trên một khối bộ nhớ liền kề trong RAM. Khi mảng đầy, nó tự động cấp phát một mảng mới có kích thước gấp đôi (hoặc gấp rưỡi), copy dữ liệu cũ sang và xóa mảng cũ.

Cách khai báo:
```cpp
vector<int> a;                              // vector rỗng
vector<int> a(n);                           // n phần tử, mặc định là 0
vector<int> a(n, x);                        // n phần tử, khởi tạo bằng x
vector<vector<int>> adj(n, vector<int>(m))  // Mảng n * m phần tử
```

Các hàm đi kèm & Cách sử dụng:

> Cú pháp `<tên_vector>.<tên_hàm>(<tham_số>)`

- `push_back(x) / emplace_back(x)`: Thêm phần tử $x$ vào cuối.
- `pop_back()`: Xóa phần tử cuối.
- `resize(n)`: Thay đổi kích thước vector thành n.
- `size()`: Lấy kích thước của vector.
- `empty()`: Kiểm tra vector có rỗng hay không.
- `clear()`: Xóa toàn bộ vector.
- `at(i)`: Truy cập phần tử thứ i, trả về $std::out_of_range$ nếu vượt giới hạn của vector.
- `front()`: Truy cập phần tử đầu.
- `back()`: Truy cập phần tử cuối.
- `data()`: Trả về con trỏ tới vùng nhớ chứa dữ liệu.
- `max_size()`: Số phần tử tối đa mà vector có thể chứa theo giới hạn implementation/allocator.
- `reverse(n)`: Khai báo trước sức chứa $n$ phần tử (tránh chi phí mở rộng vector nhiều lần).
- `capacity()`: Dung lượng bộ nhớ hiện đang được cấp phát.

> Phân biệt cực kỳ quan trọng:
> size     = số phần tử đang tồn tại
> capacity = số phần tử có thể chứa trước khi cần cấp phát lại

- `begin()`: Iterator tới phần tử đầu.
- `end()`: Iterator tới phần tử cuối.
- `rbegin()`: Reverse iterator bắt đầu từ phần tử cuối.
- `rend()`: Reverse iterator nằm trước phần tử đầu.
- `erase(v.begin() + i)`: Xóa phần tử thứ $i$.
- `erase(v.begin() + l, v.begin() + r)`: Xóa đoạn $[l,r)$.
- `insert(v.begin() + i, x)`: Chèn phần tử $x$ vào vị trí $i$.
- `insert(v.begin() + i, n, x)`: Chèn $n$ phần tử $x$ vào vị trí $i$.
- `a.insert(a.begin() + i, b.begin() + l, b.begin() + r)`: Chèn đoạn $[l,r)$ của vetor b vào vị trí $i$ của vector a.
- `a.swap(b)`: Hoán đổi nội dung 2 vector (có thể khác kích thước) với độ phức tạp $~O(1)$.

Các hàm không đi kèm nhưng cực kỳ hữu dụng:

> Cú pháp `<tên_hàm>(<tham_số>)`

- `sort(v.begin(), v.end())`: Sắp xếp tắng dần.
- `sort(v.begin(), v.end(), greater<int>())`: Sắp xếp giảm dần.
- `sort(v.begin(), v.end(), cmp)`: Sắp xếp với cách so sánh tùy chỉnh (hàm cmp trả về bool).

> Tất cả hàm sắp xếp có độ phức tạp $O(n)$

- `reverse(v.begin(), v.end())`: Đảo ngược vector.
- `find(v.begin(), v.end(), x)`: Tìm kiếm phần tử $x$.
- `count(v.begin(), v.end(), x)`: Đếm số lượng phần tử $x$.
- `*min_element(v.begin(), v.end())`: Tìm phần tử nhỏ nhất.
- `*max_element(v.begin(), v.end())`: Tìm phần tử lớn nhất.
- `accumulate(v.begin(), v.end(), 0LL)`: Tính tổng vector.

> Tất cả các hàm trên chạy với độ phức tạp $O(n)$

Đối với vector đã sort:
- `binary_search(v.begin(), v.end(), x)`: Trả về true nếu $x$ có tồn tại trong mảng và ngược lại.
- `lower_bound(v.begin(), v.end(), x)`: Vị trí đầu tiên $>= x$.
- `upper_bound(v.begin(), v.end(), x)`: Vị trí đầu tiên $> x$.

> Tất cả các hàm trên chạy với độ phức tạp $O(log_2(n))$

Xóa toàn bộ phần tử có giá trị $x$:
```cpp
v.erase(
    remove(v.begin(), v.end(), x),
    v.end()
);
```

Sau khi sort có thể dùng hàm này để nén các phần tử trùng nhau:
```cpp
v.erase(unique(v.begin(), v.end()), v.end());
```

Độ phức tạp:
- Truy cập $a[i]$: $O(1)$
- Thêm/xóa ở cuối: $O(1)$ (trung bình).
- Thêm/xóa ở giữa hoặc đầu: $O(N)$ (tuyệt đối tránh dùng insert hay erase ở giữa vector).

*b. `deque` (Hàng đợi hai đầu)*

Cơ chế: Khác với `vector`, `deque` không lưu dữ liệu trên một dải bộ nhớ liền kề duy nhất. Nó cấp phát các "khối" (chunks) bộ nhớ cố định rời rạc và dùng một mảng con trỏ ở giữa để quản lý các khối này. Điều này cho phép mở rộng không gian lưu trữ ở cả hai đầu $O(1)$ mà không cần copy/dời toàn bộ mảng cũ.

Cách khai báo:
```cpp
deque<int> dq;              // deque rỗng
deque<int> dq(n);           // n phần tử, mặc định là 0
deque<int> dq(n, x);        // n phần tử, khởi tạo bằng x
```

*c. `stack` (Ngăn xếp)*

*d. `queue` (Hàng đợi)*

*e. `priority_queue` (Hàng đợi ưu tiên/Heap)*

*f. `set` & `multiset` (Tập hợp)*

*g. `map` (Bảng ánh xạ)*

*h. `unordered_map` & `unordered_set` (Bảng băm)*

### **4. Xử lí Bit (Bitwise Tricks)**

### **5. Kiểu dữ liệu và xử lý số lớn (Data types & Overflows)**

### **6. Cấu trúc dữ liệu mở rộng (PBDS - Policy Based Data Structures)**

### **7. Lambda & Custom Comparators**

### **8. Kỹ thuật sinh số ngẫu nhiên (Randomization)**

# **II. KỸ THUẬT VÀ TƯ DUY CƠ BẢN (BASIC TECHNIQUES)**

### **1. Mảng cộng dồn (Prefix sum)**

### **2. Mảng hiệu (Difference Array)**

### **3. Rời rạc hóa (Coordinate Compression)**

### **4. Hai con trỏ (Two Pointers)**

### **5. Cửa sổ trượt (Sliding Window)**

### **6. Cấu trúc đơn điệu (Monotonic Stack/Monotonic Queue)**

### **7. Chặt nhị phân (Mảng & Chặt nhị phân kết quả)**

### **8. Tìm kiếm tam phân (Ternary Search)**

### **9. Meet-in-the-Middle**

### **10. Vét cạn & Duyệt**

### **11. Đệ quy có nhớ & Khử đệ quy**

### **12. Thuật toán tham lam (Greedy)**

### **13. Đóng góp của từng phần tử (Contribution Technique)**

# **III: Cấu trúc dữ liệu (Data Structures)**

## **SEGMENT TREE & FENWICK TREE**

### **1. Fenwick Tree (Binary Indexed Tree - BIT)**

### **2. Segment Tree (Cây IT)**

### **3. Segment Tree - Lazy Propagation (Cập nhật đoạn)**

### **4. Segment Tree trên không gian lớn (Dynamic Segment Tree)**

### **5. Persistent Segment Tree**

### **6. IT Walk (Chặt nhị phân trên Segment Tree)**

### **7. Merge Sort Tree**

## **XỬ LÝ TIỀN TỐ & XÂU**

### **8. Cây tiền tố (Trie)**

## **CẤU TRÚC CHIA CĂN**

### **9. Chia căn (Squaroot Decomposition)**

### **10. Thuật toán Mo (Mo's Algorithm)**

## **CẤU TRÚC KHÁC**

### **11. Sparse Table (Bảng thưa)**

### **12. Disjoint Set Union (DSU - Cấu trúc tập hợp rời rạc)**
