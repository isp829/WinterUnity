플레이어 애니메이션 만들기  
=======================
![lecture3-1-A-1](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-1.png)
* player에 animator asset을 추가해준다.  
--------------------------  
![lecture3-1-A-2](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-2.png)
* Project tab에서 Animator Controller도 하나 만들어준다.  
--------------------------  
![lecture3-1-A-3](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-3.PNG)
![lecture3-1-A-4](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-4.PNG)
* 애니메이션에 쓸 모션들을 드래그해서 Scene이나 Hierarchy에 올려놓는다.   
* 올려놓으면 Project에 새로운 애니메이션들과 애니메이션 컨트롤러 들이 생긴다.  
--------------------------   
![lecture3-1-A-5](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-5.png)
* Animator tab을 열어준다.   
--------------------------  
![lecture3-1-A-6](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-6.png)
* 가만히 있을때 모션을 위해서 빈 상태를 하나 만들고  
아까 만든 걷는 애니메이션과 점프 애니메이션을 넣어준다.  
--------------------------  
![lecture3-1-A-7](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-7.png)
* player가 할 수 있는 행동이 3가지 밖에 없다면 그림과 같이 될것이다.  
각각 어떤 조건을 가져야 저렇게 움직일지 생각해보자.  
--------------------------  
![lecture3-1-A-8](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-8.png)
* animator에서 상태조절을 위한 bool parameter "walk"와 "jump"를 만들어준다.  
--------------------------  
![lecture3-1-A-12](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-12.png)
* 각각의 state에 마우스 오른쪽 클릭을 해서 Make Transition으로 쌍방으로 만들어주자.  
--------------------------------------    
![lecture3-1-A-9](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-9.png)
* 방향표들을 확인해보면 건너가는 시간들이 있는데 다 0초로 수정해주자.  
--------------------------  
![lecture3-1-A-10](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-10.PNG)
* 각각의 전환이 이루어지려면 어떤 상황일지 생각해보면서 "walk"와 "jump"의 상태를 적어둔다.  
--------------------------  
![lecture3-1-A-13](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-13.PNG)  
![lecture3-1-A-11](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-11.PNG)  
* 코드를 수정해주자 animator를 추가해주고, animator의 bool값을 바꿔줄 명령어들을 어디다 써야할지 생각해보자.    
--------------------------  

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.Mathematics;
public class test : MonoBehaviour
{
    public float moveSpeed = 5.0f; //플레이어 이동 속도
    public float jumpPower = 5.0f; //플레이어 점프 힘
    public Rigidbody2D rigid;
    float horizontal; //왼쪽, 오른쪽 방향값을 받는 변수
    public bool isground;//값을 받을 bool값
    public Transform groundCheck;//player발위치
    public float groundRadius = 0.2f;//측정할 범위
    public LayerMask whatIsGround;//어떤 layer를 측정할지
    public Animator animator;//애니메이터 추가
    private void Start()
    {
        rigid = GetComponent<Rigidbody2D>();
    }
    private void FixedUpdate()
    {
        isground = Physics2D.OverlapCircle(groundCheck.position, groundRadius, whatIsGround);
        if (isground == true)
        {
            animator.SetBool("jump", false);
        }
        Move(); //플레이어 이동
        Jump(); //점프   
    }
    void Jump()
    {
        if (Input.GetButton("Jump")) //점프 키가 눌렸을 때//ground이면서 스페이스바 누르면 
        {
            if (isground == true) //점프 중이지 않을 때
            {
                animator.SetBool("jump", true);//점프하면 애니메이터 설정 jump true.
                rigid.AddForce(Vector2.up * jumpPower, ForceMode2D.Impulse); //위쪽으로 힘을 준다.
                isground = false;
            }
            else return; //점프 중일 때는 실행하지 않고 바로 return.
        }
    }
    void Move()
    {
        horizontal = Input.GetAxis("Horizontal");
        if (horizontal != 0)
        {
            animator.SetBool("walk", true);//애니메이터 설정 walk를 true.
            if (horizontal >= 0) this.transform.eulerAngles = new Vector3(0, 0, 0);
            else this.transform.eulerAngles = new Vector3(0, 180, 0);
        }
        else 
        {
            animator.SetBool("walk", false);//좌우로 안움직이면 애니메이터 설정 walk false.
        }
        Vector3 dir = math.abs(horizontal) * Vector3.right; //변수의 자료형을 맞추기 위해 생성한 새로운 Vector3 변수
        this.transform.Translate(dir * moveSpeed * Time.deltaTime); //오브젝트 이동 함수
    }
}
```
* 바뀐 코드의 전문이다. 이번에도 그냥 배끼지 말고 왜 여기에 썼는지 생각하면서 해보자.  
-----------------------------  
![lecture3-1-A-14](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-14.png)   
* 코드를 추가해줬으니까 animator에 아까 추가한 animator를 추가해주고, animation controller에 아까 만들어준 controller를 넣어준다.  
--------------------------  
![lecture3-1-A-15](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-A/3-1-A-15.PNG)          
* 실행해 보면 상황따라 애니메이션들이 잘 적용되는걸 알 수 있다.    
--------------------------  
[목차로](https://github.com/isp829/HU/blob/master/README.md)  
[다음](https://github.com/isp829/HU/blob/master/lecture/lecture3-2.md)  
-----------------------------
    
