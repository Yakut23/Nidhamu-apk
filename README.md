# NidhamuApp — Mafaili ya Capacitor + GitHub Actions (Build APK)

Hii ni mradi kamili wa [Capacitor](https://capacitorjs.com) unaobeba NidhamuApp (faili la `www/index.html`) na
kuigeuza kuwa APK ya Android. Kujenga (build) kunafanyika moja kwa moja kwenye **GitHub Actions** — hauitaji
Android Studio wala kompyuta yenye nguvu.

## Muundo wa mafaili

```
nidhamu-apk/
├── package.json                     ← dependencies za Capacitor
├── capacitor.config.json            ← jina la app, appId, folder ya web
├── www/
│   └── index.html                   ← NidhamuApp yenyewe (tayari na icons + Germinate Tech branding)
├── resources/
│   ├── icon.png                     ← icon yako (1024x1024, imetengenezwa kutoka picha uliyotuma)
│   └── splash.png                   ← splash screen (2732x2732)
├── .github/workflows/build-apk.yml  ← "robot" inayojenga APK kila unapo-push
├── .gitignore
└── README.md
```

## Hatua za ku-build APK (kupitia GitHub)

### 1. Tengeneza repository mpya GitHub
- Nenda [github.com/new](https://github.com/new)
- Ipe jina mfano `nidhamu-app`
- Chagua **Public** au **Private** (zote zinafanya kazi na Actions bure)
- Bonyeza **Create repository**

### 2. Pakia (upload) mafaili haya yote
Njia rahisi zaidi bila terminal:
- Fungua repo yako mpya GitHub
- Bonyeza **Add file → Upload files**
- Buruta (drag & drop) folder nzima ya `nidhamu-apk` (au faili zote ndani yake pamoja na muundo wa folda: `www/`, `resources/`, `.github/workflows/`)
- Andika ujumbe mfano "Initial commit" kisha **Commit changes**

*(Kama unatumia git kwenye terminal/Android Termux badala yake:)*
```bash
cd nidhamu-apk
git init
git add .
git commit -m "NidhamuApp Capacitor setup"
git branch -M main
git remote add origin https://github.com/JINA_LAKO/nidhamu-app.git
git push -u origin main
```

### 3. Actions itaanza kujenga APK kiotomatiki
Mara tu unapo-push/upload kwenye branch `main`:
- Nenda tab **Actions** kwenye repo yako
- Utaona workflow "Build NidhamuApp APK" ikiendesha (inachukua ~5-8 dakika)
- Ukitaka kuianzisha wewe mwenyewe bila push mpya: bonyeza **Run workflow** (workflow_dispatch)

### 4. Pakua APK yako
- Workflow ikimaliza (alama ya kijani ✓), bonyeza kwenye run hiyo
- Chini kabisa utaona sehemu ya **Artifacts**
- Bonyeza `NidhamuApp-debug-apk` kupakua faili la `.zip` lenye `app-debug.apk` ndani yake
- Fungua zip, chukua `app-debug.apk`, hamishia kwenye simu yako ya Android, fungua ku-install
  (huenda ukahitaji kuruhusu "Install from unknown sources" kwenye mipangilio ya simu)

## Maelezo muhimu

- **appId**: `com.germinatetech.nidhamuapp` — hii ndiyo "jina la kipekee" la app yako duniani kote kwenye
  Android/Play Store. Ukitaka kuibadilisha, badilisha `appId` kwenye `capacitor.config.json` KABLA ya kujenga
  (na ikiwezekana ubadilishe mara moja tu — kuibadilisha baadaye kunaweza kusababisha matatizo ya update kwenye
  simu ambazo tayari zime-install toleo la zamani).
- **Icon**: workflow inatumia `resources/icon.png` kuunda icons zote za ukubwa mbalimbali (launcher icons,
  adaptive icons) kiotomatiki kupitia zana ya `@capacitor/assets`.
- **APK hii ni "debug" build** — inafaa kwa kujaribu/kutumia binafsi. Ukitaka kuipakia Google Play Store,
  utahitaji "release build" yenye signing key rasmi (tunaweza kuongeza hatua hiyo baadaye ukihitaji).
- Ukibadilisha chochote kwenye `www/index.html` (app yenyewe), fanya tu commit/push mpya — Actions itajenga
  APK mpya kiotomatiki kila wakati.

## Kama gradlew build itashindwa (error)
- Angalia "Actions" tab → bonyeza run iliyoshindwa → soma logs za step iliyokosea (kwa kawaida ni step ya
  mwisho "Build debug APK")
- Sababu za kawaida: appId isiyo sahihi (inapaswa kuwa herufi ndogo, vitone `.`, bila nafasi), au icon.png
  kuwa na ukubwa tofauti na 1024x1024 (tayari tumeshahakikisha hii ni sahihi).
