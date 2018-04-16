---
layout: post
title: UnityChan 애니메이션 적용
---

UnityChan 애니메이션 적용



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