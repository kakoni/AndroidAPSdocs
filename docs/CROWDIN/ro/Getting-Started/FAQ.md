# FAQ pentru cei care folosesc bucla

Cum se pot adăuga întrebări în secțiunea FAQ: urmați aceste [instrucțiuni](../make-a-PR.md)

# General

## Pot doar să descarc fișierul de instalare AndroidAPS?

Nu. Nu există nici un fişier apk descărcabil pentru AndroidAPS. Trebuie să ți-l [construiești](../Installing-AndroidAPS/Building-APK.md) singur. Iată de ce:

AndroidAPS este utilizat pentru a controla pompa şi a administra insulină. Under current regulations in Europe, all systems classed as IIa or IIb are medical devices that require regulatory approval (a CE mark) which needs various studies and sign offs. Distribuirea unui dispozitiv nereglementat este ilegală. Reglementări similare există şi în alte părţi ale lumii.

This regulation is not restricted just to sales (in the meaning of getting money for something) but applies to any distribution (even giving away for free). Building a medical device for yourself is the only way to use the app within these regulations.

De aceea fișierele apk nu sunt disponibile.

## Cum să încep?

În primul rând, trebuie să **ai componentele hardware compatibile pentru loop**:

* A [supported insulin pump](./Pump-Choices.md), 
* un [telefon cu Android](Phones.md) (sistemul Apple iOS nu este suportat de AndroidAPS-poti verifica [iOS Loop](https://loopkit.github.io/loopdocs/)) şi 
* un [sistem continuu de monitorizare a glicemiei](../Configuration/BG-Source.rst). 

În al doilea rând, trebuie să **configurezi hardware-ul**. Vezi [example setup with step-by-step tutorial](Sample-Setup.md).

În al treilea rând, trebuie să **configurați componentele software**: AndroidAPS și sursa CGM/FGM.

În al patrulea rând, trebuie să învățați și **să înțelegeți design-ul de referință OpenAPS pentru a vă verifica factorii de tratament**. Principiile fundamentale ale buclei închise se bazează pe faptul că rata de bazală şi raportul carbohidraților sunt exacte. Toate recomandările presupun că nevoile dvs. bazale sunt îndeplinite și orice vârfuri sau văi pe care le vedeți sunt un rezultat al altor factori care necesită, prin urmare, unele ajustări punctuale (exercițiu, stres etc.). Ajustările pe care le poate face bucla sunt limitate pentru siguranță (a se vedea bazala temporară maximă permisă în [designul de referință al OpenAPS](https://openaps.org/reference-design/)), ceea ce înseamnă că nu veți dori să pierdeți doza permisă pe corecții necesare pentru a repara greșelile bazalei. Dacă, de exemplu, ajungeți în mod frecvent la rate temporare joase atunci când se apropie o masă, atunci este foarte probabil că sunt necesare ajustări ale bazalei. Puteți folosi [autotune](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autotune.html#phase-c-running-autotune-for-suggested-adjustments-without-an-openaps-rig) pe o bază mare de date pentru a afla sugestii despre cum ați putea ajusta ratele bazale și/sau ISF-ul și dacă ratele de carbohidrați trebuie schimbate. Sau puteți testa și stabili ratele bazale [folosind metoda tradițională](https://integrateddiabetes.com/basal-testing/).

## Ce beneficii am dacă folosesc bucla?

### Protecția parolei

Dacă nu doriți ca setările dumneavoastră să poată fi schimbate cu ușurință, puteți stabili o parolă pentru a proteja secțiunea de setări prin selectarea opțiunii ”parolă pentru setări” și introducerea unei parole. Data viitoare când veți intra în meniul de setări, vi se va cere o parolă înainte de a vi se permite accesul. Dacă veți dori să eliminați parola mai târziu, mergeți la "parolă pentru setări" și ștergeți textul.

### Ceasuri cu Android Wear

Dacă intenționați să folosiți aplicația de ceas Wear pentru a bolusa sau pentru a schimba setări, atunci va trebui să activați notificările pentru AndroidAPS. Confirmarea acțiunilor vine sub formă de notificare.

### Deconectează pompa

If you take your pump off for showering, bathing, swimming, sports or other activities you must let AndroidAPS know that no insulin is delivered to keep IOB correct.

The pump can be disconnected using the Loop Status icon on the [AndroidAPS Home Screen](./Screenshots.md#loop-status).

### Recomandări care se bazează nu doar pe o singură citire CGM

For safety, recommendations made are based on not one CGM reading but the average delta. Therefore, if you miss some readings it may take a while after getting data back before AndroidAPS kicks in looping again.

### Referințe suplimentare

There are several blogs with good tips to help you understand the practicalities of looping:

* [Fine-tuning Settings](https://seemycgm.com/2017/10/29/fine-tuning-settings/) See my CGM
* [Why DIA matters](https://seemycgm.com/2017/08/09/why-dia-matters/) See my CGM
* [Limiting meal spikes](https://diyps.org/2016/07/11/picture-this-how-to-do-eating-soon-mode/) #DIYPS
* [Hormones and autosens](https://seemycgm.com/2017/06/06/hormones-2/) See my CGM

## Ce echipament de urgență este recomandat să iau cu mine?

You have to have the same emergency equipment with you like every other T1D with insulin pump therapy. When looping with AndroidAPS it is strongly recommended to have the following additional equipment with or near to you:

* Battery pack and cables to charge your smartphone, watch and (if needed) BT reader or Link device
* Pump batteries
* Current [apk](../Installing-AndroidAPS/Building-APK.md) and [preferences files](../Usage/ExportImportSettings.rst) for AndroidAPS and any other apps you use (e.g. xDrip+, BYO Dexcom) both locally and in the cloud (Dropbox, Google Drive).

## How can I safely and securely attach the CGM/FGM?

You can tape it. There are several pre-perforated 'overpatches' for common CGM systems available (search Google, eBay or Amazon). Some loopers use the cheaper standard kinesiology tape or rocktape.

You can fix it. You can also purchase upper arm bracelets that fix the CGM/FGM with a band (search Google, eBay or Amazon).

# Setări AndroidAPS

The following list aims to help you optimize settings. It may be best to start at the top and work to the bottom. Aim to get one setting right before changing another. Work in small steps rather than making large changes at once. You can use [Autotune](https://autotuneweb.azurewebsites.net/) to guide your thinking, although it should not be followed blindly: it may not work well for you or in all circumstances. Note that settings interact with one another - you can have 'wrong' settings that work well together in some circumstances (e.g. if a too-high basal happens to be at the same time as a too-high CR) but do not in others. This means that you need to consider all the settings and check they work together in a variety of circumstances.

## Durata de Acțiune a Insulinei (DIA)

### Descriere & testare

The length of time that insulin decays to zero.

This is quite often set too short. Most people will want at least 5 hours, potentially 6 or 7.

### Impact

Too short DIA can lead to low BGs. Și invers.

If DIA is too short, AAPS thinks too early that your previous bolus is all consumed, and, at still elevated glucose, will give you more. (Actually, it does not wait that long, but predicts what would happen, and keeps adding insulin). This essentially creates ‘insulin stacking’ that AAPS is unaware of.

Example of a too-short DIA is a high BG followed by AAPS over-correcting and giving a low BG.

## Planificare rată bazală (U/h)

### Descriere & testare

The amount of insulin in a given hour time block to maintain BG at a stable level.

Test your basal rates by suspending loop, fasting, waiting for say 5 hours after food, and seeing how BG changes. Repeat a few times.

If BG is dropping, basal rate is too high. Și invers.

### Impact

Too high basal rate can lead to low BGs. Și invers.

AAPS ‘baselines’ against the default basal rate. If basal rate is too high, a ‘zero temp’ will count as a bigger negative IOB than it should. This will lead to AAPS giving more subsequent corrections than it should to bring IOB ultimately to zero.

So, a basal rate too high will create low BGs both with the default rate, but also some hours hence as AAPS corrects to target.

Conversely a basal rate too low can lead to high BGs, and a failure to bring levels down to target.

## Factor de sensibilitate la insulină (ISF) (mmol/l/U sau mg/dl/U)

### Descriere & testare

The drop in BG expected from dosing 1U of insulin.

Assuming correct basal, you can test this by suspending loop, checking IOB is zero, and taking a few glucose tablets to get to a stable ‘high’ level.

Then take an estimated amount of insulin (as per current 1/ISF) to get to your target BG.

Be careful as this is quite often set too low. Too low means 1 U will drop BG faster than expected.

### Impact

**Lower ISF** (i.e. 40 instead of 50) meaning insulin drops your BG less per unit. This leads to a more aggressive / stronger correction from the loop with **more insulin**. If the ISF is too low, this can lead to low BGs.

**Higher ISF** (i.e. 45 instead of 35) meaning insulin drops your BG more per unit. This leads to a less aggressive / weaker correction from the loop with **less insulin**. If the ISF is too high, this can lead to high BGs.

**Example:**

* BG is 190 mg/dl (10,5 mmol) and target is 100 mg/dl (5,6 mmol). 
* So, you want correction of 90 mg/dl (= 190 - 110).
* ISF = 30 -> 90 / 30 = 3 units of insulin
* ISF = 45 -> 90 / 45 = 2 units of insulin

An ISF that is too low (not uncommon) can result in ‘over corrections’, because AAPS thinks it needs more insulin to correct a high BG than it actually does. This can lead to ‘roller coaster’ BGs (esp. when fasting). In this circumstance you need to increase your ISF. This will mean AAPS gives smaller correction doses, and this will avoid over-correcting a high BG resulting in a low BG.

Conversely, an ISF set too high can result in under-corrections, meaning your BG remains above target – particularly noticeable overnight.

## Raportul insulină la carbohidraţii (IC) (g/U)

### Descriere & testare

The grams of carbohydrate for each unit of insulin.

Some people also use I:C as abbreviation instead of IC or talk about carb ratio (CR).

Assuming correct basal, you can test by checking IOB is zero and that you are in-range, eating exactly known carbs, and take an estimated amount of insulin based on current insulin to carb ratio. Best is to eat food your normally eat at that time of day and count its carbs precisely.

> **NOTĂ:**
> 
> În unele ţări europene, se utilizează unităţi de pâine pentru determinarea cantității de insulină necesară pentru alimente. At the beginning 1 bread unit equal to 12g of carbs, later some changed to 10g of carbs.
> 
> În acest model s-a stabilit cantitatea de carbohidrați și cantitatea de insulină a fost variabilă. ("Cât insulină este necesară pentru a acoperi o felie de pâine?")
> 
> Când se utilizează IC, cantitatea de insulină este fixă iar cantitatea de carbohidraţi este variabilă. ("Câte grame de carbohidrati pot fi acoperite de o singură unitate de insulină?")
> 
> Example:
> 
> Bread unit factor (BU = 12g carbs): 2,4 U/BU -> You need 2,4 units of insulin when you eat one bread unit.
> 
> IC corespunzător: 12g / 2,4 U = 5,0 g/U -> 5,0gr carbohidrați pot fi acoperiți cu o singură unitate de insulină.
> 
> Factor BU 2,4 U / 12g ===> IC = 12g / 2,4 U = 5,0 g/U
> 
> Tabelele de conversie sunt disponibile online, respectiv [aici](https://www.mylife-diabetescare.com/files/media/03_Documents/11_Software/FAS/SOF_FAS_App_KI-Verha%CC%88ltnis_MSTR-DE-AT-CH.pdf).

### Impact

**Lower IC** = less food per unit, i.e. you are getting more insulin for a fixed amount of carbs. Can also be called ‘more aggressive’.

**Higher IC** = more food per unit, i.e. you are getting less insulin for a fixed amount of carbs. Can also be called ‘less aggressive’.

If after meal has digested and IOB has returned to zero, your BG remains higher than before food, chances are IC is too large. Conversely if your BG is lower than before food, IC is too small.

# Algoritm APS

## De ce se afișează "dia:3" în secțiunea "OpenAPS AMA" deși am o altă DIA setată în profilul meu?

![AMA 3h](../images/Screenshot_AMA3h.png)

In AMA, DIA actually doesn't mean the 'duration of insulin acting'. It is a parameter, which used to be connected to the DIA. Now, it means, 'in which time should the correction be finished'. It has nothing to do with the calculation of the IOB. In OpenAPS SMB, there is no need for this parameter any longer.

## Profil

### De ce să folosesc o durată minimă de 5 ore pentru DIA (Durata de acțiune a insulinei) în loc de 2-3 ore?

Well explained in [this article](https://www.diabettech.com/insulin/why-we-are-regularly-wrong-in-the-duration-of-insulin-action-dia-times-we-use-and-why-it-matters/). Don't forget to `ACTIVATE PROFILE` after changing your DIA.

### Ce determină bucla să îmi scadă frecvent glicemia la valori hipoglicemice fără COB?

First of all, check your basal rate and make a no-carb basal rate test. If it is correct, this behavior is typically caused by a too low ISF. A too low ISF looks typically like this:

![ISF too low](../images/isf.jpg)

### Ce cauzează vârfuri mari postprandiale în bucla închisă?

First of all, check your basal rate and make a no-carb basal rate test. If it is correct and your BG is falling to your target after carbs are fully absorbed, try to set an 'eating soon' temp target in AndroidAPS some time before the meal or think about an appropriate prebolus time with your endocrinologist. If your BG is too high after the meal and still too high after carbs are fully absorbed, think about decreasing your IC with your endocrinologist. If your BG is too high while COB and too low after carbs are fully absorbed, think about increasing your IC and an appropriate prebolus time with your endocrinologist.

# Alte setări

## Setări Nightscout

### AndroidAPS NSClient spune „nu este permis” și nu încarcă date. Ce pot face?

In NSClient check 'Connection settings'. Maybe you actually are not in an allowed WLAN or you have activated 'Only if charging' and your charging cable is not attached.

## Setări CGM

### De ce AndroidAPS spune că 'sursa de glicemie nu suportă filtrarea avansată'?

If you do use another CGM/FGM than Dexcom G5 or G6 in xDrip native mode, you'll get this alert in AndroidAPS OpenAPS-tab. See [Smoothing blood glucose data](../Usage/Smoothing-Blood-Glucose-Data-in-xDrip.md) for more details.

## Pompă

### Unde să montați pompa pe corp?

There are innumerable possibilities to place the pump. It does not matter if you are looping or not.

### Baterii

Looping can reduce the pump battery faster than normal use because the system interacts through bluetooth far more than a manual user does. It is best to change battery at 25% as communication becomes challenging then. You can set warning alarms for pump battery by using the PUMP_WARN_BATT_P variable in your Nightscout site. Tricks to increase battery life include:

* reduce the length of time the LCD stays on (within pump settings menu)
* reduce the length of time the backlight stays on (within pump settings menu)
* select notification settings to a beep rather than vibrate (within pump settings menu)
* only press the buttons on the pump to reload, use AndroidAPS to view all history, battery level and reservoir volume.
* AndroidAPS app may often be closed to save energy or free RAM on some phones. When AndroidAPS is reinitialized at each startup it establishes a Bluetooth connection to the pump, and re-reads the current basal rate and bolus history. This consumes battery. To see if this is happening, go to Preferences > NSClient and enable 'Log app start to NS'. Nightscout will receive an event at every restart of AndroidAPS, which makes it easy to track the issue. To reduce this happening, whitelist AndroidAPS app in the phone battery settings to stop the app power monitor closing it down.
    
    For example, to whitelist on a Samsung phone running Android Pie:
    
    * Go to Settings -> Device Care -> Battery 
    * Scroll until you find AndroidAPS and select it 
    * De-select "Put app to sleep"
    * ALSO go to Settings -> Apps -> (Three circle symbol in the top-right of the screen) select "special access" -> Optimize battery usage
    * Scroll to AndroidAPS and make sure it is de-selected.

* clean battery terminals with alcohol wipe to ensure no manufacturing wax/grease remains.

* for [Dana R/RS pumps](../Configuration/DanaRS-Insulin-Pump.md) the startup procedure draws a high current across the battery to purposefully break the passivation film (prevents loss of energy whilst in storage) but it doesn't always work to break it 100%. Either remove and reinsert battery 2-3 times until it does show 100% on screen, or use battery key to briefly short circuit battery before insertion by applying to both terminals for a split second.
* see also more tips for [particular types of battery](../Usage/Accu-Chek-Combo-Tips-for-Basic-usage#battery-type-and-causes-of-short-battery-life)

### Schimbarea rezervoarelor și a canulelor

The change of cartridge cannot be done via AndroidAPS but must be carried out as before directly via the pump.

* Long press on "Open Loop"/"Closed Loop" on the Home tab of AndroidAPS and select 'Suspend Loop for 1h'
* Now disconnect the pump and change the reservoir as per pump instructions.
* Also priming and filling tube and cannula can be done directly on the pump. In this case use [PRIME/FILL button](../Usage/CPbefore26#pump) in the actions tab just to record the change.
* Once reconnected to the pump continue the loop by long pressing on 'Suspended (X m)'.

The change of a cannula however does not use the "prime infusion set" function of the pump, but fills the infusion set and/or cannula using a bolus which does not appear in the bolus history. This means it does not interrupt a currently running temporary basal rate. On the Actions (Act) tab, use the [PRIME/FILL button](../Usage/CPbefore26#pump) to set the amount of insulin needed to fill the infusion set and start the priming. If the amount is not enough, repeat filling. You can set default amount buttons in the Preferences > Other > Fill/Prime standard insulin amounts. See the instruction booklet in your cannula box for how many units should be primed depending on needle length and tubing length.

## Fundal

You can find the AndroidAPS wallpaper for your phone on the [phones page](../Getting-Started/Phones#phone-background).

## Utilizare zilnică

### Igienă

#### Ce trebuie făcut când se face duș sau baie?

You can remove the pump while taking a shower or bath. For this short period of time you may not need it, but you should tell AAPS that you've disconnected so that the IOB calculations are correct. See [description above](../Getting-Started/FAQ#disconnect-pump).

### Serviciu

Depending on your job, you may choose to use different treatment factors on workdays. As a looper you should consider a [profile switch](../Usage/Profiles.md) for your typical working day. For example, you may switch to a profile higher than 100% if you have a less demanding job (e.g. sitting at a desk), or less than 100% if you are active and on your feet all day. You could also consider a high or low temporary target or a [time shift of your profile](../Usage/Profiles#time-shift) when working much earlier or later than regular, of if you work different shifts. If you are using [Nightscout profiles](../Configuration/Config-Builder#ns-profile), you can also create a second profile (e.g. 'home' and 'workday') and do a daily profile switch to the profile you actually need.

## Activități de agrement

### Sporturi

Trebuie să vă reluați vechile obiceiuri sportive din vremurile dinaintea buclei. Dacă doar ați consuma mai mulți carbohidrați decât înainte, sistemul de buclă închisă îi va recunoaşte şi îi va corecta în mod corespunzător.

Deci, ați avea mai mulți carbohidrați la bord, dar în același timp bucla ar contracara și va elibera insulina.

Când folosiți bucla ar trebui să încercați acești pași:

* Make a [profile switch](../Usage/Profiles.md) < 100%.
* Set an [activity temp target](../Usage/temptarget#activity-temp-target) above your standard target.
* If you are using SMB make sure ["Enable SMB with high temp targets"](../Usage/Open-APS-features#enable-smb-with-high-temp-targets) and ["Enable SMB always"](../Usage/Open-APS-features#enable-smb-always) are disabled.

Pre- and post-processing of these settings is important. Faceţi schimbările la timp, înainte de sport şi luaţi în considerare efectul de umplere cu glucoză a muşchilor.

Dacă faceţi sport în mod regulat în aceeași perioadă a zilei (adică clasă de sport în sala) puteţi lua în considerare utilizarea de [automatizare](../Usage/Automation.rst) pentru schimbare profil şi TT. Automatizarea bazată pe locaţie ar putea fi de asemenea o idee, dar face preprocesarea mai dificilă.

Procentul de schimbare a profilului, valoarea pentru ținta temporară a activității tale și momentul cel mai bun pentru modificări sunt setări individuale. Începeți pe partea sigură dacă sunteți în căutarea valorii corecte pentru dvs. (începeți cu un procent mai mic și cu un TT mai mare).

### Sex

You can remove the pump to be 'free', but you should tell AndroidAPS so that the IOB calculations are correct. See [description above](../Getting-Started/FAQ#disconnect-pump).

### Consumul de alcool

Drinking alcohol is risky in closed loop mode as the algorithm cannot predict the alcohol influenced BG correctly. You have to check out your own method for treating this using the following functions in AndroidAPS:

* Deactivating closed loop mode and treating the diabetes manually or
* setting high temp targets and deactivating UAM to avoid the loop increasing IOB due to an unattended meal or
* do a profile switch to noticeably less than 100% 

When drinking alcohol, you always have to have an eye on your CGM to manually avoid a hypoglycemia by eating carbs.

### În repaus

#### Cum pot să fac buclă în timpul nopții fără radiații mobile sau WIFI?

Many users turn the phone into airplane mode at night. If you want the loop to support you when you are sleeping, proceed as follows (this will only work with a local BG-source such as xDrip+ or ['Build your own Dexcom App'](../Hardware/DexcomG6#if-using-g6-with-build-your-own-dexcom-app), it will NOT work if you get the BG-readings via Nightscout):

1. Activați modul avion în telefon.
2. Aşteptaţi până când modul avion este activ.
3. Activați Bluetooth.

You are not receiving calls now, nor are you connected to the internet. But the loop is still running.

Some people have discovered problems with local broadcast (AAPS not receiving BG values from xDrip+) when phone is in airplane mode. Go to Settings > Inter-app settings > Identify receiver and enter `info.nightscout.androidaps`.

![xDrip+ identificare receptor prin setări de bază inter-aplicații](../images/xDrip_InterApp_NS.png)

### Călătorind

#### Cum să facem faţă schimbărilor de fus orar?

With Dana R and Dana R Korean you don't have to do anything. For other pumps see [time zone travelling](../Usage/Timezone-traveling.md) page for more details.

## Subiecte medicale

### Spitalizare

If you want to share some information about AndroidAPS and DIY looping with your clinicians, you can print out the [guide to AndroidAPS for clinicians](../Resources/clinician-guide-to-AndroidAPS.md).

### Programare medicală la endocrinologul dumneavoastră

#### Raportare

You can either show your Nightscout reports (https://YOUR-NS-SITE.com/report) or check [Nightscout Reporter](https://nightscout-reporter.zreptil.de/).