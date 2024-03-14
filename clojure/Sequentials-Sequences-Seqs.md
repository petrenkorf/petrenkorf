
### Sequential

We call a collection that holds a serie of values without reordering them as Sequential. It is one of the three broad
categories of collection types along with Sets and Maps.

### Sequence
Sequence is a sequential collection that represents a serie of values that may or may not exist yet. They may be values 
from a concrete collection as the following:

```clojure

(def my-values '(1 2 3 4 5))

```

or values that are computed as necessary as the following example (Remember that once we call the funtion `map`, the
values of this sequence are not evaluted immediatelly).

```clojure

(def my-incremented-values (map inc [1 2 3 4 5]))

```

### Seq

Seq (pronounced as "seek") is any object that implements the seq API, which supports the functions 'first' and 'rest'.
Vectors and Lists are the simplest examples of objects implementing this API.
