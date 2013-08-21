++++++++++++++++++++
并发相关算法
++++++++++++++++++++

:Author: Young King
:Update: 2013-08-21
:Version: 0.1

.. contents::
   :depth: 2


无锁算法
===================
设计通用的无锁算法很困难，替代方案是设计无锁的数据结构。

无锁的堆栈
----------------

::

    class Node {
        Node * next;
        int data;
    };

    // stable ‘head of list’, not an real Node 
    Node * head; 

    void push(int t) {
        Node* node = new Node(t);
        do{
            node->next = head;
        } while (!cas(&head, node, node->next));;
    }

    bool pop(int& t) {
        Node* current = head;
        while(current) {
            if(cas(&head, current->next, current)){
                t = current->data; // problem? 
                return true;
            }
            current = head;
        }
        return false;
    }

无锁堆栈的缺陷(ABA 问题)
-------------------------------
线程P1读到堆栈head的值为 A, 然后P1被抢占切换到 P2, 线程 P2先后 pop 掉 A、B，然后
再把A push 回去，此时切回到P1, 它读到的 head 仍然还是 A，认为堆栈没变就会让 cas 生效，
但其实堆栈里的内容已经发生了改变。 (外表看起来一样，其实里面的东西被调包了)。


双保险的CAS
------------------------------
