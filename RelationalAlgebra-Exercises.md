# Q1. Find all pizzas eaten by at least one female over the age of 20.

    \project_{pizza} (\select_{age >20 and gender='female'} Person \join Eats);

# Q2. Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.)

    \project_{name} ( \select_{gender='female' and pizzeria='Straw Hat'} (Person \join Eats \join Serves));

# Q3. Find all pizzerias that serve at least one pizza for less than \$10 that either Amy or Fay (or both) eat. 

    \project_{pizzeria} ( \select_{ (name='Amy' or name='Fay') and price<10 } (Person \join Eats \join Serves));

# Q4. Find all pizzerias that serve at least one pizza for less than \$10 that both Amy and Fay eat. 

    \project_{pizzeria} (
    \project_{pizza,pizzeria}( \select_{ (name='Amy' and price<10) } (Person \join Eats \join Serves) )
       \join
    \project_{pizza,pizzeria}( \select_{ (name='Fay' and price<10) } (Person \join Eats \join Serves) )
    );

# Q5. Find the names of all people who eat at least one pizza served by Dominos but who do not frequent Dominos. 

    \project_{name} \select_{pizzeria='Dominos'}  (Person \join Eats \join Serves)
     \diff
    \project_{name} \select_{pizzeria='Dominos'} (Person \join Frequents);

# Q6. Find all pizzas that are eaten only by people younger than 24, or that cost less than \$10 everywhere they're served. 

    ( \project_{pizza} Serves
       \diff
      \project_{pizza} \select_{price >= 10} Serves
    )
    \union
    (
        \project_{pizza} (Person \join Eats)
         \diff
        \project_{pizza} (
        \select_{age >=24} (Person \join Eats)
        )
    );

# Q7. Find the age of the oldest person (or people) who eat mushroom pizza.
// (This query is quite challenging; congratulations if you get it right.)

    \project_{age} \select_{pizza='mushroom'} (Person \join Eats)
     \diff
    \project_{a1} (
    \rename_{a1} (\project_{age} \select_{pizza='mushroom'} (Person \join Eats))
      \join_{a1<a2}
    \rename_{a2} (\project_{age} \select_{pizza='mushroom'} (Person \join Eats))
    )
    ;

# Q8. Find all pizzerias that serve only pizzas eaten by people over 30. 
// (This query is quite challenging; congratulations if you get it right.)


    \project_{pizzeria} Serves

    \diff

    \project_{pizzeria} (
    (
        // Rest of the pizza
        \project_{pizza} Eats
          \diff
        \project_{pizza} (\select_{age > 30} (Person \join Eats))
        )
         \join
        Serves
    ) ;


# Q9. Find all pizzerias that serve every pizza eaten by people over 30. 
// (This query is very challenging; extra congratulations if you get it right.)

(\project_{pizzeria}Serves) 

\diff 

(\project_{pizzeria} (
  (\project_{pizzeria} Serves) 
    \cross 
  (\project_{pizza}(\select_{age>30}Person \join Eats)) 
  \diff 
  (\project_{pizzeria,pizza} (
  (\select_{age>30}Person \join Eats) \join Serves)))
);
