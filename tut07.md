## Augabe 1

* `A = (λ t s u . s)`
* `B = (λ t s . s t)`
* `C = (λ c . c c)`
* `D = C C`
* `E = (λ e t . e t e)`

## Aufgabe 2
### Aufgabenteil (a)

```
⟨pow⟩ ⟨2⟩ = (λnfz.n(λgx.g(gx)) f z) ⟨2⟩
         β
         ⇒ (λfz.⟨2⟩(λgx.g(gx)) f z)
         = (λfz.(λxy.x(xy)) (λgx.g(gx)) f z)
         β
         ⇒ (λfz.(λy.(λgx.g(gx))((λgx.g(gx)) y)) f z)
         β
         ⇒ (λfz.(λy.(λgx.g(gx))(λx.y(yx))) f z)
         β
         ⇒ (λfz.(λy.(λx.(λx.y(yx))((λx.y(yx)) x))) f z)
         β
         ⇒ (λfz.(λy.(λx.(λx.y(yx))(y(yx)))) f z)
         β
         ⇒ (λfz.(λy.(λx.y(y(y(yx))))) f z)
         β
         ⇒ (λfz.(λx.f(f(f(fx)))) z)
         β
         ⇒ (λfz.f(f(f(f z))))
         = ⟨4⟩
```

### Aufgabenteil (b)

Für eine nat. Zahl `n` ∈ ℕ, gilt: `⟨pow⟩ ⟨n⟩ ⇒* ⟨2ⁿ⟩`.
Der Term berechnet also für jede nat. Zahl `n` den Wert `2ⁿ`.

__Erklärung__:
Das Church-Numeral `⟨2⟩` ist eine Funktion, die ihr erstes Argument (eine beliebige Funktion) zweimal auf das zweite Argument anwendet.
Es ist also eine Funktion `g₂`, sodass `g₂(f)(z) = f(f(z))`.
(Wir schreiben hier `g₂(f)(z)` statt `g₂(f, z)`, um partielle Applikationen darstellen zu können.
 So bindet `g₂(f)` nur das erste Argument `f` an die Funktion `g₂`.
 Desweiteren ist diese partielle Applikation linksassoziativ, also implizit als `(g₂(f))(z)` geklammert.)
Im Term `⟨pow⟩` findet man nun den Teilterm `n (λgx.g(gx)) f z`, der nach Substitution von `n` durch `⟨2⟩` gleich der Applikation `⟨2⟩ ⟨2⟩ f z` ist.
Diese Applikation ist nun also gleich
```
( g₂(g₂)(f) )(z) = ( g₂(g₂(f)) )(z)
                = g₂( g₂(f) )(z)
                = g₂(f)( g₂(f)(z) )
                = g₂(f)( f(f(z)) )
                = f(f( f(f(z)) )).
```
Also die verfache Anwendung von `f` auf `z`.

Für das Church-Numeral `g₃` hätten wir nun z.B.
```
( g₃(g₂)(f) )(z) = ( g₂(g₂(g₂(f))) )(z)
                = g₂( g₂(g₂(f)) )(z)
                = g₂(g₂(f)) ( g₂(g₂(f)) (z) )
                = g₂(g₂(f)) ( g₂(g₂(f)) (z) )
                = g₂(g₂(f)) ( f(f(f(f(z)))) )
                = f(f(f(f( f(f(f(f(z)))) )))).
```
Also die achtfache Anwendung von `f` auf `z`.
Wir sehen so, dass jede Applikation von `g₂` in `g₂(g₂( … g₂(f) …))` die Anwendungen von `f` verdoppelt.

### Aufgabenteil (c)

Der Term `(λbnfz.nbfz) ⟨b⟩ ⟨n⟩` berechnet den Wert bⁿ.

## Aufgabe 3
### Aufgabenteil (a)
```
(λfx.ffx) (λy.x) z
   `---´  `----´
    GV: {x} FV: {x}
α
⇒ (λfx₁.ffx₁) (λy.x) z
β
⇒ (λx₁.(λy.x)(λy.x)x₁) z
       `------------´  -
        GV: {y}        FV: {z}
β
⇒ (λy.x) (λy.x) z
       -  `----´
       GV: ∅ FV: {x}
β
⇒ x z
```

### Aufgabenteil (b)
*Nebenrechnung*:
```
⟨Y⟩⟨F⟩ = (λh.(λy.h(yy))(λy.h(yy))) ⟨F⟩
     β
     ⇒ (λy.⟨F⟩(yy))(λy.⟨F⟩(yy)) = ⟨YF⟩
     β
     ⇒ ⟨F⟩((λy.⟨F⟩(yy))(λy.⟨F⟩(yy))) = ⟨F⟩⟨YF⟩
```

```
⟨Y⟩⟨F⟩⟨6⟩⟨5⟩⟨3⟩
β
⇒* ⟨F⟩⟨YF⟩⟨6⟩⟨5⟩⟨3⟩
β
⇒* ⟨ite⟩ (⟨iszero⟩ (⟨sub⟩ ⟨6⟩ ⟨5⟩))
                  (⟨add⟩ ⟨5⟩ ⟨3⟩)
                  (⟨succ⟩ (⟨YF⟩ (⟨pred⟩ ⟨6⟩) (⟨succ⟩ ⟨5⟩) (⟨mult⟩ ⟨2⟩ ⟨3⟩)))
⇒* ⟨succ⟩ (⟨F⟩ ⟨YF⟩ ⟨5⟩ ⟨6⟩ ⟨6⟩)
⇒* ⟨succ⟩ (⟨ite⟩ (⟨iszero⟩ (⟨sub⟩ ⟨5⟩ ⟨6⟩))
                          (⟨add⟩ ⟨6⟩ ⟨6⟩)
                          (⟨succ⟩ (⟨YF⟩ (⟨pred⟩ ⟨5⟩) (⟨succ⟩ ⟨6⟩) (⟨mult⟩ ⟨2⟩ ⟨6⟩))))
⇒* ⟨succ⟩ ⟨12⟩
⇒* ⟨13⟩
```

### Aufgabenteil (c)
```
⟨G⟩ = (λgxy. ⟨ite⟩ (⟨iszero⟩ x)
                  (⟨mult⟩ 2 (⟨succ⟩ y))
                  (⟨ite⟩ (⟨iszero⟩ y)
                        (⟨mult⟩ 2 (⟨succ⟩ x))
                        (⟨add⟩ ⟨4⟩ (g (⟨pred⟩ x) (⟨pred⟩ y)))))
```

## Zusatzaufgabe 1
### Aufgabenteil (a)

```
(λzx.zx(λy.yx)) (λy.zx) (λz.z)
   `---------´  `-----´
    GV: {x,y}    FV: {z,x}
α
⇒ (λzx₁.zx₁(λy.yx₁)) (λy.zx) (λz.z)
β
⇒ (λx₁.(λy.zx)x₁(λy.yx₁)) (λz.z)
       `---------------´ `-----´
        GV: {y}           FV: ∅
β
⇒ (λy.zx) (λz.z) (λy.y(λz.z))
      `-´  `----´
       GV: ∅ FV: ∅
β
⇒ z x (λy.y(λz.z))
```

### Aufgabenteil (b)
```
⟨F⟩ = (λfxy. ⟨ite⟩ (⟨iszero⟩ (⟨pred⟩ x))
                  (⟨mult⟩ 2 y)
                  (⟨add⟩ y (⟨mult⟩ (⟨succ⟩ x) (f (⟨pred⟩ x) (⟨add⟩ x y))))
```

### Aufgabenteil (c)
*Nebenrechnung*:
```
⟨Y⟩⟨G⟩ = (λh.(λy.h(yy))(λy.h(yy))) ⟨G⟩
     β
     ⇒ (λy.⟨G⟩(yy))(λy.⟨G⟩(yy)) = ⟨YG⟩
     β
     ⇒ ⟨G⟩((λy.⟨G⟩(yy))(λy.⟨G⟩(yy))) = ⟨G⟩⟨YG⟩
```

```
⟨Y⟩⟨G⟩⟨3⟩⟨0⟩
β
⇒* ⟨G⟩⟨YG⟩⟨3⟩⟨0⟩
β
⇒* ⟨ite⟩ (⟨iszero⟩ ⟨0⟩) (⟨succ⟩ ⟨3⟩) (⟨YG⟩ ⟨4⟩ (⟨pred⟩ ⟨0⟩))
β
⇒* ⟨4⟩
```

## Zusatzaufgabe 2
__Induktionsbasis__:
Sei `x :: Int` eine beliebige Ganzzahl.
Wir zeigen nun, dass `product (yield (Node x [])) = yieldProd (Node x [])`.
```
product (yield (Node x []))
 = product [x]              (#4)
 = x                        (L1)
 = yieldProd (Node x [])    (#8)
```

__Induktionsschritt__:
Sei `k ≥ 1` und `t₁ :: IntTree, …, tₖ :: IntTree`, sodass, für alle `i` ∈ { `1`, …, `k` },
``` product (yield tᵢ) = yieldProd tᵢ ``` (`IV`).

Weiterhin sei `x :: Int` eine beliebige Ganzzahl.
Wir zeigen nun, dass `product (yield (Node x [t₁, …, tₖ])) = yieldProd (Node x [t₁, …, tₖ])`.
```
product (yield (Node x [t₁, …, tₖ]))
 = product (concat (map yield [t₁, …, tₖ]))             (#5)
 = product (concat [yield t₁, …, yield tₖ])             (L2)
 = product [product (yield t₁), …, product (yield tₖ)]  (L3)
 = product [yieldProd t₁, …, yieldProd tₖ]              (IV)
 = yieldProd (Node x [t₁, …, tₖ])                       (#9)
```
