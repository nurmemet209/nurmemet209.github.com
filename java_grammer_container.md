#java 容器

* Vector与ArrayList
```java

public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    ......
    private static final long serialVersionUID = 8683452581122892189L;

    /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;
    ......

        public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
}

```

```java

public class Vector<E>
    extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    /**
     * The array buffer into which the components of the vector are
     * stored. The capacity of the vector is the length of this array buffer,
     * and is at least large enough to contain all the vector's elements.
     *
     * <p>Any array elements following the last element in the Vector are null.
     *
     * @serial
     */
    protected Object[] elementData;

    /**
     * The number of valid components in this {@code Vector} object.
     * Components {@code elementData[0]} through
     * {@code elementData[elementCount-1]} are the actual items.
     *
     * @serial
     */
    protected int elementCount;

    .....

      public synchronized void addElement(E obj) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = obj;
    }

    ......
}

```

通过源码可以看出Vector跟ArrayList都是线性存储结构，其中Vector是线程安全的，ArrayList不是，还有一些实现细节


以下是LinkedList源码，可以看出LinkedList是链表存储结构，节省内存但是，增删该查有点麻烦
```java

public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     * Invariant: (first == null && last == null) ||
     *            (first.prev == null && first.item != null)
     */
    transient Node<E> first;

    /**
     * Pointer to last node.
     * Invariant: (first == null && last == null) ||
     *            (last.next == null && last.item != null)
     */
    transient Node<E> last;
}


```


HashSet源码如下,hashSet在内部是用HashMap实现的，允许有空置，不允许重复
```java


public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    static final long serialVersionUID = -5024744406713321676L;

    private transient HashMap<E,Object> map;

    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

    /**
     * Constructs a new, empty set; the backing <tt>HashMap</tt> instance has
     * default initial capacity (16) and load factor (0.75).
     */
    public HashSet() {
        map = new HashMap<>();
    }
    ....
}


```


HashMap源码,允许key有空置，允许value有空置，key不重复，非线程安全
```java

public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {

    private static final long serialVersionUID = 362498820763181265L;

    .....

}


```

HashTable 源码,可以看出hashtable是线程安全的，不允许key为空，不允许value为空，同样key不能重复
```java

public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {

    /**
     * The hash table data.
     */
    private transient Entry<?,?>[] table;

    /**
     * The total number of entries in the hash table.
     */
    private transient int count;

    /**
     * The table is rehashed when its size exceeds this threshold.  (The
     * value of this field is (int)(capacity * loadFactor).)
     *
     * @serial
     */
    private int threshold;

    /**
     * The load factor for the hashtable.
     *
     * @serial
     */
    private float loadFactor;

    .....

        public synchronized boolean isEmpty() {
        return count == 0;
    }


    .....
}

```


TreeMap源码，非线程安全，允许value为空，不允许key为空，key不能重复
```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
{
    /**
     * The comparator used to maintain order in this tree map, or
     * null if it uses the natural ordering of its keys.
     *
     * @serial
     */
    private final Comparator<? super K> comparator;

    private transient Entry<K,V> root;

    /**
     * The number of entries in the tree
     */
    private transient int size = 0;

    /**
     * The number of structural modifications to the tree.
     */
    private transient int modCount = 0;
    ....


    final Entry<K,V> getEntry(Object key) {
    }
    ....
}

```

```java
import java.util.*;

/**
 * Created by nurmemet on 2016/11/9.
 */
public class StringTest {

    public static void main(String[] ars) {

        Vector<String> vector = new Vector<>();
        vector.add("test");
        for (int i = 0; i < vector.size(); i++) {

        }
        ArrayList<String> list = new ArrayList();

        LinkedList<String> linkedList = new LinkedList();


        System.out.println("arraylist");
        HashMap<String, String> hasMap = new HashMap();

        hasMap.put("nur", "test");
        hasMap.put("nur1", "test");
        hasMap.put(null, "dfsdf");
        hasMap.put("123", null);
        for (String key : hasMap.keySet()) {
            System.out.println(hasMap.get(key));
        }
        System.out.println("hashset");

        Set<String> set = new HashSet<>();
        set.add("nur");
        set.add("nur");
        set.add(null);
        Iterator<String> iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        System.out.println("hashtable");

        Hashtable<String, String> hashtable = new Hashtable();
        hashtable.put("nur", "nurmemet");
        //hashtable.put("nur2",null);//不允许value为空
        //hashtable.put(null,"test"); //不允许key为空
        for (String key : hashtable.keySet()
                ) {
            System.out.println(hashtable.get(key));
        }
        ;

        System.out.println("treemap");
        TreeMap<String, String> treeMap = new TreeMap();
        treeMap.put("nur", "nurmemet");
        //treeMap.put(null,"nullkey");//不允许key为空
        treeMap.put("nullvalue", null);
        for (String key : treeMap.keySet()
                ) {
            System.out.println(treeMap.get(key));
        }
    }
}



```