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

Grunden til vi ikke måler accuracy, er fordi en naiv model der bare gætter >50K, vil have ret
3/4 gange. 

Fordi datasættet er ubalanceret bør vi bruge stratified train_test split, 
for at sikre os at der er en lige fordeling. Altså at de har samme fordeling af
labels. Fjerne sampling bias. 


Vi starter med at klargøre dataen:
- Fordi education og education_num repræsenterer det samme, men education_num allerede er
- numerisk, samt har et hierakisk forhold hvor højere tal = højere udd. niveau, vælger vi at beholde
- education_num og fjerne education

For at vi kan arbejde med dataen skal vi lave alle str værdierne til numeriske, derfor vælger vi at 
one-hot-encode følgende features, fordi de ikke har noget internt hieraki
- occupation
- workclass
- marital-status
- relationship-status
- race
- native-country

native country har 42 forskellige, hvilket vil give 42 features hvis vi one-hot encoder,
derfor har vi taget et valg om at tage lavet det til 10, ved at samle alt efter 9 i en 10'er
kaldet other

Sex og Income har kun to udfald, henholdsvis Male/Female <=50K eller >50K
Derfor bruger vi binary encoding her.

Splitter det først i 80/20 til træning og test
splitter derefter træning i train og val
Vi bruger stratify begge gange for at sikre ens fordeling af labels pga ubalance.

Vi genbruger alle ovenstående trin i begge modeller, men fremadrettet deler vi op,
da der er forskellige metoder og krav til hvordan vi encoder.

Da det er binær classification <=50k eller >50K bruger vi binary cross-entropy
som loss function.

MLP:

I forhold til de manglende værdier, så lavede vi alle '?' om til NaN.
Vi kan vælge mellem at erstatte dem med andre værdier, medianer etc, behandle dem om sin egen
kategori, eller fjerne dem.

Hvis man brugte dem som sin egen kategori, ville man antage at de blev sat af samme grund.
Hvis man fjerne alle rækker med NaN efter vi replacede ? med NaN, ender vi på 45222 rækker,
hvilket vi syntes er okay at miste, for at vores datasæt er rent. 
Fordelingen mellem de to kategorier steg også meget  meget minimalt, men ikke den forkerte vej.

i preprocessing fejlede den fordi den ikke kunne finde 'France', hvilke betyder at
valideringssættet ikke havde en kategori som train havde, derfor smider vi handle_unkown=ignore på

Første model overfittede tydeligt efter 4-5 epochs, så der blev tilføjet early stopping med
en pat = 10, samt restore best weights = true, så vi får de bedste vægte og ikke bare de sidste.

Primært valgt StandardScaler fordi vi havde features som capital-gain/loss, som har meget store
outliers, som MinMaxScaler er følsom overfor, fordi den vil presse medianen meget tæt på 0

MLP - Fintuning

0.7497 - recall: 0.6106
f1        : 0.6731
loss      : 0.3203
precision : 0.7497
recall    : 0.6106

Neuroner pr. lag max fra 128 til 256
Lag max 3 til max 10
Epochs 10 > 30
EarlyStopping
Fjernet SGD optimizer

f1        : 0.6285
loss      : 0.3238
precision : 0.7952
recall    : 0.5196

har sat samme widst i hvert hidden layer.

f1        : 0.6663
loss      : 0.3234

Vi skal have kigget på: 
Lav corr_matrix og stratify efter det, og ikke på target.
da vi gerne vil have lige fordeling af de vigtigste features

Til eksamen
datasæt og hvordan det ser ud
hvad problemstillingen er, og de valg vi har taget med daata.
arkitektur, hvorfor valgt dette
sammenligning / resultat