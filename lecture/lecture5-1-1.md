적 기본행동 만들어주기2  
=======================
![5-1-10](https://github.com/isp829/HU/blob/master/images/lecture5/5-1/5-1-10.PNG)  
* 실행해보면 enemy가 날라다니면서 아무것도 못하게 된다.   
* 어떻게 해야 enemy를 걸어다니게 할까?  
------------------------------------ 
![5-1-11](https://github.com/isp829/HU/blob/master/images/lecture5/5-1/5-1-11.PNG)  
* x,y좌표 둘다 추적하는 Vector2 targetV 말고 x좌표만 추적하는 Vector2를 새로 만들어주자.  
---------------------------  
![5-1-12](https://github.com/isp829/HU/blob/master/images/lecture5/5-1/5-1-12.PNG)  
* 실행해보면 enemy가 이제 날라다니지는 않는다.   
---------------------------  
![5-1-13](https://github.com/isp829/HU/blob/master/images/lecture5/5-1/5-1-13.PNG)  
* 근데 실행하다보면 wayNumber 1일때 wayNumber 2에 해당되는곳으로 유인하고 거리가 멀어지면
  enemy가 wayNumber1로 가야하지만 이미 enemy 내부에 gameObject가 들어있어서 OnTriggerEnter가 발동을 안한다.  
* 어떻게 해야 이 문제를 해결 할 수 있을까?  
---------------------------  
![5-1-14](https://github.com/isp829/HU/blob/master/images/lecture5/5-1/5-1-14.PNG)  
* OnTrigger를 쓰면서 해결해보고 싶었으나 글쓴이의 머리로는 할 수 없었다.  
대신 target과 enemy의 위치를 비교하여 실행을 하였다. 
* 만약 OnTrigger를 그대로 쓰면서 해결할 수 있는 방법을 아는 사람은 발표해보자.  
---------------------------  
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class enemy : MonoBehaviour
{
    public float enemySpeed;
    public Transform target;
    public Transform[] waypoint;
    public int wayNumber;
    public Transform player;
    public float dist;
    public float checkDist;
    
    void Update()
    {
        dist = Vector2.Distance(transform.position, player.position);
        if (dist > checkDist)
        {
            target = waypoint[wayNumber];
        }
        else 
        {
            target = player;
        }
        Vector2 onlyXTarget=new Vector2(target.position.x,transform.position.y);
        transform.position = Vector2.MoveTowards(transform.position, onlyXTarget, enemySpeed * Time.deltaTime);
        if (target.position.x <= transform.position.x)//얼굴뒤집기
        {
            this.transform.eulerAngles = new Vector3(0, 180, 0);
        }
        else
        {
            this.transform.eulerAngles = new Vector3(0, 0, 0); 
        }
        if(target.position.x==transform.position.x)//y축 추가로 넣어도 되는데 넣을꺼면 바닥에 딱붙여야됨
        {  
            wayNumber++;
            wayNumber = wayNumber % 2;
            target = waypoint[wayNumber];
        }
    }
}

```
* 코드 전문이다.  
---------------------------  
[목차로](https://github.com/isp829/HU/blob/master/README.md)  
[다음](https://github.com/isp829/HU/blob/master/lecture/lecture4-1.md)   
-----------------------------

