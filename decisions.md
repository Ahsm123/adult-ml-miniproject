6/5
Which architectures did we choose and why?

Opgaven siger vi skal vælge 2 algoritmer (arkitekturer). Vi kan vælge MLP, CNN
random forest, GBT.

Da CNN er til billedgenkendelse, og Random Forest + GBT er samme familie,
vælger vi at sammenligne MLP med GBT. 

Vi har 48000+ rækker, hvilket vi vurderer at være nok til MLP

Hvilke metrics bruger vi til at måle hvor god vores arkitektur er, og hvorfor valgte vi den/dem?

- Vi kan se ved at visualisere count på target af det består af ca 1/4 > 50K og 3/4 <=50K
- Dette betyder at det er et imbalanced datasæt og vi har derfor valgt at bruge 
- recall, precision og f1 da accuracy er misvisende.

Vi starter med at klargøre dataen:
- Fordi education og education_num repræsenterer det samme, men education_num allerede er
- numerisk, samt har et hierakisk forhold hvor højere tal = højere udd. niveau, vælger vi at beholde
- education_num og fjerne education





