# [Coding Guideline for Unity C#](https://www.richardfu.net/coding-guideline-for-unity-c/)

![](https://i0.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Coding-Guideline-for-Unity-C.jpg)

As a programmer, following a coding standard is crucial to ensure code readability and understandability. While Unity and C# lack a definitive coding standard, you have the [freedom to code as you prefer](http://wiki.c2.com/?UncleBobOnCodingStandards). In this article, we present our internal style and formatting guidelines developed over the years to create a comfortable coding environment.

Our **3 golden rules** shape our style standard:
1. Adhere to Unity's style as described in the [Scripting Reference](https://docs.unity3d.com/ScriptReference/).
2. Follow Microsoft's [C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions) for the rest.
3. Implement minor tweaks to enhance code clarity.

#### Folder Structure

Group similar files in root folders:

```
Assets
  Animations       Animation clips
  Editor           Editor-specified scripts, prefabs, etc
  Fonts            Fonts used in the game
  Materials        Texture materials
  Prefabs          In-game prefabs
  Resources        Unity resources assets
  Scenes           Scenes
  Scripts          Code scripts, organized in sub-folders
  Sounds           Musics, sounds
  <CompanName>     Company folder shared across projects
    <ModuleName>   Module name, e.g. DailyLogin
      Scripts      Scripts within the module
      Prefabs      Prefabs within the module
      ...          Other assets grouped in folders
    ...            Other modules
  Textures         Image textures
    UI             Texture used for UI
    Icons          App icons
```

> We maintain a `<CompanyName>` folder containing well-structured modules that sync across projects using Git [submodules](https://www.git-scm.com/book/en/v2/Git-Tools-Submodules). This approach allows us to reuse common code and facilitate rapid prototyping.

#### File Naming

File naming is straightforward, always use PascalCase except for images:

-   Folders use PascalCase: `FolderName`/
-   Images use hyphen naming: `image-name-64x64.jpg`
-   The rest uses PascalCase: `ScriptName.cs`, `PrefabName.prefab`, `SceneName.unity`

> Images are often given hyphenated names because they are commonly used in both our website and press kit. Hyphenated naming is the prevailing convention for web images and is also favorable for [search engine optimization](https://www.natalleblas.com/optimise-images-for-seo/). Employing consistent names allows us to save time by avoiding the need for renaming and makes it effortless to locate the same image when transitioning between Unity and the web.

#### Scripts Naming

Scripts follow specific naming rules to convey their purpose:

-   `XxxPanel`, `XxxSlot`, `XxxButton`, etc for UI:
    `MenuPanel`, `AchievementSlot`, `CoinShopButton`
-   `XxxManager`, for master scripts controlling specific workflows (only ONE instance in the scene): 
    `DailyMissionManager`, `AchievementManager`
-   `XxxController`, for scripts controlling a game object (one or many in the scene):
    `PlayerController`, `BossController`, `BackgroundControler`
-   `XxxDatabase`, for a database (e.g. CSV) containing data rows:
    `WeaponDatabase`, `CardDatabase`
-   `XxxData`, for data rows in a CSV database:
    `WeaponData`, `CardData`
-   `XxxItem`, for in-game item instances:
    `CardItem`, `CharacterItem`
-   `XxxGenerator`, for scripts instantiating GameObjects:
    `ObjectGenerator`, `LandGenerator`
-   `XxxSettings`, for settings scripts inherent to Unity's [`ScriptableObject`](https://docs.unity3d.com/Manual/class-ScriptableObject.html) class:
    `AchievementSettings`, `DailyLoginSettings`
-   `XxxEditor`, for editor-only scripts inherent to Unity’s `[Editor](https://docs.unity3d.com/ScriptReference/Editor.html)` class:
    `TutorialTaskEditor`, `AchievementSettingsEditor`

> The distinction between a `Manager` and a `Controller` lies in the fact that a Manager should typically be implemented as a singleton or static entity, overseeing specific game logic that may involve multiple objects and assets. In contrast, a `Controller` is responsible for governing an individual object and can have multiple instances. For instance, within a scene, there may be several `EnemyController`s, each managing a distinct enemy. We will delve deeper into this topic in an upcoming article dedicated to singletons.

> The differentiation between `Data` and `Item` is that an `Item` represents an in-game instance, and in most cases, it encapsulates a `Data` component. For example, `CardData` encompasses all the preset attributes of a card, while `CardItem` incorporates the `CardData` and additional attributes that vary for different players, such as the card's level. We will explore this concept further in another article focused on data structures.

#### Variable Naming

Variable naming uses camelCase to convey their purpose without needing to read through all the code:

-   Prefix with an underscore for private and protected variables:
    `_itemCount`, `_controller`, `_titleText`
-   All capital letters for constants:
    `SHOW_SUB_MENU_LEVEL`, `MAX_HP_COUNT`, `BASE_DAMAGE`
-   All capital letters as PREFS_XXXX for PlayerPref keys:
    `private const string PREFS_LAST_FREE_DRAW = “LastFreeDraw”;`  
    `PlayerPrefs.GetInt(PREFS_LAST_FREE_DRAW)`
-   Use `xxxGO` for scene GameObject variables:
    `optionButtonGO`, `backgroundMaskGO`
-   Use `xxxPrefab` for scene GameObject variables:
    `weaponSlotPrefab`, `explosionPrefab`
-   Use `xxxTF` for Transform variables:
    `weaponTF`, `armTF`
-   Use `xxx<Component>` for all other components, abbreviated if necessary:
    `eyesSpriteRenderer` / `eyesSR`, `runAnimation` / `runAnim`, `attckAnimationClip` / `attackAC`, `victoryAudioClip` / `victoryAC`
-   Use xxxxs for arrays:
    `slotPrefabs = new Prefabs[0]`  
    `achievementIds = new int [0]`
-   Use xxxxList for `List` and xxxxDict for `Dictionary`:
    `weaponTransformList = new List()`  
    `achievementProgressDict = new Dictionary()`
-   Use nounVerbed for callback event:
    `public static event UnityAction gameStarted`  
    `public static UnityAction characterDied`

> In Unity, the convention for naming private/protected variables is to use the prefix `"m_"` and for static variables, `"s_"` (as indicated in their [BitBucket](https://bitbucket.org/Unity-Technologies/assetbundlegraphtool/src/0e498d12964d7cd1b86728289ff6f8321d127db4/Runtime/AssetBundleBuildMap.cs)). However, we've observed that this practice can make the code less readable. As a result, we have adopted a simplified approach by using a single underscore "_" for both private/protected and static variables to enhance code clarity.

> Callback event follow Unity’s event naming convention, e.g. `[SceneManager.activeSceneChanged](https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager-activeSceneChanged.html)`.

#### Functions Naming

-   Functions use PascalCase, starting with a verb and followed by a noun if necessary:
    `Reward()`, `StartGame()`, `MoveToCenter()`
-   Callback functions for UI button clicks are named as `OnXxxClick`:
    `OnStartClick()`, `OnCancelClick()`
-   Callback functions use `OnNounVerbed`:
    `OnChestOpened()`, `OnBuyWeaponConfirmed()`

#### Member Properties

If a function has no input, use a member property (get/set property) in PascalCase:
`bool IsRewarded {get { return …} }`  
Or
`bool IsRewarded => ...;`

Member properties are used where a variable can only be set internally but accessed publicly:
`public int Id { get; private set; }`  
`public Sprite IconSprite { get; private set; }`

#### Variables / Functions orders

Consistent order saves time searching for specific variables or functions. Here's the preferred order:

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

You can also consider grouping variables and functions within a region in larger scripts, but ensure that the script remains manageable and doesn't become overly extensive, to the point where it should be split into multiple scripts. This can help improve code organization and readability.

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

We use **K&R** style, which is preset in Mac Visual Studio:

![](https://i1.wp.com/www.richardfu.net/wp-content/uploads/2020/10/KR-style-in-Mac-Visual-Studio.jpg?resize=1022%2C691&ssl=1)

In Windows, you can manually configure it to match the K&R style:

![](https://i2.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-1.jpg?resize=1024%2C173&ssl=1)

![](https://i2.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-2.jpg?resize=1024%2C492&ssl=1)

![](https://i2.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-3.jpg?resize=1024%2C766&ssl=1)

![](https://i0.wp.com/www.richardfu.net/wp-content/uploads/2020/10/Windows-coding-format-4.jpg?resize=1024%2C107&ssl=1)

#### Brackets and Newlines

For single-line conditional statements, no brackets are needed:

```
if (1 == 1)
  DoSomethingFancy();
if (1 == 1) return true;
if (1 == 1) {
  FirstThing();
  LastThing();
}
```

> Numerous developers, Unity included, have a preference for inserting newlines after an open bracket "{." While this practice can lend a sense of tidiness and symmetry to the code, it can also generate numerous unnecessary lines, leading to more frequent and longer script scrolling. Every bit of efficiency matters, as even spending an extra second on scrolling each day can add up to wasting five minutes of valuable time over the course of a year.

#### Comments

Single line comments should include a space after the slashes:  
`// Notice the space after the two slashes`  
Temporarily commenting out code should not include a space after the slashes, to be differentiated from the normal comment:  
`//noSpace.Code();`  
`TODO` comments should include the space after the slashes and then 2 spaces after the colon following the TODO  
`// TODO: Notice the space after the slashes and the 2 spaces after the colon of todo`

----------

That concludes our coding guideline. It's worth noting that these guidelines may expand as you incorporate more recent .Net syntax. If you're interested in further best practices, there are additional valuable [guidelines](https://github.com/justinwasilenko/Unity-Style-Guide) to [explore](http://www.arreverie.com/blogs/unity3d-best-practices-folder-structure-source-control/). Feel free to embrace this coding style, as it promotes clean and readable code, contributing to the enhancement of the game development community. 😊
