# Nieparametryczna analiza czynników wpływających na wyniki w nauce i modelowanie predykcyjne z wykorzystaniem uczenia maszynowego

English version: [README.md](README.md)

*Student Performance Factors*: https://www.kaggle.com/datasets/lainguyn123/student-performance-factors/data

<img width="1325" height="742" alt="image" src="https://github.com/user-attachments/assets/3d2f9e80-ef58-4570-ac07-49a69250c8f3" />

*Liczba Badanych Uczniów (6 378) na dashboardzie jest mniejsza niż liczba obserwacji (6 607) w analizie, ponieważ do dashboardu nie wprowadzono imputacji braków danych, zostały one usunięte.*

## O projekcie

Projekt skupia się na nieparametrycznej i ekonometrycznej analizie zależności między wynikami w nauce a czynnikami potencjalnie na nie wpływającymi oraz budowie modelu uczenia maszynowego przewidującego wyniki z egzaminu. Badanie przeprowadzono na próbie 6 607 obserwacji. Analiza łączy w sobie kompleksowe czyszczenie danych, eksploracyjną analizę statystyk opisowych, testowanie założeń o normalności i jednorodności wariancji wymaganych do ANOVA, nieparametryczne testy U Manna-Whitneya, Kruskala-Wallisa z testami post-hoc, badanie korelacji oraz budowę i ewaluację modeli predykcyjnych.

---

## W skrócie

**Cel:** identyfikacja zmiennych wpływających na wyniki z egzaminu i dobór najlepszego modelu uczenia maszynowego.

**Dane:** publiczny zbiór z Kaggle, 6 607 obserwacji. 235 braków danych uzupełnionych z wykorzystaniem metody MICE.

**Podejście:** przygotowanie i czyszczenie danych → eksploracyjna analiza danych → testowanie założeń → analiza zależności → budowa i ewaluacja modeli predykcyjnych → dashboard w Power BI.

**Model:** Regresja liniowa (wybrany spośród regresji liniowej, Random Forest i Gradient Boosting).

| Metryka | Wartość |
| :---: | :---: |
| R^2 | 0,6952 |
| RMSE | 2,2804 |
| MAE | 0,5218 |

**Najważniejsze wnioski:**

* Płeć, godziny snu i typ szkoły nie wpływają na wyniki w nauce.
* Wykryto zależności między wynikami z egzaminu a poziomem motywacji, dostępem do internetu, dostępem do zasobów edukacyjnych czy dochodami rodziców.
* Obecność skrajnych wyników, które pogarszają jakość modeli.
* Silnie liniowy charakter próby.

**Zawartość repozytorium:**

student_performance.ipynb: czyszczenie i przygotowanie danych, eksploracyjna analiza danych, weryfikacja założeń, testy statystyczne, modelowanie, interpretacja 
student_performance_dashboard_pl.pbix: dashboard analityczny w Power BI
requirements.txt: biblioteki

Jak uruchomić: pip install -r `requirements.txt`, następnie notebook: `student_performance.ipynb`

---

## Zmienne

6 607 obserwacji

Zbiór danych zawiera informacje o frekwencji, zaangażowaniu rodziców, nawyków uczniów związanych z nauką oraz innych aspektach mogących wpływać na wyniki szkolne.

* `Hours_Studied` - liczba godzin spędzanych tygodniowo na naukę,
* `Attendance` - frekwencja na lekcjach (%),
* `Parental_Involvement` - poziom zaangażowania rodziców w edukację ucznia,
* `Access_to_Resources`- dostępność edukacyjnych zasobów,
* `Extracurricular_Activities` - uczęszczanie na dodatkowe zajęcia,
* `Sleep_Hours`	- średnia liczba godzin snu,
* `Previous_Scores`	- wyniki z poprzednich egzaminów,
* `Motivation_Level`	- poziom motywacji,
* `Internet_Access`	- dostępność internetu,
* `Tutoring_Sessions` - liczba korepetycji w miesiącu,
* `Family_Income`	- poziom dochodu rodziny,
* `Teacher_Quality`	- jakość nauczycieli,
* `School_Type` - rodzaj szkoły,
* `Peer_Influence`	- wpływ równieśników na edukację,
* `Physical_Activity`	- średnia liczba godzin poświęcanych na aktywność fizyczną w tygodniu,
* `Learning_Disabilities`	- trudności w nauce,
* `Parental_Education_Level`	- najwyższy typ wykształcenia rodziców,
* `Distance_from_Home`	- odległość od domu,
* `Gender`	- płeć,
* `Exam_Score`	- wynik egzaminu końcowego.

---

## Architektura projektu

1. Przygotowanie i czyszczenie danych (Python)
2. Eksploracyjna analiza danych (EDA) (Python)
3. Weryfikacja założeń o normalności i jednorodności wariancji (Python)
4. Analiza zależności z wykorzystaniem testów nieparametrycznych (Python)
5. Przygotowanie danych do modeli ML (Python)
6. Trening i ewaluacja modeli regresji liniowej, Random Forest i Gradient Boosting (Python)
7. Interpretacja ważności cech (Python)
8. Budowa analitycznego dashboardu (Power BI) 
9. Wnioski, rekomendacje, ograniczenia i dalsze kroki

---

## Wykorzystane narzędzia

* **Obsługa danych**: `pandas`, `numpy`
* **Imputacja danych**: `sklearn.experimental` (`enable_iterative_imputer`), `sklearn.impute` (`IterativeImputer`)
* **Wizualizacja danych**: `seaborn`, `matplotlib`, `pylab`
* **Statystyka**: `scikit_posthocs`, `scipy` (`stats`), `statsmodels`
* **Uczenie maszynowe**: `sklearn.ensemble` (`GradientBoostingRegressor`, `RandomForestRegressor`), `sklearn.linear_model` (`LinearRegression`), `sklearn.metrics` (`mean_absolute_error`, `mean_squared_error`, `r2_score`), `sklearn.model_selection` (`RandomizedSearchCV`, `train_test_split`)
* **Dashboard:** Power BI Desktop

---

## Krok 1: Przygotowanie i czyszczenie danych

### Duplikaty

Zbiór został sprawdzony pod kątem występowania duplikatów. Nie wykazano istnienia zduplikowanych rekordów.

### Braki danych

Wykryto w sumie 235 braków danych (78 dla `Teacher_Quality`, 90 dla `Parental_Education_Level`, 67 dla `Distance_from_Home`). W celu ich uzupełnienia zastosowano metodę imputacji MICE (*Multiple Imputation by Chained Equations*).

### Wartości odstające

Analiza wykresów pudełkowych (*boxplots*) wykazała obecność wartości odstających dla 3 zmiennych ciągłych (`Hours_Studied`, `Tutoring_Sessions`, `Exam_Score`). W przypadku `Exam_Score` odnotowano wiele wysokich wyników z egzaminu jako skrajne, co prawdopodobnie wynika ze specyfiki zbioru. Obserwacje zachowano do dalszej analizy.

---

## Krok 2: Eksploracyjna Analiza Danych (EDA)

**Zmienne numeryczne:**
> * **Wyniki egzaminu końcowego** wśród badanych uczniów wahają się od 55 do 101 punktów. **Przeciętny uczeń zdobywa 67 punktów.** Większość egzaminowanych **otrzymuje od 65 do 69 punktów**, a wyższy wynik osiąga jedynie 25% badanych.
> * **Uczniowie na nauce spędzają średnio 20 godzin tygodniowo**, co daje niemal 3 godziny dziennie.
> * **Frekwencja większości badanych waha się od 70% do 90%**, ze średnią równą 80%. Najniższa wynosi 60%.
> * Większość uczniów **poświęca na sen średnio 7 godzin**, jednak występują ekstremalnie niskie przypadki (4 godziny) oraz ekstremalnie wysokie (10 godzin).
> * **Średni wynik z poprzednich egzaminów był wyższy niż z egzaminu końcowego (75 vs 67).**
> * Większość uczniów uczęszcza miesięcznie **na 1 lub 2 sesje korepetycji w miesiącu**. Duża liczba badanych nie korzysta wcale z takiego rozwiązania.
> * Badani średno przeznaczają **3 godziny tygodniowo na aktywność fizyczną**.

**Zmienne kategoryczne:**
> * W badanej próbie aż połowa uczniów deklaruje **umiarkowany udział rodziców w ich nauce (3 362, 50.9%).** Duży udział ma miejsce w przypadku 28.9% (1 908) uczniów, a mały tylko w 20.2% (1 337).
> * **Połowa badanych uczniów ma średni dostęp do zasobów naukowych (3 319, 50.2%).** Wysoki dostęp deklaruje prawie 30% wszystkich uczniów (1 975), a **niski niemal 20%. Niski dostęp do zasobów wspomagających naukę może stanowić problem i powodować gorsze oceny, nie można ignorować tej obserwacji, zwłaszcza w momencie, gdy dotyka to aż 1 313 uczniów.**
> * Niemal **60% wszystkich uczniów uczęszcza na dodatkowe zajęcia pozalekcyjne (3 938).**
> * **Jedynie 20% badanych deklaruje wysoki poziom motywacji (1 319).** Wśród uczniów przeważa grupa o średnim (3 351, 50.7%) i niskim (1 937, 29.3%) poziomie. Warto zwrócić uwagę na poziom motywacji i wpływać na ich samozaparcie.
> * **Brak dostępu do internetu nie stanowi problemu w grupie badanych uczniów.** Zgłasza go jedynie 7.6% z nich (499). Mimo to, nie można ignorować potencjalnego negatywnego wpływu na wyniki szkolne, nawet wśród tak nielicznej grupy osób.
> * **Większość uczniów pochodzi z rodzin o niskim (2 672, 40.4%) i średnim (2 666, 40.4%) dochodzie.**
> * **11.1% osób (735) zgłasza niski poziom jakości nauczania.** Stanowi to sygnał do pogłębionej analizy i podjęcia działań w tym zakresie.
> * Spośród badanych uczniów, prawie 70% z nich uczęszcza do szkół publicznych (4 598). Szkoły prywatne stanowią 30.4% (2 009).
> * **Wpływ rówieśników na edukację jest przez większość uczniów oceniany na pozytywny (2 638, 39.9%) lub neutralny (2 592, 39.2%)**, jednak istnieje grupa określający go jako negatywny (1 377, 20.8%), co jest alarmujące. 
> * **Grupa uczniów zmagających się z trudnościami w nauce stanowi 10.5% badanych (695).** Są to osoby, które potrzebują odpowiedniego podejścia i toku nauki.
> * Najliczniejsza grupa uczniów wskazuje, że ich rodzice mają **wykształcenie średnie (3 313, 50.1%).**
> * **10% badanych określa odległość do domu jako dużą (658)**, co potencjalnie może mieć negatywny wpływ na wyniki. 
> * Wśród badanych **przeważają mężczyźni (3 814, 57.7%).** Kobiety stanowią 42.3% grupy (2 793).

---

## Krok 3: Weryfikacja założeń o normalności i jednorodności wariancji

### Test normalności D'Agostino-Pearsona i wykresy Q-Q

Z uwagi na dużą liczebność próby (n = 6 607) do weryfikacji normalności zastosowano test D'Agostino-Pearsona oparty na pomiarze skośności i kurtozy, będąc odpornym na duże liczebności i zapewniając wysoką moc testu. 

**Rozkład nie jest zbliżony do normalnego**, na co przede wszystkim wpływ mają grube ogony (wartości odstające), widoczne na wykresach Q-Q.

### Test jednorodności wariancji Levene'a

Założenie o jednorodności wariancji zostało spełnione dla wszystkich zmiennych. 

---

## Krok 4: Analiza zależności z wykorzystaniem testów nieparametrycznych

Wybór testów nieparametrycznych do analizy zależności został podyktowany brakiem spełnienia założenia o normalności rozkładu.

### Nieparametryczny test U Manna-Whitneya 

Test **U Manna-Whitneya** dla dwóch grup wykazał istotne różnice dla zmiennych `Extracurricular_Activites`, `Internet_Access` i `Learning_Disabilities`. Brak statystycznych różnic wykazano dla `Gender` i `School_Type`. Wyniki testu sugerują wpływ dodatkowych zajęć, dostępu do internetu i trudności w nauce na wyniki z egzaminu, jednak wartości mediany oscylujące wokół 67 dla wszystkich grup i analiza wykresów *box-plot* wskazują na brak dużych różnic w rozkładzie wyników w grupach. Przy tak **dużej próbie (n = 6 607) należy pamiętać o ekstremalnej czułości** na minimalne odchylenia. 

W przypadku grupy charakteryzującej się **brakiem trudności w nauce** można zauważyć **więcej wartości odstających** w porównaniu do grupy uczniów z trudnościami, co skłania do wniosku, że takie osoby częściej osiągają wyższe wyniki. Podobna sytuacja ma miejsce przy **dostępie do internetu**, gdzie **grupa deklarująca takowy dostęp** również charakteryzuje się **większą liczbą skrajnych wyników**. Mimo to, wartości środkowe nadal pozostają zbliżone.

### Nieparametryczny test Kruskala-Wallisa i test post-hoc Dunna

Test **Kruskala-Wallisa** wykazał istotne różnice między grupami dla wszystkich zmiennych (`Teacher_Quality`, `Motivation_Level`, `Parental_Education_Level`, `Parental_Involvement`, `Access_to_Resources`, `Family_Income`, `Peer_Influence`, `Distance_from_Home`). Podobnie jak w przypadku testa U Manna-Whitneya należy pamiętać o **dużym rozmiarze próby (n = 6 607)**, przez który test staje się bardzo czuły. Wartości mediany dla wszystkich grup są zbliżone do 67, a wykresy *box-plot* nie wskazują na duże róznice w rozkładzie. Mimo to, warto przeanalizować widoczne na nich **wartości odstające**.

> * Test Kruskala-Wallisa wykazuje statystyczną istotność wpływu **jakości nauczania (`Teacher_Quality`) na wyniki egzaminu (`Exam_Score`)**. Według testu *post-hoc* Dunna z poprawką Bonferroniego wszystkie grupy różnią się między sobą. Na wykresach *box-plot* widać, że rozkłady mimo to pozostają do siebie podobne. Najważniejszą różnicą jest większa liczba skrajnych wyników w grupie `Medium` i `High` w porównaniu do grupy `Low`. Można wnioskować, że **osoby z grupy o najniższej jakości nauczania rzadziej otrzymują tak wysokie wyniki jak pozostałe grupy**.
> * Test wskazuje że wyniki egzaminu **są zróżnicowane według poziomu motywacji (`Motivation_Level`)**, a nawet że wszystkie grupy różnią się istotnie między sobą. Mimo to, analiza median i wykresów *box-plot* przynosi podobne wnioski jak w przypadku jakości nauczania.
> * Podobne wnioski niesie ze sobą wnikliwa analiza testów i wykresów przeprowadzonych dla `Parental_Education_Level`, `Parental_Involvement`, `Family_Income`, `Peer_Influence`.
> * Grupy według dostępu do zasobów (`Access_to_Resources`) teoretycznie istotnie różnią się między sobą ze względu na wyniki egzaminu, jednak przebiega to analogicznie jak dla wcześniejszych zmiennych. Ciekawą obserwacją jest mniejsza liczba wartości odstających dla grupy `Low` niż dla `Medium` i `High`, co pociąga za sobą wniosek, że **uczniowie o niskim poziomie dostępu do zasobów nauczania rzadziej osiągają skrajnie wysokie oceny niż grupy o średnim i wysokim dostępie.**
> * W przypadku odległości od domu (`Distance_from_Home`) najwięcej wartości ekstremalnych zauważyć można dla grup `Near` i `Moderate`, podczas gdy grupa `Far` ma ich mniej. Sugeruje to, że **uczniowie o niedużym dystansie od domu częściej otrzymują najwyższe wyniki z egzaminu niż uczniowie, u których ten dystans jest duży.**

### Korelacja Spearmana

**Wszystkie analizowane istotne zależności są dodatnie.** Test Spearmana jako jedyną zmienną niepowiązaną z wynikiem egzaminu wskazał liczbę przespanych godzin (`Sleep_Hours`).

> * Największą korelację z wynikiem egzaminu wykazują obecność (`Attendace`, *rho* = 0,67) i godziny poświęcone na naukę (`Hours_Studied`, *rho* = 0,48). Wyniki te znajdują potwierdzenie na wykresach zależności.
> * Umiarkowaną zależność z wynikiem egzaminu wykazują poprzednie wyniki (`Previous_Scores`, *rho* = 0,19) i liczba sesji korepetycji (`Tutoring_Sessions`, *rho* = 0,16).
> * Znikoma korelacja została zaobserwowana między wynikiem egzaminu a aktywnością fizyczną (`Physical_Activity`, *rho* = 0,03).

---

## Krok 5: Przygotowanie danych do modeli ML

Przed modelowaniem wykluczono **nieistotne zmienne**, czyli `Sleep_Hours`, `Gender` i `School_Type`. Następnie **zakodowano kategoryczne zmienne**, które miały zostać wykorzystane do budowy modeli. Dane **podzielono na zbiór treningowy i testowy** z zachowaniem proporcji 80/20.

## Krok 6: Trening i ewaluacja modeli regresji liniowej, Random Forest i Gradient Boosting

| Metryka | Regresja liniowa | Random Forest | Gradient Boosting |
| :--- | :---: | :---: | :---: |
| R^2 | 0,6952 | 0,5945 | 0,6565 |
| RMSE | 2,2804 | 2,6305 | 2,4209 |
| MAE | 0,5218 | 1,2119 | 0,8472 |

Najlepsze wyniki według wszystkich kryteriów osiąga model bazowy, czyli **regresja liniowa.** Zdobywa **najwyższy współczynnik determinacji** `R2` (0,6952 vs 0,5945 vs 0,6565) oraz **najniższy błąd średniokwadratowy `RMSE`** (2,2804 vs 2,6305 vs 2,4209) i **najniższy średni błąd bezwględny `MAE`** (0,5218 vs 1,2119 vs 0,8472). 

> * Wyższość regresji liniowej nad modelami opartymi na drzewach decyzyjnych (Random Forest, Gradient Boosting) potwierdza, że relacje pomiędzy zmiennymi objaśniającymi a wynikiem egzaminu mają **charakter silnie liniowy**. W tym przypadku złożone algorytmy nieliniowe miały tendencję do nadmiernego dopasowania (*overfittingu*) na zbiorze treningowym.
> * Analiza wizualna wykresów ujawnia powtarzalny wzorzec dla wszystkich trzech modeli, jakim jest **wyraźne niedoszacowanie wyników w prawym górnym rogu** (dla uczniów ze skrajnie wysokimi wynikami powyżej 80-90 punktów). Oznacza to, że żaden z modeli nie uchwycił w pełni specyfiki nieliniowego „sufitu” dla najlepszych osiągnięć.

Regresja liniowa nie tylko generuje najniższe błędy predykcji, ale oferuje też **pełną interpretowalność współczynników**, co czyni ją optymalnym wyborem biznesowym i badawczym.

Dodatkowo, mimo próby dostrojenia hiperparametrów dla drugiego najlepszego modelu (Gradient Boosting), jego **wyniki nadal pozostały gorsze niż wyniki dla regresji liniowej** (`R2`: 0,6711 vs 0,6952, `RMSE`: 2,3690 vs 2,2804, `MSE`: 0,7625 vs 0,5218).

---

## Krok 7: Interpretacja ważności cech

Największy pozytywny wpływ na wyniki egzaminu ma **pozytywny wpływ rówieśników na naukę, dostęp do internetu i mały dystans do domu.**
> * Uczniowie, którzy określają wpływ swoich rówieśników jako pozytywny osiągają średnio o 1,1 punktu z egzaminu więcej niż uczniowie, którzy określają go jako negatywny, przy założeniu *ceteris paribus*.
> * Osoby z dostępem do internetu mają średnio o 0,9 punktu wyższe wyniki z egzaminu niż osoby bez dostępu do internetu, przy założeniu *ceteris paribus*.
> * Uczniowie, którzy przemierzają mały dystans między szkołą a domem uzyskują średnio o 0,9 więcej punkta z egzaminu niż uczniowie, którzy muszą przebyć dużą odległość, przy założeniu *ceteris paribus*.

Największy negatywny wpływ na wyniki egzaminu ma **niski poziom dostępu do zasobów edukacyjnych, niski poziom zaangażowania rodziców w naukę ucznia, pochodzenie z rodziny o niskim dochodzie oraz niski poziom motywacji.**
> * Osoby o małym dostępie do odpowiednich materiałów naukowych mają średnio o 2 punkty niższe wyniki z egzaminu niż osoby o dużym dostępie do takowych zasobów, przy założeniu *ceteris paribus*.
> * Uczniowie o niskim poziomie zaangażowania rodziców w ich edukację osiągają średnio o 1,9 punktu mniej z egzaminu niż osoby, których rodzice wykazują duże zaangażowanie w ich naukę, przy założeniu *ceteris paribus*.
> * Niezmotywowani do nauki uczniowie zdobywają średnio o 1,1 punktu mniej z egzaminu niż ci zmotywowani, przy założeniu *ceteris paribus*.

**Najmniejszy wpływ na wynik według modelu regresji liniowej mają poprzednie wyniki, aktywność fizyczna i frekwencja**, mimo jej dużego współczynnika korelacji ze zmienną.

---

## Krok 8: Wnioski, rekomendacje, ograniczenia i dalsze kroki

### Kluczowe wnioski

> * **Do wyższych wyników** w nauce w dużym stopniu przyczyniają się **dostęp do internetu, pozytywny wpływ rówieśników oraz mały dystans do domu.** Nieobojętny jest też dostęp do zasobów edukacyjnych oraz zaangażowanie rodziców.
> * **Niższe wyniki** wiążą się między innymi z **brakiem motywacji czy niskim dostępie do zasobów i internetu.**
> * * Płeć, godziny snu i typ szkoły nie wpływają na wyniki w nauce.
> * Wbrew przypuszczeniom zakładającym przewagę zaawansowanych algorytmów, **klasyczna regresja liniowa osiągnęła najlepsze rezultaty**, potwierdzając silnie **liniowy charakter** relacji w badanej próbie uczniów.
> * Przy tak dużej próbie (n = 6 607) **testy statystyczne wykazały ekstremalną czułość**, różnice istotne statystycznie często nie przekładały się na duże różnice w wartościach median, które dla większości podgrup oscylowały wokół 67 punktów.

### Rekomendacje

> * **Wyrównywanie szans i zasobów:** Instytucje edukacyjne powinny priorytetowo wspierać uczniów z ograniczonym dostępem do materiałów i internetu, ponieważ braki te korelują z największymi stratami punktowymi (sięgającymi 2 punktów).
> * **Wsparcie zaangażowania i motywacji:** Programy naprawcze i opiekuńcze powinny skupiać się na aktywizowaniu rodziców oraz budowaniu motywacji uczniów do samodzielnej nauki.
> * **Monitoring środowiska rówieśniczego:** Należy zwracać uwagę na grupy zgłaszające negatywny wpływu otoczenia rówieśniczego oraz wspierać uczniów pokonujących duże odległości do szkoły.

### Ograniczenia

> * **Syntetyczny charakter danych:** Analizowany zbiór ma charakter syntetyczny, co oznacza, że wygenerowane relacje i struktura danych mogą odbiegać od pełnej złożoności i naturalnego szumu występującego w realnych procesach edukacyjnych.
> * **Problematyka skrajnych wyników:** Modele predykcyjne wykazują powtarzalną tendencję do niedoszacowania wartości w przypadku uczniów osiągających skrajnie wysokie, odstające wyniki w prawym ogonie rozkładu, co utrudnia pełne odwzorowanie najlepszych rezultatów.
> * **Potencjalny wyciek danych (*data leakage*):** Imputacja braków danych z wykorzystaniem metody MICE została przeprowadzona na całym zbiorze przed podziałem na część treningową i testową, co mogło w niewielkim stopniu wpłynąć na optymistyczne szacunki metryk modeli.
> * **Metodologia testów statystycznych:** Przy tak dużej próbie (n = 6 607) weryfikacja wariancji i średnich mogłaby opierać się na klasycznej analizie wariancji (ANOVA) ze względu na właściwości Centralnego Twierdzenia Granicznego. Wybór testów nieparametrycznych miał jednak charakter celowego zabiegu demonstracyjnego i eksploracyjnego.
> * **Brak zaawansowanych miar:** W obecnej wersji projektu pominięto obliczanie miar wielkości efektu (*effect size*) dla testów statystycznych, opierając się głównie na klasycznych współczynnikach regresji i korelacji Spearmana.
> * **Brak zaawansowanych miar i interpretowalności:** W obecnej wersji projektu pominięto obliczanie miar wielkości efektu (*effect size*) dla testów statystycznych oraz zastosowanie zaawansowanych algorytmów wyjaśnialności modeli typu SHAP (*SHapley Additive exPlanations*), opierając się głównie na klasycznych współczynnikach regresji i korelacji Spearmana.

### Dalsze kroki

> * **Pogłębiona walidacja modeli:** Przejście od prostej budowy i ewaluacji do bardziej rygorystycznych procedur walidacyjnych, takich jak wielokrotna walidacja krzyżowa (*k-fold cross-validation*) z uwzględnieniem imputacji danych ściśle wewnątrz pętli treningowej.
> * **Systematyczna selekcja zmiennych:** Wdrożenie zaawansowanych technik selekcji cech w celu optymalizacji struktury modelu.
> * **Rozbudowa interpretowalności**: Zaimplementowanie metod analizy SHAP do oceny zarówno globalnego, jak i lokalnego wpływu poszczególnych zmiennych na predykcje algorytmów.
> * **Analiza wielkości efektu:** Uzupełnienie statystyk nieparametrycznych o miary wielkości efektu, co pozwoliłoby precyzyjniej odróżnić istotność statystyczną wymuszoną dużą próbną od realnego znaczenia praktycznego zjawisk.

---
