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

การเรียกใช้คำสั่งนี้จะจัดรูปแบบโค้ด Rust ทั้งหมดในเครตปัจจุบัน สิ่งนี้ควรเปลี่ยนเฉพาะรูปแบบโค้ดเท่านั้น ไม่ใช่ความหมายของโค้ด สำหรับข้อมูลเพิ่มเติมเกี่ยวกับ `rustfmt` ดูได้ที่ [เอกสารประกอบ][rustfmt].

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

You can also use the `cargo fix` command to transition your code between
different Rust editions. Editions are covered in [Appendix E][editions].

### More Lints with Clippy

The Clippy tool is a collection of lints to analyze your code so you can catch
common mistakes and improve your Rust code.

To install Clippy, enter the following:

```console
$ rustup component add clippy
```

To run Clippy’s lints on any Cargo project, enter the following:

```console
$ cargo clippy
```

For example, say you write a program that uses an approximation of a
mathematical constant, such as pi, as this program does:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = 3.1415;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

Running `cargo clippy` on this project results in this error:

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

This error lets you know that Rust already has a more precise `PI` constant
defined, and that your program would be more correct if you used the constant
instead. You would then change your code to use the `PI` constant. The
following code doesn’t result in any errors or warnings from Clippy:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = std::f64::consts::PI;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

For more information on Clippy, see [its documentation][clippy].

[clippy]: https://github.com/rust-lang/rust-clippy

### IDE Integration Using `rust-analyzer`

To help IDE integration, the Rust community recommends using
[`rust-analyzer`][rust-analyzer]<!-- ignore -->. This tool is a set of
compiler-centric utilities that speaks the [Language Server Protocol][lsp]<!--
ignore -->, which is a specification for IDEs and programming languages to
communicate with each other. Different clients can use `rust-analyzer`, such as
[the Rust analyzer plug-in for Visual Studio Code][vscode].

[lsp]: http://langserver.org/
[vscode]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

Visit the `rust-analyzer` project’s [home page][rust-analyzer]<!-- ignore -->
for installation instructions, then install the language server support in your
particular IDE. Your IDE will gain abilities such as autocompletion, jump to
definition, and inline errors.

[rust-analyzer]: https://rust-analyzer.github.io
[editions]: appendix-05-editions.md
