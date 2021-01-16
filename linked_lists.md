---
title: Linked Lists
category: Data structures
tags: [WIP]
prism_languages: [cpp]
updated: 2021-01-16
weight: -1
---

Intro
-------------------------------------

Reversing
-------------------------------------

### Iterative

- Advance `next`.
- Reverse: Point `curr->next` to the `prev` node.
- Advance `prev` and `curr`.
- Make the last `prev` the new `head` of the list.

```cpp
void List_reverse(List *list)
{
    List_node *curr = list->head;
    List_node *next = 0;
    List_node *prev = 0;

    while(curr)
    {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }

    list->head = prev;
}
```

### Recursive

- Set the **stop** condition
- **Recurse** with tail
- **Apply** the change (make the next node point to head)
- **Return** the head

```cpp
List_node *List_do_reverse(List_node *head)
{
    if (head == NULL || head->next == NULL)
    {
        return head;
    }

    List_node *reversed_list_head = List_do_reverse(head->next);
    head->next->next = head;
    head->next = 0;
    return reversed_list_head;
}

void List_reverse_recursive(List *list)
{
    list->head = List_do_reverse(list->head);
}
```
