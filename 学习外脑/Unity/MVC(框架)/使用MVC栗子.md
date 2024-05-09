#永久 #Unity 

![[MVC基本概念#^675089]]

![[MVC基本概念#^e1441c]]


## 界面
### 主面板 MainView.cs
```csharp
public class MainView : MonoBehaviour
{
    public Button btnRole;
    public Button btnSkill;

    public Text txtName;
    public Text txtLev;
    public Text txtMoney;
    public Text txtGem;
    public Text txtPower;

    // 通过传入的数据来更新外部面板
    public void UpdateInfo(PlayerModel data)
    {
        txtName.text = data.PlayerName;
        txtLev.text = $"Lv.{data.Lev}";
        txtMoney.text = data.Money.ToString();
        txtGem.text = data.Gem.ToString();
        txtPower.text = data.Power.ToString();
    }
}
```

### 角色面板 RoleView.cs
```csharp
public class RoleView : MonoBehaviour
{
    public Button btnLevUp;
    public Button btnClose;

    public Text txtLev;
    public Text txtHp;
    public Text txtAtk;
    public Text txtDef;
    public Text txtCrit;
    public Text txtMiss;
    public Text txtLuck;

    // 根据数据模型的更新来更新控件
    public void UpdateInfo(PlayerModel data)
    {
        txtLev.text = $"Lev.{data.Lev}";
        txtHp.text = data.Hp.ToString();
        txtAtk.text = data.Atk.ToString();
        txtDef.text = data.Def.ToString();
        txtCrit.text = data.Crit.ToString();
        txtMiss.text = data.Miss.ToString();
        txtLuck.text = data.Luck.ToString();
    }
}
```




## 数据
### 玩家数据 唯一 PlayerModel.cs
```csharp
public class PlayerModel
{
    public string PlayerName => _playerName;
    private string _playerName;

    public int Lev => _lev;
    private int _lev;

    public int Money => _money;
    private int _money;

    public int Gem => _gem;
    private int _gem;
    public int Power => _power;
    private int _power;

    public int Hp => _hp;
    private int _hp;
    public int Atk => _atk;
    private int _atk;
    public int Def => _def;
    private int _def;
    public int Crit => _crit;
    private int _crit;
    public int Miss => _miss;
    private int _miss;
    public int Luck => _luck;
    private int _luck;

    private static PlayerModel _instance = null;
    public static PlayerModel Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new PlayerModel();
                _instance.Init();
            }
            return _instance;
        }
    }

    // 通知外部更新的事件            通过它来和外部建立联系而不是直接获取外部面板
    private event Action<PlayerModel> _updateEvent;
    public void AddEventListener(Action<PlayerModel> action)
    {
        _updateEvent += action;
    }
    public void MoveEventListener(Action<PlayerModel> action)
    {
        _updateEvent -= action;
    }

    // 通知
    private void UpdateInfo() => _updateEvent?.Invoke(_instance);

    public void Init()
    {
        _playerName = PlayerPrefs.GetString("PlayerName", "黄先生");
        _lev = PlayerPrefs.GetInt("PlayerLev", 1);
        _money = PlayerPrefs.GetInt("PlayerMoney", 0);
        _gem = PlayerPrefs.GetInt("PlayerGem", 0);
        _power = PlayerPrefs.GetInt("PlayerPrefs", 100);

        _hp = PlayerPrefs.GetInt("PlayerHp", 100);
        _atk = PlayerPrefs.GetInt("PlayerAtk", 100);
        _def = PlayerPrefs.GetInt("PlayerDef", 0);
        _crit = PlayerPrefs.GetInt("PlayerCrit", 100);
        _miss = PlayerPrefs.GetInt("PlayerMiss", 50);
        _luck = PlayerPrefs.GetInt("PlayerLuck", 0);
    }

    public void LevUp()
    {
        _lev++;
        _hp += 100;
        _atk += 100;
        _def += 20;
        _crit += _lev * 50;
        _miss += 50;
        _luck += 10;

        // 改变后保存
        SaveData();
    }

    public void SaveData()
    {
        PlayerPrefs.SetString("PlayerName", _playerName);
        PlayerPrefs.SetInt("PlayerMoney", _money);
        PlayerPrefs.SetInt("PlayerGem", _gem);
        PlayerPrefs.SetInt("PlayerPower", _power);

        PlayerPrefs.SetInt("PlayerLev", _lev);
        PlayerPrefs.SetInt("PlayerHp", _hp);
        PlayerPrefs.SetInt("PlayerAtk", _atk);
        PlayerPrefs.SetInt("PlayerDef", _def);
        PlayerPrefs.SetInt("PlayerCrit", _crit);
        PlayerPrefs.SetInt("PlayerMiss", _miss);
        PlayerPrefs.SetInt("PlayerLuck", _luck);

        // 保存完通知
        UpdateInfo();
    }
}
```



## 控制
### MainController.cs
挂在和MainView同一个对象上
```csharp
public class MainController : MonoBehaviour
{
    // 管理的MainView面板
    private MainView _mainView;

    private static MainController _controller;
    public static MainController Controller => _controller;

    // 负责面板的显隐
    // 按下M会执行此函数, 但是不需要创建多个MainPanel, 所以有个if判断
    public static void ShowMe()
    {
        if (_controller == null)
        {
            // 主面板的实例化, 并设置父对象
            GameObject res = Resources.Load<GameObject>("UI/MainPanel");
            GameObject obj = Instantiate(res);
            obj.transform.SetParent(GameObject.Find("Canvas").transform, false);

            _controller = obj.GetComponent<MainController>();
        }
        _controller.gameObject.SetActive(true);
    }
    public static void HideMe() => _controller?.gameObject.SetActive(false);

    private void Start()
    {
        // 获取同样挂载在一个对象(MainPanel)的MainView脚本对象, 来进行界面更新
        _mainView = this.GetComponent<MainView>();
        // 第一次的MainView更新
        _mainView.UpdateInfo(PlayerModel.Instance);

        _mainView.btnRole.onClick.AddListener(() =>
        {
            RoleController.ShowMe();
        });
        // 如果数据层发生更新就会执行UpdateInfo函数, 然后进行界面的更新
        PlayerModel.Instance.AddEventListener(UpdateInfo);
    }

    // 界面的更新
    private void UpdateInfo(PlayerModel data) => _mainView?.UpdateInfo(data);


    // 当挂载的对象没了之后要把在数据层的监听给删掉
    void OnDestroy()
    {
        PlayerModel.Instance.MoveEventListener(UpdateInfo);
    }
}
```


### RoleController.cs
挂载在和RoleView面板上
```csharp
public class RoleController : MonoBehaviour
{
    // 管理的角色面板
    private RoleView _roleView;
    private static RoleController _controller = null;
    public static RoleController Controller => _controller;

    // 角色面板的显隐
    // 点击角色按钮执行此函数, 但是不需要创建多个RolePanel, 所以有个if判断
    public static void ShowMe()
    {
        if (_controller == null)
        {
            // 角色面板的实例化, 并且设置父对象
            GameObject res = Resources.Load<GameObject>("UI/RolePanel");
            GameObject obj = Instantiate(res);
            obj.transform.SetParent(GameObject.Find("Canvas").transform, false);

            _controller = obj.GetComponent<RoleController>();
        }
        _controller.gameObject.SetActive(true);
    }
    public static void HideMe() => _controller?.gameObject.SetActive(false);

    void Start()
    {
        _roleView = this.GetComponent<RoleView>();
        // 第一次更新角色面板
        _roleView.UpdateInfo(PlayerModel.Instance);

        _roleView.btnClose.onClick.AddListener(() =>
        {
            HideMe();
        });
        _roleView.btnLevUp.onClick.AddListener(() =>
        {
            // 使用数据模块来升级    
            PlayerModel.Instance.LevUp();
        });

        // 数据层更新后界面层的更新
        PlayerModel.Instance.AddEventListener(UpdateInfo);
    }

    // 界面的更新
    private void UpdateInfo(PlayerModel data) => _roleView?.UpdateInfo(data);

    // 当挂载的对象没了之后要把在数据层的监听给删掉
    void OnDestroy()
    {
        PlayerModel.Instance.MoveEventListener(UpdateInfo);
    }
}
```