# Change Preventers

การแก้ไขอะไรจุดหนึ่งในโค้ด บางครั้งมันก็ส่งผลต่อส่วนอื่น ๆ ของโปรแกรม ทำให้ต้องตามไปแก้ในจุดอื่นต่อด้วย ซึ่งทำให้การพัฒนานั้นซับซ้อน และมีต้นทุนที่สูง

## สารบัญ
* [Divergent Change](#divergent-change)
* [Shotgun Surgery](#shotgun-surgery)
* [Parallel Inheritance Hierarchies](#parallel-inheritance-hierarchies)

## Divergent Change
หมายถึงการแก้ไขหลาย ๆ จุดเพื่อที่จะปรับคลาสเพียงคลาสเดียว
### สัญญาณ และอาการ
เมื่อพบว่าต้องแก้ไขจุดที่ไม่เกี่ยวข้องกันหลาย ๆ จุดเพื่อที่จะปรับคลาสนั้น เช่น เพิ่มตัวแปรที่มี Type แบบใหม่ ซึ่งต้องทำการแก้ไข Method เพื่อให้ร้องรับกับตัวแปรใหม่นี้ด้วย

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/divergent-change-1.png" />
</p>

### เหตุผลของปัญหา
การออกแบบโครงสร้างโปรแกรมที่แย่ หรือการคัดลอกโค้ดจากที่อื่นมาแปะมั่วซั่วนั้นมักทำให้เกิดปัญหา
### การแก้ไขปัญหา
* แบ่งออกเป็นคลาสย่อยตามพฤติกรรมของคลาสนั้น เพื่อให้คลาสแต่ละคลาสเป็นไปตามหลักการ Single Responsibility Principle ทำให้โค้ดมีความชัดเจน เข้าใจง่าย และมีความน่าเชื่อถือมากขึ้นด้วย
* แต่ถ้าคลาสคนละคลาสมีพฤติกรรมที่ใกล้เคียงกัน อาจจะต้องรวมคลาสโดยการใช้ความสามารถ Inheritance เช่น วิธี Extract Superclass โดยการสร้างคลาสหนึ่งขึ้นมาโดยมีมีพฤติกรรมของคลาสเป็นพฤติกรรมที่เหมือนกันของคลาสที่ต้องการรวมกันนั้น แล้วค่อยให้คลาสที่คลาสกันนั้น Extends ออกมาอีกที หรือแบบคือวิธี Extract Subclass ซึ่งเหมาะกับกรณีที่คลาสหนึ่งมีพฤติกรรมบางส่วนเหมือนกับอีกคลาส ก็ให้คลาสนี้ Extends อีกคลาสไปเลย
### ผลที่ได้
* ปรับปรุงให้การจัดการโค้ดง่ายขึ้น
* ลดการซ้ำซ้อนของโค้ด
* ทำให้คนอื่นเข้ามาช่วยเขียนต่อได้ง่ายขึ้น

## Shotgun Surgery
หมายถึงการแก้ไขจุดหนึ่งเพื่อที่จะปรับหลายคลาสไปพร้อม ๆ กัน (ตรงข้ามกับ Divergent Change)
### สัญญาณ และอาการ
การแก้ไขที่แก้แค่เพียงจุดเล็ก ๆ ที่ส่งผลต่อคลาสอื่น ๆ หลายคลาส

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/shotgun-surgery-1.png" />
</p>

### เหตุผลของปัญหา
บางครั้งการการแบ่งคลาสเป็นคลาสย่อยมากเกินไปอย่าง Divergent Change ก็อาจกลายเป็นปัญหาได้อีกเหมือนกัน
### การแก้ไขปัญหา
รวมคลาสที่มีพฤติกรรมคล้ายกันเข้ากับอีกคลาสหนึ่งไปเลย (Move Method และ Move Field)หรือถ้าแต่ละคลาสไม่เหมาะที่จะเอาคลาสอื่นมารวมก็ให้สร้างคลาสใหม่ขึ้นแล้วย้ายเอาพฤติกรรมที่คล้ายกันน้ันไปรวมกันในนั้นแทน แล้วถ้าปรากฎว่าเหลือโค้ดอยู่เพียงเล็กน้อยในคลาสจากการย้่ายก็ให้ย้ายไปรวมกันให้หมดเลย (Inline Class)
### ผลที่ได้
* ปรับปรุงให้การจัดการโค้ดง่ายขึ้น
* ลดการซ้ำซ้อนของโค้ด
* ดูแลรักษาโค้ดง่าย

## Parallel Inheritance Hierarchies
### สัญญาณ และอาการ
เมื่อสร้างคลาสย่อยสำหรับคลาสหนึ่งแล้วพบว่าจะต้องไปสร้างคลาสย่อยเพื่อคลาสอื่น ๆ อีกเหมือนกัน

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/parallel-inheritance-hierarchies-1.png" />
</p>

### เหตุผลของปัญหา
มันยังดี และไม่ใช่ประเด็นเมื่อโครงสร้างยังเล็ก ๆ แต่นับวันเมื่อเพิ่มคลาสเข้าไปเรื่อย ๆ การแก้ไขก็ยากขึ้นเรื่อย ๆ เหมือนกัน
### การแก้ไขปัญหา
เมื่อสร้างคลาสย่อยสำหรับคลาสหนึ่งแล้วอีกคลาสที่คล้ายกัน เดิมที่จะต้องไปสร้างคลาสย่อยเหมือนกันก็อาจจะใช้วิธีย้ายพฤติกรรมของคลาสย่อยรวมเข้ากับคลาสหลักนั้นไปเลย (Move Method และ Move Field)
### ผลที่ได้
* ลดการซ้ำซ้อนของโค้ด
* สามารถช่วยปรับปรุงให้การจัดการโค้ดง่ายขึ้น