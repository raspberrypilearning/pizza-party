## Aggiungi i clicker automatici

Aggiungi un aiutante che prepara pizze ogni secondo, anche quando il giocatore smette di cliccare. Il giocatore può assumere lo stesso tipo di aiutante più di una volta, e ognuno di essi aggiunge una pizza in più al secondo.

> [!TASK]
>
> Aggiungi uno sprite di supporto. Scegli un personaggio o una macchina che sembri in grado di funzionare automaticamente. Il progetto dimostrativo utilizza uno chef.
>
> <img src="images/chef.png" alt="The demo project's chef helper." width="300" height="300" style="object-fit: contain;">
>
> Utilizza il tuo assistente, oppure salva [lo sprite dello chef](images/chef-sprite.png) e importalo con **Importa**.

> [!TASK]
>
> Seleziona l'aiutante e usa la casella **Dimensione** sotto lo Stage per adattarlo. Lo chef del progetto dimostrativo ha una dimensione del 30%.
>
> Trascinalo su una parte vuota dello Stage dove non coprirà il punteggio o l'equipaggiamento. Il progetto dimostrativo mette lo chef sulla destra.

> [!TASK]
>
> Aggiungi il suono `Clang`{:class="block3sound"} allo sprite di supporto.

> [!TASK]
>
> Crea una variabile chiamata `chef`{:class="block3variables"}. Questo campo memorizza il numero di chef assunti dal giocatore.

> [!TASK]
>
> Crea una variabile chiamata `prezzo chef`{:class="block3variables"}. Memorizza il costo delle pizze per il prossimo chef.

> [!TASK]
>
> Crea una variabile chiamata `pizze al secondo`{:class="block3variables"}. Mostra quante pizze preparano tutti gli aiutanti in ogni secondo.

> [!TASK]
>
> Fai clic su `Stage`{:class="block3looks"} e assegna i valori iniziali alle variabili degli aiutanti sul click alla bandierina verde.
>
> Il primo aiutante costa `50`, quindi la rotella tagliapizza si sblocca comunque per prima a `25`.
>
> ![Selezione dello Stage, a destra dell'elenco degli sprite.](images/select-stage.png)
>
> ```blocks3
> when green flag clicked
> set [pizzas v] to (0)
> set [pizzas per click v] to (1)
> +set [chefs v] to (0)
> +set [chef price v] to (50)
> +set [pizzas per second v] to (0)
> ```

Clicca sulla bandierina verde. Le nuove variabili dovrebbero mostrare `chef 0`, `prezzo chef 50`, e `pizze al secondo 0`.

> [!TASK]
>
> Sullo sprite dell'aiutante, inizia il suo script di acquisto. Cliccando sull'assistente, si spende il prezzo corrente e si assume un assistente.
>
> Nel blocco `cambia`{:class="block3variables"} va usato di un numero negativo per spendere le pizze. `0 - prezzo dello chef` converte il prezzo in quel numero negativo.
>
> <p align="center"><img src="images/chef.png" alt="Chef sprite icon." width="96" height="96" style="object-fit: contain;"></p>
>
> ```blocks3
> when this sprite clicked
> start sound (Clang v)
> change [pizzas v] by ((0) - (chef price))
> change [chefs v] by (1)
> ```

> [!TASK]
>
> Aggiungi un blocco per far sì che il prossimo aiutante costi circa il 15% in più.
>
> Multiplying by `1.15` adds 15% to the old price. `round`{:class="block3operators"} keeps the new price as a whole number.
>
> ```blocks3
> when this sprite clicked
> start sound (Clang v)
> change [pizzas v] by ((0) - (chef price))
> change [chefs v] by (1)
> +set [chef price v] to (round ((chef price) * (1.15)))
> ```

The first helper costs 50 pizzas. After buying it, `chefs` should be `1` and `chef price` should be `58`.

> [!TIP]
>
> A **progression curve** controls how quickly a game gets harder, faster, or more expensive as the player improves.

> [!TASK]
>
> Make the helper appear only when the player can afford the current price.
>
> Scratch has no `greater than or equal to` block. Because the score uses whole numbers, checking for `pizzas > chef price - 1` does the same thing.
>
> <p align="center"><img src="images/chef.png" alt="Chef sprite icon." width="96" height="96" style="object-fit: contain;"></p>
>
> ```blocks3
> when green flag clicked
> set drag mode [not draggable v]
> hide
> forever
> if <(pizzas) > ((chef price) - (1))> then
> show
> else
> hide
> end
> end
> ```

Click the green flag and click the main sprite. The helper should stay hidden up to 49 pizzas and appear when the score reaches 50.

> [!TASK]
>
> Click the `Stage`{:class="block3looks"}. In `My Blocks`{:class="block3custom"}, click **Make a Block**, name it `update pizzas per second`{:class="block3custom"}, and build its definition.
>
> Each chef makes one pizza per second, so the rate is the same as the number stored in `chefs`{:class="block3variables"}.
>
> ![Selecting the Stage, to the right of the sprite list.](images/select-stage.png)
>
> ![The Make a Block button in the My Blocks palette.](images/make-a-block.png)
>
> ```blocks3
> define update pizzas per second
> set [pizzas per second v] to (chefs)
> ```

> [!TIP]
>
> In many programming languages, a reusable block of code like this is called a **function**.

> [!TASK]
>
> Still on the Stage, add a script that runs the new block whenever a helper asks for the rate to be updated.
>
> In the `when I receive`{:class="block3events"} block, open the message menu, choose **New message**, and name it `update`.
>
> ```blocks3
> when I receive (update v)
> update pizzas per second :: custom
> ```

> [!TASK]
>
> Return to the helper sprite. Broadcast `update` at the end of its buy script so the Stage recalculates the rate after every purchase.
>
> ```blocks3
> when this sprite clicked
> start sound (Clang v)
> change [pizzas v] by ((0) - (chef price))
> change [chefs v] by (1)
> set [chef price v] to (round ((chef price) * (1.15)))
> +broadcast (update v)
> ```

> [!TASK]
>
> On the Stage, finish the green flag script with the game's clock. Every second, it adds the current `pizzas per second`{:class="block3variables"} rate to the score.
>
> ```blocks3
> when green flag clicked
> set [pizzas v] to (0)
> set [pizzas per click v] to (1)
> set [chefs v] to (0)
> set [chef price v] to (50)
> update pizzas per second :: custom
> forever
> wait (1) seconds
> change [pizzas v] by (pizzas per second)
> end
> ```

> [!TIP]
>
> A regular moment when a game updates its numbers is called a **tick**. This clicker has one tick every second.

Click the green flag, earn 50 pizzas, and buy one helper. Check that `chefs` becomes `1` and `pizzas per second` becomes `1`, then stop clicking. The score should rise by one every second.

The helper is a repeatable upgrade. When the score reaches its new price, the same sprite appears again so the player can hire another one.
