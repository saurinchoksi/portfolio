# Sequence Expert Sanitization: Project Plan for Claude Code

## Context

Choksi is building a portfolio website for Creative Technologist roles. He needs to demonstrate a tool he built called **Sequence Expert** — a browser-based utility that lets content teams browse, search, and play audio sequences from XML-defined narrative pipelines.

**The problem:** The tool currently loads real project data with proprietary book codes (HDD, GLENN, FEED, etc.) and internal file naming conventions. This data cannot appear in portfolio screenshots or screen recordings.

**The solution:** Create a `/sanitized_demo/` folder with dummy XML data and placeholder audio files that the tool can load for portfolio demonstration purposes.

---

## File Locations

- **Sequence Expert tool:** `/Users/choksi/Perforce/choksi_wandalone/web_scripts/sequence_expert/`
- **Real XML data (reference only):** `/Users/choksi/Perforce/choksi_wandalone/`
- **Sanitized demo folder:** `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/`

The tool loads data via relative paths starting from `BookManifest.xml`. The sanitized folder needs to mirror the expected directory structure.

---

## Dummy Data Naming Conventions

### Book Codes (10 total)
Replace all real book codes with these generic alternatives:
- `DINO` (primary demo book — fully populated)
- `DINO2`, `FARM`, `SAFARI`, `OCEAN`, `TRAIN`, `SPACE`, `ABC1`, `NUM1`, `TEST`

### Audio File Naming Pattern
- Old: `HDD_NA_LetsPlayAGame_5.wv`
- New: `DINO_NA_LetsPlayAGame_07.wv`

Format: `{BOOK}_{SPEAKER}_{Description}_{##}.wv`
- `NA` = Narrator
- `SFX` = Sound Effect

### Sequence ID Pattern
- Old: `seq_S000_intro`
- New: `seq_001_intro`

---

## Required Directory Structure

```
/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/
├── BookManifest.xml
└── BOOKS/
    └── DINO/
        ├── KC_Activities/
        │   ├── DINO_ActivityManifest.xml
        │   ├── DINO_Story/
        │   │   ├── DINO_Story_MediaSequences.xml
        │   │   └── DINO_Story_AssetManifest.xml
        │   ├── DINO_Game01/
        │   │   ├── DINO_Game01_MediaSequences.xml
        │   │   └── DINO_Game01_AssetManifest.xml
        │   └── DINO_Game02/
        │       ├── DINO_Game02_MediaSequences.xml
        │       └── DINO_Game02_AssetManifest.xml
        └── KC_Assets/
            └── [placeholder .wv and .lit files]
```

---

## TODO: Files to Create

### 1. BookManifest.xml

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BookManifest.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<bookManifest xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="XmlSchema/BookManifest.xsd">
    <globalWandUi id="GlobalWandUi" activityManifest="GlobalWandUi/KC_Activities/KC_ActivityManifest.xml" />
    
    <book id="DINO" activityManifest="BOOKS/DINO/KC_Activities/DINO_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="DINO2" activityManifest="BOOKS/DINO2/KC_Activities/DINO2_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="FARM" activityManifest="BOOKS/FARM/KC_Activities/FARM_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="SAFARI" activityManifest="BOOKS/SAFARI/KC_Activities/SAFARI_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="OCEAN" activityManifest="BOOKS/OCEAN/KC_Activities/OCEAN_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="TRAIN" activityManifest="BOOKS/TRAIN/KC_Activities/TRAIN_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="SPACE" activityManifest="BOOKS/SPACE/KC_Activities/SPACE_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="ABC1" activityManifest="BOOKS/ABC1/KC_Activities/ABC1_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="NUM1" activityManifest="BOOKS/NUM1/KC_Activities/NUM1_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
    <book id="TEST" activityManifest="BOOKS/TEST/KC_Activities/TEST_ActivityManifest.xml" globalUiTar="GlobalWandUi.tar" />
</bookManifest>
```

---

### 2. DINO_ActivityManifest.xml

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BOOKS/DINO/KC_Activities/DINO_ActivityManifest.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<activityManifest>
   <book fbsPath="wandV2"/>
   <activity code="Story">
      <authoring file="DINO_Story/DINO_Story_Activity.xml"/>
      <layout file="DINO_Story/DINO_Story_Activity.xml"/>
      <sequence file="DINO_Story/DINO_Story_MediaSequences.xml"/>
      <assets file="DINO_Story/DINO_Story_AssetManifest.xml"/>
   </activity>
   <activity code="Game01">
      <authoring file="DINO_Game01/DINO_Game01_Activity.xml"/>
      <layout file="DINO_Game01/DINO_Game01_Activity.xml"/>
      <sequence file="DINO_Game01/DINO_Game01_MediaSequences.xml"/>
      <assets file="DINO_Game01/DINO_Game01_AssetManifest.xml"/>
   </activity>
   <activity code="Game02">
      <authoring file="DINO_Game02/DINO_Game02_Activity.xml"/>
      <layout file="DINO_Game02/DINO_Game02_Activity.xml"/>
      <sequence file="DINO_Game02/DINO_Game02_MediaSequences.xml"/>
      <assets file="DINO_Game02/DINO_Game02_AssetManifest.xml"/>
   </activity>
</activityManifest>
```

---

### 3. DINO_Story_MediaSequences.xml

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BOOKS/DINO/KC_Activities/DINO_Story/DINO_Story_MediaSequences.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequences xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="../../../../XmlSchema/KC_MediaSequences.xsd">

    <sequence id="seq_001_intro">
        <media audioId="audio_DINO_WelcomeToTheStory_01" ledId="led_GENERAL_4X" volume="100"/>
        <wait ms="300"/>
        <media audioId="audio_DINO_LetsBegin_02" ledId="led_GENERAL_2X" volume="100"/>
    </sequence>

    <sequence id="seq_002_spread01">
        <media audioId="audio_DINO_OnceUponATime_03" ledId="led_GENERAL_4X" volume="100"/>
        <wait ms="400"/>
        <media audioId="audio_DINO_ThereWasADinosaur_04" ledId="led_GENERAL_3X" volume="100"/>
        <media audioId="audio_SFX_DinoRoar_01" volume="80"/>
    </sequence>

    <sequence id="seq_003_spread02">
        <media audioId="audio_DINO_HeLovedToPlay_05" ledId="led_GENERAL_4X" volume="100"/>
        <wait ms="200"/>
        <media audioId="audio_DINO_EveryDay_06" ledId="led_GENERAL_2X" volume="100"/>
    </sequence>

    <sequence id="seq_010_gamePrompt">
        <media audioId="audio_DINO_LetsPlayAGame_07" ledId="led_GENERAL_4X" volume="100"/>
        <wait ms="500"/>
        <media audioId="audio_DINO_PointAtTarget_08" ledId="led_GAME_Highlight" volume="100"/>
    </sequence>

    <sequence id="seq_011_correctResponse">
        <media audioId="audio_SFX_PositiveInput_01" ledId="led_GlobalPositiveInput" volume="100"/>
        <media audioId="audio_DINO_GreatJob_09" ledId="led_GENERAL_3X" volume="100"/>
    </sequence>

    <sequence id="seq_012_incorrectResponse">
        <media audioId="audio_DINO_TryAgain_10" ledId="led_GENERAL_2X" volume="100"/>
        <wait ms="300"/>
    </sequence>

    <sequence id="seq_UI_Selection">
        <media audioId="audio_SFX_Selection_01" ledId="led_Selection" volume="100"/>
    </sequence>

    <sequence id="seq_UI_ActivityComplete">
        <media audioId="audio_SFX_Complete_01" ledId="led_ActivityComplete" volume="100"/>
        <media audioId="audio_DINO_YouDidIt_11" ledId="led_GENERAL_4X" volume="100"/>
    </sequence>

    <sequence id="seq_KillSwitch">
        <media audioId="audio_Silence_10ms" channel="1"/>
    </sequence>

</sequences>
```

---

### 4. DINO_Story_AssetManifest.xml

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BOOKS/DINO/KC_Activities/DINO_Story/DINO_Story_AssetManifest.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<assetManifest>

    <audio id="audio_Silence_10ms" file="Silence_10ms.wv"/>

    <audio id="audio_DINO_WelcomeToTheStory_01" file="DINO_NA_WelcomeToTheStory_01.wv"/>
    <audio id="audio_DINO_LetsBegin_02" file="DINO_NA_LetsBegin_02.wv"/>
    <audio id="audio_DINO_OnceUponATime_03" file="DINO_NA_OnceUponATime_03.wv"/>
    <audio id="audio_DINO_ThereWasADinosaur_04" file="DINO_NA_ThereWasADinosaur_04.wv"/>
    <audio id="audio_DINO_HeLovedToPlay_05" file="DINO_NA_HeLovedToPlay_05.wv"/>
    <audio id="audio_DINO_EveryDay_06" file="DINO_NA_EveryDay_06.wv"/>
    <audio id="audio_DINO_LetsPlayAGame_07" file="DINO_NA_LetsPlayAGame_07.wv"/>
    <audio id="audio_DINO_PointAtTarget_08" file="DINO_NA_PointAtTarget_08.wv"/>
    <audio id="audio_DINO_GreatJob_09" file="DINO_NA_GreatJob_09.wv"/>
    <audio id="audio_DINO_TryAgain_10" file="DINO_NA_TryAgain_10.wv"/>
    <audio id="audio_DINO_YouDidIt_11" file="DINO_NA_YouDidIt_11.wv"/>

    <audio id="audio_SFX_DinoRoar_01" file="SFX_DinoRoar_01.wv"/>
    <audio id="audio_SFX_PositiveInput_01" file="SFX_PositiveInput_01.wv"/>
    <audio id="audio_SFX_Selection_01" file="SFX_Selection_01.wv"/>
    <audio id="audio_SFX_Complete_01" file="SFX_Complete_01.wv"/>

    <led id="led_GENERAL_4X" file="STRY_NAR_4X.lit"/>
    <led id="led_GENERAL_3X" file="STRY_NAR_3X.lit"/>
    <led id="led_GENERAL_2X" file="STRY_NAR_2X.lit"/>
    <led id="led_GAME_Highlight" file="UI_GAME_Highlight.lit"/>
    <led id="led_GlobalPositiveInput" file="UI_GlobalPositiveInput.lit"/>
    <led id="led_Selection" file="UI_Selection.lit"/>
    <led id="led_ActivityComplete" file="UI_ActivityComplete.lit"/>

</assetManifest>
```

---

### 5. DINO_Game01 Files

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BOOKS/DINO/KC_Activities/DINO_Game01/`

**DINO_Game01_MediaSequences.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequences xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <sequence id="seq_Game01_Intro">
        <media audioId="audio_DINO_Game01_Intro" ledId="led_GENERAL_4X" volume="100"/>
    </sequence>
    <sequence id="seq_Game01_Prompt">
        <media audioId="audio_DINO_Game01_Prompt" ledId="led_GAME_Highlight" volume="100"/>
    </sequence>
    <sequence id="seq_Game01_Correct">
        <media audioId="audio_SFX_PositiveInput_01" ledId="led_GlobalPositiveInput" volume="100"/>
    </sequence>
</sequences>
```

**DINO_Game01_AssetManifest.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<assetManifest>
    <audio id="audio_DINO_Game01_Intro" file="DINO_NA_Game01_Intro.wv"/>
    <audio id="audio_DINO_Game01_Prompt" file="DINO_NA_Game01_Prompt.wv"/>
    <audio id="audio_SFX_PositiveInput_01" file="SFX_PositiveInput_01.wv"/>
    <led id="led_GENERAL_4X" file="STRY_NAR_4X.lit"/>
    <led id="led_GAME_Highlight" file="UI_GAME_Highlight.lit"/>
    <led id="led_GlobalPositiveInput" file="UI_GlobalPositiveInput.lit"/>
</assetManifest>
```

---

### 6. DINO_Game02 Files

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BOOKS/DINO/KC_Activities/DINO_Game02/`

**DINO_Game02_MediaSequences.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sequences xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <sequence id="seq_Game02_Intro">
        <media audioId="audio_DINO_Game02_Intro" ledId="led_GENERAL_4X" volume="100"/>
    </sequence>
    <sequence id="seq_Game02_Prompt">
        <media audioId="audio_DINO_Game02_Prompt" ledId="led_GAME_Highlight" volume="100"/>
    </sequence>
</sequences>
```

**DINO_Game02_AssetManifest.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<assetManifest>
    <audio id="audio_DINO_Game02_Intro" file="DINO_NA_Game02_Intro.wv"/>
    <audio id="audio_DINO_Game02_Prompt" file="DINO_NA_Game02_Prompt.wv"/>
    <led id="led_GENERAL_4X" file="STRY_NAR_4X.lit"/>
    <led id="led_GAME_Highlight" file="UI_GAME_Highlight.lit"/>
</assetManifest>
```

---

### 7. Placeholder Audio/LED Files

Location: `/Users/choksi/Perforce/choksi_wandalone/sanitized_demo/BOOKS/DINO/KC_Assets/`

Create empty placeholder files using `touch`:

```bash
# Audio files
touch Silence_10ms.wv
touch DINO_NA_WelcomeToTheStory_01.wv
touch DINO_NA_LetsBegin_02.wv
touch DINO_NA_OnceUponATime_03.wv
touch DINO_NA_ThereWasADinosaur_04.wv
touch DINO_NA_HeLovedToPlay_05.wv
touch DINO_NA_EveryDay_06.wv
touch DINO_NA_LetsPlayAGame_07.wv
touch DINO_NA_PointAtTarget_08.wv
touch DINO_NA_GreatJob_09.wv
touch DINO_NA_TryAgain_10.wv
touch DINO_NA_YouDidIt_11.wv
touch DINO_NA_Game01_Intro.wv
touch DINO_NA_Game01_Prompt.wv
touch DINO_NA_Game02_Intro.wv
touch DINO_NA_Game02_Prompt.wv
touch SFX_DinoRoar_01.wv
touch SFX_PositiveInput_01.wv
touch SFX_Selection_01.wv
touch SFX_Complete_01.wv

# LED pattern files
touch STRY_NAR_4X.lit
touch STRY_NAR_3X.lit
touch STRY_NAR_2X.lit
touch UI_GAME_Highlight.lit
touch UI_GlobalPositiveInput.lit
touch UI_Selection.lit
touch UI_ActivityComplete.lit
```

---

## Verification

After creating all files, verify the tool loads the sanitized data:

1. The tool's `libraryLoader.js` loads `BookManifest.xml` first
2. It expects paths like: `BOOKS/DINO/KC_Activities/DINO_ActivityManifest.xml`
3. Then: `BOOKS/DINO/KC_Activities/DINO_Story/DINO_Story_MediaSequences.xml`
4. Assets from: `BOOKS/DINO/KC_Assets/`

You may need to modify the tool's base path or serve from the sanitized_demo folder.

---

## Summary Checklist

- [ ] Create directory structure under `/sanitized_demo/`
- [ ] Create `BookManifest.xml` with 10 book codes
- [ ] Create `DINO_ActivityManifest.xml` with 3 activities
- [ ] Create `DINO_Story_MediaSequences.xml` with 10 sequences
- [ ] Create `DINO_Story_AssetManifest.xml` with audio/LED mappings
- [ ] Create `DINO_Game01_MediaSequences.xml` and `AssetManifest.xml`
- [ ] Create `DINO_Game02_MediaSequences.xml` and `AssetManifest.xml`
- [ ] Create 27 placeholder files in `KC_Assets/`
- [ ] Test tool loads "DINO" from dropdown
