# SCSS 7-1 Template

Một template SCSS nhỏ gọn để luyện tập xây dựng thư viện CSS. Template sử dụng kiến trúc **7-1**, Sass Module System (`@use`) và bộ design token thân thiện với người lười thích dùng Tailwind CSS.

## Cấu trúc thư mục

```text
scss-7-1-template/
├── abstracts/
│   ├── _variables.scss
│   ├── _functions.scss
│   └── _mixins.scss
├── base/
│   ├── _reset.scss
│   └── _typography.scss
├── components/
│   ├── _buttons.scss
│   └── _cards.scss
├── layout/
│   ├── _header.scss
│   └── _footer.scss
├── pages/
│   └── _home.scss
├── themes/
│   └── _theme.scss
├── vendors/
│   └── _vendor.scss
├── main.scss
├── package.json
└── pnpm-lock.yaml
```

### `abstracts/`

Chứa các thành phần nền tảng không trực tiếp tạo UI độc lập. Đây là nơi định nghĩa token và các công cụ dùng lại ở những thư mục khác.

- **`_variables.scss`**: định nghĩa design token:
  - Font family: `$font-sans`, `$font-mono`.
  - Font size: `xs`, `sm`, `base`, `lg`, `xl`, `2xl` đến `5xl`.
  - Font weight và line height.
  - Spacing theo scale gần Tailwind, ví dụ `1` = `0.25rem`, `4` = `1rem`.
  - Màu semantic như `primary`, `success`, `warning`, `error`, `text`, `bg`, `surface`, `border`.
  - Border radius, box shadow, breakpoint và container max-width.
  - Các alias cũ như `$color-primary`, `$color-text`, `$color-bg`, `$color-error` để giữ tương thích.
- **`$css-values`**: từ điển các giá trị CSS thường dùng, được nhóm theo thuộc tính để dễ tra cứu:
  - `display`
  - `position`
  - `overflow`
  - `flex-direction`
  - `flex-wrap`
  - `justify-content`
  - `align-items`
  - `text-align`
  - `text-transform`
  - `object-fit`
  - `cursor`
  - `visibility`
- **`_functions.scss`**: các hàm truy xuất token và chuyển đổi giá trị:
  - `token($group, $key)`: lấy token theo nhóm.
  - `color($name)`: lấy màu semantic.
  - `theme-color($name)`: lấy màu semantic bằng CSS custom property để hỗ trợ light/dark mode.
  - `spacing($key)`: lấy giá trị spacing.
  - `css-value($property, $value)`: tra cứu giá trị CSS thường dùng.
  - `display()`, `position()`, `overflow()`: helper tra cứu nhanh cho các nhóm CSS phổ biến.
  - `flex-direction()`, `justify-content()`, `align-items()`: helper cho layout Flexbox.
  - `z-index($layer)`: lấy z-index theo layer như `modal` hoặc `toast`.
  - `to-rem($pixels)`: chuyển pixel sang rem.
- **`_mixins.scss`**: các pattern CSS có thể tái sử dụng:
  - `flex-layout($direction, $justify, $align, $wrap, $gap)`: cấu hình Flexbox tổng quát.
  - `flex-center($direction, $gap)`: căn giữa theo cả hai trục.
  - `flex-left/right($direction, $align, $gap)`: đặt phần tử ở đầu/cuối trục chính.
  - `flex-top/bottom($direction, $justify, $gap)`: đặt phần tử ở đầu/cuối trục phụ.
  - `flex-between/around($direction, $align, $gap)`: phân bố khoảng trống trên trục chính.
  - `flex-column($justify, $align, $gap)`: shorthand cho layout dạng cột.
  - `grid-layout($columns, $rows, $gap, $justify-items, $align-items)`: layout Grid tổng quát.
  - `grid-columns($columns, $gap)`: tạo số cột có kích thước bằng nhau.
  - `grid-auto-fit/fill($min-width, $gap)`: tạo Grid responsive tự động.
  - `grid-center()`: căn giữa item theo hai trục.
  - `grid-sidebar-layout($sidebar-width, $gap)`: layout sidebar và nội dung chính.
  - `col-span($span)` và `row-span($span)`: cho item chiếm nhiều cột hoặc hàng.
  - `col-range($start, $end)` và `row-range($start, $end)`: đặt item theo grid line cụ thể.
  - `respond-to($breakpoint)`: áp dụng từ breakpoint trở lên.
  - `respond-down($breakpoint)`: áp dụng bên dưới breakpoint.
  - `respond-between($min-breakpoint, $max-breakpoint)`: áp dụng trong một khoảng breakpoint.
  - `respond-only($breakpoint)`: áp dụng riêng trong phạm vi một breakpoint.
  - `respond-landscape()` và `respond-portrait()`: responsive theo hướng màn hình.
  - `container-query($name, $min-width)`: responsive theo kích thước container.
  - `focus-ring($color)`: tạo trạng thái focus dễ nhìn.
  - `visually-hidden`: ẩn nội dung khỏi giao diện nhưng vẫn cho screen reader đọc.
  - `truncate`: cắt text dài bằng dấu `...`.
  - `transition($properties, $duration, $timing-function, $delay)`: áp dụng transition chung.
  - `keyframes($name)`: khai báo keyframes có thể tái sử dụng.
  - `animation($name, $duration, $timing-function, $iteration-count, ...)`: cấu hình animation.
  - `animate-fade-in/out()`: fade phần tử vào hoặc ra.
  - `animate-slide-up/down()`: slide phần tử theo trục dọc.
  - `animate-scale-in()`: scale phần tử khi xuất hiện.
  - `animate-spin()`, `animate-pulse()`, `animate-bounce()`, `animate-shake()`: các animation lặp phổ biến.
  - `hover-lift($distance, $shadow, $duration)`: nâng card khi hover.
  - `hover-scale($scale, $duration)`: phóng to nhẹ khi hover.
  - `hover-opacity($opacity, $duration)`: thay đổi opacity khi hover.
  - `focus-visible($color, $width, $offset)`: tạo focus ring cho keyboard navigation.
  - `reduce-motion` và `motion-safe`: xử lý accessibility cho người dùng hạn chế animation.

### `base/`

Chứa các style nền tảng áp dụng cho toàn ứng dụng, không gắn với một component cụ thể.

- **`_reset.scss`**: reset box sizing, margin, padding và một số hành vi mặc định của media/input.
- **`_typography.scss`**: nơi định nghĩa typography toàn cục như `body`, heading, link hoặc các class văn bản. File hiện là điểm mở rộng cho typography của dự án.

### `components/`

Chứa các UI component có thể tái sử dụng ở nhiều trang.

- **`_buttons.scss`**: style cho button, các variant, kích thước và trạng thái tương tác.
- **`_cards.scss`**: style cho card và các biến thể nội dung dạng card.

### `layout/`

Chứa các phần cấu trúc lớn của giao diện, thường xuất hiện xuyên suốt nhiều trang.

- **`_header.scss`**: header, navigation và các vùng điều hướng chính.
- **`_footer.scss`**: footer và các khu vực thông tin cuối trang.

### `pages/`

Chứa style chỉ dành cho một page hoặc một màn hình cụ thể.

- **`_home.scss`**: style riêng cho trang home. Các page mới có thể thêm partial tương ứng, ví dụ `_about.scss` hoặc `_dashboard.scss`.

### `themes/`

Chứa các biến thể giao diện theo theme.

- **`_theme.scss`**: định nghĩa CSS custom properties cho light/dark mode và tự động nhận diện theme của hệ điều hành.

### `vendors/`

Chứa style override hoặc tích hợp thư viện bên thứ ba.

- **`_vendor.scss`**: điểm tập trung cho CSS của thư viện ngoài hoặc các override cần thiết. Nên giữ phần này độc lập để dễ thay thế dependency.

### `main.scss`

Entry point của toàn bộ stylesheet. File này nạp các module theo thứ tự 7-1:

1. Abstracts.
2. Vendors.
3. Base.
4. Layout và components.
5. Pages và themes.

Các partial bắt đầu bằng `_` không được compile trực tiếp. Chỉ cần compile `main.scss` để tạo CSS đầu ra.

### `package.json` và `pnpm-lock.yaml`

- **`package.json`**: khai báo dependency của dự án. Hiện template sử dụng Dart Sass dưới dạng dev dependency.
- **`pnpm-lock.yaml`**: khóa chính xác phiên bản dependency khi cài đặt bằng pnpm.

## Cài đặt

### Yêu cầu

- Node.js phiên bản LTS.
- pnpm.

Nếu chưa có pnpm, có thể kích hoạt qua Corepack:

```bash
corepack enable
```

### Cài dependency

Từ thư mục gốc của template, chạy:

```bash
pnpm install
```

### Compile SCSS

Compile `main.scss` thành một file CSS:

```bash
pnpm exec sass main.scss dist/main.css --no-source-map
```

Sau đó thêm file CSS đã build vào HTML:

```html
<link rel="stylesheet" href="./dist/main.css" />
```

## Ví dụ sử dụng

### Sử dụng entry point trong một trang HTML

```html
<!doctype html>
<html lang="vi">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="./dist/main.css" />
    <title>SCSS 7-1 Template</title>
  </head>
  <body>
    <header class="site-header">
      <div class="container">Frontend practice</div>
    </header>
  </body>
</html>
```

### Tạo component sử dụng token và mixin

Trong một partial thuộc `components/`, có thể import abstracts bằng namespace:

```scss
@use "../abstracts/functions" as fn;
@use "../abstracts/mixins" as mx;

.profile-card {
  padding: fn.spacing(6);
  color: fn.color(text);
  background-color: fn.color(surface);
  border: 1px solid fn.color(border);
  border-radius: fn.token(radius, lg);
  box-shadow: fn.token(shadow, sm);
  // mixin này là sao?

  @include mx.respond-to(md) {
    display: flex;
    gap: fn.spacing(8);
  }
}
```

### Light/dark mode

Theme dùng CSS custom properties nên component chỉ cần dùng semantic token, không cần biết màu light hay dark cụ thể:

```scss
@use "../abstracts/functions" as fn;

body {
  color: fn.theme-color(text);
  background-color: fn.theme-color(bg);
}

.card {
  color: fn.theme-color(text);
  background-color: fn.theme-color(surface);
  border: 1px solid fn.theme-color(border);
}

.link {
  color: fn.theme-color(primary);
}
```

Mặc định là light mode. Nếu người dùng không chọn thủ công, template tự dùng `prefers-color-scheme` của hệ điều hành:

```html
<html lang="vi">
  <head>
    <meta name="color-scheme" content="light dark" />
  </head>
</html>
```

Để ép light hoặc dark mode, thêm `data-theme` vào phần tử gốc:

```html
<html lang="vi" data-theme="dark">
```

Các giá trị được hỗ trợ là `light` và `dark`. `data-theme="dark"` có độ ưu tiên cao hơn cài đặt hệ điều hành. Để chuyển lại chế độ tự động, xóa thuộc tính `data-theme`.

Ví dụ toggle bằng JavaScript:

```html
<button type="button" id="theme-toggle" aria-pressed="false">
  Toggle theme
</button>

<script>
  const root = document.documentElement;
  const toggle = document.querySelector("#theme-toggle");

  toggle.addEventListener("click", () => {
    const isDark = root.dataset.theme === "dark";
    root.dataset.theme = isDark ? "light" : "dark";
    toggle.setAttribute("aria-pressed", String(!isDark));
  });
</script>
```

Nếu muốn giữ lựa chọn sau khi reload, lưu `root.dataset.theme` vào `localStorage` và đọc nó trước khi render giao diện.

### Tra cứu giá trị CSS thay vì nhớ giá trị thô

Không cần nhớ tất cả giá trị của các thuộc tính phổ biến. Dùng `css-value()` để tra cứu theo nhóm thuộc tính:

```scss
@use "../abstracts/functions" as fn;

.layout {
  display: fn.css-value(display, flex);
  position: fn.css-value(position, relative);
  overflow: fn.css-value(overflow, hidden);
  justify-content: fn.css-value(justify-content, between);
  align-items: fn.css-value(align-items, center);
}
```

Các helper ngắn tương ứng với nhóm được dùng thường xuyên:

```scss
.toolbar {
  display: fn.display(inline-flex);
  flex-direction: fn.flex-direction(row);
  justify-content: fn.justify-content(evenly);
  align-items: fn.align-items(center);
}
```

Tên trong map là tên dễ nhớ, còn function trả về giá trị CSS chuẩn. Ví dụ `between` trả về `space-between`, `start` trả về `flex-start`, và `end` trả về `flex-end`.

Các nhóm giá trị hiện có:

| Nhóm | Một số key |
| --- | --- |
| `display` | `block`, `flex`, `grid`, `none` |
| `position` | `relative`, `absolute`, `fixed`, `sticky` |
| `overflow` | `visible`, `hidden`, `clip`, `auto` |
| `flex-direction` | `row`, `row-reverse`, `column`, `column-reverse` |
| `flex-wrap` | `nowrap`, `wrap`, `wrap-reverse` |
| `justify-content` | `start`, `center`, `between`, `around`, `evenly` |
| `align-items` | `start`, `center`, `end`, `baseline`, `stretch` |
| `text-align` | `left`, `center`, `right`, `justify` |
| `text-transform` | `none`, `uppercase`, `lowercase`, `capitalize` |
| `object-fit` | `contain`, `cover`, `fill`, `none` |
| `cursor` | `auto`, `default`, `pointer`, `grab`, `not-allowed` |
| `visibility` | `visible`, `hidden`, `collapse` |

Nếu truyền sai nhóm hoặc key, Sass sẽ dừng compile và báo lỗi cụ thể. Điều này giúp phát hiện typo sớm:

```scss
// Error: Unknown `space-betwen` value for CSS property group `justify-content`.
.toolbar {
  justify-content: fn.css-value(justify-content, space-betwen);
}
```

### Tạo button responsive có focus state

```scss
@use "../abstracts/functions" as fn;
@use "../abstracts/mixins" as mx;

.button {
  padding: fn.spacing(3) fn.spacing(5);
  color: fn.color(white);
  background-color: fn.color(primary);
  border: 0;
  border-radius: fn.token(radius, md);
  cursor: pointer;

  @include mx.transition(background-color, transform);

  &:hover {
    background-color: fn.color(primary-dark);
    transform: translateY(-1px);
  }

  &:focus-visible {
    @include mx.focus-ring();
  }
}
```

### Tạo layout container và flex center

```scss
@use "../abstracts/mixins" as mx;
@use "../abstracts/functions" as fn;

.hero {
  @include mx.container(fn.spacing(4));
  @include mx.flex-center(column, fn.spacing(4));
  min-height: 60vh;
  text-align: center;
}
```

### Sử dụng các Flexbox mixin

```scss
@use "../abstracts/functions" as fn;
@use "../abstracts/mixins" as mx;

.toolbar {
  @include mx.flex-between($gap: fn.spacing(4));
}

.form-fields {
  @include mx.flex-stack(fn.spacing(4));
}

.tag-list {
  @include mx.flex-cluster(fn.spacing(2));
}

.custom-layout {
  @include mx.flex-layout(
    $direction: row,
    $justify: space-evenly,
    $align: center,
    $wrap: wrap,
    $gap: fn.spacing(6)
  );
}
```

### Sử dụng các Grid mixin

```scss
@use "../abstracts/functions" as fn;
@use "../abstracts/mixins" as mx;

.product-grid {
  @include mx.grid-columns(3, fn.spacing(6));
}

.responsive-grid {
  @include mx.grid-auto-fit(16rem, fn.spacing(6));
}

.dashboard {
  @include mx.grid-sidebar-layout(16rem, fn.spacing(6));
}

.centered-content {
  @include mx.grid-center;
}
```

`grid-layout()` là mixin tổng quát khi cần tự chỉ định số cột, số hàng và cách căn item:

```scss
@use "../abstracts/functions" as fn;
@use "../abstracts/mixins" as mx;

.custom-grid {
  @include mx.grid-layout(
    $columns: repeat(3, 1fr),
    $rows: auto,
    $gap: fn.spacing(4),
    $justify-items: center,
    $align-items: start
  );
}
```

`$justify-items` và `$align-items` căn nội dung của từng grid item trong cell; chúng không căn toàn bộ grid tracks trong container.

`grid-auto-fit()` sẽ ưu tiên lấp đầy hàng bằng các cột đang có, còn `grid-auto-fill()` giữ lại các track trống khi container còn không gian:

```scss
.fit-grid {
  @include mx.grid-auto-fit(18rem, fn.spacing(4));
}

.fill-grid {
  @include mx.grid-auto-fill(18rem, fn.spacing(4));
}
```

Các mixin `col-span()` và `row-span()` dùng cho grid item khi muốn item chiếm nhiều track:

```scss
.featured-card {
  @include mx.col-span(2);
  @include mx.row-span(2);
}
```

Khi cần đặt item theo grid line cụ thể, dùng `col-range()` và `row-range()`:

```scss
.hero-card {
  @include mx.col-range(1, -1);
  @include mx.row-range(2, 4);
}
```

### Animation, transition và hover

```scss
@use "../abstracts/mixins" as mx;

@include mx.keyframes(fade-in-up) {
  from {
    opacity: 0;
    transform: translateY(1rem);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero {
  @include mx.animation(fade-in-up, 500ms, ease-out);
}

.modal {
  @include mx.animate-scale-in(0.96, 200ms);
}

.loading-spinner {
  @include mx.animate-spin;
}

.notification {
  @include mx.animate-slide-up;
}

.card {
  @include mx.hover-lift;
  @include mx.focus-visible;
}

.avatar {
  @include mx.hover-scale(1.05);
}

.muted-action {
  @include mx.hover-opacity(0.65);
}
```

Các preset animation không tự động áp dụng media query để bạn vẫn có thể kiểm soát chính xác thời điểm chạy. Khi cần tôn trọng reduced motion, bọc preset trong `motion-safe`:

```scss
.notification {
  @include mx.motion-safe {
    @include mx.animate-slide-up;
  }

  @include mx.reduce-motion {
    animation: none;
  }
}
```

`transition()` nhận một property hoặc một list property. Ví dụ:

```scss
.button {
  @include mx.transition(
    (background-color, color, transform),
    200ms,
    ease-out
  );
}
```

Để tôn trọng cài đặt giảm chuyển động của người dùng, đặt animation trong `motion-safe` hoặc tắt hiệu ứng bổ sung trong `reduce-motion`:

```scss
.notification {
  @include mx.motion-safe {
    @include mx.animation(slide-in, 250ms);
  }

  @include mx.reduce-motion {
    animation: none;
    transition: none;
  }
}
```

### Responsive theo breakpoint

Các breakpoint được định nghĩa trong `abstracts/_variables.scss`:

| Tên   | Giá trị  |
| ----- | -------- |
| `sm`  | `640px`  |
| `md`  | `768px`  |
| `lg`  | `1024px` |
| `xl`  | `1280px` |
| `2xl` | `1536px` |

```scss
@use "../abstracts/mixins" as mx;

.navigation {
  display: none;

  // Mobile-first: hiển thị từ md trở lên.
  @include mx.respond-to(md) {
    display: flex;
  }

  // Chỉ hiển thị menu mobile bên dưới md.
  @include mx.respond-down(md) {
    display: block;
  }
}

.dashboard {
  @include mx.respond-between(md, xl) {
    grid-template-columns: repeat(2, 1fr);
  }
}

.hero-title {
  @include mx.respond-only(lg) {
    max-width: 42rem;
  }
}
```

`respond-down(md)` dùng `max-width: 767.98px`, còn `respond-only(md)` dùng khoảng `768px` đến `1023.98px`. Khoảng hở `0.02px` giúp các media query liền kề không bị chồng lấn.

Responsive theo hướng màn hình:

```scss
.gallery {
  @include mx.respond-landscape {
    grid-auto-flow: column;
  }

  @include mx.respond-portrait {
    grid-auto-flow: row;
  }
}
```

Khi component phụ thuộc vào không gian của parent thay vì viewport, dùng container query:

```scss
.card-list {
  container: cards / inline-size;
}

.card {
  @include mx.container-query(cards, 32rem) {
    display: flex;
    gap: 1rem;
  }
}
```

### Dùng utility cho nội dung accessibility

```scss
@use "../abstracts/mixins" as mx;

.skip-link {
  @include mx.visually-hidden;

  &:focus {
    position: fixed;
    inset: 1rem auto auto 1rem;
    width: auto;
    height: auto;
    padding: 0.5rem 1rem;
    margin: 0;
    overflow: visible;
    clip: auto;
    white-space: normal;
  }
}
```

## Quy ước mở rộng

- Thêm token dùng chung vào `abstracts/_variables.scss`, không hard-code lại trong component.
- Dùng `@use` thay cho `@import`.
- Component dùng ở nhiều nơi thuộc `components/`; style chỉ dùng cho một page thuộc `pages/`.
- Không đặt CSS selector của component vào `abstracts/`.
- Khi thêm partial mới, import nó trong `main.scss` theo đúng thứ tự phụ thuộc.
- Các tên token mới nên giữ cách đặt tên semantic, ví dụ `surface`, `border`, `muted`, thay vì gắn trực tiếp với một màu cụ thể.

## Mục tiêu luyện tập

Template phù hợp để thực hành:

- Xây dựng component từ yêu cầu UI.
- Thiết kế responsive theo breakpoint.
- Tạo variant và trạng thái tương tác.
- Duy trì design token nhất quán.
- Tách CSS theo component, layout và page.
- Chuẩn bị câu trả lời phỏng vấn về Sass, CSS architecture, accessibility và maintainability.
