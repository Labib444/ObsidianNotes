
```C#
public record Person(string F, string L);

public class ReverseArray : IComparer<int>
{
	public int Compare(int x, int y)
	{
		// Custom comparison logic
		// Implement your own comparison rules here
		// For example:
		return ((int)y).CompareTo((int)x);
		//return y - x;
	}
}

//* Main Class Below *//
		int i = 1;
        uint ui = 1;
        long l = 1;
        ulong ul = 1;
        double d = 1;
        decimal dc = 1;
        char c = 'a';
        String s = "s";

        //Convert.ToInt32(s);
        //Int32.Parse(s);

        int[] arr = new int[5];
        int[,] arr2D = new int[5, 5];
        int[][] arr2Djagged = new int[5][];

        arr2D = new int[,] { { 1, 2, 3, 4, 5 }, { 1, 2, 3, 4, 5 } };
        int[,] arr2Da = { { 1, 2, 3, 4, 5 }, { 1, 2, 3, 4, 5 } };

        arr2Djagged = new int[][] { new int[] { 1, 2, 3, 4, 5 }, new int[] { 1, 2, 3, 4, 5 } };
        arr2Djagged[0] = new int[] { 1, 2, 3, 4, 5 };

        //public record Person(string F, string L); // Record Type (reference type and immutable)
        Person p = new Person("L", "A");
        Console.WriteLine(p.F + p.L);

        var v = new { Name = "Labib" }; // Anonymous Type (reference type and read-only)
        Console.WriteLine(v.Name);

        Tuple<int, int, int> tuple = new Tuple<int, int, int>(1, 2, 3); //Reference Type Tuple (System.Tuple)
        Tuple<int, int, int> tupleA = (1, 2, 3).ToTuple();
        Tuple<int, int, int> tupleB = Tuple.Create(1, 2, 3);
        List<Tuple<int, int, int>> tupleList = new List<Tuple<int, int, int>>();
        tupleList.Add((1, 2, 3).ToTuple());

        (int, int, int) tupleValueType = (1, 2, 3); //Value Type Tuple (System.ValueTuple)
        List<(int, int, int)> tupleListA = new List<(int, int, int)>();
        tupleListA.Add((1, 2, 3));

        Console.WriteLine(tupleList.GetType());
        Console.WriteLine(tupleListA.GetType());

        Dictionary<int, int> dict = new Dictionary<int, int>();
        if (dict.TryGetValue(2, out int value))
        {
            Console.WriteLine(value);
            Console.WriteLine(dict[2]);
            Console.WriteLine(dict.ContainsKey(2));
            Console.WriteLine(dict.ContainsValue(2));
        }

        HashSet<int> map = new HashSet<int>();
        if (map.TryGetValue(2, out int mapValue))
        {
            Console.WriteLine(mapValue);
            Console.WriteLine(map.Contains(2));
        }

        Random random = new Random();
        List<int> list = Enumerable.Range(0, 10).Select(x => x = random.Next(x)).ToList();
        Enumerable.Empty<int>().ToList();
        list.ForEach(x => Console.Write(x + " "));


        List<int> test = Enumerable.Range(0, 100).ToList(); // provide 100,000,000 to see the difference
        int length = test.Count();
        
        Stopwatch sw = new Stopwatch();    
        sw.Start();
        int index = 0;
        foreach(var item in test)
        {
            //Console.WriteLine( item + " " + index );
            index++;
        }
        sw.Stop();
        Console.WriteLine(sw.ElapsedMilliseconds);

        sw.Reset();
        Console.WriteLine(sw.ElapsedMilliseconds);
        sw.Start();
        foreach (var item in test.Select( (x, i) => new {x, i} ) )
        {
            //Console.WriteLine( item + " " + index );
        }
        sw.Stop();
        Console.WriteLine(sw.ElapsedMilliseconds);

        sw.Reset();
        Console.WriteLine(sw.ElapsedMilliseconds);
        sw.Start();
        for(int j = 0; j < length; j++)
        {
            //
        }
        sw.Stop();
        Console.WriteLine(sw.ElapsedMilliseconds);

        List<int> ilist = Enumerable.Range(0, 10).ToList();
        List<List<int>> ilist2D = new List<List<int>>
        {
             Enumerable.Range(0, 5).ToList(),
             Enumerable.Range(0, 5).ToList(),
             Enumerable.Range(0, 5).ToList(),
             Enumerable.Range(0, 5).ToList(),
             Enumerable.Range(0, 5).ToList()
        };
        Dictionary<int, int> dictA = test.ToDictionary(x => x, x => x);
        Dictionary<int, List<int>> dictB = ilist2D.ToDictionary(x => ilist2D.IndexOf(x));

        String str = "Labib";
        String strA = new String("Labib");
        String strB = Convert.ToString(new char[5] { 'l', 'a', 'b', 'i', 'b' });

        Char[] cArray  = new Char[] { 'l', 'a', 'b', 'i', 'b' };
        char[] cArrayA = new char[5] { 'l', 'a', 'b', 'i', 'b' };
        char[] cArrayC = str.ToCharArray();


        Console.WriteLine(str.Split(" "));
        Console.WriteLine(String.Join(" ", str.ToCharArray()));
		
        Char.IsNumber('1');
        Char.IsDigit('1');
        Array.Reverse(cArray);
        Array.Reverse(cArrayA);

        StringBuilder stringBuilder = new StringBuilder("Labib"); //StringBuilder is mutable best for large text concatenations and such

        int[] sortArr = new int[] { 5, 23, 3, 7, 1 };
        Array.Sort(sortArr, (x, y) => x - y); //It is large to small
        Console.WriteLine("Binary Search :" + Array.BinarySearch(sortArr, 23)); // You have to send a sorted list(small to large) in the BinarySearch to work
        Array.Sort(sortArr, (x, y) => y - x); //It is large to small
        Console.WriteLine("Binary Search :" + Array.BinarySearch(sortArr, 23, new ReverseArray())); // You have to send a sorted list(small to large) in the BinarySearch to work

        sortArr.ToDictionary(x => x).ToList().Sort( (k, v) => k.Value.CompareTo(v.Value) ); // When Dictionary to List makes List<KeyValuePair<int, int>> (Sorting is done inplace)
        sortArr.ToDictionary(x => x).OrderByDescending(x => x);


        Stack<int> stack = new Stack<int>();
        Queue<int> queue = new Queue<int>();
        stack.Push(1);
        stack.Pop();
        queue.Enqueue(1);
        queue.Dequeue();
        LinkedList<int> linkedList = new LinkedList<int>();
        
        /*
            public class ReverseArray : IComparer
            {
                public int Compare(object x, object y)
                {
                    // Custom comparison logic
                    // Implement your own comparison rules here
                    // For example:
                    return ((int)y).CompareTo((int)x);
                }
            }

            public class ReverseArray : IComparer<int>
            {
                public int Compare(int x, int y)
                {
                    // Custom comparison logic
                    // Implement your own comparison rules here
                    // For example:
                    return ((int)y).CompareTo((int)x);
                }
            }
        */

        //IComparable (used in interfaces or classes), IComparer( need to make a new class for it), Comparison (just a delegate (x, y) => x.CompareTo(y) or x - y;
        //IEnumerable, ICollection, IEnumerator, Enumerable
        //Char, String, Array, Random
```