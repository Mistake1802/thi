# KHUÔN MẪU LÀM ĐỀ THI CTDL (Cấu trúc dữ liệu) — Rút ra từ 5 đề đã giải

## 0. Cấu trúc đề chung
- Luôn 2 câu, mỗi câu **5 điểm**, chia 5 ý **a) → e)**, mỗi ý ~1 điểm.
- **Câu 1: luôn về CÂY** (chỉ đổi "lớp áo": sách, hành tinh, thư mục, động vật...)
- **Câu 2: luôn về ĐỒ THỊ** (chỉ đổi cách lưu: danh sách kề / ma trận kề, và hướng: vô hướng / có hướng)
- Ý a) = khai báo struct. Ý b), c), d) = 3 hàm xử lý. Ý e) = `main()` gọi lại các hàm với dữ liệu/tình huống cụ thể trong đề.

**Chiến thuật ôn thi:** học thuộc *khung 3 hàm* dưới đây cho từng dạng cây/đồ thị, khi đi thi chỉ cần đổi tên trường dữ liệu (`pages` → `distance` → `weight`...) là dùng lại được ngay.

---

## 1. CÂU 1 — Dạng cây, có 2 kiểu hay gặp

### Dạng A: Cây tìm kiếm nhị phân (BST)
**Dấu hiệu nhận biết:** đề nói *"cây tìm kiếm nhị phân"*, có câu **"khóa là ..."** (số trang, khoảng cách, số nguyên...).

```c
typedef struct Data { char name[100]; int key; } Data;   // key = trường dùng để so sánh
typedef struct Node {
    Data data;
    struct Node *left, *right;
} Node;

// b) THÊM (insert theo khóa, giống BST chuẩn)
Node* insert(Node *root, char *name, int key) {
    if (!root) { /* tạo node mới */ return root; }
    if (key < root->data.key) root->left = insert(root->left, name, key);
    else if (key > root->data.key) root->right = insert(root->right, name, key);
    return root;
}

// c) LIỆT KÊ theo điều kiện trên khóa (duyệt inorder)
void listByCondition(Node *root) {
    if (!root) return;
    listByCondition(root->left);
    if (root->data.key < A || root->data.key > B) printf(...);
    listByCondition(root->right);
}

// d) KIỂM TRA TỒN TẠI THEO TÊN — LƯU Ý: khóa cây là "key" số, không phải "tên"
//    => KHÔNG dùng được thuật toán tìm kiếm BST bình thường, PHẢI duyệt toàn cây
int existsByName(Node *root, char *name) {
    if (!root) return 0;
    if (strcmp(root->data.name, name) == 0) return 1;
    return existsByName(root->left, name) || existsByName(root->right, name);
}
```
> Bẫy thường gặp: sinh viên hay quên là ý d) tìm theo **tên** chứ không theo **khóa**, nên không thể "rẽ trái/phải" như tìm kiếm BST — phải duyệt hết cây.

### Dạng B: Cây tổng quát (mỗi node có nhiều con, không có khóa sắp xếp)
**Dấu hiệu nhận biết:** đề nói *"cây tổng quát"*, có hình vẽ cây phân cấp (thư mục, phân loại động vật...), không có khái niệm "khóa".

```c
typedef struct Node {
    char name[100];
    int value;                 // 0 nếu là "nhóm/thư mục", >0 nếu là phần tử thật
    struct Node *firstChild, *nextSibling;
} Node;

// Hàm tìm node theo tên — DÙNG CHUNG cho cả câu b) và c)
Node* findNode(Node *root, char *name) {
    if (!root) return NULL;
    if (strcmp(root->name, name) == 0) return root;
    Node *f = findNode(root->firstChild, name);
    if (f) return f;
    return findNode(root->nextSibling, name);
}

// b) THÊM con vào node cha tên x
int addChild(Node *root, char *parentName, char *name, int value) {
    Node *p = findNode(root, parentName);
    if (!p) return 0;
    // gắn node mới vào cuối danh sách con của p
}

// c) KIỂM TRA TỒN TẠI
int exists(Node *root, char *name) { return findNode(root, name) != NULL; }

// d) LIỆT KÊ theo điều kiện (duyệt toàn cây, thường phải LOẠI các node "nhóm" có value=0)
void listByCondition(Node *root) {
    if (!root) return;
    if (root->value > 0 && /* điều kiện */) printf(...);
    listByCondition(root->firstChild);
    listByCondition(root->nextSibling);
}
```
> Bẫy thường gặp: các node "trung gian" (thư mục, nhóm phân loại) thường có `value = 0` — nếu không loại ra, chúng sẽ lọt vào kết quả liệt kê sai.

---

## 2. CÂU 2 — Đồ thị, 3 hàm gần như bất biến

Dù đề cho **danh sách kề** hay **ma trận kề**, vô hướng hay có hướng, 3 hàm b), c), d) luôn làm đúng 3 việc:

| Ý | Việc cần làm | Thuật toán |
|---|---|---|
| b) | Đỉnh k nối trực tiếp với ai | Đọc trực tiếp danh sách kề / dòng ma trận |
| c) | Từ x đến y qua ít nhất mấy đỉnh trung gian, đỉnh nào | **BFS** tìm đường đi ngắn nhất, số trung gian = số đỉnh trên đường − 2 |
| d) | Đồ thị có mấy cụm/thành phần liên thông, gồm đỉnh nào | **BFS/DFS** từ mọi đỉnh chưa thăm, mỗi lần lặp = 1 cụm |

```c
// c) BFS lấy đường đi ngắn nhất x -> y, trả về mảng path[] và độ dài
int bfsPath(Graph *g, int x, int y, int path[]) {
    int prev[MAXN], visited[MAXN] = {0}, queue[MAXN], front=0, rear=0;
    queue[rear++] = x; visited[x] = 1;
    while (front < rear) {
        int u = queue[front++];
        for (int v = 0; v < g->n; v++)
            if (ke(u,v) && !visited[v]) { visited[v]=1; prev[v]=u; queue[rear++]=v; }
    }
    if (!visited[y]) return -1;
    // truy ngược prev[] để dựng lại path[]
}

// d) Đếm thành phần liên thông
void connectedComponents(Graph *g) {
    int visited[MAXN] = {0}, comp = 0;
    for (int s = 0; s < g->n; s++) {
        if (visited[s]) continue;
        comp++;
        // BFS/DFS từ s, in ra tất cả đỉnh thăm được -> đó là 1 cụm
    }
}
```

**3 biến thể của Câu 2 hay gặp:**
1. **Vô hướng + danh sách kề** (Đề 01, 02, 04): `adj[i][j]` set cả 2 chiều khi thêm cạnh.
2. **Vô hướng + ma trận kề** (Đề 03): input là ma trận n×n có sẵn, chỉ cần đọc vào, thuật toán y hệt bản 1.
3. **Có hướng** (đề HK1 2023-2024, quan hệ "biết nhà"): `adj[u][v]` chỉ set 1 chiều; **không có khái niệm "cụm liên thông"** (vì bất đối xứng), thay vào đó thường hỏi "x có thể biết nhà y không, qua ít nhất mấy trung gian" — vẫn dùng BFS như trên nhưng chỉ đi theo chiều mũi tên.

---

## 3. Checklist khi vào phòng thi
1. Đọc đề, xác định ngay: Câu 1 là **BST** hay **cây tổng quát**? Câu 2 là **danh sách kề** hay **ma trận kề**, **vô hướng** hay **có hướng**?
2. Copy khung struct + 3 hàm mẫu phù hợp ở trên, đổi tên trường dữ liệu theo đề.
3. Với câu "kiểm tra tồn tại theo tên" trong cây có khóa số (BST) — nhớ **duyệt toàn cây**, không tìm theo khóa.
4. Với câu "liệt kê theo điều kiện" trong cây tổng quát — nhớ **loại node value = 0** (nhóm/thư mục) nếu đề chỉ hỏi về "phần tử thật".
5. Viết `main()` sau cùng — chỉ là lắp dữ liệu mẫu trong đề vào các hàm đã viết, không cần thuật toán mới.
