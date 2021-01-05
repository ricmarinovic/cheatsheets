---
title: Linked Lists
category: Data structures
tags: [WIP]
prism_languages: [c]
updated: 2021-01-03
weight: -1
---

Intro
-------------------------------------

Reversing
-------------------------------------

### Iterative

```c
void List_reverse(List *list)
{
    List_node *cur = list->head;
    List_node *next = 0;
    List_node *prev = 0;

    while(cur)
    {
        next = cur->next;
        cur->next = prev;
        prev = cur;
        cur = next;
    }
    list->head = prev;
}
```

### Recursive

```c
List_node *List_do_recur_reverse(List_node *head)
{
    if (head == NULL || head->next == NULL)
    {
        return head;
    }

    List_node *reversed_list_head = List_do_recur_reverse(head->next);
    head->next->next = head;
    head->next = 0;
    return reversed_list_head;
}

void List_recur_reverse(List *list)
{
    list->head = List_do_recur_reverse(list->head);
}
```
