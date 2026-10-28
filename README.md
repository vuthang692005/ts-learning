## Phần 1: Cơ bản

### 1.1. `any` và `unknown` khác nhau thế nào? Khi nào nên dùng `unknown`?

Trả lời:

### 1.2. `interface` và `type` khác nhau ở đâu? Em sẽ chọn cái nào và vì sao?

Trả lời:

### 1.3. `null` và `undefined` khác nhau thế nào? Tuỳ chọn `strictNullChecks` có tác dụng gì?

Trả lời:

### 1.4. Optional chaining `?.` và nullish coalescing `??` dùng để làm gì? Cho ví dụ.

Trả lời:

---

## Phần 2: Hệ thống kiểu (so sánh với Java)

### 2.1. Structural typing là gì? Cho ví dụ hai kiểu khác tên nhưng vẫn gán được cho nhau. Java xử lý trường hợp này thế nào?

Trả lời:

### 2.2. Union type là gì? Với một biến kiểu `string | number`, làm sao biết lúc chạy nó đang là kiểu nào?

Trả lời:

### 2.3. Discriminated union là gì? Viết một ví dụ mô tả trạng thái đơn hàng.

Trả lời:

### 2.4. Vì sao trong TypeScript hiện đại, nhiều người dùng union of literals (ví dụ `'S' | 'M' | 'L'`) thay cho `enum`?

Trả lời:

### 2.5. Kiểu dữ liệu của TypeScript có tồn tại lúc chạy (runtime) không? Điều này khác Java thế nào và ảnh hưởng gì khi viết code?

Trả lời:

---

## Phần 3: Generic và utility type

### 3.1. Viết một hàm generic `first<T>(arr: T[])` trả về phần tử đầu tiên của mảng. Kiểu trả về nên là gì?

Trả lời:

### 3.2. Cú pháp `<T extends ...>` dùng để làm gì? Cho ví dụ.

Trả lời:

### 3.3. Khi viết API cập nhật sản phẩm (chỉ gửi lên những trường cần sửa), em sẽ định nghĩa kiểu dữ liệu đầu vào thế nào? Dùng utility type nào?

Trả lời:

---

## Phần 4: Bất đồng bộ

### 4.1. `async/await` hoạt động thế nào? JavaScript chạy một luồng thì làm sao xử lý được nhiều request cùng lúc?

Trả lời:

### 4.2. Nếu quên `await` khi gọi một hàm `async` thì chuyện gì xảy ra? Cho ví dụ.

Trả lời:

### 4.3. Xử lý lỗi trong hàm `async` như thế nào?

Trả lời:

---

## Phần 5: Thực hành

### 5.1. Khai báo kiểu props cho một component React `ProductList` nhận vào danh sách sản phẩm và một hàm `onSelect` được gọi khi chọn một sản phẩm.

Trả lời:

### 5.2. Khai báo kiểu cho body của request `POST /products` trong Express, và viết handler sử dụng kiểu đó.

Trả lời:

### 5.3. Đoạn code dưới đây có lỗi gì? Sửa lại cho đúng.

```ts
interface Product {
  id: number;
  name: string;
  price?: number;
}

function getTotal(products: Product[]): number {
  return products.reduce((sum, p) => sum + p.price, 0);
}

async function loadProducts(): Product[] {
  const res = await fetch("/api/products");
  return res.json();
}
```

Trả lời:
