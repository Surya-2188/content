

#  C లో రికర్షన్ (Recursion)


## 1\. మనకు లూప్స్ (Loops) ఉండగా ఈ రికర్షన్ ఎందుకు?


ముందు పాఠాల్లో మనం ఏం చూసాం?


 * **Functions:** కోడ్‌ని మళ్ళీ మళ్ళీ వాడుకోవడానికి ఉపయోగపడతాయి.
 * **Loops:** ఒకే పనిని చాలాసార్లు రిపీట్ చేయడానికి వాడతాం.


తర్వాత మనం మ్యాథ్స్ లో రెండు క్లాసిక్ ఉదాహరణలు చూసాం: **Factorial (ఫ్యాక్టోరియల్)** మరియు **Fibonacci (ఫిబోనాక్సీ)**.
ఈ రెండింటిలోనూ ఒక కామన్ పాయింట్ ఉంది:


> **"ఇప్పుడున్న ఆన్సర్ కావాలంటే, దానికంటే ముందున్న ఆన్సర్ తెలియాలి."**


### 1.1 ఫ్యాక్టోరియల్ – పాత రిజల్ట్ ని గుణించడం


మ్యాథ్స్ ప్రకారం:
$$n! = 1 \times 2 \times 3 \times \cdots \times n$$


మనం $n!$ ని స్టెప్-బై-స్టెప్ ఇలా చేస్తాం:


1. `result = 1` తో మొదలుపెడతాం.
2. 1 తో గుణిస్తాం $\rightarrow$ `result = 1`.
3. 2 తో గుణిస్తాం $\rightarrow$ `result = 2`.
4. 3 తో గుణిస్తాం $\rightarrow$ `result = 6`.
   ...
   ప్రతి స్టెప్ లోనూ, పాత `result` ని వాడుకుంటాం.


**C Loop Version (లూప్ పద్ధతి):**


```c
#include <stdio.h>


int factorial_iter(int n) {
   int i;
   int result = 1;


   for (i = 1; i <= n; i++) {
       result = result * i;   // పాత result ని వాడుకుంటున్నాం
   }


   return result;
}


int main(void) {
   int n = 5;
   int val = factorial_iter(n);
   printf("Factorial of %d is %d\n", n, val);
   return 0;
}
```


ఇక్కడ గమనించండి:


 * ఫంక్షన్ ని **ఒక్కసారే** పిలిచాం: `factorial_iter(n)`.
 * ఆ ఒక్క కాల్ లోపలే, లూప్ తిరుగుతూ `result` ని అప్డేట్ చేస్తుంది.


మరి లూప్స్ తోనే పని అవుతున్నప్పుడు, ఈ రికర్షన్ ఎందుకు?
దీనికి సమాధానం తెలియాలంటే, అసలు ఫంక్షన్స్ లోపల ఏం జరుగుతుందో చూడాలి.


-----


## 2\. ఫంక్షన్ యొక్క రెండు దశలు: Definition vs. Call


రికర్శన్ గురించి తెలుసుకునే ముందు, C లో ఫంక్షన్‌కి రెండు స్టేజెస్ ఉంటాయని గుర్తుంచుకోండి.


### 2.1 Stage 1 – Function Definition (రూల్)


ఫంక్షన్ **ఎలా** పనిచేయాలి అని కంపైలర్ కి చెప్పేది ఇదే. ఇది ఒక బ్లూప్రింట్ లాంటిది.


```c
#include <stdio.h>


// Definition (నిర్వచనం)
void greet(void) {
   printf("Hello\n");
}
```


ఇది ఒక మ్యాథ్స్ రూల్ లాంటిది: *"ఎప్పుడైతే greet ని పిలుస్తావో, అప్పుడు Hello అని ప్రింట్ చెయ్యి."*


### 2.2 Stage 2 – Function Call (ట్రిగ్గర్)


ఇక్కడ మనం ఫంక్షన్‌ని నిజంగా **వాడతాం**.


```c
// Call inside main
int main(void) {
   greet();     // function call (ఇక్కడ పిలుస్తున్నాం)
   return 0;
}
```


ఈ లైన్‌లో మనం రూల్ రాయట్లేదు, *"ఆ greet లో రాసిన రూల్ ని రన్ చెయ్యి"* అని అడుగుతున్నాం.


### 2.3 అసలు రికర్షన్ ఎక్కడ జరుగుతుంది?


రికర్శన్ అనేది ఒక స్పెషల్ సిచువేషన్. ఎప్పుడైతే:
**ఫంక్షన్ డెఫినిషన్ (Stage 1) లోపలే, మళ్ళీ అదే ఫంక్షన్ కాల్ (Stage 2) కనిపిస్తుందో, దాన్నే రికర్షన్ అంటాం.**


 * **Normal:** `main` $\rightarrow$ `funcA` ని పిలుస్తుంది. అది పని పూర్తి చేసి వెనక్కి వస్తుంది.
 * **Nested:** `main` $\rightarrow$ `funcA` ని పిలుస్తుంది, ఆ లోపల `funcA` $\rightarrow$ `funcB` ని పిలుస్తుంది.
 * **Recursive:** `main` $\rightarrow$ `funcA` ని పిలుస్తుంది, ఆ లోపల `funcA` $\rightarrow$ మళ్ళీ **`funcA`** నే పిలుస్తుంది (కాకపోతే చిన్న నంబర్ తో).


ఈ "సెల్ఫ్-కాల్" (Self-call) నే మనం రికర్షన్ అంటాం.


-----


## 3\. కొత్త డేటా స్ట్రక్చర్: Stack (కాల్స్ ఎలా స్టోర్ అవుతాయి?)


రికర్శన్ అర్థం కావాలంటే, C లాంగ్వేజ్ ఫంక్షన్ కాల్స్ ని ఎలా స్టోర్ చేసుకుంటుందో తెలియాలి.


### 3.1 అసలు Stack అంటే ఏంటి?


Stack అనేది వస్తువులను దాచుకునే ఒక పద్ధతి. దీని రూల్:
**LIFO – Last In, First Out (చివరగా వచ్చింది, మొదట వెళ్తుంది).**


**రియల్ లైఫ్ ఉదాహరణలు:**


1. **ప్లేట్ల దొంతర:** మీరు ప్లేట్లను ఒకదాని మీద ఒకటి పెడతారు. తీసేటప్పుడు ఎప్పుడూ పైనున్న ప్లేట్ నే తీస్తారు. మధ్యలోంచి లాగలేం కదా\!
2. **వైవా (Viva) కి పిలవడం:** ఒక గదిలోకి స్టూడెంట్స్ ఒకరి వెనుక ఒకరు వెళ్ళారనుకోండి. చివరగా వెళ్ళిన స్టూడెంట్ డోర్ దగ్గరే ఉంటాడు కాబట్టి, సార్ పిలిస్తే అతనే ముందు బయటకు వస్తాడు.


**ముఖ్యమైన పాయింట్స్:**


 * మధ్యలో ఉన్నవాటిని కదిలించలేం.
 * ఎప్పుడూ పైన (Top) మాత్రమే కలుపుతాం (`push`), పైన ఉన్నదాన్నే తీస్తాం (`pop`).


-----


## 4\. The Call Stack: C లో ఫంక్షన్ కాల్స్ ఎలా ఉంటాయి?


C లాంగ్వేజ్ లోపల ఫంక్షన్ కాల్స్ ని మేనేజ్ చేయడానికి ఈ **Stack** నే వాడుతుంది. దీన్ని **Call Stack** అంటారు.


ప్రతి రన్నింగ్ ఫంక్షన్‌ని ఒక **బాక్స్ (Frame)** లాగా ఊహించుకోండి.


1. `main` స్టార్ట్ అయినప్పుడు, దాని బాక్స్ స్టాక్‌లో పడుతుంది.
2. `main` గనక `func1` ని పిలిస్తే, `func1` బాక్స్ దాని పైన పడుతుంది.
3. `func1` గనక `func2` ని పిలిస్తే, `func2` బాక్స్ ఇంకా పైన పడుతుంది.


**గోల్డెన్ రూల్:**


> **చివరగా పిలిచిన ఫంక్షనే, అందరికంటే ముందు పని ముగించుకుని వెళ్తుంది.**
> ఇదే LIFO (Last In, First Out).


### 4.1 Figure 1 ని అర్థం చేసుకోవడం – Stack ఎలా పెరుగుతుంది? ఎలా తగ్గుతుంది?


పైన ఉన్న డయాగ్రామ్ (Figure 1) చూడండి. ప్రతి చిన్న బాక్స్ ఒక **Stack Frame**. బాణం గుర్తు (stack top) ఎప్పుడూ ప్రస్తుతం రన్ అవుతున్న ఫంక్షన్‌ని చూపిస్తుంది. దీన్ని ఒక కథలా చూద్దాం.


**Panel 1 – `main` calls `func1()`**


 * **Stack:** `main` (కింద) $\rightarrow$ `func1()` (పైన).
 * **ఏం జరిగింది?** ప్రోగ్రామ్ `main` లో మొదలైంది. `main` లోపల `func1()` ని పిలిచాం. వెంటనే `func1` బాక్స్ స్టాక్ పైన చేరింది. `func1` పూర్తయ్యేదాకా `main` వెయిట్ చేస్తూ ఉంటుంది.


**Panel 2 – `func1()` calls `func2()`**


 * **Stack:** `main` $\rightarrow$ `func1` $\rightarrow$ `func2`.
 * `func1` కోడ్ లోపల `func2();` అని ఉంది.
 * CPU `func1` ని పాజ్ (Pause) చేసి, `func2` బాక్స్‌ని పైన పెడుతుంది. ఇప్పుడు `func2` రన్ అవుతోంది.


**Panel 3 – `func2()` calls `func3()`**


 * **Stack:** `main` $\rightarrow$ `func1` $\rightarrow$ `func2` $\rightarrow$ `func3`.
 * `func3` బాక్స్ పైన చేరింది. ఇప్పుడు కింద ఉన్న `func2`, `func1`, `main` ముగ్గురూ వెయిటింగ్.


**Panels 4, 5 – ఇంకా లోపలికి...**


 * `func3` $\rightarrow$ `func4` ని... అది `func5` ని... అది `func6` ని పిలుస్తూ పోయాయి.
 * కొత్త కాల్ వచ్చినప్పుడల్లా ఒక బాక్స్ పైన యాడ్ అవుతుంది.
 * పాత ఫంక్షన్స్ అన్నీ అక్కడే ఫ్రీజ్ (Freeze) అయిపోయి ఉంటాయి.


**Final Panel – వెనక్కి రావడం (Returning)**


 * ఆకుపచ్చ బాణం గుర్తు వెనక్కి వచ్చే దారిని చూపిస్తోంది.
 * `func6` పని అయిపోయాక, దాని బాక్స్ ని తీసేస్తారు (**Pop**). కంట్రోల్ మళ్ళీ `func5` కి వస్తుంది.
 * `func5` అయిపోయాక, అది పోతుంది. `func4` కి వస్తుంది.
 * అలా చివరికి మళ్ళీ `main` కి వస్తుంది.


**Figure 1 మనకు ఏం నేర్పిందంటే:**
ప్రతి ఫంక్షన్ కాల్ ఒక కొత్త బాక్స్‌ని స్టాక్ మీద పెడుతుంది. పని అయిపోయాక ఆ బాక్స్ పోతుంది. రికర్శన్‌లో కూడా సరిగ్గా ఇదే జరుగుతుంది – తేడా ఏంటంటే, అన్ని బాక్సుల పేర్లు **ఒకటే ఉంటాయి** (ఉదాహరణకు `fact(5)`, `fact(4)`...). కానీ జరిగే పద్ధతి సేమ్.


-----


## 5\. అసలు రికర్షన్ అంటే ఏంటి (Formally)?


ఇప్పుడు డెఫినిషన్ చూద్దాం:
**తనని తానే పిలుచుకునే ఫంక్షన్‌ని "Recursive Function" అంటారు.**


ప్రతి రికర్సివ్ ఫంక్షన్‌కి రెండు భాగాలు ఖచ్చితంగా ఉండాలి:


1. **Base Case (ఆగే పాయింట్):** ఇక్కడ ఫంక్షన్ తనని తాను పిలవదు. డైరెక్ట్ గా ఆన్సర్ ఇచ్చేస్తుంది.
2. **Recursive Case (తిరిగే పాయింట్):** ఇక్కడ ఫంక్షన్ తనని తానే పిలుచుకుంటుంది, కానీ చిన్న ఇన్పుట్ తో. ఇది మెల్లగా Base Case వైపు వెళ్తుంది.


ఒకవేళ Base Case లేకపోయినా, లేదా మనం రాసిన లాజిక్ Base Case వైపు వెళ్ళకపోయినా... ఆ ఫంక్షన్ అనంతంగా (infinitely) పిలుస్తూనే ఉంటుంది. స్టాక్ నిండిపోయి, ప్రోగ్రామ్ క్రాష్ అవుతుంది. దీన్నే **Stack Overflow** అంటారు.


-----


## 6\. Factorial: మ్యాథ్స్ నుండి C కి


### 6.1 మ్యాథ్స్ లో రికర్షన్


$n$ అనే నంబర్ కి ఫ్యాక్టోరియల్:


$$
n! =
\begin{cases}
1 & \text{if } n = 0 \\
n \times (n-1)! & \text{if } n > 0
\end{cases}
$$


ఇది ఆల్రెడీ రికర్సివ్ గానే ఉంది: $n!$ కావాలంటే, మనకు $(n-1)!$ తెలియాలి. $0! = 1$ దగ్గర ఆగిపోవాలి.


### 6.2 Iterative (Loop) C Version


ఇందాక చూసిన `factorial_iter` లో:


 * `main` ఒకసారే `factorial_iter(5)` ని పిలుస్తుంది.
 * ఆ ఒక్క బాక్స్ లోపలే లూప్ తిరుగుతుంది.
 * **Stack:** `n` ఎంత పెద్దదైనా సరే, స్టాక్ లో ఒక్క బాక్స్ (Frame) మాత్రమే ఉంటుంది.


### 6.3 Recursive C Version


ఇప్పుడు మ్యాథ్స్ ఫార్ములాని డైరెక్ట్ గా C కోడ్ లా మారుద్దాం:


```c
#include <stdio.h>


// Recursive factorial
int fact(int n) {
   if (n == 0) {              // Base Case (ఆగే చోటు)
       return 1;
   } else {                   // Recursive Case (తిరిగే చోటు)
       return n * fact(n - 1);
   }
}


int main(void) {
   int n = 5;
   int result = fact(n);
   printf("Recursive: %d! = %d\n", n, result);
   return 0;
}
```


**`fact` ఫంక్షన్ లోపల:**


 * `if (n == 0) { return 1; }`: ఒకవేళ `n` సున్నా అయితే, మళ్ళీ పిలవద్దు. 1 రిటర్న్ చెయ్యి.
 * `return n * fact(n - 1);`: ఒకవేళ `n > 0` అయితే, ముందు `fact(n - 1)` కనుక్కో, దాన్ని `n` తో గుణించి పంపు. ఇక్కడే ఫంక్షన్ తనని తాను పిలుస్తోంది.


-----


## 7\. Call Stack: Iterative vs. Recursive (అసలు తేడా ఎక్కడ?)


మెమరీలో ఏం జరుగుతుందో చూద్దాం. ఇది చాలా ముఖ్యం.


### 7.1 Iterative stack for `factorial_iter(5)`


 * **Call chain:** `main` $\rightarrow$ `factorial_iter(5)`.
 * **Stack:** కింద `main`, పైన `factorial_iter`. అంతే.
 * ఒక్క బాక్స్ లోనే `i`, `result` మారుతూ ఉంటాయి.


### 7.2 Recursive stack for `fact(5)`


 * **Call chain:** `main` $\rightarrow$ `fact(5)` $\rightarrow$ `fact(4)` $\rightarrow$ `fact(3)` $\rightarrow$ `fact(2)` $\rightarrow$ `fact(1)` $\rightarrow$ `fact(0)`.


అత్యంత లోతుకి వెళ్ళినప్పుడు ($n=0$), స్టాక్ ఇలా ఉంటుంది:


**Top of stack (పైన)**
$\downarrow$
`fact frame (n=0)`
`fact frame (n=1)`
`fact frame (n=2)`
`fact frame (n=3)`
`fact frame (n=4)`
`fact frame (n=5)`
`main frame`


చూసారా? ఒకే ఫంక్షన్ `fact` కి సంబంధించిన **6 బాక్సులు** స్టాక్ మీద ఉన్నాయి. ప్రతి బాక్సులో వేరే `n` ఉంది.


**Unwinding (వెనక్కి రావడం):**


1. `fact(0)` అంటుంది: "నా ఆన్సర్ **1**". బాక్స్ లేచిపోతుంది.
2. `fact(1)` అంటుంది: "ఓహో, అది 1 కదా, అయితే $1 \times 1 = 1$". 1 రిటర్న్ చేస్తుంది. బాక్స్ లేచిపోతుంది.
3. `fact(2)` అంటుంది: "అయితే $2 \times 1 = 2$".
4. `fact(3)` అంటుంది: "అయితే $3 \times 2 = 6$".
5. `fact(4)` అంటుంది: "అయితే $4 \times 6 = 24$".
6. చివరికి `fact(5)` అంటుంది: "అయితే $5 \times 24 = 120$". ఈ 120 ని `main` కి ఇస్తుంది.


ఈ లాజిక్ వల్ల `fact(5)` పని అవ్వాలంటే, `fact(4)` పని అవ్వాలి. ఇది LIFO పద్ధతిలో కరెక్ట్ గా సెట్ అవుతుంది.


-----


## 8\. Fibonacci: సేమ్ కాన్సెప్ట్, కానీ ఎక్కువ కొమ్మలు


**మ్యాథ్స్ డెఫినిషన్:**
$$F_0 = 0, \quad F_1 = 1, \quad F_n = F_{n-1} + F_{n-2}$$


**Recursive C Version:**


```c
int fib(int n) {
   if (n == 0) return 0;        // base case 1
   if (n == 1) return 1;        // base case 2
  
   // Recursive case: రెండు సార్లు పిలుస్తుంది
   return fib(n - 1) + fib(n - 2);
}
```


ఇక్కడ, `fib(5)` కాల్ చేస్తే అదొక **చెట్టు (Tree)** లాగా మారుతుంది.


 * `fib(5)` వెళ్లి `fib(4)` ని మరియు `fib(3)` ని పిలుస్తుంది.
 * `fib(4)` వెళ్లి మళ్ళీ `fib(3)` ని, `fib(2)` ని పిలుస్తుంది.


స్టాక్ మాత్రం ఒకేసారి ఒకదానితోనే డీల్ చేస్తుంది (LIFO), కానీ చాలా ఎక్కువ పని జరుగుతుంది.


-----


## 9\. Base Case మరియు Stack Overflow


రికర్శన్ రాసేటప్పుడు **Base Case** ప్రాణం లాంటిది.


**ఎందుకు?**
ఒకవేళ Base Case లేకపోతే, ఫంక్షన్ ఎప్పటికీ ఆగదు. పిలుస్తూనే ఉంటుంది. స్టాక్ మెమరీ లిమిటెడ్ గా ఉంటుంది కాబట్టి, అది నిండిపోగానే ప్రోగ్రామ్ **Stack Overflow** వచ్చి క్రాష్ అవుతుంది.


**ఉదాహరణ (Base case లేదు):**


```c
int bad_fact(int n) {
   return n * bad_fact(n - 1);   // ❌ ఆగే చోటు లేదు (No base case)
}
```


**ఉదాహరణ (తప్పు దారి):**


```c
int bad_fact2(int n) {
   if (n == 0) return 1;
   return n * bad_fact2(n + 1);  // ❌ 0 వైపు వెళ్ళట్లేదు, దూరం వెళ్తోంది
}
```


**స్టూడెంట్స్ కి రూల్:**
ప్రతి రికర్సివ్ ఫంక్షన్ కి:


1. ఒక సరైన Base Case ఉండాలి.
2. రికర్శన్ కాల్స్ ఆ Base Case వైపు వెళ్ళేలా ఉండాలి.


-----


## 10\. రికర్సివ్ ఫంక్షన్స్ రాయడానికి Template


ఏదైనా కొత్త రికర్సివ్ ఫంక్షన్ రాయాలంటే, ఈ 4 స్టెప్స్ ఫాలో అవ్వండి:


1. **Base case ని గుర్తించండి:** ఎప్పుడు ఆగాలి? (ఉదా: $n == 0$).
2. **C లో Base case రాయండి:**
   ```c
   if (n == 0) return 1;
   ```
3. **చిన్న ఇన్పుట్ తో Recursive case రాయండి:**
   Factorial: `return n * fact(n - 1);`
4. **దిశ (Direction) చెక్ చేయండి:**
   మనం $n$ నుండి మొదలుపెడితే, చివరికి Base case కి తగులుతామా?
     * $n \rightarrow n-1 \rightarrow n-2 \rightarrow \dots \rightarrow 0$ (కరెక్ట్ ✔)
     * $n \rightarrow n+1 \rightarrow n+2 \dots$ (తప్పు ✘)


-----


## 11\. రికర్శన్ లో చేసే సాధారణ తప్పులు (Common Mistakes)


స్టూడెంట్స్ తరచుగా చేసే **25 తప్పులు** ఇక్కడ ఉన్నాయి. వీటిని జాగ్రత్తగా చూడండి.


**Mistake 1 – No base case**


```c
int fact(int n) {
   return n * fact(n - 1);   // ❌ అనంతమైన రికర్శన్
}
```


*సమస్య:* ఆగే కండిషన్ లేదు. స్టాక్ ఓవర్ ఫ్లో అవుతుంది.


**Mistake 2 – Wrong base case value**


```c
int fact(int n) {
   if (n == 0) return 0;     // ❌ 1 రిటర్న్ చేయాలి
   return n * fact(n - 1);
}
```


*సమస్య:* `fact(0)` సున్నా అవుతుంది. దాన్ని దేనితో గుణించినా సున్నానే వస్తుంది. మొత్తం ఆన్సర్ 0 అయిపోతుంది.


**Mistake 3 – Base case never reached (wrong direction)**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n + 1);   // ❌ 5 -> 6 -> 7 ... (0 ఎప్పటికీ రాదు)
}
```


**Mistake 4 – Negative inputs not handled**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n - 1);   // ❌ fact(-3) -> fact(-4) ...
}
```


*పరిష్కారం:* `if (n < 0)` అని ఒక చెక్ పెట్టాలి.


**Mistake 5 – Missing return for recursive step**


```c
int fact(int n) {
   if (n == 0) return 1;
   fact(n - 1);              // ❌ వాల్యూ కనుక్కున్నాం కానీ రిటర్న్ చేయలేదు
}
```


*సమస్య:* ఫంక్షన్ ఏ వాల్యూ ఇవ్వదు (Garbage value వస్తుంది).


**Mistake 6 – Using `n--` in the recursive call**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n--);     // ❌ పాత n పాస్ అవుతుంది, తర్వాత తగ్గుతుంది
}
```


*సమస్య:* `fact(5)` మళ్ళీ `fact(5)` నే పిలుస్తుంది. ఇన్ఫినిట్ లూప్. ఎప్పుడూ `n - 1` వాడాలి.


**Mistake 7 – Forgetting to multiply in factorial**


```c
int fact(int n) {
   if (n == 0) return 1;
   return fact(n - 1);       // ❌ "* n" మర్చిపోయాం
}
```


*సమస్య:* అన్ని ఫ్యాక్టోరియల్స్ 1 అయిపోతాయి.


**Mistake 8 – Mixing printing with returning**


```c
void fact_print(int n) {
   if (n == 0) {
       printf("1\n");
   } else {
       // ❌ void ఫంక్షన్ ని లెక్కల్లో వాడలేం
       printf("%d\n", n * fact_print(n - 1));
   }
}
```


*సమస్య:* `fact_print` అనేది `void`, అది నంబర్ ఇవ్వదు.


**Mistake 9 – Using global accumulator incorrectly**


```c
int acc = 1; // Global


int fact(int n) {
   if (n == 0) {
       return acc;
   }
   acc = acc * n;
   return fact(n - 1);
}
```


*సమస్య:* ఒక్కసారి పనిచేస్తుంది. రెండోసారి పిలిస్తే పాత వాల్యూ అలాగే ఉంటుంది కాబట్టి తప్పు ఆన్సర్ వస్తుంది.


**Mistake 10 – Wrong base cases in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 1;     // ❌ 0 కి 0 రావాలి
   if (n == 1) return 1;
   return fib(n - 1) + fib(n - 2);
}
```


**Mistake 11 – Missing one base case in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 0;
   // ❌ n == 1 మిస్ అయ్యింది
   return fib(n - 1) + fib(n - 2);
}
```


*సమస్య:* `fib(1)` పిలిచినప్పుడు, అది `fib(-1)` ని పిలుస్తుంది. క్రాష్ అవుతుంది.


**Mistake 12 – Same argument twice in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;
   return fib(n - 1) + fib(n - 1);  // ❌ రెండోది fib(n-2) ఉండాలి
}
```


**Mistake 13 – Off-by-one when printing Fibonacci**


```c
void print_fib(int n) {
   int i;
   for (i = 0; i < n; i++) {
       printf("%d ", fib(i + 1));   // ❌ F0 ని మిస్ చేస్తున్నారు
   }
}
```


**Mistake 14 – Unnecessary loop inside recursive function**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;


   int i, sum = 0;
   for (i = 0; i < n; i++) {       // ❌ అనవసరమైన లూప్
       sum = fib(n - 1) + fib(n - 2);
   }
   return sum;
}
```


*సమస్య:* రికర్శన్ అంటేనే రిపిటీషన్. మళ్ళీ లోపల లూప్ ఎందుకు? డబుల్ పని అవుతుంది.


**Mistake 15 – No guard for negative n in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;
   return fib(n - 1) + fib(n - 2);  // ❌ నెగిటివ్ నంబర్ ఇస్తే క్రాష్
}
```


**Mistake 16 – Mixing iteration and recursion wrongly**


```c
int fact(int n) {
   int i, result = 1;
   for (i = 1; i <= n; i++) {
       result *= i;
       return fact(n - 1);   // ❌ లూప్ మొదటిసారే రిటర్న్ అయిపోతుంది
   }
   return result;
}
```


*పరిష్కారం:* అయితే లూప్ వాడండి, లేదా రికర్శన్ వాడండి. రెండూ కలపొద్దు.


**Mistake 17 – Unreachable code after return**


```c
int fib(int n) {
   if (n == 0) return 0;
   return fib(n - 1);
   return fib(n - 2);        // ❌ ఈ లైన్ ఎప్పటికీ రన్ అవ్వదు
}
```


**Mistake 18 – Assuming recursion “remembers” values**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;
   int a = fib(n - 1);
   int b = fib(n - 2);
   int c = fib(n - 1);       // ❌ మళ్ళీ క్యాలిక్యులేట్ చేస్తుంది (గుర్తుపెట్టుకోదు)
   return a + b;
}
```


*సమస్య:* ప్రతి కాల్ కొత్తదే. కంప్యూటర్ పాతవి గుర్తుపెట్టుకోదు (Memoization వాడితే తప్ప).


**Mistake 19 – Ignoring return value**


```c
int fact(int n) {
   if (n == 0) return 1;
   fact(n - 1);              // ❌ రిజల్ట్ ని పట్టించుకోలేదు
   return n;                 // n ని మాత్రమే రిటర్న్ చేస్తుంది
}
```


**Mistake 20 – Very deep recursion (Stack Overflow)**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n - 1);   // లాజిక్ కరెక్టే
}
```


*సమస్య:* `fact(50000)` అని పిలిస్తే, స్టాక్ మెమరీ సరిపోదు. ప్రోగ్రామ్ క్రాష్ అవుతుంది.


**Mistake 21 – Returning before doing required work**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n - 1);  
   printf("Done\n");         // ❌ ఇది ఎప్పటికీ ప్రింట్ అవ్వదు
}
```


*సమస్య:* `return` జరగ్గానే ఫంక్షన్ ఆగిపోతుంది.


**Mistake 22 – Using recursion where stopping condition is not obvious**


```c
int weird(int n) {
   if (n % 2 == 0) return n;
   return weird(n * 3 + 1);   // ❌ ఇది ఆగుతుందో లేదో తెలియదు
}
```


*సలహా:* బిగినర్స్ ఫ్యాక్టోరియల్ లాంటి క్లియర్ ప్రాబ్లమ్స్ తో స్టార్ట్ చేయాలి.


**Mistake 23 – Returning wrong type conceptually**


```c
char grade(int n) {
   if (n == 0) return fact(0);  // ❌ int ని ఇస్తోంది, కానీ కావాల్సింది char
   return 'A';
}
```


**Mistake 24 – Recursion inside `main` without clear design**


```c
int main(void) {
   int x;
   scanf("%d", &x);
   if (x > 0) {
       main();   // ❌ main ని మళ్ళీ పిలవకూడదు
   }
   return 0;
}
```


*సమస్య:* `main` రికర్శన్ కి వాడకూడదు. కన్ఫ్యూజింగ్ గా ఉంటుంది.


**Mistake 25 – Blindly translating a loop to recursion**
*తప్పు పద్ధతి:*


```c
void print_down_rec(int n) {
   while (n > 0) {
       printf("%d ", n);
       print_down_rec(n - 1);    // ❌ లూప్ + రికర్శన్ = గందరగోళం
       n--;
   }
}
```


*సమస్య:* రికర్శన్ లూప్ ని **రీప్లేస్** చేయాలి, లూప్ లోపల ఉండకూడదు.


-----


## 12\. ముగింపు (Final Conclusion)


మ్యాథ్స్ లోనూ, ప్రోగ్రామింగ్ లోనూ చాలా పనులకు ఈ పాటర్న్ ఉంటుంది:


> **"ఇప్పుడున్న వాల్యూ కావాలంటే... పాత వాల్యూ మీద ఏదో ఫార్ములా వేయాలి."**


రికర్శన్ ఇలాంటి వాటికి పర్ఫెక్ట్ గా సెట్ అవుతుంది ఎందుకంటే:


1. ప్రతి రికర్సివ్ కాల్ ఒక చిన్న పనిని సూచిస్తుంది.
2. **Call Stack** ఆటోమేటిక్ గా ఎవరి రిజల్ట్ కోసం ఎవరు వెయిట్ చేస్తున్నారో గుర్తుపెట్టుకుంటుంది.
3. చివరగా **Last In, First Out** పద్ధతిలో, చిన్న ప్రాబ్లమ్ (Base case) ముందు సాల్వ్ అయ్యి, ఆ ఆన్సర్ పైవాళ్ళకి అందుతుంది.


C లాంగ్వేజ్ లో, ఫంక్షన్ కాల్స్ అన్నీ Stack లో స్టోర్ అవుతాయి కాబట్టి రికర్శన్ స్మూత్ గా పనిచేస్తుంది.


**రికర్శన్ అంటే మ్యాజిక్ కాదు.** అది జస్ట్ ఫంక్షన్స్ తమని తామే పిలుచుకోవడం. ఆ బుక్-కీపింగ్ (Book-keeping) అంతా Call Stack చూసుకుంటుంది అంతే\!

