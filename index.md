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

*a. $vector$ (Mảng động)*

Cơ chế: Lưu trữ dữ liệu trên một khối bộ nhớ liền kề trong RAM. Khi mảng đầy, nó tự động cấp phát một mảng mới có kích thước gấp đôi (hoặc gấp rưỡi), copy dữ liệu cũ sang và xóa mảng cũ.

Cách khai báo:
```cpp
vector<int> a;                              // vector rỗng
vector<int> a(n);                           // n phần tử, mặc định là 0
vector<int> a(n, x);                        // n phần tử, khởi tạo bằng x
vector<vector<int>> adj(n, vector<int>(m))  // Mảng n * m phần tử
```

Các hàm đi kèm & Cách sử dụng:
> Cú pháp <tên_vector>.<tên_hàm>(<tham_số>)
- $push_back(x) / emplace_back(x)$: Thêm phần tử $x$ vào cuối.
- $pop_back()$: Xóa phần tử cuối.
-  $resize(n)$: Thay đổi kích thước vector thành n.
-  $size()$: Lấy kích thước của vector.
-  $empty()$: Kiểm tra vector có rỗng hay không.
-  $clear()$: Xóa toàn bộ vector.
-  $at(i)$: Truy cập phần tử thứ i, trả về $std::out_of_range$ nếu vượt giới hạn của vector.
-  $front()$: Truy cập phần tử đầu.
-  $back()$: Truy cập phần tử cuối.
-  $data()$: Trả về con trỏ tới vùng nhớ chứa dữ liệu.
-  $max_size()$: Số phần tử tối đa mà vector có thể chứa theo giới hạn implementation/allocator.
-  $reverse(n)$: Khai báo trước sức chứa $n$ phần tử (tránh chi phí mở rộng vector nhiều lần).
-  $capacity()$: Dung lượng bộ nhớ hiện đang được cấp phát.
> Phân biệt cực kỳ quan trọng:
> size     = số phần tử đang tồn tại
> capacity = số phần tử có thể chứa trước khi cần cấp phát lại
- $begin()$: Iterator tới phần tử đầu.
- $end()$: Iterator tới phần tử cuối.
- $rbegin()$: Reverse iterator bắt đầu từ phần tử cuối.
- $rend()$: Reverse iterator nằm trước phần tử đầu.
- $erase(v.begin() + i)$: Xóa phần tử thứ $i$.
- $erase(v.begin() + l, v.begin() + r)$: Xóa đoạn $[l,r)$.
- $insert(v.begin() + i, x)$: Chèn phần tử $x$ vào vị trí $i$.
- $insert(v.begin() + i, n, x)$: Chèn $n$ phần tử $x$ vào vị trí $i$.
- $a.insert(a.begin() + i, b.begin() + l, b.begin() + r)$: Chèn đoạn $[l,r)$ của vetor b vào vị trí $i$ cuả vector a.
- $a.swap(b)$: Hoán đổi nội dung 2 vector (có thể khác kích thước) với độ phức tạp $~O(1)$.

Các hàm không đi kèm nhưng cực kỳ hữu dụng:
> Cú pháp <tên_hàm>(<tham_số>)
- $sort(v.begin(), v.end())$: Sắp xếp tắng dần.
- $sort(v.begin(), v.end(), greater<int>())$: Sắp xếp giảm dần.
- $sort(v.begin(), v.end(), cmp)$: Sắp xếp với cách so sánh tùy chỉnh (hàm cmp trả về bool).
> Tất cả hàm sắp xếp có độ phức tạp $O(n)$
- $reverse(v.begin(), v.end())$: Đảo ngược vector.
- $find(v.begin(), v.end(), x)$: Tìm kiếm phần tử $x$.
- $count(v.begin(), v.end(), x)$: Đếm số lượng phần tử $x$.
- $*min_element(v.begin(), v.end())$: Tìm phần tử nhỏ nhất.
- $*max_element(v.begin(), v.end())$: Tìm phần tử lớn nhất.
- $accumulate(v.begin(), v.end(), 0LL)$: Tính tổng vector.
> Tất cả các hàm trên chạy với độ phức tạp $O(n)$

Đối với vector đã sort:
- $binary_search(v.begin(), v.end(), x)$: Trả về true nếu $x$ có tồn tại trong mảng và ngược lại.
- $lower_bound(v.begin(), v.end(), x)$: Vị trí đầu tiên $>= x$.
- $upper_bound(v.begin(), v.end(), x)$: Vị trí đầu tiên $> x$.
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

### **4. Xử lí Bit (Bitwise Tricks)**
