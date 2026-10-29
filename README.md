## Phần 1: Cơ bản

### 1.1. `any` và `unknown` khác nhau thế nào? Khi nào nên dùng `unknown`?

Trả lời: cả any lẫn unknown đều có thể chứa bất kỳ giá trị nào, nhưng unknown ép buộc việc xử lý dữ liệu phải an toàn trước khi sử dụng. Any có thể tùy ý gọi hàm, truy cập thuộc tính hay gán vào biến khác mà không bị cảnh báo, trong khi unknown bị cấm thực hiện bất kỳ thao tác nào (gọi hàm, truy cập thuộc tính...) và không thể gán cho kiểu khác (trừ any hoặc chính nó) nếu chưa kiểm tra kiểu hoặc ép kiểu. Nên dùng unknown cho bất kỳ dữ liệu đầu vào nào chưa xác định trước cấu trúc như kết quả parse JSON, dữ liệu nhận về từ API/bên thứ ba.

### 1.2. `interface` và `type` khác nhau ở đâu? Em sẽ chọn cái nào và vì sao?

Trả lời: interface dùng để định nghĩa khuôn mẫu (contract) cho đối tượng, class với khả năng kế thừa bằng extends, tự động gộp thuộc tính khi trùng tên (declaration merging) và được trình biên dịch tối ưu hiệu năng bộ nhớ đệm tốt hơn; trong khi type là bí danh kiểu dữ liệu linh hoạt hơn nhiều, có thể biểu diễn mọi thứ từ kiểu nguyên thủy, tuple cho đến union hay intersection nhưng không thể gộp khai báo trùng tên. Nên ưu tiên chọn interface làm mặc định khi định nghĩa cấu trúc object, component props, DTO, service contracts và đặc biệt là khi viết hoặc mở rộng các thư viện bên thứ ba (nhờ declaration merging mà không sợ lỗi); và chỉ dùng type khi cần xử lý các kiểu dữ liệu nâng cao như union, tuple hoặc tính toán kiểu phức tạp (conditional/utility types).

### 1.3. `null` và `undefined` khác nhau thế nào? Tuỳ chọn `strictNullChecks` có tác dụng gì?

Trả lời: undefined biểu thị biến đã được khai báo nhưng chưa hề được gán giá trị hoặc thuộc tính không hề tồn tại, trong khi null là một giá trị được lập trình viên chủ động gán vào để biểu thị trạng thái "rỗng" hoặc cố ý không có dữ liệu; tùy chọn strictNullChecks (khi bật) sẽ cấm gán null và undefined vào các kiểu dữ liệu khác như string, number hay object, buộc bạn phải khai báo rõ ràng kiểu kết hợp (union type như string | null | undefined) và kiểm tra giá trị trước khi truy cập thuộc tính/gọi hàm.

### 1.4. Optional chaining `?.` và nullish coalescing `??` dùng để làm gì? Cho ví dụ.

Trả lời: Optional chaining (?.) dùng để đọc thuộc tính hoặc gọi hàm một cách an toàn mà không làm crash chương trình nếu đối tượng phía trước là null hoặc undefined (nó lập tức ngắt chuỗi và trả về undefined), trong khi nullish coalescing (??) là toán tử cung cấp giá trị mặc định dự phòng chỉ khi biểu thức bên trái là null hoặc undefined (khác với || vốn coi cả số 0, chuỗi rỗng "" hay false là giá trị sai).

- Ví dụ: const age = user?.age ?? 18; (Nếu user bị null/undefined, code không bị crash mà lấy giá trị dự phòng 18; nếu user.age = 0, ?? vẫn giữ đúng số 0 chứ không bị nhảy sang 18 như khi dùng || ).

---

## Phần 2: Hệ thống kiểu (so sánh với Java)

### 2.1. Structural typing là gì? Cho ví dụ hai kiểu khác tên nhưng vẫn gán được cho nhau. Java xử lý trường hợp này thế nào?

Trả lời: Structural typing là cơ chế kiểm tra kiểu dữ liệu dựa trên hình dạng/cấu trúc thuộc tính của đối tượng thay vì tên gọi của nó, nghĩa là chỉ cần hai kiểu có chung tập thuộc tính và kiểu dữ liệu tương thích thì chúng được coi là cùng loại và hoàn toàn gán được cho nhau.

- Ví dụ: interface User { name: string; age: number; } và interface Employee { name: string; age: number; role: string; } dù khác tên nhưng ta vẫn gán biến kiểu Employee cho biến kiểu User bình thường vì nó thỏa mãn đủ cấu trúc mà User cần.

Ngược lại, Java sử dụng Nominal typing, nơi việc tương thích kiểu phụ thuộc hoàn toàn vào tên gọi và quan hệ kế thừa tường minh (extends hoặc implements), nên dù hai class User và Employee trong Java có các trường thuộc tính giống hệt nhau thì compiler vẫn báo lỗi không tương thích kiểu nếu gán trực tiếp cho nhau mà không có quan hệ cha-con.

### 2.2. Union type là gì? Với một biến kiểu `string | number`, làm sao biết lúc chạy nó đang là kiểu nào?

Trả lời: Union type là kiểu kết hợp trong TypeScript cho phép một biến có thể mang một trong nhiều kiểu dữ liệu khác nhau (ví dụ string | number nghĩa là giá trị có thể là chuỗi hoặc số), để xác định chính xác kiểu dữ liệu thực tế tại thời điểm chạy, ta sử dụng cơ chế Type Narrowing (thu hẹp kiểu) kết hợp với toán tử typeof của JavaScript bên trong các khối điều kiện (chẳng hạn dùng if (typeof val === "string") để TypeScript tự động hiểu bên trong khối đó val chắc chắn là chuỗi và mở khóa các hàm xử lý chuỗi, còn trong nhánh else nó sẽ tự hiểu val chắc chắn là kiểu number).

### 2.3. Discriminated union là gì? Viết một ví dụ mô tả trạng thái đơn hàng.

Trả lời: Discriminated Union là kỹ thuật kết hợp nhiều kiểu đối tượng lại với nhau bằng Union Type (|), trong đó mỗi đối tượng đều sở hữu chung một thuộc tính định danh để TypeScript dựa vào đó thực hiện Type Narrowing một cách tuyệt đối chính xác thông qua các câu lệnh rẽ nhánh (switch hoặc if/else).

- Ví dụ: mô tả trạng thái đơn hàng

```ts
type OrderState =
  | { status: "PENDING" }
  | { status: "SHIPPED"; trackingNumber: string }
  | { status: "CANCELLED"; reason: string };

function handleOrder(order: OrderState) {
  switch (order.status) {
    case "SHIPPED":
      // TypeScript tự thu hẹp kiểu và chỉ cho phép đọc trackingNumber tại đây
      console.log("Mã vận đơn:", order.trackingNumber);
      break;
    case "CANCELLED":
      console.log("Lý do hủy:", order.reason);
      break;
    default:
      console.log("Đơn hàng đang chờ xử lý");
  }
}
```

### 2.4. Vì sao trong TypeScript hiện đại, nhiều người dùng union of literals (ví dụ `'S' | 'M' | 'L'`) thay cho `enum`?

Trả lời: Trong TypeScript hiện đại, nhiều người dùng union of literals thay cho enum vì:

- Union of literals là kiểu dữ liệu thuần compile-time (TypeScript), khi biên dịch sang JavaScript nó bị xóa sạch 100%, không sinh thêm bất kỳ dòng code nào. Trong khi enum là một tính năng lai (hybrid): khi compile, TypeScript buộc phải sinh ra một hàm tự thực thi tạo ra object JavaScript thực tế, làm tăng dung lượng bundle và gây khó khăn cho kỹ thuật Tree-shaking
- Union of literals dễ dùng, không cần import lòng vòng. Với union chỉ cần truyền thẳng chuỗi 'M' vào hàm setter. Trong khi Với enum bắt buộc phải import enum vào và truyền Size.M vào hàm setter.
- Dữ liệu trả về từ API backend thường là chuỗi JSON thuần ("S", "M"). Nếu dùng union of literals, TypeScript hiểu và gán được ngay lập tức mà không cần bước parse hay ép kiểu như với enum.

### 2.5. Kiểu dữ liệu của TypeScript có tồn tại lúc chạy (runtime) không? Điều này khác Java thế nào và ảnh hưởng gì khi viết code?

Trả lời: Kiểu dữ liệu của TypeScript hoàn toàn KHÔNG tồn tại lúc chạy vì toàn bộ thông tin kiểu đều bị trình biên dịch loại bỏ sạch sẽ thông qua cơ chế Type Erasure để tạo ra JavaScript thuần; điều này khác biệt hoàn toàn với Java, nơi hệ thống kiểu vẫn được lưu giữ và kiểm tra một phần lúc runtime thông qua bytecode, Reflection hay toán tử instanceof trên các class cụ thể; hệ quả khi viết code là lập trình viên không thể dùng các kiểu của TypeScript để kiểm tra điều kiện lúc chạy (ví dụ viết if (data instanceof UserInterface) sẽ báo lỗi ngay lập tức), buộc ta phải dựa vào các công cụ kiểm tra của JavaScript thuần (typeof, instanceof với class, toán tử in), tự viết hàm Custom Type Guard, hoặc sử dụng các thư viện schema validation lúc runtime để thẩm định dữ liệu ngoại lai từ API trước khi thao tác.

---

## Phần 3: Generic và utility type

### 3.1. Viết một hàm generic `first<T>(arr: T[])` trả về phần tử đầu tiên của mảng. Kiểu trả về nên là gì?

Trả lời: Kiểu trả về của hàm nên là T | undefined, bởi vì trong trường hợp mảng truyền vào rỗng ([]), giá trị thực tế lấy ra tại chỉ số đầu tiên (arr[0]) lúc chạy sẽ là undefined — việc khai báo tường minh T | undefined sẽ cảnh báo và bắt buộc người gọi hàm phải kiểm tra tồn tại trước khi thao tác, giúp tránh các lỗi crash runtime.

### 3.2. Cú pháp `<T extends ...>` dùng để làm gì? Cho ví dụ.

Trả lời: Cú pháp <T ... extends> dùng để thiết lập ràng buộc kiểu dữ liệu cho tham số generic, bắt buộc kiểu T khi truyền vào phải thỏa mãn tối thiểu cấu trúc hoặc kế thừa từ kiểu quy định sẵn phía sau từ khóa extends, nhờ đó cho phép lập trình viên truy cập an toàn vào các thuộc tính của ràng buộc đó bên trong hàm mà compiler không báo lỗi.

- Ví dụ:

```ts
// T bị ràng buộc phải có thuộc tính length là number
function logLength<T extends { length: number }>(item: T): void {
  console.log(item.length); // Truy cập item.length an toàn, không bị báo lỗi
}

// Hợp lệ (vì string và array đều có thuộc tính .length):
logLength("hello"); // In ra: 5
logLength([1, 2, 3]); // In ra: 3

logLength(123); // Lỗi vì số không có thuộc tính length
```

### 3.3. Khi viết API cập nhật sản phẩm (chỉ gửi lên những trường cần sửa), em sẽ định nghĩa kiểu dữ liệu đầu vào thế nào? Dùng utility type nào?

Trả lời: Khi viết API cập nhật sản phẩm, ta nên sử dụng Partial<T> kết hợp với Omit<T, K> để biến các trường cần cập nhật thành tùy chọn, đồng thời loại trừ các thuộc tính cố định không được phép sửa (như id, createdAt).

- Ví dụ:

```ts
type UpdateProductDto = Partial<Omit<Product, "id" | "createdAt">>;
```

---

## Phần 4: Bất đồng bộ

### 4.1. `async/await` hoạt động thế nào? JavaScript chạy một luồng thì làm sao xử lý được nhiều request cùng lúc?

Trả lời: async/await là cú pháp bọc quanh Promise giúp viết code bất đồng bộ trông giống như đồng bộ, trong đó từ khóa async đảm bảo hàm luôn đóng gói giá trị trả về thành một Promise, còn await chỉ tạm dừng cục bộ việc thực thi các dòng code tiếp theo trong hàm đó cho đến khi Promise giải quyết xong mà không hề gây nghẽn luồng chính, lý do JavaScript chỉ chạy đơn luồng nhưng vẫn xử lý hàng nghìn request cùng lúc là nhờ kiến trúc Event Loop phối hợp với môi trường thực thi: luồng chính duy nhất chỉ làm nhiệm vụ tiếp nhận và điều phối, còn các tác vụ I/O tốn thời gian (như truy vấn Database, đọc/ghi file, gửi request mạng) sẽ được đẩy xuống cho tầng đa luồng của hệ điều hành xử lý ngầm, ngay sau đó luồng chính lập tức rảnh tay để đón nhận các request tiếp theo; khi tác vụ ngầm hoàn tất, kết quả được chuyển vào hàng đợi và Event Loop sẽ đưa nó trở lại Call Stack để hàm tiếp tục chạy nốt phần code sau await khi luồng chính rảnh rỗi.

### 4.2. Nếu quên `await` khi gọi một hàm `async` thì chuyện gì xảy ra? Cho ví dụ.

Trả lời: Nếu quên await khi gọi một hàm async, chương trình sẽ không dừng lại để đợi kết quả mà tiếp tục chạy ngay lập tức các dòng code phía sau; lúc này giá trị biến nhận được sẽ là một đối tượng Promise chưa được giải quyết thay vì dữ liệu thực tế, dẫn đến kết quả tính toán bị sai lệch hoặc văng lỗi khi cố truy cập thuộc tính bên trong; đồng thời nếu hàm async đó xảy ra lỗi, khối try-catch bao quanh sẽ không thể bắt được lỗi đó.

-Ví dụ:

```ts
async function getNameById(id: string): Promise<string> {
  // Giả lập đọc DB mất 1 giây
  return new Promise((resolve) => {
    setTimeout(() => resolve("thang"), 1000);
  });
}

async function handleCheckUser() {
  // Quên await:
  const name = getNameById("123");

  console.log("Đang chờ dữ liệu từ DB...");
  console.log(name); // In ra: Promise {[[PromiseState]]: 'pending', [[PromiseResult]]: undefined} (chưa có dữ liệu)
}

handleCheckUser();
```

### 4.3. Xử lý lỗi trong hàm `async` như thế nào?

Trả lời: Cách xử lý lỗi trong hàm async là bọc đoạn code có await vào khối try-catch (bắt được cả lỗi đồng bộ lẫn Promise.reject), trong đó cần dùng if để ép kiểu an toàn cho biến lỗi (vì TypeScript mặc định error là unknown); ngoài ra có thể gắn trực tiếp .catch() vào cuối lời gọi hàm hoặc dùng khối finally để giải phóng tài nguyên.

---

## Phần 5: Thực hành

### 5.1. Khai báo kiểu props cho một component React `ProductList` nhận vào danh sách sản phẩm và một hàm `onSelect` được gọi khi chọn một sản phẩm.

Trả lời:

```ts
// 1. Định nghĩa kiểu dữ liệu cho từng sản phẩm
export interface Product {
  id: string | number;
  name: string;
  price: number;
}

// 2. Khai báo kiểu Props cho ProductList
export interface ProductListProps {
  products: Product[];
  onSelect: (product: Product) => void;
}

// 3. Component con
export const ProductList: React.FC<ProductListProps> = ({ products, onSelect }) => {
  return (
    <ul>
      {products.map((product) => (
        <li key={product.id} onClick={() => onSelect(product)}>
          {product.name} - ${product.price}
        </li>
      ))}
    </ul>
  );
}

// 4. Component cha
export function HomePage() {
  const [productList] = useState<Product[]>([
    { id: 1, name: "Bàn phím cơ AKKO", price: 50 },
    { id: 2, name: "Chuột không dây Logitech", price: 30 },
    { id: 3, name: "Tai nghe Sony WH-1000XM5", price: 350 },
  ]);

  // click vào item nào thì chuyển hướng đến trang chi tiết item đó
  const handleSelectProduct = (product: Product) => {
    // code chuyển trang
  };

  return (
    <ProductList products={productList} onSelect={handleSelectProduct} />
  );
}
```

### 5.2. Khai báo kiểu cho body của request `POST /products` trong Express, và viết handler sử dụng kiểu đó.

Trả lời:

```ts
// Khai báo kiểu dữ liệu cho Body mà client gửi lên (CreateProductDTO)
export interface CreateProductBody {
  name: string;
  price: number;
  description?: string;
}

// Viết handler sử dụng kiểu đó
export const createProductHandler = (
  req: Request<{}, {}, CreateProductBody>,
  res: Response,
) => {
  const { name, price, description } = req.body;

  // Xử lý logic nghiệp vụ
  if (!name || price <= 0) {
    return res.status(400).json({ message: "Dữ liệu không hợp lệ" });
  }

  // trả về response
  return res.status(201).json({
    message: "Tạo sản phẩm thành công",
    data: { id: Date.now(), name, price, description },
  });
};
```

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

```ts
interface Product {
  id: number;
  name: string;
  price?: number;
}

// 1. Sửa lỗi `p.price` có thể undefined bằng `p.price ?? 0`
function getTotal(products: Product[]): number {
  return products.reduce((sum, p) => sum + (p.price ?? 0), 0);
}

// 2. Sửa kiểu trả về của hàm async thành Promise<Product[]>
async function loadProducts(): Promise<Product[]> {
  const res = await fetch("/api/products");
  return res.json();
}
```
