%{
  version: "1.6.1",
  title: "Enum",
  excerpt: """
  Μια συλλογή αλγορίθμων για την απαρίθμηση συλλογών.
  """
}
---

## Enum

Η ενότητα `Enum` περιλαμβάνει πάνω από εβδομήντα συναρτήσεις για την εργασία με τις συλλογές.
Όλες οι συλλογές που μάθαμε στο προηγούμενο μάθημα, με την εξαίρεση των τούπλων, είναι απαριθμήσιμες.

Αυτό το μάθημα θα καλύψει μόνο ένα μέρος των διαθέσιμων συναρτήσεων, αλλά μπορούμε να τις ελέγξουμε μόνοι μας.
Ας κάνουμε ένα μικρό πείραμα στο IEx.

```elixir
iex> Enum.__info__(:functions) |> Enum.each(fn({function, arity}) ->
...>   IO.puts "#{function}/#{arity}"
...> end)
all?/1
all?/2
any?/1
any?/2
at/2
at/3
...
```

Με την παραπάνω έξοδο, είναι ξεκάθαρο ότι έχουμε ένα πολύ μεγάλο ποσό λειτουργιών, και για έναν καλό λόγο.
Η απαρίθμηση βρίσκεται στον πυρήνα του συναρτησιακού προγραμματισμού, και η αξιοποίηση του μαζί με άλλα στοιχεία της Elixir, μπορεί να δώσει απίστευτη δύναμη σε έναν προγραμματιστή.

Για να δείτε τις συναρτήσεις στο σύνολό τους επισκεφθείτε τα επίσημα έγγραφα του [`Enum`](https://hexdocs.pm/elixir/Enum.html).
Για τεμπέλικη απαρίθμηση (lazy enumeration) χρησιμοποιήστε την ενότητα [`Stream`](https://hexdocs.pm/elixir/Stream.html).

### all?

Όταν χρησιμοποιούμε την `all?/2`, και το μεγαλύτερο μέρος της `Enum`, παρέχουμε μια συνάρτηση την οποία εφαρμόζουμε σε όλα τα στοιχεία της συλλογής.
Στην περίπτωση της `all?/2`, ολόκληρη η συλλογή πρέπει να επιστρέφει `true`, αλλιώς θα επιστρέψει `false`:

```elixir
iex> Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 3 end)
false
iex> Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) > 1 end)
true
```

### any?

Αντίθετα με την προηγούμενη, η `any?/2` θα επιστρέψει `true` αν τουλάχιστον ένα στοιχείο αξιολογηθεί σαν `true`:

```elixir
iex> Enum.any?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 5 end)
true
```

### chunk_every

Αν χρειαστεί να σπάσουμε την συλλογή σε μικρότερα μέρη, η `chunk_every/2` είναι μάλλον η συνάρτηση που ψάχνουμε:

```elixir
iex> Enum.chunk_every([1, 2, 3, 4, 5, 6], 2)
[[1, 2], [3, 4], [5, 6]]
```

Yπάρχουν μερικές επιλογές για την `chunk_every/4` αλλά δεν θα τις αναλύσουμε, για να μάθετε περισσότερα επισκεφθείτε την [`chunk_every/4`](https://hexdocs.pm/elixir/Enum.html#chunk_every/4) στα επίσημα κείμενα.

### chunk_by

Αν χρειάστει να ομαδοποιήσουμε την συλλογή μας βασιζόμενοι σε κάτι πέραν του μεγέθους, μπορούμε να χρησιμοποιήσουμε τη συνάρτηση `chunk_by/2`.
Δέχεται μια συλλογή και μια συνάρτηση, και όταν αλλάζει η επιστροφή της συνάρτησης, ξεκινάει μια νέα ομάδα:

```elixir
iex> Enum.chunk_by(["one", "two", "three", "four", "five"], fn(x) -> String.length(x) end)
[["one", "two"], ["three"], ["four", "five"]]
iex> Enum.chunk_by(["one", "two", "three", "four", "five", "six"], fn(x) -> String.length(x) end)
[["one", "two"], ["three"], ["four", "five"], ["six"]]
```

### map_every

Μερικές φορές η ομαδοποίηση μιας συλλογής δεν είναι αρκετή για αυτό που πρέπει να κάνουμε.
Αν αυτό ισχύει, η `map_every/3` μπορεί να είναι πολύ χρήσιμη για να έχετε πρόσβαση σε κάθε `νιοστό` στοιχείο, εφαρμόζοντας πάντα τη συνάρτηση στο πρώτο στοιχείο:

```elixir
# Εφαρμόστε συνάρτηση σε κάθε τρίτο στοιχείο
iex> Enum.map_every([1, 2, 3, 4, 5, 6, 7, 8], 3, fn x -> x + 1000 end)
[1001, 2, 3, 1004, 5, 6, 1007, 8]
```

### each

Μπορεί να είναι απαραίτητο να περάσετε όλη τη συλλογή χωρίς να παραχθεί νέα τιμή. Σε αυτή την περίπτωση χρησιμοποιούμε την `each/2`:

```elixir
iex> Enum.each(["one", "two", "three"], fn(s) -> IO.puts(s) end)
one
two
three
:ok
```

__Σημείωση__: Η συνάρτηση `each/2` επιστρέφει το άτομο `:ok`.

### map

Για να εφαρμόσουμε την συνάρτησή μας σε κάθε αντικείμενο και να φτιάξουμε μια νέα συλλογή πρέπει να απευθυνθούμε στην συνάρτηση `map/2`:

```elixir
iex> Enum.map([0, 1, 2, 3], fn(x) -> x - 1 end)
[-1, 0, 1, 2]
```

### min

Η `min/1` βρίσκει τη μικρότερη τιμή σε μια συλλογή:

```elixir
iex> Enum.min([5, 3, 0, -1])
-1
```

Η `min/2` κάνει το ίδιο, αλλά σε περίπτωση που η απαρίθμηση είναι κενή, μας επιτρέπει να ορίσουμε μια συνάρτηση για να παράξουμε την μικρότερη τιμή:

```elixir
iex> Enum.min([], fn -> :foo end)
:foo
```

### max

Η `max/1` επιστρέφει τη μεγαλύτερη τιμή στη συλλογή:

```elixir
iex> Enum.max([5, 3, 0, -1])
5
```

Η `max/2` είναι για την `max/1` ότι η `min/2` για την `min/1`:

```elixir
iex> Enum.max([], fn -> :bar end)
:bar
```

### filter

Η συνάρτηση `filter/2` μας βοηθάει στο να φιλτράρουμε την συλλογή μας ώστε να περιλαμβάνει μόνο τα στοιχεία που είναι αληθή κατά τη χρήση της παρεχόμενης συνάρτησης:

```elixir
iex> Enum.filter([1, 2, 3, 4], fn(x) -> rem(x, 2) == 0 end)
[2, 4]
```

### reduce

Με την `reduce/3` μπορούμε να μαζέψουμε την συλλογή μας σε μία τιμή.
Για να το κάνουμε αυτό, παρέχουμε έναν προεραιτικό συσσωρευτή (το `10` σε αυτό το παράδειγμα) ο οποίος περνάει στη συνάρτηση. Αν δεν παρέχουμε συσσωρευτή, χρησιμοποιείται το πρώτο στοιχείο:

```elixir
iex> Enum.reduce([1, 2, 3], 10, fn(x, acc) -> x + acc end)
16

iex> Enum.reduce([1, 2, 3], fn(x, acc) -> x + acc end)
6

iex> Enum.reduce(["a","b","c"], "1", fn(x,acc)-> x <> acc end)
"cba1"
```

### sort

Η ταξινόμηση των συλλογών μας γίνεται εύκολη όχι με μία, αλλά με δύο συναρτήσεις ταξινόμησης.

Η `sort/1` χρησιμοποιεί την [ταξινόμηση όρων](http://erlang.org/doc/reference_manual/expressions.html#term-comparisons) της Erlang για να καθορίσει την σειρά ταξινόμησης:

```elixir
iex> Enum.sort([5, 6, 1, 3, -1, 4])
[-1, 1, 3, 4, 5, 6]

iex> Enum.sort([:foo, "bar", Enum, -1, 4])
[-1, 4, Enum, :foo, "bar"]
```

Όμως η `sort/2` μας επιτρέπει να παρέχουμε μια δική μας συνάρτηση ταξινόμησης:

```elixir
# με τη συνάρτησή μας
iex> Enum.sort([%{:val => 4}, %{:val => 1}], fn(x, y) -> x[:val] > y[:val] end)
[%{val: 4}, %{val: 1}]

# χωρίς
iex> Enum.sort([%{:count => 4}, %{:count => 1}])
[%{count: 1}, %{count: 4}]
```

### uniq

Μπορούμε να χρησιμοποιήσουμε την `uniq/1` για να αφαιρέσουμε διπλότυπα από τις συλλογές μας:

```elixir
iex> Enum.uniq([1, 2, -3, -2, -1, -1, -1, -1, -1])
[1, 2, 3]
```

### uniq_by

Η συνάρτηση `uniq_by/2` επίσης αφαιρεί τα διπλότυπα από τις απαριθμήσιμες συλλογές μας, αλλά επίσης μας επιτρέπει να παρέχουμε μια συνάρτηση για να κάνουμε τη σύγκριση της μοναδικότητας.

```elixir
iex> Enum.uniq_by([%{x: 1, y: 1}, %{x: 2, y: 1}, %{x: 3, y: 3}], fn coord -> coord.y end)
[%{x: 1, y: 1}, %{x: 3, y: 3}]
```

### Enum χρησιμοποιώντας το χειριστή Capture (&)
 
 Αρκετές συναρτήσεις μέσα στην ενότητα Enum της Elixir δέχονται ανώνυμες συναρτήσεις σαν ένα όρισμα για να δουλέψουν με κάθε στοιχείο της επανάληψης που έχει περαστεί.

 Αυτές οι ανώνυμες συναρτήσεις συνήθως γράφονται σε συντομογραφία χρησιμοποιώντας τον χειριστή Capture (&).

 Εδώ υπάρχουν κάποια παραδείγματα που δείχνουν πώς ο χειριστής Capture μπορεί να υλοποιηθεί μέσα στην ενότητα Enum.
 Κάθε εκδοχή είναι λειτουργικά ίση.

#### Χρησιμοποιώντας τον χειριστή Capture με ανώνυμες συναρτήσεις 

 Εδώ έχουμε ένα τυπικό παράδειγμα από τη στάνταρ σύνταξη όταν περνάμε μια ανώνυμη συνάρτηση στην `Enum.map/2`.

 ```elixir
 iex> Enum.map([1,2,3], fn number -> number + 3 end)
 [4, 5, 6]
 ```

 Εδώ υλοποιούμε τον χειριστή capture (&), συλαμβάνοντας κάθε στοιχείο της λίστας αριθμών ([1,2,3]) και ορίζοντας κάθε στοιχείο στη μεταβλητή &1 καθώς περνιέται στη δοσμένη συνάρτηση.

 ```elixir
 iex> Enum.map([1,2,3], &(&1 + 3))
 [4, 5, 6]
 ```

 Αυτό μπορεί περαιτέρω να καθαριστεί ορίζοντας την προηγουμένως ανώνυμη συνάρτηση που χρησιμοποιεί τον χειριστή Capture σε μία μεταβλητή και να τη καλλέσει από την `Enum.map/2`.

 ```elixir
 iex> plus_three = &(&1 + 3)
 iex> Enum.map([1,2,3], plus_three)
 [4, 5, 6]
 ```

#### Χρησιμοποιώντας τον χειριστή Capture με μία ονομασμένη συνάρτηση
 
 Πρώτα δημιουργούμε την ονομασμένη συνάρτηση και στη συνέχεια την καλούμε από την ανώνυμη συνάρτηση που ορίζεται στην `Enum.map/2`.

 ```elixir
 defmodule Adding do
   def plus_three(number), do: number + 3
 end

 iex>  Enum.map([1,2,3], fn number -> Adding.plus_three(number) end)
 [4, 5, 6]
 ```

 Στη συνέχεια μπορούμε να την καθαρίσουμε χρησιμοποιώντας τον χειριστή Capture.

 ```elixir
 iex> Enum.map([1,2,3], &Adding.plus_three(&1)) 
 [4, 5, 6]
 ```

 Για να έχουμε πιο περιληπτικό συντακτικό, μπορούμε κατευθείαν να γράψουμε την ονομασμένη συνάρτηση χωρίς να συλλάβουμε σαφώς τη μεταβλητή.

 ```elixir
 iex> Enum.map([1,2,3], &Adding.plus_three/1) 
 [4, 5, 6]
 ```
