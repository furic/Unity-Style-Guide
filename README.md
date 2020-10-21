# [Coding Guideline for Unity C#](https://www.richardfu.net/coding-guideline-for-unity-c/)

As a programmer, it is very important to follow a coding standard and make sure others can read and easily understand his code. While there’s not majority guideline of coding standard in Unity and C#, [you can/should code whatever you like](http://wiki.c2.com/?UncleBobOnCodingStandards), but in this article I’m going to show you our style and format guideline within the company. We have tried to adopt different styles throughout these years and came out the one we feel most comfortable.

Our **3 golden rules** to forms our style standard:  
1. Follow Unity’s style in [Scripting Reference](https://docs.unity3d.com/ScriptReference/)  
2. The rest follow Microsoft’s [C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)  
3. Minor tweak where we believe that can make the code clearer

#### Folder Structure

All similar files should be grouped in the root folders:

```
Assets
  Animations       Animation clips
  Editor           Editor specified scripts, prefabs, etc
  Fonts            Fonts used in game
  Materials        Texture materials
  Prefabs          In-game prefabs
  Resources        Unity resources assets
  Scenes           Scenes
  Scripts          Code scripts, grouped in sub-folders
  Sounds           Musics, sounds
  SweatyChair      Company folder that share across projects
    (ModuleName1)  Module name, e.g. DailyLogin
      (Scripts)    Scripts within the module
      (Prefabs)    Prefabs within the module
      ...          Other assets grouped in folders
    ...            Other modules
  Textures         Image textures
    UI             Texture used for UI
    Icons          App icons
```

> We have a `SweatyChair` folder, which containes a number of well-structure modules and sync across projects using Git [submodules](https://www.git-scm.com/book/en/v2/Git-Tools-Submodules). We we are always reuse the commond code and able to perform rapid prototyping.

#### File Naming

File naming is simple, always use Pascal Case except for images:

-   Folders – PascalCase  
    `FolderName`/
-   Images – hyphen-  
    `image-name-64x64.jpg`
-   The rest – PascalCase  
    `ScriptName.cs`, `PrefabName.prefab`, `SceneName.unity`

> The reason images use hyphen naming is that most images are also used in our website / press-kit. Hyphen naming is the majority naming convention for web image and it’s also [search engine friendly](https://www.natalleblas.com/optimise-images-for-seo/). Using the same name can save us time renaming and easily find the same image switching between Unity and web.

#### Scripts Naming

While each script has its unique purpose and use cases, we follow these naming rules and so we can still roughly know what the script is for by simply reading the filenames:

-   `XxxPanel`, `XxxSlot`, `XxxButton`, etc for UI  
    `MenuPanel`, `AchievementSlot`, `CoinShopButton`
-   `XxxManager`, for master scripts that control specific workflow (only ONE instance in the scene)  
    `DailyMissionManager`, `AchievementManager`
-   `XxxController`, for scripts controlling a game object (one or many in the scene)  
    `PlayerController`, `BossController`, `BackgroundControler`
-   `XxxDatabase`, for a database (e.g CSV) which contains a list of data rows  
    `WeaponDatabase`, `CardDatabase`
-   `XxxData`, for data row in a CSV database  
    `WeaponData`, `CardData`
-   `XxxItem`, for in-game item instance  
    `CardItem`, `CharacterItem`
-   `XxxGenerator`, for scripts instantiate GameObjects  
    `ObjectGenerator`, `LandGenerator`
-   `XxxSettings`, for settings scripts inherent Unity’s [`ScriptableObject`](https://docs.unity3d.com/Manual/class-ScriptableObject.html) class  
    `AchievementSettings`, `DailyLoginSettings`
-   `XxxEditor`, for editor-only scripts inherent Unity’s `[Editor](https://docs.unity3d.com/ScriptReference/Editor.html)` class  
    `TutorialTaskEditor`, `AchievementSettingsEditor`

> Difference between `Manager` and `Controller` is `Manager` should be singleton or static, and it controls a specific game logic that may involve multiple objects and assets, while Controller controls an object and may have multiple instances. For example, there are multiple EnemyController in the scene and each control one enemy. We will discuss more on this in a singleton article.

> Difference between `Data` and `Item` is that `Item` is an instance in-game, and in most cases `Item` contains a `Data`. For example `CardData` has all preset attributes of a card, while `CardItem` has its `CardData` plus attributes that vary for different players, such as card level. We will talk about more this in another data structure article.

#### Variable Naming

Similar to file naming, variable naming allow us to know what the variable is for without reading though all code:

-   Use camcelCase, always as of a noun or in a form of (is/has)Abjective  
    `hasRewarded`, `currentPosition`, `isInitialized`
-   Prefix with an underscore for private and protected variables  
    `_itemCount`, `_controller`, `_titleText`
-   All capital letters for const  
    `SHOW_SUB_MENU_LEVEL`, `MAX_HP_COUNT`, `BASE_DAMAGE`
-   All capital letters and PREFS_XXXX for PlayerPref keys  
    `private const string PREFS_LAST_FREE_DRAW = “LastFreeDraw”;`  
    `PlayerPrefs.GetInt(PREFS_LAST_FREE_DRAW)`
-   Use `xxxGO` for scene GameObject variables  
    `optionButtonGO`, `backgroundMaskGO`
-   Use `xxxPrefab` for scene GameObject variables  
    `weaponSlotPrefab`, `explosionPrefab`
-   Use `xxxTF` for Transform variables  
    `weaponTF`, `armTF`
-   Use `xxxComponent` for all other components, abbreviate if needed  
    `eyesSpriteRenderer` / `eyesSR`, `runAnimation` / `runAnim`, `attckAnimationClip` / `attackAC`, `victoryAudioClip` / `victoryAC`
-   Use xxxxs for arrays  
    `slotPrefabs = new Prefabs[0]`  
    `achievementIds = new int [0]`
-   Use xxxxList for List and xxxxDict for Dictionary  
    `weaponTransformList = new List()`  
    `achievementProgressDict = new Dictionary()`
-   Use nounVerbed for callback event  
    `public static event UnityAction gameStarted`  
    `public static UnityAction characterDied`

> Unity use prefix `m_` for private/protected variables, and `s_` for static variables (see their [BitBucket](https://bitbucket.org/Unity-Technologies/assetbundlegraphtool/src/0e498d12964d7cd1b86728289ff6f8321d127db4/Runtime/AssetBundleBuildMap.cs)). We found that this makes the code very hard to read, so we leave one underscore for private/protected variables and keep it the same for both static and non-static variables.

> Callback event follow Unity’s event naming convention, e.g. `[SceneManager.activeSceneChanged](https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager-activeSceneChanged.html)`.

#### Functions Naming

-   Use PascalCase, start with a verb and followed by a noun if needed  
    `Reward()`, `StartGame()`, `MoveToCenter()`
-   Use `OnXxxxClick`, for UI button clicks  
    `OnStartClick()`, `OnCancelClick()`
-   Use `OnNounVerbed`, for callbacks  
    `OnChestOpened()`, `OnBuyWeaponConfirmed()`

#### Member Properties

If a function has no input, use member property (aka get/set property) instead, in PacelCase as well:  
`bool IsRewarded() { }`  
to  
`bool IsRewarded {get { return …} }`  
Or even in a shortened form as  
`bool IsRewarded => ...;`

Also, use member properties where a variable can only set internally but can be accessed publicly:  
`public int Id { get; private set; }`  
`public Sprite IconSprite { get; private set; }`

#### Variables / Functions orders

Having a consistent order can greatly save our time on looking for a specific variable or function:

```
MyCalss : Monobehavior
{
  // Constant variables
  public const int CONST_1;
  private const string CONST_2;

  // Static variables
  public static int static1;
  private static string _static2;
  
  // Editor-assigned variables
  [SerializeField] public Image maskImage;
  [SerializeField] private MyClass _anotherComponent;

  // Other varaibles
  public int primitiveVariables1;
  private string _primitiveVariable2;

  // Properties
  public int Property1 {get; set;}
  private string _roperty2 {get; set;}

  // Unity functions
  Awake()
  OnEnable()
  Start()
  Update()
  FixedUpdate()

  // Other custom functions
  Init()
  ...
  Reset()

  // Unity functions
  OnDisable()
  OnDestroy()

#region UNITY_EDITOR
  // Eebug functions that only runs in Editor
  [ContextMenu("...")]
  private void DebugFunction1()
  [MenuItem("Debug/...")]
  private static void DebugFunction2()
#endregion
}
```

You can also group valuables and functions in a region in large scripts (but not too large enough to be split into another script):

```
#region Timer
  private const int RESET_SECONDS = 180;
  private float _secondsLeft;
  public float canResetTime => _secondsLeft < 0;
  public void ResetTime()
  {
    _secondsLeft = RESET_SECONDS;
  }
#endregion Timer
... // Other regions
```

#### Code Formatting

We use **K&R** style which is already preset in Mac Visual Studio:

![](https://i1.wp.com/www.richardfu.net/wp-content/uploads/2020/10/KR-style-in-Mac-Visual-Studio.jpg?resize=1022%2C691&ssl=1)

While there’s not K&R in Windows (Why, Microsoft?), just a little bit effort to manually set few ticks:

![](https://i2.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-1.jpg?resize=1024%2C173&ssl=1)

![](https://i2.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-2.jpg?resize=1024%2C492&ssl=1)

![](https://i2.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-3.jpg?resize=1024%2C766&ssl=1)

![](https://i0.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-4.jpg?resize=1024%2C107&ssl=1)

#### Brackets and Newlines

For single-line conditional statements we do not need any brackets:

```
if (1 == 1)
  DoSomethingFancy();
if (1 == 1) return true;
if (1 == 1) {
  FirstThing();
  LastThing();
}
```

> Many developers, includes Unity, favourite putting newlines after open bracket ( `{` ). It may make the code looks more tidy and symmetric, but also creates a lot of unuseful lines and making script scrolling much more often and longer. Every bit count. If you spend 1 more second on scrolling a day, you probably wasted 5 minutes of valuable time a year.

#### Comments

Single line comments should include a space after the slashes:  
`// Notice the space after the two slashes`  
Temporarily commenting out code should not include a space after the slashes, to be differentiated from the normal comment:  
`//noSpace.Code();`  
`TODO` comments should include the space after the slashes and then 2 spaces after the colon following the TODO  
`// TODO: Notice the space after the slashes and the 2 spaces after the colon of todo`

----------

That’s all. This may go bigger when you adopt more latest .Net syntax. There are also a few other good [guidelines](https://github.com/justinwasilenko/Unity-Style-Guide) if you want to read [more](http://www.arreverie.com/blogs/unity3d-best-practices-folder-structure-source-control/). Free feel to follow this style if you think it’s reasonable and helpful, so we may create a better game dev world where code is clean and please to read. 😀
