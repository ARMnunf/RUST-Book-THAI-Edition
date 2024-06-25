## ภาคผนวก D - เครื่องมือการพัฒนาที่มีประโยชน์

ในภาคผนวกนี้ เราจะพูดถึงเครื่องมือการพัฒนาที่มีประโยชน์ซึ่งโครงการ Rust จัดเตรียมไว้ให้ เราจะดูเกี่ยวกับการจัดรูปแบบอัตโนมัติ วิธีการรวดเร็วในการแก้ไขคำเตือน เครื่องมือตรวจสอบโค้ด และการผสานรวมกับ IDE

### การจัดรูปแบบอัตโนมัติด้วย `rustfmt`

เครื่องมือ `rustfmt` จะจัดรูปแบบโค้ดของคุณตามรูปแบบโค้ดของชุมชน โครงการที่ทำงานร่วมกันหลายโครงการใช้ `rustfmt` เพื่อป้องกันการโต้เถียงเกี่ยวกับรูปแบบที่ควรใช้เมื่อเขียน Rust: ทุกคนจัดรูปแบบโค้ดของตนโดยใช้เครื่องมือนี้

เพื่อติดตั้ง `rustfmt` ให้ป้อนคำสั่งต่อไปนี้:

```console
$ rustup component add rustfmt
```

คำสั่งนี้จะให้คุณได้ `rustfmt` และ `cargo-fmt` คล้ายกับที่ Rust ให้ทั้ง `rustc` และ `cargo` เพื่อจัดรูปแบบโครงการ Cargo ใดๆ ให้ป้อนคำสั่งต่อไปนี้:

```console
$ cargo fmt
```

การเรียกใช้คำสั่งนี้จะจัดรูปแบบโค้ด Rust ทั้งหมดในเครตปัจจุบัน สิ่งนี้ควรเปลี่ยนเฉพาะรูปแบบโค้ดเท่านั้น ไม่ใช่ความหมายของโค้ด สำหรับข้อมูลเพิ่มเติมเกี่ยวกับ `rustfmt` ดูได้ที่ [คู่มือของ rustfmt ][rustfmt]

[rustfmt]: https://github.com/rust-lang/rustfmt

### แก้ไขโค้ดของคุณด้วย `rustfix`

เครื่องมือ rustfix รวมอยู่ในการติดตั้ง Rust และสามารถแก้ไขคำเตือนของคอมไพเลอร์โดยอัตโนมัติ ในกรณีที่มีวิธีแก้ไขปัญหาที่ชัดเจนและน่าจะเป็นสิ่งที่คุณต้องการ เป็นไปได้ว่าคุณเคยเห็นคำเตือนของคอมไพเลอร์มาก่อน ตัวอย่างเช่น พิจารณาโค้ดนี้:

<span class="filename">ชื่อไฟล์: src/main.rs</span>

```rust
fn do_something() {}

fn main() {
    for i in 0..100 {
        do_something();
    }
}
```

เรากำลังเรียกฟังก์ชัน `do_something` 100 ครั้ง แต่เราไม่เคยใช้ตัวแปร `i` ในส่วนเนื้อหาของลูป `for` Rust จะเตือนเราเกี่ยวกับเรื่องนี้:

```console
$ cargo build
   Compiling myprogram v0.1.0 (file:///projects/myprogram)
warning: unused variable: `i`
 --> src/main.rs:4:9
  |
4 |     for i in 0..100 {
  |         ^ help: consider using `_i` instead
  |
  = note: #[warn(unused_variables)] on by default

    Finished dev [unoptimized + debuginfo] target(s) in 0.50s
```

คำเตือนแนะนำให้เราใช้ `_i` เป็นชื่อแทน: เครื่องหมายขีดล่างระบุว่าเราตั้งใจให้ตัวแปรนี้ไม่ถูกใช้ เราสามารถใช้คำแนะนำนั้นโดยอัตโนมัติโดยใช้เครื่องมือ `rustfix` ด้วยการรันคำสั่ง `cargo fix`:

```console
$ cargo fix
    Checking myprogram v0.1.0 (file:///projects/myprogram)
      Fixing src/main.rs (1 fix)
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
```

เมื่อเราดูที่ *src/main.rs* อีกครั้ง เราจะเห็นว่า `cargo fix` ได้เปลี่ยนโค้ดแล้ว:

<span class="filename">ชื่อไฟล์:  src/main.rs</span>

```rust
fn do_something() {}

fn main() {
    for _i in 0..100 {
        do_something();
    }
}
```

ตัวแปรลูป `for` ตอนนี้ชื่อว่า `_i` และคำเตือนไม่ปรากฏอีกต่อไป

คุณยังสามารถใช้คำสั่ง `cargo fix` เพื่อเปลี่ยนโค้ดของคุณระหว่างฉบับ (editions) Rust ที่แตกต่างกันได้ ฉบับต่างๆ ได้กล่าวถึงใน [ภาคผนวก E][editions].

### การตรวจสอบเพิ่มเติมด้วย Clippy

เครื่องมือ Clippy เป็นชุดของการตรวจสอบเพื่อวิเคราะห์โค้ดของคุณ เพื่อให้คุณสามารถจับข้อผิดพลาดทั่วไปและปรับปรุงโค้ด Rust ของคุณได้

เพื่อติดตั้ง Clippy ให้ป้อนคำสั่งต่อไปนี้:

```console
$ rustup component add clippy
```

เพื่อเรียกใช้การตรวจสอบของ Clippy กับโครงการ Cargo ใดๆ ให้ป้อนคำสั่งต่อไปนี้:

```console
$ cargo clippy
```

ตัวอย่างเช่น สมมติว่าคุณเขียนโปรแกรมที่ใช้ค่าประมาณของค่าคงที่ทางคณิตศาสตร์ เช่น พาย (pi) ดังที่โปรแกรมนี้ทำ:

<span class="filename">ช่อไฟล์: src/main.rs</span>

```rust
fn main() {
    let x = 3.1415;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

การรัน `cargo clippy` กับโครงการนี้จะส่งผลให้เกิดข้อผิดพลาดนี้:

```text
error: approximate value of `f{32, 64}::consts::PI` found
 --> src/main.rs:2:13
  |
2 |     let x = 3.1415;
  |             ^^^^^^
  |
  = note: `#[deny(clippy::approx_constant)]` on by default
  = help: consider using the constant directly
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#approx_constant
```

ข้อผิดพลาดนี้แจ้งให้คุณทราบว่า Rust มีค่าคงที่ `PI` ที่แม่นยำกว่าอยู่แล้ว 
และโปรแกรมของคุณจะถูกต้องมากขึ้นถ้าคุณใช้ค่าคงที่นั้นแทน 
จากนั้นคุณจะเปลี่ยนโค้ดของคุณเพื่อใช้ค่าคงที่ `PI` 
โค้ดต่อไปนี้ไม่ส่งผลให้เกิดข้อผิดพลาดหรือคำเตือนใดๆ จาก Clippy:

<span class="filename">ชื่อไฟล์:  src/main.rs</span>

```rust
fn main() {
    let x = std::f64::consts::PI;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

สำหรับข้อมูลเพิ่มเติมเกี่ยวกับ Clippy ดูได้ที่ [คู่มือของ Clippy][clippy].

[clippy]: https://github.com/rust-lang/rust-clippy

### การผสานรวม IDE โดยใช้ `rust-analyzer`

เพื่อช่วยในการผสานรวม IDE ชุมชน Rust แนะนำให้ใช้ [`rust-analyzer`](https://rust-analyzer.github.io)<!-- ignore --> 
เครื่องมือนี้เป็นชุดของยูทิลิตี้ที่เน้นคอมไพเลอร์ซึ่งพูดโปรโตคอล [Language Server Protocol](http://langserver.org/)<!-- ignore -->
ซึ่งเป็นข้อกำหนดสำหรับ IDE และภาษาโปรแกรมเพื่อสื่อสารระหว่างกัน 
ไคลเอนต์ต่างๆ สามารถใช้ `rust-analyzer` ได้ เช่น [ปลั๊กอิน Rust analyzer สำหรับ Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)

[lsp]: http://langserver.org/
[vscode]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

เยี่ยมชม[หน้าหลักของโครงการ `rust-analyzer`](https://rust-analyzer.github.io)<!-- ignore --> 
สำหรับคำแนะนำในการติดตั้ง จากนั้นติดตั้งการสนับสนุน language server ใน IDE ของคุณ 
IDE ของคุณจะได้รับความสามารถต่างๆ เช่น การเติมคำอัตโนมัติ การข้ามไปยังคำจำกัดความ และข้อผิดพลาดแบบอินไลน์

[rust-analyzer]: https://rust-analyzer.github.io
[editions]: appendix-05-editions.md
