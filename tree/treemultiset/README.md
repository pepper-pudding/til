# TreeMultiSet

{% embed url="http://www.promtools.org/prom6/nightly/doc/org/processmining/framework/util/collection/TreeMultiSet.html" %}

## 생성자

```java
public final class TreeMultiset<E> extends AbstractSortedMultiset<E> implements Serializable {
    private final transient TreeMultiset.Reference<TreeMultiset.AvlNode<E>> rootReference;
    private final transient GeneralRange<E> range;
    private final transient TreeMultiset.AvlNode<E> header;
    @GwtIncompatible
    private static final long serialVersionUID = 1L;

    public static <E extends Comparable> TreeMultiset<E> create() {
        return new TreeMultiset(Ordering.natural());
    }

    public static <E> TreeMultiset<E> create(@Nullable Comparator<? super E> comparator) {
        return comparator == null ? new TreeMultiset(Ordering.natural()) : new TreeMultiset(comparator);
    }

    public static <E extends Comparable> TreeMultiset<E> create(Iterable<? extends E> elements) {
        TreeMultiset<E> multiset = create();
        Iterables.addAll(multiset, elements);
        return multiset;
    }

    TreeMultiset(TreeMultiset.Reference<TreeMultiset.AvlNode<E>> rootReference, GeneralRange<E> range, TreeMultiset.AvlNode<E> endLink) {
        super(range.comparator());
        this.rootReference = rootReference;
        this.range = range;
        this.header = endLink;
    }

    TreeMultiset(Comparator<? super E> comparator) {
        super(comparator);
        this.range = GeneralRange.all(comparator);
        this.header = new TreeMultiset.AvlNode((Object)null, 1);
        successor(this.header, this.header);
        this.rootReference = new TreeMultiset.Reference();
    }
}
```



## add()

```java
    @CanIgnoreReturnValue
    public int add(@Nullable E element, int occurrences) {
        CollectPreconditions.checkNonnegative(occurrences, "occurrences");
        if (occurrences == 0) {
            return this.count(element);
        } else {
            Preconditions.checkArgument(this.range.contains(element));
            TreeMultiset.AvlNode<E> root = (TreeMultiset.AvlNode)this.rootReference.get();
            if (root == null) {
                this.comparator().compare(element, element);
                TreeMultiset.AvlNode<E> newRoot = new TreeMultiset.AvlNode(element, occurrences);
                successor(this.header, newRoot, this.header);
                this.rootReference.checkAndSet(root, newRoot);
                return 0;
            } else {
                int[] result = new int[1];
                TreeMultiset.AvlNode<E> newRoot = root.add(this.comparator(), element, occurrences, result);
                this.rootReference.checkAndSet(root, newRoot);
                return result[0];
            }
        }
    }
```



## remove()

```java
    @CanIgnoreReturnValue
    public int remove(@Nullable Object element, int occurrences) {
        CollectPreconditions.checkNonnegative(occurrences, "occurrences");
        if (occurrences == 0) {
            return this.count(element);
        } else {
            TreeMultiset.AvlNode<E> root = (TreeMultiset.AvlNode)this.rootReference.get();
            int[] result = new int[1];

            TreeMultiset.AvlNode newRoot;
            try {
                if (!this.range.contains(element) || root == null) {
                    return 0;
                }

                newRoot = root.remove(this.comparator(), element, occurrences, result);
            } catch (NullPointerException | ClassCastException var7) {
                return 0;
            }

            this.rootReference.checkAndSet(root, newRoot);
            return result[0];
        }
    }
```
