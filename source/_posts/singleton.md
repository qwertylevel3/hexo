title: singleton
date: 2016-05-11 22:55:11
tags: c++
---
>Singleton 模板
<!--more-->

# 常用的Singleton模板：

singleton.h

```
#ifndef SINGLETON_H
#define SINGLETON_H
template<class SubClass>
class Singleton
{
protected:
    static SubClass *p;
public:
    static SubClass * instance()
    {
        if(!p)
            p = new SubClass();
        return p;
    }
protected:
    Singleton(){}
    ~Singleton(){ delete p; p = 0; }
};
template <class SubClass>
SubClass * Singleton<SubClass>::p=0;
#endif // SINGLETON_H
```

可以通过继承来使得某个类为Singleton

```
class AAA:public Singleton<BulletManager>
{
public:
    AAA();
    ~AAA();
//.......
};
```

可以用AAA::instance()来使用这个单例，如果是多重继承，记得Singleton要写在public几个继承类的最后。
