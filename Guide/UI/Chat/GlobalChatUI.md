
## Unity UI Toolkit: Dynamic Chat System

https://prnt.sc/iAAB1SgeeC7G folder Scripts Chat   

https://prnt.sc/y_uM7xr7img9 folder UI Elements

https://prnt.sc/PWaGY6Ye82DT class ChatWindow method createTabs

 
```csharp
 protected override void LoadAssets() {
     _windowTemplate = LoadAsset("Data/UI/_Elements/Game/Chat/ChatWindow");
     _tabTemplate = LoadAsset("Data/UI/_Elements/Game/Chat/ChatTab");
     _tabHeaderTemplate = LoadAsset("Data/UI/_Elements/Game/Chat/ChatTabHeader");
     _messageLabelTemplate = LoadAsset("Data/UI/_Elements/Game/Chat/MessageLabelTemplate");
 }
```
 

The logic is simple. The class takes UI elements from the folder and, with each request, makes a copy of these elements and then inserts them into each other. It's like in JS, you can find an element in a form and insert your own into it.

 

### Here is a good example:
```csharp
VisualElement tabHeaderContainer = _chatTabView.Q<VisualElement>("tab-header-container");
 ```

take the container element for tabs and insert into it as many tabs as we want

 
```csharp
_tabs <- We set the quantity in the Unity editor and set the tab names there too
 ```
 ```csharp
VisualElement tabElement = _tabTemplate.CloneTree()[0]; <- We take the ui element from the folder, make a copy of it and insert it into 

tabContainer.Add(tabElement);

  ```

We insert a new tab. You can make 2-3 or even 10 copies of these, as long as they fit. And the tabs themselves are where the listeners are, listening to the mouse click.

 
```csharp
        for (int i = 0; i < _tabs.Count; i++) {
            VisualElement tabElement = _tabTemplate.CloneTree()[0];
            // tabElement.name = _tabs[i].TabName;
            tabElement.name = _tabs[i].TabName;
            tabElement.AddToClassList("unselected-tab");

            VisualElement tabHeaderElement = _tabHeaderTemplate.CloneTree()[0];
            tabHeaderElement.name = _tabs[i].TabName;
            tabHeaderElement.Q<Label>().text = _tabs[i].TabName;

            tabHeaderContainer.Add(tabHeaderElement);
            tabContainer.Add(tabElement);

            _tabs[i].SetMessageTemplate(_messageLabelTemplate);
            _tabs[i].Initialize(_windowEle, tabElement, tabHeaderElement);
           
        }
```
 

The base element is called VisualElement. Other elements inherit from it, for example:  
 ```csharp  
TextField->VisualElement   
Label->VisualElement    
```   
So, we can search by element name and return a VisualElement without even knowing it's a    textfield or anything else. We can also search by element class. Element styles can be described    as CSS, but here they're called uss. The L2StyleSheet.uss file    

 
```csharp
.html_normal_text_no_color {
    font-size: 11px;
    margin-top: 0;
    margin-bottom: 0;
    padding-top: 0;
    padding-bottom: 0;
    white-space: normal;
    -unity-text-align: middle-left;
    display: inline;
} 
```