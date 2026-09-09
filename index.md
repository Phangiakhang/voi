---
layout: default
title: "Tổng hợp kiến thức 📚"
nav_order: 1
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
{: .no_toc }

<details open markdown="block">
  <summary>
    Nhấn để đóng/mở mục lục
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

# **I. PHẦN SỐ HỌC**

### **1. Kiểm tra số nguyên tố**

*a. Thuật toán cơ bản $O(n)$*

Cách đơn giản nhất để kiểm tra tính nguyên tố của số tự nhiên là trực tiếp sử dụng định nghĩa số nguyên tố:
> Số tự nhiên $n \geq 2$ là số nguyên tố khi và chỉ khi $n$ không chia hết cho các số tự nhiên $2, 3,\dots, n-1$.

Cài đặt:
```cpp
bool prime(int n)
{
    if (n < 2) return false;
    for (int i = 2; i < n; ++i)
        if (n % i == 0)
            return false;
    return true;
}
```
*b. Thuật toán tối ưu $O(\sqrt{n})$*

Để tối ưu thuật toán trên, nhận xét rằng nếu $n$ có một ước $d$ sao cho $n-1 \geq d \geq \sqrt{n}$ thì $\dfrac {n}{d}$ cũng là ước của $n$ và có $1 < \dfrac {n}{d} \leq \sqrt{n}$. Suy ra nếu $n$ không chia hết cho các số tự nhiên lớn hơn $1$ và không vượt quá $\sqrt{n}$ thì $n$ cũng không chia hết cho các số tự nhiên lớn hơn $\sqrt{n}$. Từ đó, thay vì xét tính chia hết của $n$ cho $i = 2, 3,\dots, n-1$ ta chỉ cần xét $i = 2, 3,\dots, [\sqrt{n}]$.

Cài đặt:
```cpp
bool prime(int n)
{
    if (n < 2) return false;
    for (int i = 2; i*i <= n; ++i)
        if (n % i == 0)
            return false;
    return true;
}
```
*c. Thuật toán nâng cao*

Để ý nếu số nguyên tố $n$ lẻ thì $n$ không chia hết cho một số chẵn bất kì. Do đó nếu $n > 2$, ta chỉ cần xét các số $i$ lẻ thuộc đoạn $[2, [\sqrt{n}]]$. Tương tự, nếu $n > 3$ thì ta chỉ cần xét $i$ là các số không chia hết cho $3$. Từ hai nhận xét trên, nếu $n > 3$ thì ta chỉ cần xét các số $i$ sao cho $i$ chia $6$ dư $1$ hoặc $5$.

Cài đặt:
```cpp
bool prime(int n)
{
    if (n == 2 || n == 3)
        return true;
    if (n < 3 || n % 2 == 0 || n % 3 == 0)
        return false;
    for (int i = 5; i * i <= n; i += 6)
        if (n % i == 0 || n % (i + 2) == 0)
            return false;
    return true;
}
```
**Chú ý:** Có thể sử dụng nhiều số nguyên tố đầu tiên để tối ưu thuật toán hơn. Về lý thuyết, nếu $k$ là số số nguyên tố được dùng càng lớn thì vòng lặp chạy càng nhanh. Tuy nhiên, với $k = 50$, độ phức tạp vòng lặp ***for***  là $O(\dfrac {\sqrt{n}}{10})$. Và kể cả với $k = 6 \cdot 10^5$ thì độ phức tạp thuật của vòng lặp ***for***  vẫn là $O(\dfrac {\sqrt{n}}{30})$.

Do đó thuật toán này có thể không đủ nhanh để giải quyết giới hạn $n \leq 10^{18}$, hoặc $n \leq 10^9$ nhưng phải kiểm tra $10^6$ số $n$ trở lên. Để giải quyết các bài toán có giới hạn lớn như thế, ta phải sử dụng đến các phương pháp xác suất.

### **2. Sàng nguyên tố Eratosthenes (Sieve of Eratosthenes)**

Khi cần tìm ra các số nguyên tố từ $1$ đến $n$, ta có thể duyệt từng số và kiểm tra tính nguyên tố của nó. Và ý tưởng đó cho ta một thuật toán $O(\sqrt{n})$.

> Tuy nhiên, một nhà toán học cổ Hy Lạp tên là Eratosthenes đã "phát minh" ra một "thuật toán" hiệu quả hơn. Ban đầu, Eratosthenes đã lấy lá cọ và ghi tất cả các số từ 2 cho đến 100. Sau đó, ông đã chọc thủng các hợp số và giữ nguyên các số nguyên tố. Bảng số nguyên tố còn lại trông rất giống một cái sàng. Cho đến ngày nay, "thuật toán" này được phổ biến rộng rãi với cái tên **sàng nguyên tố Eratosthenes.**

- Ban đầu, ta cho tất cả số từ $2$ đến $n$ vào sàng và đánh dấu tất cả các số. (Các số không được đánh dấu sau cùng sẽ bị loại khỏi sàng).
- Duyệt lần lượt các số từ $2$ đến $n$. Nếu số đang xét:
    - Đã được đánh dấu $\Rightarrow$ số nguyên tố: ta bỏ đánh dấu tất cả các bội (khác chính nó) của số nguyên tố này để loại các bội ấy ra khỏi sàng.
    - Không được đánh dấu $\Rightarrow$ hợp số: ta bỏ qua số này.
- Sau khi duyệt xong các số còn lại trong sàng, hay nói cách khác các số được đánh dấu là số nguyên tố.

Cài đặt:
```cpp
const int maxn = 1e6 + 7;
vector <bool> prime(maxn, true);
void sieve(int n)
{
    prime[0] = prime[1] = false;
    for (int i = 2 ; i <= n ; ++i) {
        if (prime[i]) {
            for (int j = i*2 ; j <= n ; j += i)
                prime[j] = false;
        }
    }
}
```
**Độ phức tạp thời gian: $O(n \log(\log n))$**

**Độ phức tạp không gian: $O(n)$**

> **Nhận xét**  
> Xét $X = k \cdot p$ là bội của số nguyên tố $p$.  
> Nếu như $p < X < p^2$, ta có $1 < k < p$. Ta suy ra $k$ phải có một ước nguyên tố nhỏ hơn $p$.  
> Vì thế, $X = k \cdot p$ đã bị sàng loại đi trong các vòng lặp trước đó và ta **chỉ cần xét $X \geq p^2$.**

Dựa vào *nhận xét* trên, ta có cải tiến như sau:
```cpp
const int maxn = 1e6 + 7;
vector <bool> prime(maxn, true);
void sieve(int n)
{
    prime[0] = prime[1] = false;
    for (int i = 2 ; i * i <= n ; ++i) {
        if (prime[i]) {
            for (int j = i*i ; j <= n ; j += i)
                prime[j] = false;
        }
    }
}
```
Dưới đây là hình minh họa cho cải tiến trên. *Nguồn: [Wikipedia](https://vi.wikipedia.org/wiki/S%C3%A0ng_Eratosthenes)*

![Sàng Eratosthenes](https://upload.wikimedia.org/wikipedia/commons/b/b8/Animation_Sieb_des_Eratosthenes_%28vi%29.gif)

### **3. Phân tích thừa số nguyên tố**

### **4. Phi hàm Euler**

### **5. Nghịch đảo modulo**

### **6. Sàng nguyên tố $10^{12}$**

### **7. Thuật toán Rabin - Miller kiểm tra số nguyên tố**

### **8. Bao hàm loại trừ - Inclusion Exclusion**

# **II. STANDARD TEMPLATE LIBRARY (STL)**

Thư viện STL gồm 4 phần chính:
- Iterators: con trỏ lặp
- Containers: các cấu trúc dữ liệu lưu trữ
- Algorithms: các thuật toán
- Functions: các hàm
  
## **1. ITERATOR**

Iterator (Con trỏ lặp) là một con tror dùng để trỏ tới các phần tử của các Container (CTDL lưu trữ) trong thư viện STL. Mỗi container có một iterator của nó.

Iterator thường dùng để duyệt (lặp) qua các phần tử của container bằng cách sử dụng phép so sánh và các phép toán trên con trỏ với iterator.

Vì Iterator là con trỏ nên có thể sử dụng các phép toán và thao tác tương tự như một con trỏ.

- Phép so sánh: bằng (==) hoặc khác (!=)  
- Phép gán: =  
- Phép toán: cộng (+) hoặc trừ (-) với các số nguyên  
- Phép lấy giá trị: *

Khai báo Iterator:
```cpp
// Khai báo 
<Kiểu_container>::iterator it;
// Trỏ đến phần tử đầu tiên của container
it = <tên_container>.begin();
// Trỏ đến vị trí kết thúc của container (vị trí ngay sau phần tử cuối cùng)
it = <tên_container>.end();
// Duyệt qua container
for (it = <tên_container>.begin() ; it != <tên_container>.end() ; ++it) {
    // Code
}
```
Tất cả các hàm iterator này đều có độ phức tạp là $O(1)$.
> Các đối tượng của thư viện STL thuộc các lớp (Class) trong **namespace std**. Do đó, để thuận tiện khi lập trình, chúng ta nên thêm dòng **using namespace std;** vào đầu chương trình

## **2. CONTAINER**

Một container là một đối tượng cụ thể lưu trữ một tập hợp các đối tượng khác (các phần tử của nó). Nó được thực hiện như các lớp mẫu (class templates).

Container quản lý không gian lưu trữ cho các phần tử của nó và cung cấp một hàm thành viên (member function) để truy cập tới chúng, hoặc trực tiếp thông qua các biến lặp (iterator).

Container có các cấu trúc thường sử dụng trong lập trình như: mảng động - dynamic arrays (vector), hàng đợi - queues (queue), hàng đợi ưu tiên - heaps (priority queue), danh sách liên kết - linked list (list), cây - tree (set), mảng ánh xạ - associative arrays (map), $\dots$

Nhiều container chưa một số hàm thành viên giống nhau. Quyết định sử dụng container nào cho nhu cầu cụ thể nói chung không chỉ phụ thuộc vào các hàm được cung cấp mà còn dựa vào độ hiệu quả của các hàm thành viên của nó (độ phức tạp của các hàm). Điều này đặc biệt đúng với container dãy (sequence containers), mà trong đó có sự khác nhau về độ phức tạp đối với các thao tác chèn/xóa phần tử hay truy cập phần tử.

Sau đây là các container thường dùng nhất.

### **2.1 Vector (Mảng động)**

### **2.2 List (Danh sách liên kết)**

### **2.3 Stack (Ngăn xếp)**

### **2.4 Queue (Hàng đợi)**

### **2.5 Priority queue (Hàng đợi ưu tiên)**

### **2.6 Set (Tập hợp)**

### **2.7 Multiset (Tập hợp trùng lặp)**

### **2.8 Map (Ánh xạ)**

## **3. ALGORITHMS (THUẬT TOÁN)**

# **III. CẤU TRÚC DỮ LIỆU VÀ GIẢI THUẬT**

### **1. Cây phân đoạn - Segment tree**

### **2. Cây chỉ số nhị phân - Fenwick tree**

### **3. Thuật toán tham lam**

### **4. Đệ quy - Vét cạn (Backtracking)**

### **5. Tổng tiền tố - Suffix Array**

*a. Tổng tiền tố mảng 1 chiều - 1D Suffix Array*

Cài đặt tiền xử lí:
```cpp
vector <int> p(n+1);
for (int i = 1 ; i <= n ; ++i)
    p[i] = p[i-1]+a[i];
```

Một số ứng dụng:

Tính nhanh tổng đoạn $[a_{i}, a_{i+1},\dots,a_{j}]$: $\boxed{p[j] - p[i-1]}$

*b. Tổng tiền tố mảng 2 chiều - 2D Suffix Array*

### **6. Cửa sổ trượt - Sliding Window**

### **7. Kỹ thuật 2 con trỏ - Two-pointer technique**

# **IV. CHẶT NHỊ PHÂN**

### **1. Một số hàm liên quan**

### **2. Chặt nhị phân trên miền thực**

# **V. QUY HOẠCH ĐỘNG - DYNAMIC PROGRAMMING (DP)**

### **1. Dãy con tăng dài nhất - Longest Increasing Subsequence (LIS)**

*a. Thuật cơ bản $O(n^2)$*

Cài đặt:
```cpp
vector <int> dp(n+1, 1);
for (int i = 1 ; i <= n ; ++i) {
    for (int j = 1 ; j < i ; ++j)
        dp[i] = max(dp[i], dp[i-1] + 1);
}
```

*b. Thuật tối ưu $O(n \log n)$*

Cài đặt:
```cpp
int n; cin >> n;
vector <int> a(n+1), b(n+1);
int ma;
for (int i = 1 ; i <= n ; ++i) cin >> a[i];
for (int i = 1 ; i <= n ; ++i) {
    int j = lower_bound(b.begin()+1, b.begin()+ma+1, a[i]) - b - 1;
    b[j+1] = a[i];
    ma = max(ma, j+1);
}
cout << ma;
```

### **2. Vali B - Knapsack 01 (Balo 01)**

*a. Tìm giá trị lớn nhất*

Cài đặt: 
```cpp
vector <vector<int>> dp(n+1, vector<int>(S+1, 0))
for (int i = 1 ; i <= n ; ++i) {
    for (int j = 1 ; j <= S ; ++j) {
        dp[i][j] = dp[i-1][j];
        if (j >= w[i])
            dp[i][j] = max(dp[i][j], dp[i-1][j-w[i]] + v[i]);
        }
}
cout << dp[n][S] << "\n";
```

*b. Truy vết*

Cài đặt:
```cpp
set <int> s;
while (n>0)
{
        if (dp[n][S]-dp[n-1][S] != 0) {
                s.insert(n);
                S -= w[n];
        }
        n--;    
}
for (auto x:s) cout << w[x] << " " << v[x] << "\n";
```

### **3. Bài toán biến đổi xâu**

Cài đặt:
```cpp
string s1,s2; cin >> s1 >> s2;
int n = s1.size(); 
int m = s2.size();
vector <vector <int>> dp(n+1, vector<int>(m+1, 0));
s1 = " " + s1;
s2 = " " + s2;
for (int i = 0 ; i <= n ; ++i)
    for (int j = 0 ; j <= m ; ++j)
    {
        if (i == 0)
            dp[0][j] = j;
        else if (j == 0)
            dp[i][0] = i;
        else if (s1[i] == s2[j])
            dp[i][j] = dp[i-1][j-1];
        else dp[i][j] = min({
            dp[i-1][j-1],
            dp[i-1][j],
            dp[i][j-1]
        }) +1;
    }
cout << dp[n][m] << "\n";
```

### **4. Dãy con chung dài nhất - Longest Common Subsequence (LCS)**

Cài đặt:
```cpp
string s1,s2; cin >> s1 >> s2;
int n = s1.size(); 
int m = s2.size();
vector <vector <int>> dp(n+1, vector<int>(m+1, 0));
s1 = " " + s1;
s2 = " " + s2;
string res = "";
for (int i = 0 ; i <= n ; ++i) {
    for (int j = 0 ; j <= m ; ++j)
    {
        if (i == 0 || j == 0)
            dp[i][j] = 0;
        else if (s1[i] == s2[j])
            dp[i][j] = dp[i-1][j-1] + 1;
        else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
    }
}
int i = n, j = m;
while (i > 0 && j > 0)
{
    if (dp[i][j] == dp[i-1][j-1] + 1 && s1[i] == s2[j]) {
        res = s1[i] + res;
        --i; --j;
    }
    else if (dp[i][j] == dp[i-1][j]) --i;
    else if (dp[i][j] == dp[i][j-1]) --j;
}
cout << res;
```

### **5. Vali A - Knapsack**

Cài đặt:
```cpp
vector<vector<int>> dp(n + 1, vector<int>(S + 1, 0));
for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= S; ++j) {
        dp[i][j] = dp[i - 1][j];
        if (j >= w[i])
            dp[i][j] = max(dp[i][j], dp[i][j - w[i]] + v[i]);
    }
}
cout << dp[n][S] << "\n";
```

### **6. Nhân ma trận**

Template chuẩn:
```cpp
const int MATRIX_SIZE = 100;
struct Matrix {
    int m, n;
    ll d[MATRIX_SIZE][MATRIX_SIZE];

    Matrix(int _m = 0, int _n = 0) {
        m = _m; n = _n;
        memset(d, 0, sizeof d);
    }

    Matrix operator + (const Matrix &a) const {
        Matrix res(m, n);
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            res.d[i][j] = d[i][j] + a.d[i][j];
            if (res.d[i][j] >= MOD) res.d[i][j] -= MOD;
        }
        return res;
    }

    Matrix operator * (const Matrix &a) const {
        int x = m, y = n, z = a.n;
        Matrix res(x, z);
        for (int i = 0; i < x; i++) for (int j = 0; j < y; j++)
        for (int k = 0; k < z; k++) {
            res.d[i][k] += 1LL * d[i][j] * a.d[j][k];
            if (res.d[i][k] >= 1LL * MOD * MOD) res.d[i][k] -= 1LL * MOD * MOD;
        }
        for (int i = 0; i < x; i++) for (int k = 0; k < z; k++) res.d[i][k] %= MOD;

        return res;
    }

    Matrix operator ^ (ll k) const {
        Matrix res(n, n);
        for (int i = 0; i < n; i++) res.d[i][i] = 1;
        Matrix mul = *this;
        while (k > 0) {
            if (k & 1) res = res * mul;
            mul = mul * mul;
            k >>= 1;
        }
        return res;
    }

    ll* operator[] (int i) {
        return d[i];
    }

    const ll* operator[] (int i) const {
        return d[i];
    }
};
```

# **VI. ĐỒ THỊ**

### **1. Tìm kiếm theo chiều sâu - Depth-First Search (DFS)**

Thuật toán tìm kiếm theo chiều sâu (thường gọi là DFS) là một thuật toán duyệt khởi đầu tại đỉnh gốc và đi xa nhất có thể theo một nhánh. Tiến trình thực hiện dfs có thể thực hiện như sau:
- Xuất phát từ đỉnh gốc
- Đánh dấu đỉnh đang xét hiện tại
- Chọn một đỉnh kề với đỉnh đang xét mà chưa bị đánh dấu. Tiếp tục xét đỉnh kề đó
- Nếu như không có đỉnh kề thoả mãn, quay trở lại xét đỉnh trước khi xét đỉnh hiện tại
- Quá trình kết thúc khi không còn đỉnh để xét

Để cài đặt thuật toán dfs, thông thường người ta sẽ sử dụng thuật đệ quy.

Cài đặt:
```cpp
vector <bool> vis(maxn, false);
void dfs(int u)
{
    cout << u << " ";
    vis[u] = true;
    for (auto v : adj[u]) {
        if (!vis[v]) 
            dfs(v);
    }
}
```

**Độ phức tạp thời gian của thuật toán là $O(n + m)$ với n là số đỉnh và m là số cạnh**

### **2. Tìm kiếm theo chiều rộng - Breadth-First Search (BFS)**

Thuật toán tìm kiếm theo chiều rộng (thường gọi là BFS) là một thuật toán duyệt bắt đầu từ gốc, sau đó lần lượt xét qua các đỉnh của một cây theo ưu tiên về độ sâu từ nhỏ đến lớn.
> Ở đây có một khái niệm mới là “độ sâu”. Độ sâu của một đỉnh được định nghĩa là số cạnh trên đường đi từ đỉnh đang xét đến đỉnh gốc.

Hình ảnh minh họa. *Nguồn: [howkteam.vn](https://f.howkteam.vn/Upload/cke/images/2_IMAGE%20TUTORIAL/8_CTDL%26GT/Bai19/1_BFS%20v%C3%A0%20DFS_Howkteam_vn.png)*

![Độ sâu của đỉnh](https://f.howkteam.vn/Upload/cke/images/2_IMAGE%20TUTORIAL/8_CTDL%26GT/Bai19/1_BFS%20v%C3%A0%20DFS_Howkteam_vn.png)

Trước hết, chúng ta sẽ cần tìm cách để thể hiện một cạnh của cây. Cách được dùng phổ biến nhất là dùng danh sách kề, tức là mỗi node sẽ có một danh sách lưu lại tất cả các node kề (node có cạnh trực tiếp) với node đó. Trong C++, các bạn có thể dùng vector để lưu danh sách kề. Để biết cụ thể cách làm, hãy đọc phần code nhé.

Tiếp theo sẽ là cách cái đặt thuật toán BFS . Tư tưởng của BFS đơn giản là như sau:

- Khởi đầu, thêm node gốc vào hàng đợi
- Lấy node đầu tiên trong hàng đợi
- Đánh dấu node đã đi qua. Đây là một chi tiết quan trọng do một cạnh trên cây đang xét là không có hướng nên nếu không đánh dấu thì một node sẽ được xét nhiều lần
- Loại bỏ node đang xét khỏi hàng đợi
- Thêm các node kề chưa được đánh dấu với node đang xét vào cuối hàng đợi
- Quá trình kết thúc khi hàng đợi rỗng

Cài đặt:
```cpp
vector <bool> vis(maxn, false);
void bfs(int s)
{
    queue <int> q;
    q.push(s);
    vis[s] = true;

    while (!q.empty()) {
        int u = q.front();
        q.pop()

        for (auto v : adj[u]) {
            if (!vis[v]) {
                q.push(v);
                vis[v];
            }
        }
    }
}
``` 

**Độ phức tạp thời gian của thuật toán là $O(n + m)$ với n là số đỉnh và m là số cạnh**

### **3. Đếm số thành phần liên thông của đồ thị vô hướng**

> **Khái niệm thành phần liên thông của đồ thị vô hướng:** Trong lý thuyết đồ thị, một thành phần liên thông của một đồ thị vô hướng là một đồ thị con trong đó giữa bất kì hai đỉnh nào đều có đường đi đến nhau, và không thể nhận thêm bất kì một đỉnh nào mà vẫn duy trì tính chất trên. Một đồ thị liên thông có đúng một thành phần liên thông, chính là toàn bộ đồ thị.

Các đỉnh được thăm sau khi thực hiện quá trình dfs hoặc bfs đều thuộc 1 thành phần liên thông (kể cả đỉnh gốc).

Vậy chúng ta sẽ duyệt qua tất cả các đỉnh của đồ thị để đếm xem có bao nhiêu thành phần liên thông. Nếu gặp 1 đỉnh chưa được thăm (tức là nó không liên thông với các thành phần liên thông đã xét trước đó) thì tăng số lượng thành phần liên thông đang đếm lên 1 và tiến hành bfs (hoặc dfs) để đánh dấu tất cả các đỉnh liên thông với đỉnh đó.

Cài đặt:
```cpp
int cnt = 0;
for (int i = 1 ; i <= n ; ++i) {
    if (!vis[i]) {
        bfs(i); // hoặc dfs(i)
        ++cnt;
    }
}
cout << cnt << "\n";
```

> Tuy thực hiện nhiều lần bfs (hoặc dfs) nhưng tổng số đỉnh được xét vẫn là n và số cạnh phải đi qua là m nên độ phức tạp của thuật toán vẫn là $O(n + m)$.

### **4. Tìm đường đi trên đồ thị không trọng số**

*a. Tìm đường đi ngắn nhất - BFS*

Cài đặt:
```cpp
// Code
```

*b. Tìm đường đi dài nhất - DFS*

Cài đặt:
```cpp
// Code
```

### **5. Thuật toán loang (Flood Fill)**

*a. Cài đặt DFS*

Cài đặt:
```cpp
vector <int> dir4 = { {0,1}, {0,-1}, {1,0}, {-1,0} } // Loang 4 hướng
vector <int> dir8 = { {-1,-1}, {-1,0}, {-1,1}, {0,-1}, 
                      {0,1}, {1,-1}, {1,0}, {1,1} } // Loang 8 hướng

void dfs(int i, int j)
{
    vis[i][j] = true;
    for (auto it : dir4) {
        int u = i + it.fi;
        int v = j + it.se;

        if (u < 1 || v < 1 || u > n || v > n) continue;

        if (!vis[u][v]) {
            dfs(u,v);
        }
    }
}
```

*b. Cài đặt BFS*

Cài đặt:
```cpp
vector <int> dir4 = { {0,1}, {0,-1}, {1,0}, {-1,0} } // Loang 4 hướng
vector <int> dir8 = { {-1,-1}, {-1,0}, {-1,1}, {0,-1}, 
                      {0,1}, {1,-1}, {1,0}, {1,1} } // Loang 8 hướng
void bfs(int i, int j)
{
    queue <pair<int,int>> q;
    q.push({i,j});
    vis[i][j] = true;
    while (!q.empty()) {
        int u = q.front().first;
        int v = q.front().second;
        q.pop();

        for (auto it : dir4) { // for(auto it : dir8)
            int x = u + it.first;
            int y = v + it.second;

            if (x < 1 || y < 1 || x > n || y > m) continue;
            if (!vis[x][y]) {
                vis[x][y] = true;
                q.push({x,y});
            }
        }
    }
}
```

### **6. Tìm đường đi ngắn nhất trên đồ thị có trọng số 0 hoặc 1 (01BFS)**

Cài đặt:
```cpp
void bfs(int k)
{
    deque<int> q;
    q.push_back(k);
    vis[k] = true;

    while (!q.empty()) {
        int x = q.front();
        q.pop_front();

        for (auto [v, w] : adj[x]) {
            if (!vis[v]) {
                if (w == 1)
                    q.push_back(v);
                else
                    q.push_front(v);
                vis[v] = true;
                vet[v] = x;
            }
        }
    }
}
```

### **7. Tìm đường đi ngắn nhất trên ma trận**

Sử dụng quy hoạch động di chuyển

### **8. Sắp xếp Topo - Thuật toán Kahn**

*a. Sắp xếp topo dựa trên DFS*

Cài đặt:
```cpp
vector<int> topo;

void dfs(int u) {
    vis[u] = true;
    for (int v : adj[u]) {
        if (!vis[v]) dfs(v);
    }
    topo.push_back(u); 
}

void topo_sort(int n) {
    for (int i = 1; i <= n; ++i) {
        if (!vis[i]) dfs(i);
    }

    reverse(topo.begin(), topo.end());
}
```

*b. Thuật toán Kahn - Xóa dần đỉnh*

Cài đặt:
```cpp
void kahn()
{
    set <int> s; // Nếu không yêu cầu sắp xếp theo stt đỉnh thì sài queue
    for (int i = n ; i > 0 ; --i) {
        if (!in[i])
            s.insert(i);
    }

    while (!s.empty()) {
        int x = *s.begin();
        s.erase(s.begin());
        res.pb(x);

        for (int it : adj[x]) {
            --in[it];
            if (!in[it]) {
                s.insert(it);
            }
        }
    }
}
```

### **9. Kiểm tra chu trình của đồ thị**

*a. Đồ thị vô hướng*

Cài đặt:
```cpp
// Code
```

*b. Đồ thị có hướng*

Cài đặt:
```cpp
// Code
```

### **10. Thuật toán Kosaraju - Đếm số thành phần liên thông mạnh trên đồ thị có hướng**

Cài đặt:
```cpp
// Code
```

### **11. Disjoint Set Union - DSU**

Cài đặt:
```cpp
vector <int> par(maxn);
vector <int> sz(maxn);

void init(int n) {
    for (int i = 1 ; i <= n ; ++i) {
        par[i] = i;
        sz[i] = 1;
    }
}

int find(int u) {
    if (par[u] == u) return u;
    return par[u] = find(par[u]);
}

bool unite(int u, int v) {
    u = find(u);
    v = find(v);
    if (u == v) return false;
    if (sz[u] > sz[v]) swap(u,v);
    par[u] = v;
    sz[v] += sz[u];
    return true;
}
```

### **12. Thuật toán Kruskal tìm cây khung nhỏ nhất (Minimum Spanning Tree)**

Cài đặt (Có truy vết):
```cpp
struct edge {
    int u,v,w;
};

void kruskal()
{
    vector <edge> mst;
    int d = 0;

    for (int i = 0 ; i < m ; ++i) {
        if (mst.size() >= n-1) break;

        edge e = edges[i];

        if (unite(e.u, e.v)) {
            mst.pb(e);
            d += e.w;
        }
    }

    cout << d << endl;
    for (edge x : mst) {
        cout << x.u << " " << x.v << " " << x.w << endl;
    }
}
```

### **13. Thuật toán Prim tìm cây khung nhỏ nhất (Minimum Spanning Tree)**

Cài đặt:
```cpp
void prim(int s)
{
    priority_queue <ii, vii, greater<ii>> pq;
    pq.push({0, s});

    while (!pq.empty()) {
        ii top = pq.top();
        pq.pop();
        int u = top.se;
        int len = top.fi;
        if (vis[u]) continue;
        else {
            d += len;
            vis[u] = true;
        }

        for (auto [v,w] : adj[u]) {
            pq.push({w,v});
        }

    }
    cout << d;
}
```

Truy vết:
```cpp
void prim(int s)
{
    priority_queue <ii, vii, greater<ii>> pq;
    pq.push({0, s});
    vector <edge> MST;
    d[s] = 0;

    while (!pq.empty()) {
        ii top = pq.top();
        pq.pop();
        int u = top.se;
        int len = top.fi;

        if (vis[u]) continue;

        res += len;
        vis[u] = true;
        if (u != s) {
            MST.pb({u, p[u], len});
        }

        for (auto [v,w] : adj[u]) {
            if (!vis[v] && w < d[v]) {
                pq.push({w,v});
                d[v] = w;
                p[v] = u;
            }
        }
    }
    cout << res << endl;
    for (edge edges : MST) 
        cout << edges.node << " " << edges.parent << " " << edges.w << endl;
}
```

### **14. Thuật toán Tarjan - Bài toán đỉnh trụ (khớp) và cạnh (cầu)**

Cài đặt:
```cpp
// Code
```

### **15. Thuật toán Dijkstra - Tìm đường đi ngắn nhất trên đồ thị có trọng số không âm**

Cài đặt:
```cpp
const int INF = numeric_limits<int>::max();
vector <int> dp(maxn, INF);
void dijkstra(int s)
{
    dp[s] = 0;
    priority_queue <ii, vii, greater<ii>> pq;
    pq.push({0, s});

    while (!pq.empty()) {
        ii top = pq.top();
        int u = top.se;
        ll len = top.fi;

        if (len > dp[u]) continue;
        for (auto it : adj[u]) {
            int v = it.fi;
            ll w = it.se;

            if (dp[v] > dp[u] + w) {
                dp[v] = dp[u] + w;
                pq.push({dp[v], v});
            }
        }
    }
}
```

### **16. Chu trình Euler và đường đi Euler**

Cài đặt:
```cpp
vector <set<int>> adj(maxn);

void euler(int s)
{
    stack <int> st;
    vi Euler;
    st.push(s);

    while (!st.empty()) {
        int u = st.top();

        if (!adj[u].empty()) {
            int v = *adj[u].begin();
            st.push(v);

            adj[u].erase(v);
            adj[v].erase(u);
        }
        else {
            st.pop();
            Euler.pb(u);
        }
    }
    if (Euler.size() < n) cout << "IMPOSSIBLE";
    else for (int &x : Euler) cout << x << " ";
}
```

### **17. Chu trình Hamilton**

Cài đặt:
```cpp
// Code
```