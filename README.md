# ICHaskellStyleGuide
A Haskell style guide that follows conventions in Imperial College 40009 Computing Practical.

## least powerful principal

least powerful principal when you can do something less powerful but do the same thing and have the same readability do the less powerful thing e.g. pattern matching < guard < case expression (less versatile) so when can you use pattern match, don't use a guard when can you use a guard, don't use a case expression


## line wrap : 80 characters

how to set ruler : go to setting in vscode, search for ruler then add a ruler at 79 line

only links can go over 80 line

ligature : https://vueschool.io/articles/vuejs-tutorials/how-to-install-jetbrains-mono-font-in-visual-studio-code/

this font will make your code looks super nice

## data declaration

align `|` with `=`

``` haskell
data Tree a = Node (Tree a) (Tree a)
            | Leaf a

data JSON = JString String
          | JNumber Double
          | JObject [(String, JSON)]
--          JList   [JSON]      no need to align this, but if you want you can
          | JList [JSON]     
          | JBool Bool
```

align the `,` and the `{`, for constructor syntax

``` haskell
data Point = Point
  { pointX :: Double
  , pointY :: Double
  }
```


## spacing

for tuples and lists, space after `,`

``` haskell
tuple2 :: (Double, Double)
tuple2 = (x, y)
tuple3 :: (Double, Double, Double)
tuple3 = (x, y, z)
list3  :: [Double]
list3  = [x, y, z]
```

basically do spacing for everything binary operators

``` haskell
x :: Double
x = 100 * 31 + 9
```

when expression is complicated, do parentheses

``` haskell
y :: Double
y = (1 * 4) + (3 * 2 / (6 ^ 20))
z :: Double
z = -1
```

can omit spacing for `:` 

``` haskell
elem :: Eq t => t -> [t] -> Bool
elem x []     = False
elem x (y:ys) = (x == y) || (x `elem` ys)
```

when precedence is not obvious, use parentheses sometimes it might be obvious for you, but not everyone

can omit spacing for `..`

``` haskell
sumTo100 :: Int
sumTo100 = sum [1..100]
```

## pattern matching

pattern matching over guard if you can, try include pre condition and type

``` haskell
-- pre : x >= 0
fib :: (Eq a, Num a, Num p) => a -> p
fib 0 = 1
fib 1 = 1
fib x = fib (x - 1) + fib (x - 2)
```

pattern matching over getter functions

do this 

``` haskell
f :: Ord b => [(b, b)] -> b
f ((a, b) : xs) = if a > b then b else f xs
```

don't do this

``` haskell
f (x:xs) = if fst x > snd x then snd x else f xs
```

only use fst when you are writing something like this

``` haskell
unzipLeft :: [(b1, b2)] -> [b1]
unzipLeft = map fst 
```

## guards

guard over `if .. then .. else`

``` haskell
getClassOfHonor :: (Ord a, Num a) => a -> String
getClassOfHonor n
  | n < 40    
    = "fail" ++ "superLooooooooooooooooooooooooooooooooooooooongString"
  | n < 50    
    = "pass"
  | n < 60    
    = "lower second" 
  | n < 70    
    = "upper second"
  | otherwise 
    = "first"
```

only do `if .. then .. else` when the expr is like this long

``` haskell
max :: Ord p => p -> p -> p
max a b = if a > b then a else b
```

or you can do `if .. then .. else` in a do block

``` haskell
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

if there are more than 2 if clause try use `case` instead, not entirely sure how to do this, maybe we can ask Nick together

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

## list comp vs map

if operation is simple and short use map

``` haskell
allAddOne :: [Integer] -> [Integer]
allAddOne = map (+ 1)
mapF :: [String] -> [Bool]
mapF = map (elem 'x' . reverse)
```

use list comp when you are mapping and filtering at the same time

``` haskell
oddSquares :: [Integer]
oddSquares = [i * i | i <- [1..], odd i]
-- I know you can do `i <- [1,3..], just to make an example
```

when the operation is complicated

``` haskell
listCompExample :: [Integer]
listCompExample = [x * x + 5 * 12 `div` 98 + x | x <- oddSquares]
```

multiple line example

``` haskell
multTable :: [String]
multTable = [show a ++ " * " ++ show b  ++ " = " ++ show (a * b) 
            | a <- [1..9], b <- [1..9], a <= b]
            -- align `|` with `[`
```


prefer 'where' over 'let .. in' in pure functions 'let' bindings are commonly used in 'do' blocks but there are some holy wars going on there thus I won't really care which one do you use, as long as it looks good

one thing to notice, try avoid have both `let .. in` and `where` in the same function definition

for comments, write it before and after function but if it is a super short one, you can put in same line

