project note for agent:

material deduplication project: เป็นโปรเจคในการ deduplicate pk ของ material 
ซึ่งเป้าหมายคือ หาว่า pk ของ material มีตัวไหนที่เป็น material เดียวกันบ้าง

logic หลักๆ คือ 
- สร้างฐานข้อมูล เพื่อใช้ทำ hybrid search (sementic + keyword(bm25) + etc.) เพื่อหา candidate material ที่มีแนวโน้มสูงว่า เป็น material ชนิดเดียวกัน
- เมื่อได้ candidate แล้วจะส่งเข้าไป llm (LLM Material Grouping) เพื่อให้ llm group ว่ามี material ไหนบ้างที่เป็น material เดียวกัน
- นำผลลัพธ์ที่ได้จาก llm เข้า graph เพื่อทำการจัด group material ที่เป็นชนิดเดียวกัน ซึ่งอาจจะมากกว่า 2 material ก็ได้ เพื่อเชื่อม connectivity ทั้งหมด ระหว่าง group
เช่น A>B , B>C จะเห็นว่ามี 2 group ทั้งๆที่เป็น material เดียวกัน เมื่อเข้า group สุดท้ายจะได้ A>B>C สุดท้ายจะได้ group ของ pk ที่เป็น material ตัวเดียวกัน (pk duplicate)


-----
เยี่ยมมาก ถัดมาผมอยากลอง test llm-material-grouping หน่อย ซึ่ง อยาก test request ของ pk 7813_78060670873000 ที่คำตอบอยู่ rank 5
ซึ่งอยากให้คุณช่วยเขียน prompt ให้หน่อย ผมจะบอกจุดประสงค์ให้ ว่าที่ใช้ llm เพื่ออะไร

เพื่อ ให้ llm สามารถระบุได้ว่า request pk ที่ส่งมา ตัวหลัก มี material candidate ตัวไหนไหม ที่มันเป็นสินค้าชนิดเดียวกันจริงๆ ซึ่งอาจจะไม่มีซ้ำ (ไม่มีสินค้าชนิดเดียวกัน)ก็ได้ แต่เมื่อไหร่ก็ตามที่มี ต้องสามารถจัด group ให้ได้ ถ้าทำเป็น response_schema output format ด้วยจะดีมากๆ ส่วน output คุณช่วย design หน่อย จะเล่าต่อว่า ผมจะต้องเอาผลลัพธ์ที่ได้จาก llm ไปต่อ graph เพื่อสุดท้ายจะต้อง จัดกลุ่มสินค้าที่ซ้ำทั้งหมดจริงๆ เป็นการเชื่อม connectivity ทั้งหมด เป็น graph

ผลลัพธ์ของ prompt ดีแล้ว เพราะมันเจอตัวที่ group เดียวกันถูก แต่ผมคิดว่า output ควรปรับ เอาเฉพาะ ตัวที่ซ้ำกันก็พอ เช่น field ที่บอกว่า มี หรือ ไม่มี (match / unmatch) แล้วก็ field ที่บอกว่า pk ที่ซ้ำคือตัวไหน ก็พอ นอกจากนั้น output ที่สรุปยังผิดอยู่ 
Actual answer from edges: 7560_75060670873346
❌ Answer NOT found in any group
   Answer is NOT in candidates at all

   เพราะมันเจอ answer


