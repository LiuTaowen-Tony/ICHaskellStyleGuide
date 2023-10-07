# ICHaskellStyleGuide

A Haskell style guide adhering to the conventions of Imperial College 40009 Computing Practical.

## Least Powerful Principle

Adhere to the Least Powerful Principle whenever possible: if a less powerful construct achieves the same result <mark>and</mark> maintains readability, prefer it over more powerful alternatives. For instance, prefer pattern matching over guards, and guards over case expressions due to their versatility. Utilize pattern matching when possible, and opt for guards over case expressions when applicable.

## Line Wrap: 80 Characters

To set the ruler in VSCode:

1. Navigate to Settings.
2. Search for "ruler".
3. Add a ruler at the 79th character.

Links are the exception to the 80-character line rule.

For enhanced code aesthetics, consider using the [JetBrains Mono Font](https://vueschool.io/articles/vuejs-tutorials/how-to-install-jetbrains-mono-font-in-visual-studio-code/) which supports ligatures.

## Data Declaration

Align `|` with `=`

```haskell
data Tree a = Node (Tree a) (Tree a)
            | Leaf a

data JSON = JString String
          | JNumber Double
          | JObject [(String, JSON)]
--        | JList   [JSON] -- Alignment optional, but can be done if preferred
          | JList [JSON]     
          | JBool Bool
```

For constructor syntax, align the `,` and the `{`.

```haskell
data Point = Point
  { pointX :: Double
  , pointY :: Double
  }
```

## Spacing

Maintain a space after `,` for tuples and lists.

```haskell
tuple2 :: (Double, Double)
tuple2 = (x, y)
tuple3 :: (Double, Double, Double)
tuple3 = (x, y, z)
list3  :: [Double]
list3  = [x, y, z]
```

Ensure spacing around binary operators.

```haskell
x :: Double
x = 100 * 31 + 9
```

For complex expressions, utilize parentheses for clarity.

```haskell
y :: Double
y = (1 * 4) + (3 * 2 / (6 ^ 20))
z :: Double
z = -1
```

Can omit spacing for `:` and `..`. But it would be also fine to have the spaces.

```haskell
elem :: Eq t => t -> [t] -> Bool
elem x []     = False
elem x (y:ys) = (x == y) || (x `elem` ys)

sumTo100 :: Int
sumTo100 = sum [1..100]
```

## Pattern Matching

Preface functions with preconditions and types where appropriate, and favor pattern matching over guard statements when possible.

```haskell
-- Pre: x >= 0
fib :: (Eq a, Num a, Num p) => a -> p
fib 0 = 1
fib 1 = 1
fib x = fib (x - 1) + fib (x - 2)
```

Opt for pattern matching over getter functions, as shown below.

```haskell
-- Preferred
f :: Ord b => [(b, b)] -> b
f ((a, b) : xs) = if a > b then b else f xs

-- Not Preferred
f (x:xs) = if fst x > snd x then snd x else f xs
```

only use fst when you are writing something like this

```haskell
unzipLeft :: [(b1, b2)] -> [b1]
unzipLeft = map fst
```

## Guards

Prefer guards over `if .. then .. else` constructs, except in cases of succinct expressions or within `do` blocks.

```haskell
getClassOfHonor :: (Ord a, Num a) => a -> String
getClassOfHonor n
  | n < 40    = "fail" ++ "superLooooooooooooooooooooooooooooooooooooooongString"
  | n < 50    = "pass"
  | n < 60    = "lower second" 
  | n < 70    = "upper second"
  | otherwise = "first"
```

Utilize `if .. then .. else` for straightforward expressions:

```haskell
max :: Ord p => p -> p -> p
max a b = if a > b then a else b
```

Or within a `do` block:

```haskell
bar :: IO ()
bar = do
  password <- getLine
  if read password == 42
    then do
      print "password correct"
      print "welcome"
    else do
      print "wrong password"
      print "try again"
```

For more than two conditions, consider using a `case` expression.

```haskell
foo :: IO ()
foo = do
  name <- getLine
  case name of
    "Anthony Field" -> do
      print "good morning"
      print "Tony"
    "Nicolas Wu"    -> do
      print "good afternoon"
      print "Nick"
    _               -> do
      print "hello"
```

## List Comprehension vs Map

For simple operations, `map` is preferable:

```haskell
allAddOne :: [Integer] -> [Integer]
allAddOne = map (+ 1)
mapF :: [String] -> [Bool]
mapF = map (elem 'x' . reverse)
```

Employ list comprehension for combined mapping and filtering tasks:

```haskell
oddSquares :: [Integer]
oddSquares = [i * i | i <- [1..], odd i]
-- Though `i <- [1,3..]` can be used, this example illustrates a point
```

For complex operations:

```haskell
listCompExample :: [Integer]
listCompExample = [x * x + 5 * 12 `div` 98 + x | x <- oddSquares]
```

Multi-line example:

```haskell
multTable :: [String]
multTable = [show a ++ " * " ++ show b  ++ " = " ++ show (a * b) 
            | a <- [1..9], b <- [1..9], a <= b]
            -- Align `|` with `[`
```

## 'where' vs 'let .. in'

In pure functions, 'where' is often preferred over 'let .. in'. However, 'let' bindings are common in 'do' blocks. While there are debates regarding the use of either, ensuring good readability is paramount. It's advisable to avoid using both `let .. in` and `where` within the same function definition.

For comments, place them before or after the function. For succinct functions, inline comments are permissible. This refined structure should enhance readability and maintain a coherent style throughout your Haskell code.


