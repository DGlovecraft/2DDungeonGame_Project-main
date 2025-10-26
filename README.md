# 2D DungeonGame Present 
>---หัวใจหลัก---
>>- ฉากเข้าเกม  
>>- ฉากการเล่น 5 ฉาก  
>> - ตัวละครสามารถเดินเปลี่ยนฉากได้ภายในการเล่นเกม  
>>- ภายในแต่ละฉากต้องมีศัตรูเดินภายในฉาก  
>>- ผู่เล่นสามารถโจมตีศัตรู และทำให้ศัตรูตายได้  
>>- ผู้เล่นสามารถได้รับความเสียหายจากศัตรู และตายได้  
>>- ฉากเกมโอเวอร์

 ## ส่วนที่1
 ### `สร้างScene`
 ที่ Assets เราจะสร้าง Scene ขึ้นมา 8 อัน เพื่อใช้สร้างด่านต่างๆที่แตกต่างกันไป พร้อมกับ หน้าเมนูและหน้าจบเกม  
 โดยเราสามารถ สร้าง Scene ใหม่ ได้ด้วยการ กดปุ่มบวกข้างล่าง Project เพื่อเพิ่ม Scene ได้ 

 <img width="923" height="392" alt="image" src="https://github.com/user-attachments/assets/f0dc8c70-0e53-4726-ae0f-e219eb2c1bef" />

  ### `ฉากเข้าเกม`
 <img width="838" height="470" alt="image" src="https://github.com/user-attachments/assets/c895d8ec-c4e5-4b42-be92-23c8374e1421" />

 DISPLAY ฉากเข้าเกม   
 จะโชว์  
 ปุ่ม `Play`  
 ปุ่ม `Tutorial`  
 ปุ่ม `Quit`  

 ### `การทำงาน-ฉากเข้าเกม`  
 โดยจะแบ่งออกเป็น 3 ปุ่ม `Play` `Tutorial`  `Quit`  

#### 1 เมื่อกด ปุ่ม `Play` จะ โหลดไปที่ Scene `level1`

 <img width="838" height="465" alt="image" src="https://github.com/user-attachments/assets/3bff7d12-e115-44cc-a727-aa41bbe3d935" />

 โดยเราจะทำการย้ายจาก หน้า Menu ไปที่ level 1 ได้ดังนี้

 ที่ Hierarchy ของ Scene `Menu`
 เราจะทำการ สร้างไฟล์ `MenuManager` เพื่อเก็บส่วนของ Canvas หน้า UI Menu ทั้งหมด ซึ่งประกอบไปด้วย `Start` `Quit-button` `Description - Button`  
 <img width="426" height="256" alt="image" src="https://github.com/user-attachments/assets/f0606184-3b1c-459b-a2f5-288be1e36cf8" />  

 ที่ `Start` เราจะทำการสร้าง Button(Lagacy) สำหรับการกดขึ้นมา  
 ที่ไฟล์สคริป `LogicScript.cs` ทำการเขียน

     public void GoToScene(string sceneName)
    {
        SceneManager.LoadScene(sceneName);
    }  

  
 ใน `Inspector` จะมีฟังก์ชัน On click() เพื่อกำหนดว่า ถ้ากด จะเกิดอะไรขึ้น
 ให้เราทำการเลือกฟังก์ชั่น `LogicScipt.GoToScene` และใส่ชื่อ Scene ที่เราอยากให้เปลี่ยนหน้า ในที่นี้ เราอยากไปที่ Scene `Level1`
 
 <img width="443" height="618" alt="image" src="https://github.com/user-attachments/assets/c1a54b68-626a-469a-952d-102f66837fc4" />       

 

 #### 2 เมื่อกด ปุ่ม `Tutorial` จะทำการเปิด Scene `Description`
 <img width="860" height="453" alt="image" src="https://github.com/user-attachments/assets/3de81e0a-2520-4612-a216-7009d93e3acc" />  

 โดยเราสามารถทำการย้ายมาหน้า Descrition ได้ดังนี้  
 ให้เราทำการเลือกฟังก์ชั่น `LogicScipt.GoToScene` และใส่ชื่อ Scene ที่เราอยากให้เปลี่ยนหน้า ในที่นี้ เราอยากไปที่ Scene `Description`
 <img width="422" height="556" alt="image" src="https://github.com/user-attachments/assets/be6e4540-8bb1-4b79-bd6c-944c7004707e" />  

 ### 3 เมื่อกด ปุ่ม `Quit` จะทำการปิดเกม หรือ ออกจากเกม
 โดยเราสามารถทำได้ดังนี้  
 ที่ไฟล์สคริป `LogicScript.cs` ทำการเขียน  

     public void QuitGame()
    {
        #if UNITY_EDITOR
            // ถ้าอยู่ใน Unity Editor จะหยุด Play Mode
            UnityEditor.EditorApplication.isPlaying = false;
        #else
            // ถ้าเป็น Build จริง จะปิดแอพ
            Application.Quit();
        #endif
        
        Debug.Log("Game Quit!");
    }

  และนำนำ ฟังก์ชันก์ `QuitGame` ไปใส่ใน `Inspector` ของ Quit - button  
  <img width="440" height="562" alt="image" src="https://github.com/user-attachments/assets/0ee263cd-5dfb-48cc-8989-6305edca0a38" />


   ## ส่วนที่2
 ### `ตัวละครเดินเปลี่ยนฉาก`  
 ที่ `Hierarchy` ของ Scene `Level1`  
 ทำการสร้าง Gameobject ชื่อ Door  
 ทำการเพิ่มคอมโพเนนท์ให้กับ Door

- Rigibody 2D ตั้งค่า Body Type → Kinematic
- Box Collider 2D ตั้งค่า IsTrigger → true แล้วทำการปรับ Collider ให้เล็กลงมาดังภาพ  
<img width="356" height="313" alt="image" src="https://github.com/user-attachments/assets/2691c6a9-573b-4967-befa-8d83e239e691" />

สร้างไฟล์ `Door.cs`  

<img width="593" height="498" alt="image" src="https://github.com/user-attachments/assets/7652d1fd-c5ff-4611-8c15-23337d692d68" />

ทำการสร้างตัวแปร `lvlToLoad` เพื่อใช้แทนชื่อของเลเวลที่เราจะทำการเปลี่ยนฉาก

สร้างฟังก์ชั่น `LoadLevel` เพื่อให้ทำการเปลี่ยนฉากไปยังฉากที่เรากำหนด

สร้างฟังก์ชั่น `OnTriggerEnter2D` เมื่อมีการชนกันของตัวละครให้ทำการเรียกใช้ฟังก์ชั่นเพื่อทำการเปลี่ยนฉาก โดยเราจะทำการ ยกเลิก BoxCollider2D ของประตูเพื่อป้องกันการเปลี่ยนฉากซ้ำซ้อน และหยุดการรับคำสั่งจากผู้เล่น ให้ตัวละครเคลื่อนไหว  

ที่ `Inspector` ของ Door ให้ทำการ ใส่ Level ที่ต้องการจะไป  

<img width="454" height="496" alt="image" src="https://github.com/user-attachments/assets/4cf55e4a-4e62-41e5-9709-0307e5f4fb02" />


## ส่วนที่3
### `ศัตรูเดินภายในฉาก`
ทำการลาก spike EnemyRedDog เข้ามาใน Hierarchy 
และสร้างไฟล์   
Enemy.cs และ EnemyRedDog.cs โดยเราจะให้คลาส Enemy เป็นคลาสหลัก แล้วใช้คลาสนี้ทำการส่งข้อมูลผ่านการสืบทอดคุณสมบัติของคลาสไปยังคลาสอื่น  
ที่ไฟล์ Enemy.cs เขียน  

<img width="389" height="321" alt="image" src="https://github.com/user-attachments/assets/7162af9f-eb98-449d-a390-96130b88e97a" />  

ที่ไฟล์ EnemyRedDog.cs เขียน  
<img width="582" height="410" alt="image" src="https://github.com/user-attachments/assets/e9a4d70d-a662-4aa9-b700-2f2540d8d760" />  
จากนั้นที่วัตถุ `EnemyRedDog` ให้ทำการเพิ่มคอมโพเนน์ต่าง ๆ ดังนี้

1. Rigibofy2D
   - กำหนด Constraints → Freeze Rotation → Z → `true`
2. CapsuleColinder2D
3. Animator สร้างเอนิเมชั่นให้กับตัวละคร ในตอนนี้ให้สร้างการเคลื่อนไหว สำหรับการเดินของตัวละคร
4. ไฟล์ `EnemyRedDog` ที่เราสร้างขึ้น  
ตรวจสอบ Layer ของตัวละครนี้ ให้อยู่ในเลย์เยอร์ใหม่ชื่อ Enemy และให้เช็คการตรวจสอบระหว่างเลยเยอร์ Enemy และ Ground

<img width="696" height="550" alt="image" src="https://github.com/user-attachments/assets/5e2014bb-424d-42f8-8371-3c0f57e93940" />   

ที่ไฟล์ `EnemyRedDog` ทำการกำหนดการเคลื่อนที่ของตัวละครผ่านตัวแปรต่าง ๆ  ดังนี้

กำหนดจุดขึ้นมาสองจุด โดยให้เป็นประเภท Transform เพื่อใช้ตรวจสอบการซ้อนทับกันของเลย์เยอร์ พร้อมทั้งกำหนดตัวแปรสำหรับเก็บค่าเมื่อเกิดการซ้อนทับกันของเลย์เยอร์  
    
<img width="295" height="144" alt="image" src="https://github.com/user-attachments/assets/7e8f929f-6411-49bb-8355-0efe056fac32" />  

สร้างฟังก์ชั่น Flip() สำหรับสั่งให้ตัวละครเปลี่ยนทิศทางการเดิน เมื่อตรวจสอบครบเงื่อนไข โดยภายในจะตรวจสอบว่า จุดอ้างอิงที่สร้างขึ้นทั้งสองจุด ได้เคลื่อนไปทับกับเลย์เยอร์ Ground หรือไม่ หากใช่เราก็จะทำการเปลี่ยนทิศทางการเดินของตัวละคร  






    public float speed = 1;
    private int direction = -1;
    public LayerMask layerToCheck;

    private void FixedUpdate()
    {
        Filp();
        rb.velocity = new Vector2(direction * speed, rb.velocity.y);
    }
    private void Filp() 
    { 
        detectGround = Physics2D.OverlapCircle(groundCheck.position, radius, layerToCheck);         
        detectWall = Physics2D.OverlapCircle(wallCheck.position, radius, layerToCheck);

        if (!detectGround || detectWall)
        {
            direction *= -1;
            transform.localScale = new Vector3(-transform.localScale.x, 1, 1);
        }
    }

สร้างฟังกชั่น OnDrawGizmos() สำหรับสร้างเส้น Gizmos เอาไว้ใช้อ้างอิงให้กับจุดของเรา  
<img width="500" height="181" alt="image" src="https://github.com/user-attachments/assets/e0c5c715-2475-4b55-91e2-fed4c61106c7" />  
สร้าง Empty Object ขึ้นมาเป็นวัตถุลูกของ `EnemyRedDog` ตั้งชื่อวัตถุทั้งสองว่า `wallCheck` และ `groundCheck` ตามลำดับ  

- `wallCheck` ให้อยู่ด้านหน้าของตัวละคร ทำหน้าที่คอยตรวจสอบว่า จุดดังกล่าว ชนกับกำแพงหรือยัง  
- `groundCheck` ให้เลื่อนลงมาข้างล่างด้านหน้า เพื่อตรวจสอบพื้น ในกรณีที่เดินไปเจอขอบของพื้น จะทำให้ไม่สามารถตรวจสอบพื้นได้  

<img width="601" height="320" alt="image" src="https://github.com/user-attachments/assets/9a199897-8ed4-4710-a859-2c11886be3a4" />

นำวัตถุที่สร้างขึ้น เข้าไปเชื่อมกับตัวแปร์ groundCheck และ wallCheck และกำหนดค่าของ layerToCheck ให้เป็น Ground  
ทีนี้เราก็สามารถ  Copy ตัวละคร `EnemyRedDog` ไปวางไว้ในด่านต่างๆของเรากี่ตัวก็ได้  
<img width="215" height="101" alt="image" src="https://github.com/user-attachments/assets/a1471cae-74e9-4a4c-951b-d48a48ad6bf6" />


  
 
