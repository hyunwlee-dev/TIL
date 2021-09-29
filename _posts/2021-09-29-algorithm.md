---
layout: post
title: "TIL java Collcetions List interface, ArrayList class 구현"
author: "hyunwlee"
---

## <span style="background-color:#028C6A; color:white">Java List.interface</span>

<span style="background-color:#7BC5AE; color:white"><strong>List interface operations</strong></span>

<img src="https://github.com/hyunwlee-dev/TIL/blob/57d42aba0af328a9df4e5741619a8f2ad3ec2a4c/images/dataStructure/listInterface.png?raw=true" style="zoom:50%;"/>

---

<span style="background-color:#7BC5AE; color:white"><strong>code List interface</strong></span>

```
package hw_interface;

/**
 * List는 1) ArrayList, 2) 단일 LinkedList, 3) 이중 LinkedList에 의해 각각 구현됩니다.
 *
 * @author hyunwlee
 * @param <E> the type of elements in this list
 * @version 1.0
 */
public interface List<E> {

    /**
     * 리스트에 요소를 추가
     *
     * @param element 리스트에 추가할 요소
     * @return 중복된 요소가 있을 경우 {@code false}를 반환, 없을경우 {@code true}를 반환
     */
    boolean add(E element);

    /**
     * 리스트에 요소를 특정 위치에 추가
     *
     * @param index 리스트에 요소를 추가할 위치
     * @param element 리스트에 추가할 요소
     */
    void add(int index, E element);

    /**
     * 리스트의 특정 위치에 있는 요소를 삭제
     *
     * @param index 리스트에서 삭제 할 위치
     * @return 삭제된 요소를 반환
     */
    E remove(int index);

    /**
     * 리스트에서 특정 요소를 삭제
     * 동일한 요소가 여러개 일 경우 가장 앞의 요소만 삭제
     *
     * @param o 리스트에서 삭제할 요소
     * @return 리스트에 삭제할 요소가 없거나 삭제에 실패 경우
     *  {@code false}를 반환 성공할 경우 {@code true}를 반환
     */
    boolean remove(Object o);

    /**
     * 리스트의 특정 위치에 있는 요소를 반환
     *
     * @param index 리스트에서 반환 할 위치
     * @return 리스트 index 위치에 있는 요소를 반환
     */
    E get(int index);

    /**
     * 리스트에서 특정 위치에 있는 요소를 새 요소로 대체
     *
     * @param index 리스트에 접근할 위치
     * @param element index 요소를 새로 대체할 요소
     */
    void set(int index, E element);

    /**
     * 리스트에 특정 요소가 몇 번째 위치에 있는지를 반환
     *
     * @param o 리스트에서 위치를 찾을 요소
     * @return 리스트에서 처음으로 요소와 일치한 위치를 반환
     * 만약 일치하는 요소가 없으면 -1 반환
     */
    boolean contains(Object o);

    /**
     * 리스트에 특정 요소가 몇 번째 위치에 있는지를 반환
     *
     * @param o 리스트에서 위치를 찾을 요소
     * @return 리스트에서 처음으로 요소와 일치하는 위치를 반환
     * 만약 일치하는 요소가 없을경우 -1을 반환
     */
    int indexOf(Object o);

    /**
     * 리스트에 있는 요소의 개수를 반환
     *
     * @return 리스트에 있는 요소 개수를 반환
     */
    int size();

    /**
     * 리스트에 요소가 비어있는지를 반환
     *
     * @return 리스트에 요소가 없을 경우 {@code true}, 요소가 있을 경우 {@code false}를 반환
     */
    boolean isEmpty();

    /**
     * 리스트에 있는 요소를 모두 삭제
     */
    void clear();
}
```



---

<span style="background-color:#7BC5AE; color:white"><strong>code ArrayList class</strong></span>

```
package hw_interface;

import java.util.Arrays;

public class ArrayList<E> implements List<E>{

    private static final int DEFAULT_CAPACITY = 10; // 최소(기본) 용량
    private static final Object[] EMPTY_ARRAY = {}; // 빈 배열

    private int size;

    Object[] array;

    public ArrayList()
    {
        this.array = EMPTY_ARRAY;
        this.size = 0;
    }

    public ArrayList(int capacity)
    {
        this.array = new Object[capacity];
        this.size = 0;
    }

    private void resize()
    {
        int array_capacity = array.length;

        if (Arrays.equals(array, EMPTY_ARRAY))
        {
            array = new Object[DEFAULT_CAPACITY];
            return;
        }

        if (size == array_capacity)
        {
            int new_capacity = array_capacity * 2;
            array = Arrays.copyOf(array, new_capacity);
            return;
        }

        if (size < (array_capacity / 2))
        {
            int new_capacity = array_capacity / 2;
            array = Arrays.copyOf(array, Math.max(new_capacity, DEFAULT_CAPACITY));
            return;
        }
    }

    @Override
    public boolean add(E element) {
        addLast(element);
        return true;
    }

    public void addLast(E element)
    {
        if (size == array.length)
            resize();
        array[size++] = element;
    }

    @Override
    public void add(int index, E element) {
        if (index > size || index < 0)
            throw new IndexOutOfBoundsException();
        if (index == size)
            addLast(element);
        else
        {
            if (size == array.length)
                resize();
            for (int i = size; i > index; i--)
                array[i] = array[i - 1];
            array[index] = element;
            ++size;
        }
    }

    @SuppressWarnings("unchecked")
    @Override
    public E remove(int index) {
        if (index >= size || index < 0)
            throw new IndexOutOfBoundsException();
        E element = (E) array[index];
        array[index] = null;
        for (int i = index; i < size - 1; i++)
            array[i] = array[i + 1];
        array[size - 1] = null;
        --size;
        resize();
        return (element);
    }

    @Override
    public boolean remove(Object o) {
        int index = indexOf(o);

        if (index == -1)
            return (false);
        remove(index);
        return (true);
    }

    @SuppressWarnings("uncheckd")
    @Override
    public E get(int index) {
        if (index >= size || index < 0)
            throw new IndexOutOfBoundsException();
        return (E) array[index];
    }

    @Override
    public void set(int index, E element) {
        if (index >= size || index < 0)
            throw new IndexOutOfBoundsException();
        else
            array[index] = element;
    }

    @Override
    public boolean contains(Object o) {
        if (indexOf(o) >= 0)
            return (true);
        else
            return (false);
    }

    @Override
    public int indexOf(Object o) {
        for (int i = 0; i < size; i++)
            if (array[i].equals(o))
                return (i);
        return (-1);
    }

    public int lasIndexOf(Object o)
    {
        for (int i = size - 1; i >= 0; i--)
            if (array[i].equals(o))
                return (i);
        return (-1);
    }

    @Override
    public int size() {
        return (size);
    }

    @Override
    public boolean isEmpty() {
        return (size == 0);
    }

    @Override
    public void clear() {
        for (int i = 0; i < size; i++)
            array[i] = null;
        size = 0;
        resize();
    }
}
```

---

부족한것

제네릭스, clone, toArray 얕은복사, 깊은복사, Exception 종류?

---

<span style="background-color:#028C6A; color:white"><strong>Reference</strong></span>

[블로그 st-lab](https://st-lab.tistory.com/146)









