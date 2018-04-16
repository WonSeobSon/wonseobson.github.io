---
layout: post
title: UnityChan 애니메이션 적용
---

 UnityChan 애니메이션 적용

 step 1. UnityChan에게 Animator Component를 추가하고 Controller에 UnityChanLocomotions을 적용합니다.

 먼저 UnityChan에서 Add Componet를 누른후 Animator를 찾아 등록한 후 아래의 그럼처럼 UnityChanLocomotions을 Controller에 드래그앤드랍 합니다.

  ![ApplyAnimator](https://lh5.googleusercontent.com/60gojQckzRr-1jcd2eJ8y_TMb86tsnSzOb6QiB7rXUZGkEleLza2QNTT9mk0vOA17nuIjVFgWBDcZJJy2RAO=w1918-h1014-rw)

 step 2. 아래의 스크립트를 작성하여 UintyChan에게 걸어줍니다.

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMove : MonoBehaviour 
{    
    public float MovePower = 10f;
    
    Animator animator;

    void Start () 
    {		
        animator = GetComponent<Animator>();
    }
	
    void Update()
    {
        animator.SetFloat("Speed", Input.GetAxis("Vertical"));
    }

    void FixedUpdate()
    {
        transform.Translate(Vector3.forward * MovePower * Input.GetAxis("Vertical"));
    }
}
```
 그 후 실행하면 화살표 위,아래에 따라 애니매이션이 작동합니다.