# Android Developer Challenge - Pokemon GO

##เนื้อเรื่อง
บริษัท Nextzy มีพนักงานที่เสพติดเกม Pokemon GO เป็นอย่างมาก ดังนั้นจึงไม่ค่อยแปลกใจซักเท่าไรนักที่พนักงานที่นี่จะเอาแต่เล่นเกมนี้อยู่ประจำจนงานการแทบจะไม่ได้ทำ 

แต่เนื่องจากบริษัท Nextzy เป็นบริษัทที่ทำเกี่ยวกับ Software ที่เน้นไปในด้าน Mobile และ Web จึงทำให้การเล่นเกมนี้แตกต่างไปจากผู้เล่นทั่วๆไปเล็กน้อย เพราะพวกเขาวางแผนกันเพื่อที่จะพยายามทำตัวช่วยเล่นเกมให้ง่ายขึ้น เพราะพวกเขาขี้เกียจออกตามหาโปเกมอนแบบสะเปะสะปะ ดังนั้นจึงเกิดไอเดียว่าอยากจะทำแอพที่สามารถช่วยพวกเขาสแกนหาโปเกมอนในรัศมีรอบๆพวกเขาได้ 

ชาว Nextzy ให้เวลาสองวันสามคืนร่วม 256 ชั่วโมง แต่ก็ยังทำไม่สำเร็จ เพราะมันยากเย็นเหลือเกิน บ่อยครั้งก็โดนแบน เพราะเหมือนกับว่าทางเจ้าของเกมรู้ตัวว่าพวกเราทำอะไรแผลงๆกันอยู่

แต่ชาว Nextzy ก็ยังไม่ย่อท้อ เพราะพวกเขายังมีคุณอยู่!!!!

คุณ คุณ และคุณเท่านั้น ที่ทำให้ใจผมผิดหวัง ♪ เอ้ย คุณเท่านั้นที่เป็นความหวังสุดท้ายของพวกเขา ที่จะช่วยสร้างแอพค้นหาโปเกมอนในรัศมีรอบๆได้

##เบาะแส
จากที่ลองถามข้อมูลจากพี่นิว โปรแกรมเมอร์ตาตี่อ้วนๆประจำบริษัทที่มีความรู้เกี่ยวกับ Web Service แต่ฝักใฝ่ในด้านมืด ได้บอกมาว่า 

>"ตัวเกมนี้ไม่ได้มีอะไรซับซ้อนนัก และหัวใจสำคัญอยู่ที่ Web Service ของตัวเกมล้วนๆ เพราะตัวเกมนั้นเป็นเสมือนเปลือกนอกให้คนเล่นเพลิดเพลินไปกับภาพภายในเกม แต่ทุกอย่างนั้นถูกกำหนดผ่านข้อมูลที่รับส่งระหว่าง Server ทั้งหมด"

หลังจากนั้นพี่นิวก็ทำตาตี่มากยิ่งขึ้นไปอีก พร้อมกับบอกว่า 

>"ไม่ต้องห่วง เพราะเจ้าคือ Android Dev ที่จะมาสานฝันของเราให้เป็นจริง แต่เรื่อง Web Service อาจจะเป็นเรื่องยากสำหรับเจ้า ดังนั้นเอาข้อมูลจากพี่ไป!!"

>"นี่คือ URL สำคัญที่พี่แอบดักข้อมูลแล้วแกะมันออกมา ทั้งหมดนี้แสดงผลลัพธ์ออกมาเป็น JSON ทั้งหมด"

```
http://<BASE_URL>/pokemon
http://<BASE_URL>/pokemon/login
http://<BASE_URL>/pokemon/catchable
```

ทั้ง 3 ตัวนี้คือ URL ที่จำเป็นสำเร็จภารกิจของคุณ

#### Server Status
```
GET http://<BASE_URL>/pokemon
```
เอาไว้เช็คเฉยๆว่าคุณสามารถเชื่อมต่อกับ Server ของตัวเกมได้หรือไม่

ถ้าเชื่อมต่อได้ก็จะส่ง JSON กลับมาแบบนี้
```
STATUS CODE : 200
{
  "message": "Server is active"
}
```

#### Login
```
POST http://<BASE_URL>/pokemon/login
```
```
**BODY (x-www-form-urlencoded)**
email
password
```
เอาไว้ล็อกอินเข้าสู่ระบบของเกม ใช้แค่ Email และ Password แบบโง่ๆแนบไปใน Body ตอนที่ Post Request ไปที่ URL ตัวนี้

และถ้าล็อกอินได้สำเร็จก็จะได้ Token มาเก็บไว้ เพื่อใช้ดึงข้อมูลอื่นๆต่อไป
```
STATUS CODE : 200
{
  "message": "Login Successful",
  "token": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
}
```

#### Get Catchable Pokemon
```
GET http://<BASE_URL>/pokemon/catchable
```
```
**HEADER**
pokemon_token
```
```
**QUERY PARAMETER**
latitude
longitude
```
เอาไว้ค้นหาโปเกมอนที่อยู่โดยรอบในรัศมี 100 เมตร นั่นหมายความว่า Service ตัวนี้จะต้องส่งพิกัดละติจูดและลองติจูดไปด้วย ให้ส่งแนบเป็น Query Parameter หรือ URL Parameter ได้เลย และบังคับว่าต้องแนบ Header สำหรับ Token ไปด้วย ไม่เช่นนั้นจะ Request ข้อมูลไม่ได้

โดยข้อมูลที่ได้นั้นจะเป็นข้อมูลโปเกมอนที่อยู่รอบๆในรัศมี 100 เมตร ที่จะส่งผลลัพธ์กลับมาให้แบบนี้
```
STATUS CODE : 200
{
  "message": "Success",
  "data": [
    {
      "expiration_timestamp": 1472356740449,
      "latitude": 13.74437,
      "longitude": 100.56117,
      "name": "Diglett",
      "number": "050",
      "id": "-KQEH4aBBNdxT_S8FuLj"
    }
  ]
}
```
* **expiration_timestamp** คือระยะเวลาที่โปเกมอนจะหายไปจากแผนที่หน่วย (เป็น Current Time Millis ของ Server)
* **latitude และ longitude** คือตำแหน่งของโปเกมอนตัวนั้น
* **name** ชื่อของโปเกมอน
* **number** หมายเลขของโปเกมอนตัวนั้นๆ
* **id** รหัสประจำตัวของโปเกมอนตัวนี้ เป็น Unique ID ไม่ซ้ำกับตัวอื่นๆ

ข้อมูลโปเกมอนที่อยู่รอบๆสามารถส่งกลับมาได้มากกว่า 1 ตัว

##คำเตือน
พี่นิว โปรแกรมเมอร์ตัวอ้วนๆได้บอก **ไว้ว่า ระวังโดน Server แบนได้นะ** เพราะเค้าสามารถตรวจสอบได้ ถ้าเค้ารู้ว่าแอบทำอะไรแบบนี้อยู่  Account นั้นก็จะถูกแบนทันที โดยมี JSON ส่งกลับมาดังนี้
```
STATUS CODE : 401
{
  "message": "This account was banned"
}
```

##รายละเอียดภารกิจ
* ให้สร้างแอพขึ้นมาหนึ่งตัวที่สามารถล็อกอินเข้าไปดูตำแหน่งโปเกมอนบนแผนที่ Google Map ได้
* แสดงตำแหน่งโปเกมอนบนแผนที่ด้วยไฟล์ภาพที่อยู่ใน Repository นี้ (ในโฟลเดอร์ image)
* ค้นหาโปเกมอนทั้งหมดที่มีอยู่รอบๆในรัศมี 2 กิโลเมตรของพิกัดนี้ 13.744728, 100.56340
* แสดงจำนวนโปเกมอนทั้งหมดที่ค้นหาเจอไว้ในแอพด้วย
* ในกรณีที่โปเกมอนตัวไหนจะหายไป (อิงเวลาจาก expiration_timestamp) ก็ควรจะหายไปจากบนแผนที่ด้วย (Extra requirement)
* สามารถกดที่โปเกมอนที่อยู่บนแผนที่แล้วรู้ชื่อโปเกมอนและหมายเลขของโปเกมอนนั้นๆได้
* และนี่คือ Account ที่ทุกคนในทีมได้เสียสละมอบให้คุณใช้ในการทำภารกิจ ซึ่งมีทั้งหมด 5 Account ด้วยกัน
```
ติดต่อกับทีมงานสิครับ
```
* ถ้า Account ทั้งหมดนี้โดนแบน นั่นหมายความว่า**ภารกิจล้มเหลว**นะ ดังนั้นจงคิดให้ดีๆว่า Server เค้าจะรู้ได้ยังไงว่าเราแอบใช้วิธีโกง และจงเลี่ยงมันซะเพื่อไม่ให้โดนแบน

##คำแนะนำ
* โปเกมอนที่ได้จาก Service มีโอกาสซ้ำ (เพราะรัศมีในการค้นหา) ดังนั้นระวังมันแสดงบนแผนที่ซ้ำล่ะ
* ควรอัปเดตข้อมูลจาก Service เป็นระยะๆ เนื่องจากข้อมูลเป็นแบบ Realtime อาจจะมีโปเกมอนโผล่ขึ้นมาใหม่ในตอนนั้นก็ได้

##สิ่งที่ต้องทำ
โค้ดทั้งหมดให้เก็บไว้ใน Github ของคุณเอง แล้วทยอยเขียนเรื่อยๆพร้อมกับ Push เก็บไว้ตลอดเวลา เพราะ Deadline ของแผนการแอพขั้นเทพตัวนี้ของเราคือตอน 23:59 ของวันนี้ ดังนั้นพวกเราจะดูจากเวลาที่คุณ Push ขึ้นมานั่นเอง

จงเขียนให้ดี อย่าให้มีบั๊ก และเขียนให้กระชับที่สุด ตามแบบฉบับธรรมเนียนของ Nextzy 

##All Hail Nextzy!!!!!!


