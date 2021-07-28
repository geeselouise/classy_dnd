# Interpreting what aspects of a DnD 5e characters are indicative of their Class

## Background:
I was roped into joining a *Dungeons and Dragons 5th edition (DnD 5e)* campaign about 2 years ago and ended up loving it. Since then, I have been a part of two other campaigns and a few other one-shot role playing games (RPGs). DnD is now one of my main hobbies that I do on the weekends with my friends and significant other. 
Game participants are either a **Dungeon Master (DM)** or **players**. The DM acts as the referee/ storyteller, and they create/ maintain the world and adventure(s) (one adventure is called a **one-shot** while multiple adventures together are a **campaign**). DMs also act as the characters that the players interact with (**non-playable characters or NPCs**). Each player creates their own charcter with a separate backstory, race, class, and abilities. Together the players form a **party** and interact with each other. The DM and players meet-up to play during what is called a **session**. During each session, the party interacts with the NPCs, problem-solve, explore, travel, and fight to gain experience and advance to new levels. 
During character creation, each player is required to chose a **class**, which influences a character's skills and abilities. There are some aspects of a character that can indicate their class, such as stats, skills, spells, etc. Additionally, players enjoy optimzing their character builds by **multiclassing**. Generally, it is used to buff a character's weakness or to add flavor/ distinguish your character. They do this by taking a level in the new class instead of progressing another level in their base class. 

## Design:
I am interested in building an interpretive classification model that utilizes DnD character sheet data to determine what the principal base class of the character is (*Barbarian, Bard, Cleric, Druid, Fighter, Monk, Paladin, Ranger, Rogue, Sorcerer, Warlock, Wizard*). To start, I plan to build a binary classifier that determines whether or not a character is multiclassing. The entire goal of this project is to ascertain which features indicate class and how their interactions influence their class.

## Data:
All character sheet data was obtained from GitHub user [oganm's](https://github.com/oganm) repository [dnddata](https://github.com/oganm/dnddata). It is a weekly updated dataset of characters that are submitted to his web applications [printSheetApp](https://oganm.com/shiny/printSheetApp/) and [interactiveSheet](https://oganm.com/shiny/interactiveSheet/). He did some exploratory data analysis (EDA) on the dataset to determine rarity of DnD characters in a [dndstats blogpost](https://oganm.github.io/dndstats/). This was based on an article by FiveThirtyEight titled: [Is Your D&D Character Rare?](https://fivethirtyeight.com/features/is-your-dd-character-rare/)

The [**dataset**](https://github.com/oganm/dnddata/blob/master/data-raw/dnd_chars_all.tsv) has 9784 character sheets, with about 19 features. Each Feature is as follows:
ip: A shortened hash of the IP address of the submitter

**finger**: A shortened hash of the browser fingerprint of the submitter

**name**: A shortened hash of character names

**race**: Race of the character as coded by the app. May be unclear as the app inconsistently codes race/subrace information. See processedRace

**background**: Background as it comes out of the application.

**date**: Time & date of input. Dates before 2018-04-16 are unreliable as some has accidentally changed while moving files around.

**class**: Class and level. Different classes are separated by | when needed.

**justClass**: Class without level. Different classes are separated by | when needed.

**subclass**: Subclass. Might be missing if the character is low level. Different classes are separated by | when needed.

**level**: Total level

**feats**: Feats chosen. Mutliple feats are separated by | when needed

**HP**: Total hit points.

**AC**: Armor Class score

**Str, Dex, Con, Int, Wis, Cha**: Ability score modifiers

**alignment**: Alignment free text field. Since it’s a free text field, it includes alignments written in many forms. See processedAlignment, good and lawful to get the standardized alignment data.

**skills**: List of proficient skills. Skills are separated by |.

**weapons**: List of weapons, separated by |. This is a free text field. See processedWeapons for the standardized version

**spells**: List of spells, separated by |. Each spell has its level next to it separated by *s. This is a free text field. See processedSpells for the standardized version

**castingStat**: Casting stat as entered by the user. The format allows one casting stat so this is likely wrong if the character has different spellcasting classes. Also every character has a casting stat even if they are not casters due to the data format.

**choices**: Character building choices. This field information about character properties such as fighting styles and skills chosen for expertise. Different choice types are separated by | when needed. The choice data is written as name of choice followed by a / followed by the choices that are separated by *s

**country**: The origin of the submitter’s IP

**countryCode**: 2 letter country code

**processedAlignment**: Standardized version of the alignment column. I have manually matched each non standard spelling of alignment to its correct form. First character represents lawfulness (L, N, C), second one goodness (G,N,E). An empty string means alignment wasn’t written or unclear.

**good, lawful**: Isolated columns for goodness and lawfulness

**processedRace**: I have gone through the way race column is filled by the app and asigned them to correct races. Also includes some common races that are not natively supported such as warforged and changelings. If empty, indiciates a homebrew race not natively supported by the app.

**processedSpells**: Formatting is same as spells. Standardized version of the spells column. Spells are matched to an official list using string similarity and some hardcoded rules.

**processedWeapons**: Formatting is same as weapons. Standardized version of the weapons column. Created like the processedSpells column.

**levelGroup**: Splits levels into groups. The groups represent the common ASI levels

**alias**: A friendly alias that correspond to each uniqe name

## Algorithm:
- The classification model algorithms for this project that will be tested are: **Logistic Regression, Random Forest, and Support Vector Machine**.
- Classification model evaluation:
    - Confusion Matrix
    - Percision
    - Recall
    - F-score
    - ROC Curves
- 5-fold cross-validation to detect overfitting and validate the model.

## Tools:
- sklearn, pandas, numpy, and matplotlib
- Tableau for visualization
    - Some EDA has already been done on the dataset. To see the dashboard click [here](https://public.tableau.com/app/profile/louisa.reilly/viz/dnd5e_char/DnD_Char?publish=yes).
- SQL for data storage

### Sources/Acknowledgements:
- Gary Gygax and Dave Arneson for creating DnD.
- Wizards of the Coast(subsidary of Hasbro) which publishes DnD guides.
- B. Ogan Mancarci for his data collection.
- Dan Quach for his 2020 [blogpost](https://towardsdatascience.com/classifying-character-classes-in-dungeons-dragons-with-machine-learning-86751240594d) about creating a classifier using Oganm's dataset.