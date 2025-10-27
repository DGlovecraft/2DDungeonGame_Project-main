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

## ส่วนที่ 4
`การโจมตีของผู้เล่น (Player Attack)`

`เพื่อให้ผู้เล่นสามารถโจมตีศัตรูได้ เราจะเพิ่มระบบ การตรวจจับการกดปุ่มโจมตี และ การชนกับศัตรู ภายในระยะที่กำหนด`
ผู้เล่นกดปุ่ม J หรือ Left Mouse Button เพื่อโจมตี

ระบบจะสั่งให้ Animator เล่นอนิเมชัน "Attack"
เมื่ออนิเมชันเข้าสู่ช่วงโจมตี จะตรวจสอบว่ามีศัตรูอยู่ในระยะ Attack Range หรือไม่
ถ้าพบศัตรู จะเรียกฟังก์ชัน TakeDamage() ใน Enemy เพื่อลด HP ของศัตรู

การตั้งค่าใน Unity

ที่ Hierarchy ของ Player
จะมีวัตถุลูกชื่อ Player Attack
(เป็นจุดอ้างอิงระยะโจมตี)

<img width="154" height="83" alt="image" src="https://github.com/user-attachments/assets/cf596cfa-db74-45ba-afc9-23acde81ac18" />

สคริปต์ PlayerAttack.cs
```
using UnityEngine;

public class PlayerAttack : MonoBehaviour
{
    public float attackDamage = 10f;
    private int enemyLayer;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        enemyLayer = LayerMask.NameToLayer("Enemy");
    }
    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.gameObject.layer == enemyLayer)
        {
            collision.GetComponent<Enemy>().TakeDamage(attackDamage);
        }
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}

```
เมื่อทำการเพิ่มสคริปต์ PlayerAttack.cs แล้ว
ผู้เล่นจะเริ่มต้นด้วยพลังโจมตี (Attack Damage) เท่ากับ 10 หน่วย
โดยสคริปต์นี้จะทำหน้าที่ตรวจจับการชนของวัตถุ Player Attack กับศัตรูที่อยู่ในเลเยอร์ Enemy
เมื่อเกิดการชน ระบบจะเรียกใช้ฟังก์ชัน TakeDamage() ในสคริปต์ Enemy เพื่อทำการลดค่า HP ของศัตรูตามค่าพลังโจมตีที่กำหนดไว้
การตั้งค่าเพิ่มเติมใน Unity
ที่วัตถุ Player Attack ให้เพิ่มคอมโพเนนต์ Polygon Collider 2D
ตั้งค่า Is Trigger → ✅ (เปิดใช้งาน)
เพิ่มสคริปต์ PlayerAttack.cs ลงในวัตถุนี้
ตั้งค่าตัวแปร Attack Damage เป็น 10 (หรือปรับเพิ่ม-ลดได้ตามต้องการ)
ตรวจสอบให้แน่ใจว่า ศัตรูทุกตัวอยู่ในเลเยอร์ Enemy
เมื่อผู้เล่นโจมตีและเกิดการชนกับศัตรู ศัตรูจะได้รับความเสียหายตามค่าพลังโจมตี
### ส่วนที่ 5
`การเดินและกระโดดของผู้เล่น (Player Movement & Jump)`
ในส่วนนี้ เราจะทำให้ตัวละครของผู้เล่นสามารถ เดินและกระโดด ได้
โดยใช้ระบบ Unity Input System ซึ่งเราจะสร้างและเชื่อมโยงกับสคริปต์ `GatherInput.cs`
เพื่อให้ควบคุมการกดปุ่มต่าง ๆ เช่น เดินซ้าย–ขวา, กระโดด และโจมตี
`ขั้นตอนที่ 1 : ติดตั้งและตั้งค่า Input System`
เปิด Window → Package Manager → Unity Registry
ติดตั้งแพ็กเกจ Input System
ไปที่ Assets → Create → Input Actions แล้วตั้งชื่อว่า Controls
เปิดไฟล์ Controls จะเห็นคอลัมน์ 3 ส่วน:
Action Maps – กลุ่มของคำสั่ง
Actions – การกระทำต่าง ๆ ของผู้เล่น
Properties – รายละเอียดของปุ่ม
สร้าง Action Map ใหม่ชื่อ Player
แล้วเพิ่ม Action ดังนี้
<img width="883" height="239" alt="image" src="https://github.com/user-attachments/assets/744e5472-b3e2-4fc2-95cd-6e23d0acb59f" />
`กด Save Asset ด้านบนเพื่อบันทึกการตั้งค่า`
`ขั้นตอนที่ 2 : สร้างสคริปต์ GatherInput.cs`
สคริปต์นี้จะเป็นตัวกลางระหว่างระบบ Input System และตัวควบคุมการเคลื่อนไหว `(PlayerMoveControls.cs)`
โดยจะรับค่าปุ่มที่กดจากผู้เล่น แล้วส่งต่อให้ตัวละครในเกม
```
using UnityEngine;
using UnityEngine.InputSystem;

public class GatherInput : MonoBehaviour
{
    private Controls myControl;
    public float valueX;
    public bool jumpInput;
    public bool tryAttack;

    public void Awake()
    {
        myControl = new Controls();
    }

    private void OnEnable()
    {
        myControl.Player.Move.performed += StartMove;
        myControl.Player.Move.canceled += StopMove;

        myControl.Player.Jump.performed += JumpStart;
        myControl.Player.Jump.canceled += JumpStop;

        myControl.Player.Attack.started += TryToAttach;
        myControl.Player.Attack.canceled += StopTryToAttack;

        myControl.Player.Enable();
    }

    public void OnDisable()
    {
        myControl.Player.Move.performed -= StartMove;
        myControl.Player.Move.canceled -= StopMove;

        myControl.Player.Jump.performed -= JumpStart;
        myControl.Player.Jump.canceled -= JumpStop;

        myControl.Player.Attack.started -= TryToAttach;
        myControl.Player.Attack.canceled -= StopTryToAttack;

        myControl.Player.Disable();
    }

    private void StartMove(InputAction.CallbackContext ctx)
    {
        valueX = ctx.ReadValue<float>();
    }

    private void StopMove(InputAction.CallbackContext ctx)
    {
        valueX = 0;
    }

    private void JumpStart(InputAction.CallbackContext ctx)
    {
        jumpInput = true;
    }

    private void JumpStop(InputAction.CallbackContext ctx)
    {
        jumpInput = false;
    }

    private void TryToAttach(InputAction.CallbackContext ctx)
    {
        tryAttack = true;
    }

    private void StopTryToAttack(InputAction.CallbackContext ctx)
    {
        tryAttack = false;
    }
```
`ขั้นตอนที่ 3 : สคริปต์ควบคุมการเดินและกระโดด PlayerMoveControls.cs`

สคริปต์นี้ใช้ข้อมูลจาก GatherInput เพื่อควบคุม Rigidbody2D ของผู้เล่น
ให้สามารถเคลื่อนไหวและกระโดดได้อย่างสมจริง
```
using System.Collections;
using UnityEngine;

public class PlayerMoveControls : MonoBehaviour
{
    public float speed = 5f;
    private GatherInput gatherInput;
    private Rigidbody2D rb;
    public float jumpForce;
    public float rayLength;
    public LayerMask groundLayer;
    public Transform leftPoint;

    private bool grounded = false;
    private int direction = 1;
    private Animator animator;
    private bool knockBack = false;

    void Start()
    {
        gatherInput = GetComponent<GatherInput>();
        rb = GetComponent<Rigidbody2D>();
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        rb.linearVelocity = new Vector2(speed * gatherInput.valueX, rb.linearVelocity.y);
        SetAnimatorValues();
    }

    private void FixedUpdate()
    {
        CheckStatus();

        if (knockBack) return;

        Move();
        JumpPlayer();
    }

    private void Move()
    {
        Flip();
        rb.linearVelocity = new Vector2(speed * gatherInput.valueX, rb.linearVelocity.y);
    }

    private void Flip()
    {
        if (gatherInput.valueX * direction < 0)
        {
            transform.localScale = new Vector3(-transform.localScale.x, transform.localScale.y, transform.localScale.z);
            direction *= -1;
        }
    }

    private void SetAnimatorValues()
    {
        animator.SetFloat("Speed", Mathf.Abs(rb.linearVelocity.x));
        animator.SetFloat("vSpeed", rb.linearVelocity.y);
        animator.SetBool("Grounded", grounded);
    }

    private void JumpPlayer()
    {
        if (gatherInput.jumpInput && grounded)
        {
            rb.linearVelocity = new Vector2(gatherInput.valueX * speed, jumpForce);
        }
        gatherInput.jumpInput = false;
    }

    private void CheckStatus()
    {
        RaycastHit2D leftCheckHit = Physics2D.Raycast(leftPoint.position, Vector2.down, rayLength, groundLayer);
        grounded = leftCheckHit;
    }

    public IEnumerator KnockBack(float forceX, float forceY, float duration, Transform otherObject)
    {
        int knockBackDirection;
        if (transform.position.x < otherObject.position.x)
            knockBackDirection = -1;
        else
            knockBackDirection = 1;

        knockBack = true;
        rb.linearVelocity = Vector2.zero;
        Vector2 theForce = new Vector2(forceX * knockBackDirection, forceY);
        rb.AddForce(theForce, ForceMode2D.Impulse);

        yield return new WaitForSeconds(duration);
        knockBack = false;
        rb.linearVelocity = Vector2.zero;
    }
```
`ส่วนที่ 6`
`ระบบพลังชีวิตและตาย (HP System + GameOver)`
ในส่วนนี้ เราจะเพิ่ม ระบบ Game Over ให้ผู้เล่นเห็นหน้าจอจบเกมเมื่อ HP หมด โดยจะแสดง UI “Game Over” พร้อมปุ่มให้เลือกว่าจะเล่นใหม่หรือออกจากเกม
เปิด Scene หลัก เช่น Level1
ใน Hierarchy → สร้าง Canvas ชื่อ LogicUI
ภายใน Canvas → สร้าง GameObject ชื่อ GameOverUI
ภายใน GameOverUI
เพิ่ม Text (UI → Text – Legacy) แสดงคำว่า Game Over
เพิ่ม Button (UI → Button – Legacy) 2 ปุ่ม
Restart → เล่นใหม่
Quit → ออกจากเกม
สร้าง Empty GameObject ชื่อ LogicManager → เพิ่ม LogicScript.cs
ลากวัตถุต่อไปนี้ลง Inspector ของ LogicManager
StartUI → วัตถุ UI ตอนเริ่มเกม (ถ้ามี)
GameOverUI → กล่อง Game Over ที่สร้างไว้
HpText → Text แสดงค่าพลังชีวิตผู้เล่น
`ขั้นตอนที่ 2 : สคริปต์ LogicScript.cs`
```
using Unity.Mathematics;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class LogicScript : MonoBehaviour
{
    public GameObject StartUI;
    public GameObject GameOverUI;
    public Text HpText;

    public void UpdatePlayerHP(float hp)
    {
        HpText.text = math.round(hp).ToString();
    }

    public void ShowGameOverUI()
    {
        GameOverUI.SetActive(true);
    }

    public void RestartGame()
    {
        Time.timeScale = 1f;
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }

    public void GoToScene(string sceneName)
    {
        Time.timeScale = 1f;
        SceneManager.LoadScene(sceneName);
    }

    public void QuitGame()
    {
#if UNITY_EDITOR
        UnityEditor.EditorApplication.isPlaying = false;
#else
        Application.Quit();
#endif
        Debug.Log("Game Quit!");
    }
}
```
ขั้นตอนที่ 3 : การเชื่อมปุ่มใน Inspector
ปุ่ม Restart → LogicScript.RestartGame()
ปุ่ม Quit → LogicScript.QuitGame()
ขั้นตอนที่ 4 : การเรียกใช้งาน Game Over
เมื่อผู้เล่น HP หมด ให้เรียก ShowGameOverUI() จาก LogicScript
ตัวอย่างใน PlayerStats.cs:
```
if (health <= 0)
{
    FindObjectOfType<LogicScript>().ShowGameOverUI();
    Time.timeScale = 0f; // หยุดเวลาในเกม
}
```


  
 


  
 
