---
layout: post
title:  "Circular FIFO in C"
date:   2014-07-17 09:28:43
categories: C
---

[cfifo][cfifo] is a very small implementation of a [circular FIFO][wiki]
in C language. It uses the [fill count][fillcount] technique, doesn't
have reference counting and isn't thread-safe, although I'm considering
creating a thread-safe version soon, so keep in touch!

Here is an usage example:

{% highlight C %}
#include <stdio.h>
#include "cfifo.h"

int main()
{
    int el;
    struct cfifo f;
    cfifo_init(&f, sizeof(int), 5);

    el = 1, cfifo_push(&f, &el);
    el = 2, cfifo_push(&f, &el);
    el = 3, cfifo_push(&f, &el);
    el = 4, cfifo_push(&f, &el);
    el = 5, cfifo_push(&f, &el);
    el = 6, cfifo_push(&f, &el); /* This one overwrites el = 1 */

    cfifo_pop(&f, &el), printf("%d\n", el); // 2
    cfifo_pop(&f, &el), printf("%d\n", el); // 3
    cfifo_pop(&f, &el), printf("%d\n", el); // 4
    cfifo_pop(&f, &el), printf("%d\n", el); // 5
    cfifo_pop(&f, &el), printf("%d\n", el); // 6
    cfifo_free(&f); /* Frees the internal pointer */

    return 0;
}
{% endhighlight %}

In the [github][cfifo] page you will find more usage examples and
instructions.

[cfifo]: https://github.com/ianliu/cfifo
[wiki]: https://en.wikipedia.org/wiki/Circular_buffer
[fillcount]: https://en.wikipedia.org/wiki/Circular_buffer#Use_a_Fill_Count
