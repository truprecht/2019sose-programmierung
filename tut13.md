## Aufgabe 1
### Aufgabenteil (a)

Wir bestimmen die Schleifeninvariante in zwei Teilen: SI = F ∧ G.
Für den ersten Teil dokumentieren wir die Belegung der Variablen während den Schleifendurchläufen.

Iteration   | `x`
------------|-----
0           | `a`
1           | `a`-3
2           | `a`-6
…           | …
i           | `a`-3i

Da `x` die einzige Variable ist, die in der Schleife manipuliert wird, können wir die Zählvariable i nicht eliminieren.
Wir können deswegen nur eine schwächere Aussage als üblich bilden: F = (`a` mod 3 = `x` mod 3).
(Der Restbetrag bei der Division durch 3 bleibt immer gleich.)

Für den zweiten Teil wählen wir eine Aussage über die Variablen in der Schleifenbedingung, sodass Sie vor, während und nach der Schleife gilt.
In der Bedingung kommt wieder nur `x` vor.
Die Vorbedingung gibt indirekt an, dass vor der Schleife (`x` ≥ 0) gilt.
Während der Schleife wird `x` dekrementiert, bis es kleiner oder gleich 2 ist.
Da es immer um 3 dekrementiert wird, wird es aber nicht kleiner als 0.
Also gilt auch während und nach der Schleife, dass (`x` ≥ 0).

SI = (`a` mod 3 = `x` mod 3) ∧ (`x` ≥ 0)

## Aufgabenteil (b)

A = SI<br>
B = SI ∧ ¬(`x` > 2)<br>
C = SI ∧ (`x` > 2)<br>
D = SI<br>
E = (`a` mod 3 = (`x` - 3) mod 3) ∧ ((`x` - 3) ≥ 0)

## Zusatzaufgabe 1
### Aufgabenteil (a)

tab_`f` = [`x`/(var, global, 1),
            `h`/(proc, 1), `g`/(proc, 2), `f`/(proc, 3),
            `a`/(var,lokal,-3), `b`/(var, lokal, -2), `c`/(var, lokal, 1)]

```
        LOAD (global, 1); LIT 1; GT; JMC 3.1.1;
        LOAD (lokal, -2); PUSH; CALL 2; JMP 3.1.3;
3.1.1:  LOAD (lokal, -3); PUSH; LOADA (global, 1); PUSH; CALL 1;
3.1.3:  LOADI (-2); LIT 1; ADD; STORE (lokal, 1);
```

## Aufgabenteil (b)

BZ |    DK | LK                      | Ref | Inp | Out
--:|------:|:------------------------|:---:|----:|:----
12 |     ε | 0:3:__0__:7             | 3   | 5   | ε
13 |       | 5:3:__0__:7             |     | ε   |
14 |     7 |                         |     |     |
15 |     ε | 5:3:__0__:7:7           |     |     |
16 |     1 |                         |     |     |
17 |     ε | 5:3:__0__:7:7:1         |     |     |
4  |       | 5:3:0:7:7:1:18:__3__    | 8   |     |
5  |       |                         |     |     |
6  |     7 |                         |     |     |
7  |   5:7 |                         |     |     |
8  |    12 |                         |     |     |
9  |    ε  | 12:3:0:7:7:1:18:__3__   |     |     |
18 |       | 12:3:__0__:7            | 3   |     |
19 |       |                         |     |     | 12
3  |       | 12                      | 0   |     |
0  |       |                         |     |     |

## Zusatzaufgabe 2
### Aufgabenteil (a)

```
avg :: [Float] -> Float
avg xs
    | len == 0  = 0
    | otherwise = sum / len
    where
        (sum, len) = sumlen xs
        sumlen :: [Float] -> (Float, Int)
        sumlen [] = (0, 0)
        sumlen (x:xs) = let (sum, len) = sumlen xs
                        in (sum + x, len + 1)
```

### Aufgabenteil (b)

```
partition :: (a -> Bool) -> [a] -> ([a], [a])
partition _ [] = ([], [])
partition p (x:xs)
    | p x       = (x:ys,   zs)
    | otherwise = (  ys, x:zs)
    where
        (ys, zs) = partition p xs
```

### Aufgabenteil (c)

```
maxrep :: [Int] -> Int
maxrep xs = maximum (repetitions xs)
    where
        -- zählt die Wiederholungen jedes Listenelements
        -- z.B. [1,1]           ↦ [2]
        --      [1,2,2,2,3,3,4] ↦ [1,3,2,1]
        --      []              ↦ [0]
        repetitions :: [Int] -> [Int]
        repetitions [] = [0]
        repetitions (x:xs) = repetitions' 0 x (x:xs)
        repetitions' acc _ [] = [acc]
        repetitions' acc y (x:xs)
            | y == x    = repetitions' (acc + 1) y xs
            | otherwise = acc : repetitions' 1 x xs
```

## Zusatzaufgabe 3

{ (σ(α, σ(γ(α), σ(x₂, x₃))), σ(α, σ(x₁, σ(x₂, σ(x₂, x₁))))) }

(Dek.) ⇒ { (α, α), (σ(γ(α), σ(x₂, x₃)), σ(x₁, σ(x₂, σ(x₂, x₁)))) }

(2×Dek.) ⇒ { (γ(α), x₁), (σ(x₂, x₃), σ(x₂, σ(x₂, x₁))) }

(Dek.) ⇒ { (γ(α), x₁), (x₂, x₂), (x₃, σ(x₂, x₁)) }

(Elem.) ⇒ { (γ(α), x₁), (x₃, σ(x₂, x₁)) }

(Vert.) ⇒ { (x₁, γ(α)), (x₃, σ(x₂, x₁)) }

(Subst.) ⇒ { (x₁, γ(α)), (x₃, σ(x₂, γ(α))) }

Allg. Unifikator: x₁ ↦ γ(α), x₂ ↦ x₂, x₃ ↦ σ(x₂, γ(α))

## Zusatzaufgabe 4

__Induktionsanfang__:
Sei `ys` = `[]` und `a, b :: Int` zwei beliebige ganze Zahlen.

```
mul b (mul a ys) = mul b (mul a [])
                 = mul b []         (Z. 2)
                 = []               (Z. 2)
                 = mul (b * a) []   (Z. 2)
                 = mul (b * a) ys
```

__Induktionsschritt__:
Sei `ys :: [Int]` eine Liste, sodass gilt:
    Für alle `a, b :: Int` gilt: mul b (mul a ys) = mul (b * a) ys      (IH)

Seien `y, a, b :: Int` ganze Zahlen.

```
mul b (mul a (y:ys))
    = mul b ((a * y) : mul a ys)         (Z. 3)
    = (b * (a * y)) : mul b (mul a ys)   (Z. 3)
    = ((b * a) * y)) : mul b (mul a ys)  (Assoziativität)
    = ((b * a) * y)) : mul (b * a) ys    (IH)
    = mul (b * a) (y:ys)                 (Z. 3)
```

## Zusatzaufgabe 5
### Aufgabenteil (a)

```
        (λfx.x(f(fx))) (λy.xy)
           ----------  -------
           GV: {x}     FV: {x}
α
⇒      (λfx₁.x₁(f(fx₁))) (λy.xy)
β
⇒      (λx₁.x₁((λy.xy)((λy.xy) x₁)))
                           --  --
                           GV: ∅ FV: {x₁}
β
⇒      (λx₁.x₁((λy.xy)(xx₁)))
                   -- -----
                   GV: ∅ FV: {x, x₁}
β
⇒      (λx₁.x₁(x(xx₁)))

```

### Aufgabenteil (b)

```
f x y
    | y == 0         = 2 * x
    | x `mod` 2 == 0 = f  x    (y-1)
    | otherwise      = f (x*2) (y-1)
```

### Aufgabenteil (c)

```
⟨Y⟩⟨F⟩ = (λh.(λy.h(yy))(λy.h(yy))) ⟨F⟩
β
⇒ (λy.⟨F⟩(yy)) (λy.⟨F⟩(yy))
β
⇒ ⟨F⟩ ((λy.⟨F⟩(yy)) (λy.⟨F⟩(yy)))
      ---------------------------
       = ⟨YF⟩
```

```
⟨Y⟩⟨F⟩⟨2⟩⟨1⟩
⇒* ⟨F⟩⟨YF⟩⟨2⟩⟨1⟩
⇒* ⟨ite⟩ (⟨iszero⟩⟨1⟩)                                  -- (⟨iszero⟩⟨1⟩) ⇒* ⟨false⟩
         (⟨mult⟩ ⟨2⟩ ⟨2⟩)
         (⟨ite⟩ (⟨iszero⟩ (⟨mod⟩ ⟨1⟩ ⟨2⟩))              -- (⟨iszero⟩ (⟨mod⟩ ⟨1⟩ ⟨2⟩)) ⇒* ⟨false⟩
                 (⟨YF⟩ ⟨2⟩         	(⟨pred⟩ ⟨1⟩))
                 (⟨YF⟩ (⟨mult⟩⟨2⟩⟨2⟩) (⟨pred⟩ ⟨1⟩)))    -- ⟨YF⟩ ⇒* ⟨F⟩⟨YF⟩, (⟨mult⟩⟨2⟩⟨2⟩) ⇒* ⟨4⟩, (⟨pred⟩ ⟨1⟩) ⇒* ⟨0⟩
⇒* ⟨F⟩⟨YF⟩⟨4⟩⟨0⟩
⇒* ⟨ite⟩ (⟨iszero⟩⟨0⟩)                                  -- (⟨iszero⟩⟨0⟩) ⇒* ⟨true⟩
         (⟨mult⟩ ⟨4⟩ ⟨2⟩)                               -- (⟨mult⟩ ⟨4⟩ ⟨2⟩) ⇒ ⟨8⟩
         (…)
⇒* ⟨8⟩
```

## Zusatzaufgabe 6

```
                     ?-  insert(<t1>, <t2>, X)
{X=tree(a, L1, R1)}  ?-  insert(tree(b, nil, nil), <t2>, L1), insert(tree(v, nil, nil), <t2>, R1).           %6
{L1=tree(b, L2, R2)} ?-  insert(nil, <t2>, L2), insert(nil, <t2>, R2), insert(tree(v, nil, nil), <t2>, R1).  %6
{L2=nil, R2=nil}     ?-* insert(tree(v, nil, nil), <t2>, R1).                                                %4
{R1=<t2>}            ?-  istree(<t2>).                                                                       %5
                     ?-* istree(nil), istree(nil), istree(nil).                                              %2
                     ?-*.                                                                                    %1

% Belegung für X: tree(a, tree(b, nil, nil), <t2>)
```
