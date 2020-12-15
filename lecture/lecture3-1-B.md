기본행동 개선하기  
=======================
![lecture3-1-B-1](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-1.PNG)
* 저렇게 코드를 짜면 큰 문제가 발생하는데 player와 ground tag가 접촉하면 무조건 isground가 되어서 점프가 가능해진다.  
즉 원하지 않는 벽점프등등의 잡기술이 생긴다.  
--------------------------
![lecture3-1-B-3](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-3.jpg)  
* 어떻게 해야 저 문제를 해결할 수 있을까?  
발표해보자.  
 ---------------------------------  
![lecture3-1-B-4](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-4.png)  
* 방법중의 하나인 Physics2D.OverlapCircle기능을 소개하겠다.  
* 단순하게 설명하면 player의 발부분에 gameObject를 설정하고,  
특정 범위내에 특정 layer가 있는지 알려주는 기능이다.    
 ---------------------------------  
 ![lecture3-1-B-5](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-5.PNG)  
* 코드에 새로운 변수들을 선언해 준다.  
 ---------------------------------  
 ![lecture3-1-B-6](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-6.PNG)  
* colliderEnter2D에 있는 isground조건들을 없애주고 Update에 새로운 isground조건을 적어준다.  
 ---------------------------------  
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
    private void Start()
    {
        rigid = GetComponent<Rigidbody2D>();
    }
    private void FixedUpdate()
    {
        isground = Physics2D.OverlapCircle(groundCheck.position, groundRadius, whatIsGround);
        Move(); //플레이어 이동
        Jump(); //점프   
    }
    void Jump()
    {
        if (Input.GetButton("Jump")) //점프 키가 눌렸을 때//ground이면서 스페이스바 누르면 
        {
            if (isground == true) //점프 중이지 않을 때
            {
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
            if (horizontal >= 0) this.transform.eulerAngles = new Vector3(0, 0, 0);
            else this.transform.eulerAngles = new Vector3(0, 180, 0);
        }
        Vector3 dir = math.abs(horizontal) * Vector3.right; //변수의 자료형을 맞추기 위해 생성한 새로운 Vector3 변수
        this.transform.Translate(dir * moveSpeed * Time.deltaTime); //오브젝트 이동 함수
    }
}

```
* 수정한 코드 전문이다. 그냥 복사붙여넣기하지 말고 무슨뜻인지는 읽으면서 작성하자.  
----------------------------------  
 ![lecture3-1-B-7](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-7.png)
 ![lecture3-1-B-8](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-8.PNG)    
* Hierarchy에서 create Empty로 gameObject를 하나 만들어서 player에게 넣어준다..
 ---------------------------------  
![lecture3-1-B-9](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-9.png)  
* gameObject의 이름도 바꿔주고 잘보이게 색깔도 바꿔주고 위치는 0,0,0 즉 player발 바로 아래에 둔다.  
 ---------------------------------  
![lecture3-1-B-10](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-10.png)
![lecture3-1-B-11](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-11.png)
![lecture3-1-B-12](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-12.png)  
* tileMap에 ground layer를 추가해준다.  
 ---------------------------------  
![lecture3-1-B-13](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-13.png)  
* 다시 player로 가서 Ground Check에 아까 만든 gameObject를,  
what is ground에 ground layer를 넣어주고, 적당한 ground Radius를 설정해준다.  
 ---------------------------------  
    
![lecture3-1-B-14](https://github.com/isp829/HU/blob/master/images/lecture3/3-1-B/3-1-B-14.PNG)  
* 실행해보면 아까있었던 벽점프는 사라진걸 알 수 있다.  
 ---------------------------------  
[목차로](https://github.com/isp829/HU/blob/master/README.md)  
[다음](https://github.com/isp829/HU/blob/master/lecture/lecture3-1-A.md)   
-----------------------------
    
