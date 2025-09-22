📖 Những gì bạn đã học trong Bài 1:
    Text Interpolation ({{ }}) - Hiển thị dữ liệu trong template
    Attribute Binding (v-bind:) - Ràng buộc thuộc tính HTML
    Event Handling (@click) - Xử lý sự kiện
    Conditional Rendering (v-if, v-show) - Hiển thị có điều kiện
    List Rendering (v-for) - Hiển thị danh sách
    Form Input Binding (v-model) - Ràng buộc dữ liệu form
    Composition API cơ bản:
    ref() - cho primitive values
    reactive() - cho objects và arrays
    computed() - tính toán tự động


| Kiểu binding          | Cú pháp                                          | Công dụng                                  | Ví dụ                                                     |
| --------------------- | ------------------------------------------------ | ------------------------------------------ | --------------------------------------------------------- |
| **Interpolation**     | `{{ variable }}`                                 | Hiển thị dữ liệu JS ra HTML (text binding) | `<p>{{ message }}</p>`                                    |
| **Attribute binding** | `v-bind:attr="variable"` hoặc `:attr="variable"` | Gán giá trị động vào attribute HTML        | `<a :href="url">Link</a>`                                 |
| **Two-way binding**   | `v-model="variable"`                             | Liên kết 2 chiều (UI ↔ JS)                 | `<input v-model="name">`                                  |
| **Event binding**     | `v-on:event="handler"` hoặc `@event="handler"`   | Bắt sự kiện người dùng                     | `<button @click="count++">Click</button>`                 |
| **Class binding**     | `:class="{ active: isActive }"`                  | Bật/tắt class dựa trên state               | `<p :class="{ active: isActive }">Text</p>`               |
| **Style binding**     | `:style="{ color: colorVar }"`                   | Gán inline style động                      | `<p :style="{ color: isRed ? 'red' : 'blue' }">Hello</p>` |


