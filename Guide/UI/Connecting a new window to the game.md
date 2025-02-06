1.OpenFolder Assets\Scripts\UI\Game\Action\
2. Create New File ActionWindow.cs
3. 

<blockquote>
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UIElements;

public class ActionWindow : L2PopupWindow
{
    private const int SLOTS_PER_ROW = 8;
    private VisualTreeAsset _slotTemplate;
    private VisualElement _basicContainer;
    private VisualElement _partyContainer;
    private VisualElement _tokenContainer;
    private VisualElement _socialContainer;

    private static ActionWindow _instance;
    public static ActionWindow Instance { get { return _instance; } }

    private void Awake()
    {
        if (_instance == null)
        {
            _instance = this;
        }
        else
        {
            Destroy(this);
        }
    }

    private void OnDestroy()
    {
        _instance = null;
    }

    protected override void LoadAssets()
    {
        _windowTemplate = LoadAsset("Data/UI/_Elements/Game/ActionWindow");
    }

    protected override void InitWindow(VisualElement root)
    {
        base.InitWindow(root);

        VisualElement dragArea = GetElementByClass("drag-area");
        DragManipulator drag = new DragManipulator(dragArea, _windowEle);
        dragArea.AddManipulator(drag);

        RegisterCloseWindowEvent("btn-close-frame");
        RegisterClickWindowEvent(_windowEle, dragArea);

        Label _windowName = (Label)GetElementById("windows-name-label");
        _windowName.text = "Actions";

        _basicContainer = GetElementById("BasicSlots");
        _partyContainer = GetElementById("PartySlots");
        _tokenContainer = GetElementById("TokenSlots");
        _socialContainer = GetElementById("SocialSlots");
    }

    protected override IEnumerator BuildWindow(VisualElement root)
    {
        InitWindow(root);
        yield return new WaitForEndOfFrame();
    }

    public override void ShowWindow()
    {
        base.ShowWindow();
        AudioManager.Instance.PlayUISound("window_open");
        L2GameUI.Instance.WindowOpened(this);
    }

    public override void HideWindow()
    {
        base.HideWindow();
        AudioManager.Instance.PlayUISound("window_close");
        L2GameUI.Instance.WindowClosed(this);
    }


}
</blockquote>

We notify Unity where to get the ui layout from
_windowTemplate = LoadAsset("Data/UI/_Elements/Game/ActionWindow"); 


We can find any class or element in the layout to change it
VisualElement dragArea = GetElementByClass("drag-area");

We pass a link to the element into the code and connect the click event
RegisterClickWindowEvent(_windowEle, dragArea);

4. Adding a scene to the hierarchy
![Unity Scene!](https://i.ibb.co/yFQ1dMHT/Game-Scene-1.png) 

5. Adding the ActionWindow class
![Add Window!](https://i.ibb.co/dJmvkXQb/Game-Scene-2.png) 

6. Open File Assets\Scripts\UI\L2GameUI
7. Add code 
		<blockquote>
			if (ActionWindow.Instance != null)
			{
				ActionWindow.Instance.AddWindow(_rootVisualContainer);
				ActionWindow.Instance.HideWindow();
			}
		</blockquote>
8. We call the code when we need to show the window to the user
<blockquote>ActionWindow.Instance.ShowWindow();</blockquote>