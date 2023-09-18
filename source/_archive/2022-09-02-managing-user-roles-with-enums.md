---
layout: post
title: "Managing User Roles With Enums"
date: 2022-09-02 12:00
comments: false
categories: laravel
---

**Maybe change this to manage actions? or simmmilar?**

See: https://timdeschryver.dev/blog/flagged-enum-what-why-and-how#why

enum Permissions : long
{
    Execute  = 1 << 0
    Write    = 1 << 1,
    Read     = 1 << 2,
    Delete   = 1 << 3,
    Send     = 1 << 4,
    Hide     = 1 << 5,
    Extract  = 1 << 6,
    Compress = 1 << 7,
}
? Kind of same purpose as gates/policy

Maybe for a users status
https://betterprogramming.pub/dont-use-boolean-arguments-use-enums-c7cd7ab1876a

Use this article as a referance: https://martinbean.dev/blog/2021/07/29/simple-role-based-authentication-laravel/

But translate it so that it uses Role Enums instead...
