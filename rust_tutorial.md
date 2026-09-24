# Rust Tutorial Project — Principles of Programming Languages

> **สำหรับนักศึกษา:** ใช้ไฟล์นี้เป็น Template สำหรับจัดทำบทเรียน Rust ของกลุ่ม  
> **Topic No.:** `21`  
> **Topic Name:** `Smart Pointer & Resource Management`  
> **Group No.:** `21`

---

## 1. Members

| # | Name | Student ID | GitHub Username | Main Responsibility |
|---|---|---|---|---|
| 1 | `วีรภัทร พิริยะสถิต` | `670710653` | `@670710653` | Concept + Code |
| 2 | `อาธารดา พรหมแทนสุด` | `670710654` | `@670710654` | Code + Demo |
| 3 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Rust vs Other Language + PPL |
| 4 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Exercises + Common Mistakes |

---

## 2. Learning Objectives

หลังจากศึกษา Topic นี้แล้ว ผู้เรียนสามารถ:

1. `[อธิบายแนวคิดสำคัญได้]`
2. `[เขียนโปรแกรม Rust ที่เกี่ยวข้องได้]`
3. `[วิเคราะห์พฤติกรรม/กฎของภาษาได้]`
4. `[เปรียบเทียบ Rust กับภาษาอื่นได้]`

---

## 3. Introduction


`ในระบบการจัดการหน่วยความจำของภาษา Rust นักพัฒนาสามารถเลือกใช้กลไกการอ้างอิงตำแหน่งหน่วยความจำ (Pointer) ได้ 2 รูปแบบหลัก`

`แบบที่1 การอ้างอิงแบบปกติ (Normal References / Raw Pointers)
ตัวชี้วัดหรือการอ้างอิงประเภท &T และ &mut T ทำหน้าที่เป็นเพียงดัชนีระบุตำแหน่งหน่วยความจำมีขนาดคงที่เท่ากับความกว้างของสถาปัตยกรรมหน่วยประมวลผล (8 ไบต์ สำหรับระบบ 64-bit) 
โดยมีบทบาทภายใต้กฎการยืมข้อมูลเท่านั้น ไม่มีสิทธิ์การเป็นเจ้าของทรัพยากรและไม่มีการทำงานส่วนเกินใด ๆ ซ่อนอยู่เบื้องหลัง`

`แบบที่2 Smart Pointers
โครงสร้างข้อมูลประเภท Structs เช่น Box<T>, Rc<T>, และ RefCell<T> ซึ่งนอกจากจะทำหน้าที่ชี้ตำแหน่งหน่วยความจำแล้ว 
ยังทำหน้าที่เป็นผู้ถือครองสิทธิ์ในทรัพยากรนั้นๆ พร้อมทั้งมีกลไกที่ถูกควบคุมผ่านโปรแกรมเบื้องหลัง เพื่อบริหารจัดการLifecycleของข้อมูล และสิทธิ์การเข้าถึงข้อมูลให้เป็นไปอย่างปลอดภัย`

---

## 4. Key Concepts

### 4.1 `[หลักการทำงานของSmart Pointer และ ลักษณะเฉพาะ]`

**คำอธิบาย**

`Smart Pointer ไม่ใช่แค่การชี้ไปยังตำแหน่งหน่วยความจำเหมือนพอยเตอร์ทั่วไปแต่เป็น โครงสร้างข้อมูลที่จำลองพฤติกรรมของพอยเตอร์ โดยมีคุณลักษณะเด่น 3 ประการ:
การเป็นเจ้าของทรัพยากร: พอยเตอร์ปกติ (&T) ทำหน้าที่เพียงแค่ "ยืม" ข้อมูลมาใช้งาน แต่ Smart Pointer จะทำหน้าที่เป็น "เจ้าของ" ข้อมูลก้อนนั้นโดยตรง มันมีสิทธิ์ขาดในการควบคุมวงจรชีวิตของข้อมูล
การเพิ่มคุณลักษณะและความสามารถ: มันถูกออกแบบมาเพื่อแก้ข้อจำกัดของระบบหน่วยความจำ เช่น การย้ายข้อมูลไปไว้บน Heap อัตโนมัติ (Box), การแชร์ข้อมูลให้เจ้าของหลายคน (Rc/Arc), หรือการปลดล็อกให้แก้ไขข้อมูลในตัวแปรที่ถูกห้ามแก้ (RefCell)
ผู้ใช้งานสามารถเรียกใช้ข้อมูลผ่าน Smart Pointer ได้เสมือนเป็นตัวแปรธรรมดา โดยไม่ต้องสั่งเปิด-ปิด หรือคำนวณตำแหน่งหน่วยความจำด้วยตัวเองในโค้ด`

**ตัวอย่าง**

```rust

```

**Explanation**

``

---

### 4.2 `[Memory allocation of Smart pointer]`

`โดยทั่วไป Pointer จะถูกออกแบบให้ใช้พื้นที่ 8bytes ในสถาปัตยกรรมของ CPU 64-bit แต่ Smart Pointer นั้นจะทำให้เกิด Overhead แบบหลีกเลี่ยงไม่ได้ในทั้ง Heap และ Stack
Stack Overhead คือพื้นที่หน่วยความจำที่ตัวควบคุม Smart Pointer ใช้จัดเก็บสถานะระบบ ตัวอย่างเช่น RefCell<T> จะใช้หน่วยความจำเพิ่มเติม 8 ไบต์บน Stack 
เพื่อเก็บตัวแปรบ่งชี้สถานะการยืม (Borrow Flag) สำหรับตรวจสอบความปลอดภัยของการเข้าถึงข้อมูลในขณะประมวลผล
Heap Overhead คือหน่วยความจำส่วนเกินที่ถูกจัดสรรควบคู่ไปกับข้อมูลจริงบน Heap เช่น Rc<T> และ Arc<T> 
ซึ่งระบบจะสร้างตัวนับจำนวนการอ้างอิงอิมพูเตชันภายนอก เพื่อประเมินรอบเวลาในการคืนสภาพหน่วยความจำอัตโนมัติ`

```rust
use std::mem::size_of;
use std::cell::RefCell;
fn main() {
    println!("=== Size Comparison of Stack ===");

    let raw_data: i32 = 42;
    println!("Size of i32 :  {} bytes", size_of::<i32>());

    let reference: &i32 = &raw_data;
    println!("Size of &i32 (Normal Pointer): {} bytes", size_of::<&i32>());

    let ref_cell: RefCell<i32> = RefCell::new(42);
    println!("Size of RefCell<i32> (Smart Pointer): {} bytes", size_of::<RefCell<i32>>());
}
```
**Explanation**

`2 บรรทัดแรกเป็นการเรียกใช้ Library พื้นฐานในการเรียกดูขนาดของข้อมูล
หลักการทำงานในบรรทัด let raw_data กำหนดแปรเป็น int32 ขนาด bit ให้เก็บค่า 42
และทำการ print เพื่อดูขนาดของตัวแปรซึ่งจะให้ขนาด 4bytes
ต่อมาสร้างตัวแปรชื่อ reference ที่เป็นตัว int ให้ชี้ไปที่ raw_data และจะ print เพื่อแสดงขนาดของ pointer(ขนาดของตัวแปรreference) ซึ่งจะให้ผลลัพธ์เป็น 8bytes ตามขนาดสถาปัตยกรรมของ 64-bit เพื่อชี้ทางไปหาข้อมูลตัวอื่น
ตัวแปรตัวสุดท้ายอย่าง ref_cell หรือก็คือตัว Smart pointer ที่จะเก็บค่า int และกำหนดค่าเป็น 42 พอใช้คำสั่ง Print จะให้ขนาดเป็น 16bytes`

`หมายเหตุ 1 (เนื่องจากในภาษา Rust ไม่ยอมให้มีค่า Null แบบใน Java เราเลยต้องกำหนดค่าก่อนเพราะ Smart Pointer ไม่ใช่ตัวแปรทั่วไปแต่มันเป็น Object ชนิดนึงแทนที่เราจะประกาศค่าปกติเหมือนตัวแปรแรกเราประกาศให้ Smart Pointer ของเราเป็นตัวที่เก็บค่านั้นเลย
และเวลาใครจะใช้ค่าในนี้ค่อย Point มาหาและเพราะว่า Smart Pointer เป็นเจ้าของข้อมูลมันจริงอัพเดทค่าในตัวมันได้ด้วย แต่ต้องแลกกับการที่ขนาดของ Smart Pointer ในหน่วยความจำนั้นมีขนาดใหญ่มากกว่าการประกาศตัวแปรทั่วไป)`

`หมายเหตุ 2 (ขนาดจริงๆของ Smart Pointer ในตัวอย่างนี้คือ 12 bytes แบ่งได้ตามนี้ Refcell<i32> = 8 bytes เป็นการจองพื้นที่บน stack , i32 = 4 bytes เป็นการจองพื้นที่บน heap เหตุผลที่ว่าผลลัพธ์เป็น 16bytes เกิดจากการตัวจัดสรรหน่วยความจำมักจะทำงานได้ลำบากหาก byte เหล่านั้นไม่สามารถหารด้วย8ลงตัว)`

---

### 4.3 `[การใช้Box<T>เบื้องต้น]`

`[อธิบายแนวคิด]`

```rust
// Rust code
```

---

### 4.4 `[การใช้Rc<T>เบื่องต้น]`

`[อธิบายแนวคิด]`

```rust
// Rust code
```

---

### 4.5 `[การใช้Refcell<T>เบื่องต้น]`

`[อธิบายแนวคิด]`

```rust
// Rust code
```

---

## 5. Important Syntax / Rules

| Syntax / Rule | Meaning | Example |
|---|---|---|
| `Box<T>` | `สร้างข้อมูลบน Heap แทนการสร้างบน Stack` | `ใช้เมื่อไม่ทราบขนาดของข้อมูลขณะ Runtime หรือต้องการจะส่งต่อความเป็นเจ้าของของข้อมูลโดยไม่ต้อง Copy ค่าเหล่านั้น` |
| `Rc<T>` | `อนุญาตให้มีตัวแปรหลายตัวเป็นเจ้าของค่านี้รวมถึงการอ้างอิงแบบปกติด้วย` | `เมื่อหลายๆส่วนในโปรแกรมต้องการอ่านค่าเดียวกันแต่ไม่ทราบว่าส่วนไหนจะใช้เป็นคนสุดท้าย` |
| `RefCell<T>` | `บังคับใช้กฏการยืมนะตอน Runtime แทนที่จะเป็นตอน compile` | `เมื่อต้องการจะเปลี่ยนแปลงค่าแม้ว่าค่านั้นจะเป็นค่าอ้างอิง(Pointer)ที่เปลี่ยนแปลงไม่ได้` |

### Important Rules

1. `Box<T> บังคับให้มีเจ้าของข้อมูลได้แค่คนเดียวและเมื่อเจ้าของค่านั้นหลุดนอกขอบเขตของค่าที่กำหนดเอาไว้ระบบจะทำการคืนพื้นที่ heap นั้นๆทันที`
   
2. `Rc<T> จะคอยนับว่ามีตัวแปรตัวไหนถือครองค่านี้บ้างและจะมีระบบป้องกัน Data races(การแย่งกันใช้ข้อมูล) หรือการทำให้ข้อมูลส่วนนั้นเกิดความเสียหายด้วยการที่ตัวแปรเหล่านั้นจะทำได้แค่ถือครองค่าแต่จะไม่สามารถแก้ไขค่าใน Rc<T> ได้และจะคืนพื้นที่บนหน่วยความจำเมื่อไม่มีใครอ้างอิงค่าในนี้`
 
3. `Refcell<T> ในภาษา Rust การกำอ้างอิงหรือยืมค่าจะต้องระบุให้ชัดเจนว่าค่านี้เป็นค่าที่เปลี่ยนแปลงได้หรือไม่ได้มิฉะนั้นจะไม่สามารถ Compile ได้ แต่ Refcell<T> สามารถเปลี่ยนแปลงค่าเหล่านั้นขณะ Runtime ได้ด้วยวิธี Interior mutability ข้อควรระวังเพราะ Refcell<T> จะบังคับการยืมค่าตอน Runtime มันจะคอยเช็คเสมอว่าใครกำลังยืม .borrow() หากพยายามจะยืมค่าในขณะที่กำลังมีค่าอื่นใช้งานอยู่ Refcell<T> จะเกิดอาการลกส่งผลให้ Program Crash ในทันที เพื่อป้องกันอาการลกของ Refcell<T> ต้องทำให้มั่นใจว่าไม่มีใครยืมค่านั้นก่อนจะทำการยืมค่านั้นเสมอ`

---

## 6. Runnable Code Examples

> **ข้อกำหนด:** Code ทุกตัวต้อง Compile และ Run ได้จริงก่อนนำมาใส่ในเอกสาร

### Example 1 — `Box<T>`

**Purpose:** `เก็บข้อมูลบน Heap แทน Stack และแก้ปัญหา recursive type ที่ compiler ไม่รู้ขนาดล่วงหน้า (เช่น Linked List)`

```rust
#[derive(Debug)]
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};

fn sum_list(list: &List) -> i32 {
    match list {
        Cons(value, next) => value + sum_list(next),
        Nil => 0,
    }
}

fn main() {
    // สร้าง linked list: 1 -> 2 -> 3 -> Nil
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));

    println!("List: {:?}", list);
    println!("Sum of list = {}", sum_list(&list));

    // ตัวอย่างง่าย ๆ อีกแบบ: เก็บค่าธรรมดาไว้บน heap
    let boxed_number = Box::new(42);
    println!("Boxed number = {}", boxed_number);
    // boxed_number จะถูก deallocate อัตโนมัติเมื่อออกจาก scope (Drop)
}
```

**Expected Output**

```text
List: Cons(1, Cons(2, Cons(3, Nil)))
Sum of list = 6
Boxed number = 42
```

**Explanation**

<ul>
    <li>enum List { Cons(i32, Box<List>), Nil } — ถ้าไม่มี Box ตรงนี้ Rust จะ error ทันที เพราะ List จะมีขนาดไม่จำกัด (แต่ละ Cons มี List อีกตัวซ้อนอยู่ข้างใน ไม่รู้จบ) การใส่ Box<List> ทำให้ Rust รู้ขนาดที่แน่นอน เพราะ Box คือ pointer ที่มีขนาดคงที่ (ชี้ไปยัง heap)</li>
    <li>sum_list() recursive function เดินไล่ตาม pointer ไปเรื่อย ๆ จนเจอ Nil</li>
    <li>Box::new(42) คือตัวอย่างง่าย ๆ ของการย้ายค่าไปเก็บบน heap แล้วเมื่อ boxed_number หมด scope มันจะถูก deallocate อัตโนมัติ (ผ่าน Drop ที่ Rust ทำให้ built-in)</li>
</ul>


---

### Example 2 — `Rc<T>`

**Purpose:** `แชร์ข้อมูลเดียวกันระหว่างหลาย "เจ้าของ" (multiple owners) แบบ single-thread โดยนับจำนวนผู้ถืออ้างอิง (reference counting)`

```rust
use std::rc::Rc;

#[derive(Debug)]
struct Owner {
    name: String,
}

fn main() {
    let owner = Rc::new(Owner {
        name: String::from("Shared Resource"),
    });

    println!("Reference count after creation = {}", Rc::strong_count(&owner));

    // clone() ที่นี่ไม่ได้ copy ข้อมูลจริง แค่เพิ่มตัวนับ (increment counter)
    let owner_clone1 = Rc::clone(&owner);
    println!("Reference count after clone1 = {}", Rc::strong_count(&owner));

    {
        let owner_clone2 = Rc::clone(&owner);
        println!("Reference count after clone2 = {}", Rc::strong_count(&owner));
        println!("owner_clone2 points to: {:?}", owner_clone2);
    } // owner_clone2 หมด scope ตรงนี้ ตัวนับจะลดลง

    println!("Reference count after clone2 dropped = {}", Rc::strong_count(&owner));
    println!("owner = {:?}, owner_clone1 = {:?}", owner, owner_clone1);
}
```

**Expected Output**

```text
Reference count after creation = 1
Reference count after clone1 = 2
Reference count after clone2 = 3
owner_clone2 points to: Owner { name: "Shared Resource" }
Reference count after clone2 dropped = 2
owner = Owner { name: "Shared Resource" }, owner_clone1 = Owner { name: "Shared Resource" }
```

**Explanation**

<ul>
    <li>Rc::new(...) สร้างข้อมูลบน heap พร้อม counter เริ่มต้นที่ 1</li>
    <li>Rc::clone(&owner) ไม่ได้ copy ข้อมูลจริง แค่เพิ่มตัวเลขนับ (strong count) — นี่คือจุดต่างสำคัญจาก .clone() ของ type ทั่วไป</li>
    <li>เมื่อ owner_clone2 หลุด scope (ปิด {}) ตัวนับลดลงอัตโนมัติ เพราะ Rc implement Drop ไว้ให้แล้ว</li>
    <li>ข้อมูลจริงจะถูกลบก็ต่อเมื่อ ตัวนับกลับมาเป็น 0 เท่านั้น (คือเมื่อ owner ทุกตัวหมด scope)</li>
</ul>

---

## 7. Common Mistakes

### Mistake 1 — `[ชื่อข้อผิดพลาด]`

**Problem**

`[อธิบายปัญหา]`

**Incorrect Code**

```rust
// Incorrect example
```

**Correct Code**

```rust
// Correct example
```

**Why?**

`[อธิบายสาเหตุ]`

---

### Mistake 2 — `[ชื่อข้อผิดพลาด]`

**Problem**

`[อธิบายปัญหา]`

**Incorrect Code**

```rust
// Incorrect example
```

**Correct Code**

```rust
// Correct example
```

**Why?**

`[อธิบายสาเหตุ]`

---

## 8. Exercises

> จัดทำแบบฝึกหัด **2 ข้อ** ที่สอดคล้องกับ Topic และมีระดับความยากเหมาะสม

### Exercise 1 — `[ชื่อโจทย์]`

**Problem**

`[เขียนโจทย์]`

**Hint**

`[คำใบ้]`

**Solution**

```rust
// Solution code
```

**Explanation**

`[อธิบายแนวทางแก้]`

---

### Exercise 2 — `[ชื่อโจทย์]`

**Problem**

`[เขียนโจทย์]`

**Hint**

`[คำใบ้]`

**Solution**

```rust
// Solution code
```

**Explanation**

`[อธิบายแนวทางแก้]`

---

## 9. PPL Perspective

> **ส่วนนี้เป็นหัวใจของรายวิชา Principles of Programming Languages**

วิเคราะห์ Topic นี้ในมุมมองของ Programming Languages

### 9.1 Syntax

`[Topic นี้เกี่ยวข้องกับ syntax อย่างไร]`

### 9.2 Semantics

`[คำสั่ง/construct เหล่านี้มีความหมายหรือพฤติกรรมอย่างไร]`

### 9.3 Type System

`[เกี่ยวข้องกับ type system อย่างไร ถ้ามี]`

### 9.4 Memory / Resource Management

`[เกี่ยวข้องกับ memory หรือ resource management อย่างไร ถ้ามี]`

### 9.5 Abstraction / Other PPL Concepts

`[อธิบาย abstraction, scope, binding, paradigm หรือแนวคิด PPL อื่นที่เกี่ยวข้อง]`

### 9.6 Why Rust?

`[Rust ใช้แนวคิดนี้เพื่อเพิ่ม safety, reliability หรือ performance อย่างไร]`

---

## 10. Rust vs. Other Language

**Comparison Language:** `[Python / C / C++ / Java / Kotlin / ...]`

| Aspect | Rust | Other Language |
|---|---|---|
| Syntax | `[อธิบาย]` | `[อธิบาย]` |
| Semantics / Behavior | `[อธิบาย]` | `[อธิบาย]` |
| Type System | `[อธิบาย]` | `[อธิบาย]` |
| Memory Management | `[อธิบาย]` | `[อธิบาย]` |
| Safety | `[อธิบาย]` | `[อธิบาย]` |

### Rust Example

```rust
// Rust code
```

### `[Other Language]` Example

```python
# Other language code
```

### Analysis

`[อธิบายความแตกต่างที่สำคัญ และเหตุผลด้านการออกแบบภาษา]`

---

## 11. Teach Your Topic

การนำเสนอมีสมาชิก **4 คน คนละประมาณ 5 นาที**

| Member | Responsibility | Time |
|---|---|---:|
| Member 1 | Concept + Short Code Illustration | 5 min |
| Member 2 | Detailed Code + Live Demo | 5 min |
| Member 3 | Rust vs Other Language + PPL Analysis | 5 min |
| Member 4 | Exercises + Common Mistakes + Challenge | 5 min |

### Individual Contribution

**Member 1**

`[สิ่งที่รับผิดชอบ]`

**Member 2**

`[สิ่งที่รับผิดชอบ]`

**Member 3**

`[สิ่งที่รับผิดชอบ]`

**Member 4**

`[สิ่งที่รับผิดชอบ]`

> สมาชิกทุกคนต้องสามารถอธิบาย Code ของกลุ่มได้ ไม่ใช่เฉพาะส่วนที่ตนเองเขียน

---

## 12. References

> แนะนำให้มีอย่างน้อย **4 แหล่งอ้างอิง** และควรใช้เอกสารทางการเป็นหลัก

1. `[The Rust Programming Language — Rust Book]`
2. `[Rust by Example / Rust Reference]`
3. `[Official documentation ที่เกี่ยวข้องกับ Topic]`
4. `[แหล่งอ้างอิงเพิ่มเติม]`

---

## 13. AI Usage Declaration

สามารถใช้ AI เป็นเครื่องมือช่วยเรียนรู้และพัฒนาได้ แต่สมาชิกทุกคนต้องเข้าใจและสามารถอธิบายผลงานของกลุ่มได้

| AI Tool | Purpose | How the Result Was Verified |
|---|---|---|
| `Claude` | `ออกแบบโค้ดตัวอย่าง` | `ตรวจสอบโดยการนำมา run ผ่านโปรแกรมและเว็บไซต์ Rust Playground` |
| `[AI tool]` | `[ใช้เพื่ออะไร]` | `[ตรวจสอบอย่างไร]` |

### Declaration

- [ ] Code ทุกส่วนที่นำเสนอได้รับการ Compile และทดสอบแล้ว
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ที่นำเสนอได้
- [ ] ตรวจสอบข้อมูลจากแหล่งอ้างอิงที่น่าเชื่อถือแล้ว
- [ ] ระบุการใช้ AI อย่างโปร่งใส

**รายละเอียดการใช้ AI**

`[อธิบายว่าใช้ AI ในขั้นตอนใด และสมาชิกตรวจสอบผลลัพธ์อย่างไร]`

---

## 14. GitHub Contribution

| Member | Issues | Commits | Pull Requests | Code Reviews | Contribution |
|---|---:|---:|---:|---:|---|
| Member 1 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 2 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 3 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 4 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |

### Teamwork Reflection

**How did your team collaborate?**

`[อธิบายกระบวนการทำงานร่วมกัน]`

**Problems encountered**

`[ปัญหาที่พบ]`

**How did you solve them?**

`[วิธีแก้ปัญหา]`

---

## 15. Final Checklist

- [ ] Learning Objectives ครบ 3–4 ข้อ
- [ ] Key Concepts ครบถ้วน
- [ ] Syntax / Rules
- [ ] Runnable Code Examples
- [ ] Code Compile และ Run ได้จริง
- [ ] Common Mistakes
- [ ] Exercises 2 ข้อ พร้อม Solutions
- [ ] PPL Perspective
- [ ] Rust vs Other Language
- [ ] References อย่างน้อย 4 แหล่ง
- [ ] AI Usage Declaration
- [ ] GitHub Contribution
- [ ] สมาชิกทั้ง 4 คนมีส่วนร่วม
- [ ] สมาชิกทั้ง 4 คนพร้อมนำเสนอคนละ 5 นาที
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ของกลุ่มได้

---

## Submission Information

**Repository:** `[GitHub repository URL]`

**Chapter Path:** `[เช่น chapters/01-introduction/]`

**Final PR:** `#[PR number]`

**Submitted by:** `[Group XX]`

**Date:** `[YYYY-MM-DD]`
