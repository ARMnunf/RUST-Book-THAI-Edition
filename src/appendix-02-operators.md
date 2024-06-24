## ภาคผนวก B: ตัวดำเนินการและสัญลักษณ์

ภาคผนวกนี้ประกอบด้วยอภิธานศัพท์ของไวยากรณ์ Rust รวมถึงตัวดำเนินการและสัญลักษณ์อื่นๆ ที่ปรากฏโดยลำพังหรือในบริบทของพาธ เจเนริก ข้อจำกัดของเทรต มาโคร แอตทริบิวต์ คอมเม้นต์ ทูเพิล และวงเล็บ

### ตัวดำเนินการ

ตาราง B-1 แสดงตัวดำเนินการใน Rust พร้อมตัวอย่างการใช้งาน คำอธิบายสั้นๆ และข้อมูลว่าสามารถโอเวอร์โหลดได้หรือไม่ หากตัวดำเนินการสามารถโอเวอร์โหลดได้ จะแสดงเทรตที่เกี่ยวข้องสำหรับการโอเวอร์โหลดด้วย

<span class="caption">ตาราง B-1: ตัวดำเนินการ</span>

| ตัวดำเนินการ | ตัวอย่าง | คำอธิบาย | โอเวอร์โหลดได้หรือไม่? |
|----------|---------|-------------|---------------|
| `!` | `ident!(...)`, `ident!{...}`, `ident![...]` | การขยายมาโคร | |
| `!` | `!expr` | การกลับบิตหรือค่าตรรกะ | `Not` |
| `!=` | `expr != expr` | การเปรียบเทียบการไม่เท่ากัน | `PartialEq` |
| `%` | `expr % expr` | เศษจากการหาร | `Rem` |
| `%=` | `var %= expr` | กำหนดค่าเศษจากการหาร | `RemAssign` |
| `&` | `&expr`, `&mut expr` | การยืม | |
| `&` | `&type`, `&mut type`, `&'a type`, `&'a mut type` | ประเภทตัวชี้ที่ยืมมา | |
| `&` | `expr & expr` | AND แบบบิต | `BitAnd` |
| `&=` | `var &= expr` | AND แบบบิตและการกำหนดค่า | `BitAndAssign` |
| `&&` | `expr && expr` | AND ทางตรรกะแบบลัดวงจร | |
| `*` | `expr * expr` | การคูณ | `Mul` |
| `*=` | `var *= expr` | การคูณและการกำหนดค่า | `MulAssign` |
| `*` | `*expr` | การดีรีเฟอเรนซ์ | `Deref` |
| `*` | `*const type`, `*mut type` | ตัวชี้แบบดิบ | |
| `+` | `trait + trait`, `'a + trait` | การรวมข้อจำกัดของเทรต | |
| `+` | `expr + expr` | การบวก | `Add` |
| `+=` | `var += expr` | การบวกและการกำหนดค่า | `AddAssign` |
| `,` | `expr, expr` | ตัวคั่นอาร์กิวเมนต์และองค์ประกอบ | |
| `-` | `- expr` | การลบเชิงเลขคณิต | `Neg` |
| `-` | `expr - expr` | การลบ | `Sub` |
| `-=` | `var -= expr` | การลบและการกำหนดค่า | `SubAssign` |
| `->` | `fn(...) -> type`, <code>&vert;...&vert; -> type</code> | ประเภทการคืนค่าของฟังก์ชันและคลอสเซอร์ | |
| `.` | `expr.ident` | การเข้าถึงสมาชิก | |
| `..` | `..`, `expr..`, `..expr`, `expr..expr` | ช่วงแบบไม่รวมด้านขวา | `PartialOrd` |
| `..=` | `..=expr`, `expr..=expr` | ช่วงแบบรวมด้านขวา | `PartialOrd` |
| `..` | `..expr` | Struct literal update syntax | |
| `..` | `variant(x, ..)`, `struct_type { x, .. }` | “And the rest” pattern binding | |
| `...` | `expr...expr` | (Deprecated, use `..=` instead) In a pattern: inclusive range pattern | |
| `/` | `expr / expr` | การหาร | `Div` |
| `/=` | `var /= expr` | การหารและการกำหนดค่า | `DivAssign` |
| `:` | `pat: type`, `ident: type` | ข้อจำกัด | |
| `:` | `ident: expr` | Struct field initializer | |
| `:` | `'a: loop {...}` | ป้ายกำกับลูป | |
| `;` | `expr;` | Statement and item terminator | |
| `;` | `[...; len]` | Part of fixed-size array syntax | |
| `<<` | `expr << expr` | การเลื่อนบิตไปทางซ้าย | `Shl` |
| `<<=` | `var <<= expr` | การเลื่อนบิตไปทางซ้ายและกำหนดค่า | `ShlAssign` |
| `<` | `expr < expr` | การเปรียบเทียบการน้อยกว่า | `PartialOrd` |
| `<=` | `expr <= expr` | การเปรียบเทียบการน้อยกว่าหรือเท่ากับ | `PartialOrd` |
| `=` | `var = expr`, `ident = type` | การกำหนดค่า/ความเท่าเทียม | |
| `==` | `expr == expr` | การเปรียบเทียบความเท่ากัน | `PartialEq` |
| `=>` | `pat => expr` | Part of match arm syntax | |
| `>` | `expr > expr` | การเปรียบเทียบการมากกว่า | `PartialOrd` |
| `>=` | `expr >= expr` | การเปรียบเทียบการมากกว่าหรือเท่ากับ | `PartialOrd` |
| `>>` | `expr >> expr` | การเลื่อนบิตไปทางขวา | `Shr` |
| `>>=` | `var >>= expr` | การเลื่อนบิตไปทางขวาและกำหนดค่า | `ShrAssign` |
| `@` | `ident @ pat` | Pattern binding | |
| `^` | `expr ^ expr` | XOR แบบบิต | `BitXor` |
| `^=` | `var ^= expr` | XOR แบบบิตและกำหนดค่า | `BitXorAssign` |
| <code>&vert;</code> | <code>pat &vert; pat</code> | ทางเลือกของรูปแบบ | |
| <code>&vert;</code> | <code>expr &vert; expr</code> | OR แบบบิต | `BitOr` |
| <code>&vert;=</code> | <code>var &vert;= expr</code> | OR แบบบิตและกำหนดค่า | `BitOrAssign` |
| <code>&vert;&vert;</code> | <code>expr &vert;&vert; expr</code> | OR ทางตรรกะแบบลัดวงจร | |
| `?` | `expr?` | การส่งต่อข้อผิดพลาด | |

### สัญลักษณ์ที่ไม่ใช่ตัวดำเนินการ

รายการต่อไปนี้แสดงสัญลักษณ์ทั้งหมดที่ไม่ทำหน้าที่เป็นตัวดำเนินการ นั่นคือ ไม่ได้ทำงานเหมือนการเรียกฟังก์ชันหรือเมธอด

ตาราง B-2 แสดงสัญลักษณ์ที่ปรากฏโดยลำพังและใช้ได้ในหลายตำแหน่ง

<span class="caption">ตาราง B-2: ไวยากรณ์แบบสแตนด์อโลน</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `'ident` | ชื่ออายุการใช้งานหรือป้ายกำกับลูป |
| `...u8`, `...i32`, `...f64`, `...usize`, etc. | Numeric literal of specific type |
| `"..."` | String literal |
| `r"..."`, `r#"..."#`, `r##"..."##`, etc. | Raw string literal, escape characters not processed |
| `b"..."` | Byte string literal; constructs an array of bytes instead of a string |
| `br"..."`, `br#"..."#`, `br##"..."##`, etc. | Raw byte string literal, combination of raw and byte string literal |
| `'...'` | Character literal |
| `b'...'` | ASCII byte literal |
| <code>&vert;...&vert; expr</code> | คลอสเซอร์ |
| `!` | Always empty bottom type for diverging functions |
| `_` | “Ignored” pattern binding; also used to make integer literals readable |

ตาราง B-3 แสดงสัญลักษณ์ที่ปรากฏในบริบทของพาธผ่านลำดับชั้นของโมดูลไปยังรายการ

<span class="caption">ตาราง B-3: ไวยากรณ์ที่เกี่ยวข้องกับพาธ</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `ident::ident` | พาธของเนมสเปซ |
| `::path` | พาธเทียบกับรากของเครต (i.e., an explicitly absolute path) |
| `self::path` | พาธเทียบกับโมดูลปัจจุบัน (i.e., an explicitly relative path). |
| `super::path` | พาธเทียบกับโมดูลแม่ของโมดูลปัจจุบัน |
| `type::ident`, `<type as trait>::ident` | ค่าคงที่ ฟังก์ชัน และประเภทที่เกี่ยวข้อง |
| `<type>::...` | Associated item for a type that cannot be directly named (e.g., `<&T>::...`, `<[T]>::...`, etc.) |
| `trait::method(...)` | Disambiguating a method call by naming the trait that defines it |
| `type::method(...)` | Disambiguating a method call by naming the type for which it’s defined |
| `<type as trait>::method(...)` | Disambiguating a method call by naming the trait and type |

ตาราง B-4 แสดงสัญลักษณ์ที่ปรากฏในบริบทของการใช้พารามิเตอร์ประเภทเจเนริก

<span class="caption">ตาราง B-4: เจเนริก</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `path<...>` | ระบุพารามิเตอร์สำหรับประเภทเจเนริกในประเภท (e.g., `Vec<u8>`) |
| `path::<...>`, `method::<...>` | ระบุพารามิเตอร์สำหรับประเภท ฟังก์ชัน หรือเมธอดเจเนริกในนิพจน์ มักเรียกว่า turbofish (เช่น  `"42".parse::<i32>()`) |
| `fn ident<...> ...` | กำหนดฟังก์ชันเจเนริก |
| `struct ident<...> ...` | กำหนดโครงสร้างเจเนริก |
| `enum ident<...> ...` | กำหนดอีนัมเจเนริก |
| `impl<...> ...` | Define generic implementation |
| `for<...> type` | Higher-ranked lifetime bounds |
| `type<ident=type>` | A generic type where one or more associated types have specific assignments (e.g., `Iterator<Item=T>`) |

ตาราง B-5 แสดงสัญลักษณ์ที่ปรากฏในบริบทของการจำกัดพารามิเตอร์ประเภทเจเนริกด้วยข้อจำกัดของเทรต

<span class="caption">ตาราง B-5: ข้อจำกัดของเทรต</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `T: U` | Generic parameter `T` constrained to types that implement `U` |
| `T: 'a` | Generic type `T` must outlive lifetime `'a` (meaning the type cannot transitively contain any references with lifetimes shorter than `'a`) |
| `T: 'static` | Generic type `T` contains no borrowed references other than `'static` ones |
| `'b: 'a` | Generic lifetime `'b` must outlive lifetime `'a` |
| `T: ?Sized` | Allow generic type parameter to be a dynamically sized type |
| `'a + trait`, `trait + trait` | Compound type constraint |

ตาราง B-6 แสดงสัญลักษณ์ที่ปรากฏในบริบทของการเรียกหรือกำหนดมาโครและระบุแอตทริบิวต์บนรายการ

<span class="caption">ตาราง B-6: มาโครและแอตทริบิวต์</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `#[meta]` | แอตทริบิวต์ภายนอก |
| `#![meta]` | แอตทริบิวต์ภายใน |
| `$ident` | การแทนที่มาโคร |
| `$ident:kind` | Macro capture |
| `$(…)…` | Macro repetition |
| `ident!(...)`, `ident!{...}`, `ident![...]` | Macro invocation |

ตาราง B-7 แสดงสัญลักษณ์ที่ใช้สร้างคอมเม้นต์

<span class="caption">ตาราง B-7: คอมเม้นต์</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `//` | คอมเม้นต์บรรทัดเดียว |
| `//!` | คอมเม้นต์เอกสารภายในบรรทัดเดียว |
| `///` | คอมเม้นต์เอกสารภายนอกบรรทัดเดียว |
| `/*...*/` | คอมเม้นต์แบบบล็อก |
| `/*!...*/` | คอมเม้นต์เอกสารภายในแบบบล็อก |
| `/**...*/` | คอมเม้นต์เอกสารภายนอกแบบบล็อก |

ตาราง B-8 แสดงสัญลักษณ์ที่ปรากฏในบริบทของการใช้ทูเพิล

<span class="caption">ตาราง B-8: ทูเพิล</span>

| สัญลักษณ์ | คำอธิบาย |
|--------|-------------|
| `()` | Empty tuple (aka unit), both literal and type |
| `(expr)` | นิพจน์ในวงเล็บ |
| `(expr,)` | Single-element tuple expression |
| `(type,)` | Single-element tuple type |
| `(expr, ...)` | นิพจน์ทูเพิล |
| `(type, ...)` | Tuple type |
| `expr(expr, ...)` | Function call expression; also used to initialize tuple `struct`s and tuple `enum` variants |
| `expr.0`, `expr.1`, etc. | Tuple indexing |

ตาราง B-9 แสดงบริบทที่ใช้วงเล็บปีกกา

<span class="caption">ตาราง B-9: วงเล็บปีกกา</span>

| บริบท | คำอธิบาย |
|---------|-------------|
| `{...}` | Block expression |
| `Type {...}` | `struct` literal |

ตาราง B-10 แสดงบริบทที่ใช้วงเล็บเหลี่ยม

<span class="caption">ตาราง B-10: วงเล็บเหลี่ยม</span>

| บริบท | คำอธิบาย |
|---------|-------------|
| `[...]` | Array literal |
| `[expr; len]` | Array literal containing `len` copies of `expr` |
| `[type; len]` | Array type containing `len` instances of `type` |
| `expr[expr]` | Collection indexing. Overloadable (`Index`, `IndexMut`) |
| `expr[..]`, `expr[a..]`, `expr[..b]`, `expr[a..b]` | Collection indexing pretending to be collection slicing, using `Range`, `RangeFrom`, `RangeTo`, or `RangeFull` as the “index” |
