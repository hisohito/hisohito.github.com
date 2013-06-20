---
layout: post
title: "A Functional Way to sort Linked List"
date: 2013-06-18 12:09
comments: true
published: false
categories:
---
It is a quick overview of sorting algorithms on functional implementation of <a href="https://en.wikipedia.org/wiki/Linked_list">linked list</a>

By the definition in <a href="http://books.google.ru/books/about/Introduction_to_algorithms.html?id=VK9hPgAACAAJ&redir_esc=y">Cormen book</a> <i>linked list</i> is a data structure in which the objects are arranged in a linear order. So, each node contains datum and reference to the next node in list. 
In every functional language linked list has an implementation as immutable data structure (cannot be modified after creating). Linked list has <i>O(1)</i> for prepend and access to tail/head and <i>O(n)</i> for delete and search operation in worst case. So lets take a look at sorting algoritms of linked lists in popular functional languages.

<!-- more -->
<ol>
<li>What sorting algorithms are used for linked lists?</li> 

In <b>Scala</b> linked list is in class <a href="https://github.com/scala/scala/blob/master/src/library/scala/collection/immutable/List.scala">List.scala</a>. 
Implementation of sort algorithms is in <a href="http://www.scala-lang.org/archives/downloads/distrib/files/nightly/docs/library/index.html#scala.collection.SeqLike">SeqLike trait</a> (template trait for sequences - special cases of iterable collections). 

{% codeblock Sorted method from SeqLike trait lang:scala %}
 def sorted[B >: A](implicit ord: Ordering[B]): Repr = {
    val len = this.length
    val arr = new ArraySeq[A](len)
    var i = 0
    for (x <- this.seq) {
      arr(i) = x
      i += 1
    }
    java.util.Arrays.sort(arr.array, ord.asInstanceOf[Ordering[Object]])
    val b = newBuilder
    b.sizeHint(len)
    for (x <- arr) b += x
    b.result
  }
{% endcodeblock %}

Here we can assume that in scala there is no native written algorithm. When you need to sort linked list, the following actions will be performed: list will be copied to array, sorted by standart java method, the result will be copied to a new linked list. Using of java Arrays method - is quite tricky and clever because operation of copy linked list to another array takes normally only O(n). Nowadays <a href"http://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html">java.util.Arrays</a> uses a <a href="https://www.google.ru/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CCgQFjAA&url=http%3A%2F%2Fiaroslavski.narod.ru%2Fquicksort%2FDualPivotQuicksort.pdf&ei=xybDUervF-XT4QT1sICIAw&usg=AFQjCNEFHjfbRfsZBuC1knc7pH6NL8e2aA&sig2=YQ8GR1N43Ff16Q3nN_fckQ&bvm=bv.48175248,d.bGE&cad=rjt">Dual-Pivot Quicksort by Vladimir Yaroslavskiy</a>, Jon Bentley, and Joshua Bloch. Performance of this algorithm is O(n log(n)) on many data sets that cause other quicksorts to degrade to quadratic performance, and is typically faster than traditional (one-pivot) Quicksort implementations. So it's a very pragmatic way to do this.
</br>
Haskell
</br>
F#
</br>
Closure
</br>

<li>If its merge sort, how do they split list into two parts (Do they use length of the list?)</li>

<li>Is merge sort asymptotically optimal (memory + time)?</li>

<li>Can we do it better then merge sort?</li>

<li>What about heaps? Can we use heap sort instead (Can we build a binary heap from linked list in O(n) time)?</li>

</ol>

