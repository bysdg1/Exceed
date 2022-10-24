
-- game:IsLoaded
    if not game:IsLoaded() then 
        repeat game.Loaded:Wait(5)
        until game:IsLoaded() 
    end
-- game:IsLoaded

-- ReJoin
    spawn(function()
        while true do wait()
            getgenv().rejoin = game:GetService("CoreGui").RobloxPromptGui.promptOverlay.ChildAdded:Connect(function(Kick)
                if not _G.TP_Ser and _G.Rejoin then
                    if Kick.Name == 'ErrorPrompt' and Kick:FindFirstChild('MessageArea') and Kick.MessageArea:FindFirstChild("ErrorFrame") then
                        game:GetService("TeleportService"):Teleport(game.PlaceId)
                        wait(50)
                    end
                end
            end)
        end
    end)
-- ReJoin

-- DelayClick
    local VirtualUser=game:service'VirtualUser'
    game:service'Players'.LocalPlayer.Idled:connect(function()
        VirtualUser:CaptureController()
        VirtualUser:ClickButton2(Vector2.new())
    end)

    spawn(function()
        while wait(3) do
            game:GetService'VirtualUser':CaptureController()
        end
    end)
-- DelayClick

-- UI
    local ScreenGui = Instance.new("ScreenGui")
    local ImageButton = Instance.new("ImageButton")

    ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling


    do 
        local GUI = game.CoreGui:FindFirstChild("ExceedHUB");
        if GUI then 
            GUI:Destroy();
        end;
        if _G.Color == nil then
            _G.Color = Color3.fromRGB(255,0,0)
        end 
    end


    local UserInputService = game:GetService("UserInputService")
    local TweenService = game:GetService("TweenService")

    local function MakeDraggable(topbarobject, object)
        local Dragging = nil
        local DragInput = nil
        local DragStart = nil
        local StartPosition = nil

        local function Update(input)
            local Delta = input.Position - DragStart
            local pos =
                UDim2.new(
                    StartPosition.X.Scale,
                    StartPosition.X.Offset + Delta.X,
                    StartPosition.Y.Scale,
                    StartPosition.Y.Offset + Delta.Y
                )
            local Tween = TweenService:Create(object, TweenInfo.new(0.2), {Position = pos})
            Tween:Play()
        end

        topbarobject.InputBegan:Connect(
            function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                    Dragging = true
                    DragStart = input.Position
                    StartPosition = object.Position

                    input.Changed:Connect(
                        function()
                            if input.UserInputState == Enum.UserInputState.End then
                                Dragging = false
                            end
                        end
                    )
                end
            end
        )

        topbarobject.InputChanged:Connect(
            function(input)
                if
                    input.UserInputType == Enum.UserInputType.MouseMovement or
                    input.UserInputType == Enum.UserInputType.Touch
                then
                    DragInput = input
                end
            end
        )

        UserInputService.InputChanged:Connect(
            function(input)
                if input == DragInput and Dragging then
                    Update(input)
                end
            end
        )
    end

    local Update = {}

    function Update:Window(text,logo,keybind)
        local uihide = false
        local abc = false
        local logo = logo or 0
        local currentpage = ""
        local keybind = keybind or Enum.KeyCode.RightControl
        local yoo = string.gsub(tostring(keybind),"Enum.KeyCode.","")
        
        local ExceedHUB = Instance.new("ScreenGui")
        ExceedHUB.Name = "ExceedHUB"
        ExceedHUB.Parent = game.CoreGui
        ExceedHUB.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

        local Main = Instance.new("Frame")
        Main.Name = "Main"
        Main.Parent = ExceedHUB
        Main.ClipsDescendants = true
        Main.AnchorPoint = Vector2.new(0.5,0.5)
        Main.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        Main.BackgroundTransparency = 1.000
        Main.Position = UDim2.new(0.5, 0, 0.47, 0)
        Main.Size = UDim2.new(0, 0, 0, 0)
        
        Main:TweenSize(UDim2.new(0, 803, 0, 590),"Out","Quad",0.4,true)

        local MCNR = Instance.new("UICorner")
        MCNR.Name = "MCNR"
        MCNR.Parent = Main

        local Top = Instance.new("Frame")
        Top.Name = "Top"
        Top.Parent = Main
        Top.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        Top.Size = UDim2.new(0, 803, 0, 27)

        local TCNR = Instance.new("UICorner")
        TCNR.Name = "TCNR"
        TCNR.Parent = Top

        local Name = Instance.new("TextLabel")
        Name.Name = "Name"
        Name.Parent = Top
        Name.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        Name.BackgroundTransparency = 1
        Name.TextXAlignment = Enum.TextXAlignment.Center
        Name.Size = UDim2.new(0, 803, 0, 27)
        Name.Font = Enum.Font.GothamSemibold
        Name.Text = text
        Name.TextColor3 = Color3.fromRGB(225, 225, 225)
        Name.TextSize = 17.000

        local FPS = Instance.new("TextLabel")
        FPS.Name = "FPS"
        FPS.Parent = Top
        FPS.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        FPS.BackgroundTransparency = 1
        FPS.TextXAlignment = Enum.TextXAlignment.Center
        FPS.Size = UDim2.new(0, 100, 0, 27)
        FPS.Position = UDim2.new(0.7, 0, 0, 0)
        FPS.Font = Enum.Font.GothamSemibold
        FPS.Text = "FPS : N/A"
        FPS.TextColor3 = Color3.fromRGB(225, 225, 225)
        FPS.TextSize = 17.000

        spawn(function()
            while wait() do
                pcall(function()
                    local Stats = game:GetService("Stats")
                    local FrameRateManager = Stats and Stats:FindFirstChild("FrameRateManager")
                    local RenderAverage = FrameRateManager and FrameRateManager:FindFirstChild("RenderAverage")
                    TextFPS = math.floor(1000/RenderAverage:GetValue())
                    FPS.Text = "FPS : "..TextFPS
                    if TextFPS < 10 then
                        HopLowServer()
                        FPS.TextColor3 = Color3.fromRGB(255, 0, 0)
                    else
                        FPS.TextColor3 = Color3.fromRGB(0, 255, 0)
                    end
                end)
            end
        end)

        local BindButton = Instance.new("TextButton")
        BindButton.Name = "BindButton"
        BindButton.Parent = Top
        BindButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        BindButton.BackgroundTransparency = 1.000
        BindButton.Position = UDim2.new(0.847561002, 0, 0, 0)
        BindButton.Size = UDim2.new(0, 100, 0, 27)
        BindButton.Font = Enum.Font.GothamSemibold
        BindButton.Text = "[ "..string.gsub(tostring(keybind),"Enum.KeyCode.","").." ]"
        BindButton.TextColor3 = Color3.fromRGB(100, 100, 100)
        BindButton.TextSize = 11.000
        
        BindButton.MouseButton1Click:Connect(function ()
            BindButton.Text = "[ ... ]"
            local inputwait = game:GetService("UserInputService").InputBegan:wait()
            local shiba = inputwait.KeyCode == Enum.KeyCode.Unknown and inputwait.UserInputType or inputwait.KeyCode

            if shiba.Name ~= "Focus" and shiba.Name ~= "MouseMovement" then
                BindButton.Text = "[ "..shiba.Name.." ]"
                yoo = shiba.Name
            end
        end)

        local Tab = Instance.new("Frame")
        Tab.Name = "Tab"
        Tab.Parent = Main
        Tab.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        Tab.BackgroundTransparency = 1
        Tab.Position = UDim2.new(0, 5, 0, 4)
        Tab.Size = UDim2.new(0, 250, 0, 20)

        local TCNR = Instance.new("UICorner")
        TCNR.Name = "TCNR"
        TCNR.Parent = Tab

        local ScrollTab = Instance.new("ScrollingFrame")
        ScrollTab.Name = "ScrollTab"
        ScrollTab.Parent = Tab
        ScrollTab.Active = true
        ScrollTab.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        ScrollTab.BackgroundTransparency = 1
        ScrollTab.Size = UDim2.new(0, 350, 0, 20)
        ScrollTab.ScrollingDirection = Enum.ScrollingDirection.X
        ScrollTab.CanvasSize = UDim2.new(0, 0, 0, 0)
        ScrollTab.ScrollBarThickness = 1

        local PLL = Instance.new("UIListLayout")
        PLL.Name = "PLL"
        PLL.Parent = ScrollTab
        PLL.FillDirection = Enum.FillDirection.Horizontal
        PLL.SortOrder = Enum.SortOrder.LayoutOrder
        PLL.VerticalAlignment = Enum.VerticalAlignment.Center
        PLL.Padding = UDim.new(0, 0)

        local PPD = Instance.new("UIPadding")
        PPD.Name = "PPD"
        PPD.Parent = ScrollTab
        PPD.PaddingLeft = UDim.new(0, 0)
        PPD.PaddingTop = UDim.new(0, -25)

        local Page = Instance.new("Frame")
        Page.Name = "Page"
        Page.Parent = Main
        Page.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        Page.BackgroundTransparency = 0.3
        Page.Position = UDim2.new(0.0, 0, 0.055000003, 0)
        Page.Size = UDim2.new(0, 200, 0, 554)

        local PCNR = Instance.new("UICorner")
        PCNR.Name = "PCNR"
        PCNR.Parent = Page

        local Page1 = Instance.new("Frame")
        Page1.Name = "Page1"
        Page1.Parent = Main
        Page1.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        Page1.BackgroundTransparency = 0.3
        Page1.Position = UDim2.new(0.251, 0, 0.055000003, 0)
        Page1.Size = UDim2.new(0, 200, 0, 554)

        local PCNR1 = Instance.new("UICorner")
        PCNR1.Name = "PCNR1"
        PCNR1.Parent = Page1

        local Page2 = Instance.new("Frame")
        Page2.Name = "Page2"
        Page2.Parent = Main
        Page2.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        Page2.BackgroundTransparency = 0.3
        Page2.Position = UDim2.new(0.501, 0, 0.055000003, 0)
        Page2.Size = UDim2.new(0, 200, 0, 554)

        local PCNR2 = Instance.new("UICorner")
        PCNR2.Name = "PCNR2"
        PCNR2.Parent = Page2

        local Page3 = Instance.new("Frame")
        Page3.Name = "Page3"
        Page3.Parent = Main
        Page3.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        Page3.BackgroundTransparency = 0.3
        Page3.Position = UDim2.new(0.751, 0, 0.055000003, 0)
        Page3.Size = UDim2.new(0, 200, 0, 554)

        local PCNR3 = Instance.new("UICorner")
        PCNR3.Name = "PCNR3"
        PCNR3.Parent = Page3

        local MainPage = Instance.new("Frame")
        MainPage.Name = "MainPage"
        MainPage.Parent = Page
        MainPage.ClipsDescendants = true
        MainPage.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        MainPage.BackgroundTransparency = 1.000
        MainPage.Size = UDim2.new(0, 200, 0, 554)

        local PageList = Instance.new("Folder")
        PageList.Name = "PageList"
        PageList.Parent = MainPage

        local MainPage1 = Instance.new("Frame")
        MainPage1.Name = "MainPage1"
        MainPage1.Parent = Page1
        MainPage1.ClipsDescendants = true
        MainPage1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        MainPage1.BackgroundTransparency = 1.000
        MainPage1.Size = UDim2.new(0, 200, 0, 554)

        local PageList1 = Instance.new("Folder")
        PageList1.Name = "PageList1"
        PageList1.Parent = MainPage1

        local MainPage2 = Instance.new("Frame")
        MainPage2.Name = "MainPage2"
        MainPage2.Parent = Page2
        MainPage2.ClipsDescendants = true
        MainPage2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        MainPage2.BackgroundTransparency = 1.000
        MainPage2.Size = UDim2.new(0, 200, 0, 554)

        local PageList2 = Instance.new("Folder")
        PageList2.Name = "PageList2"
        PageList2.Parent = MainPage2

        local MainPage3 = Instance.new("Frame")
        MainPage3.Name = "MainPage3"
        MainPage3.Parent = Page3
        MainPage3.ClipsDescendants = true
        MainPage3.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        MainPage3.BackgroundTransparency = 1.000
        MainPage3.Size = UDim2.new(0, 200, 0, 554)

        local PageList3 = Instance.new("Folder")
        PageList3.Name = "PageList3"
        PageList3.Parent = MainPage3

        local UIPageLayout = Instance.new("UIPageLayout")

        UIPageLayout.Parent = PageList
        UIPageLayout.SortOrder = Enum.SortOrder.LayoutOrder
        UIPageLayout.EasingDirection = Enum.EasingDirection.InOut
        UIPageLayout.EasingStyle = Enum.EasingStyle.Quad
        UIPageLayout.FillDirection = Enum.FillDirection.Vertical
        UIPageLayout.Padding = UDim.new(0, 15)
        UIPageLayout.TweenTime = 0.400
        UIPageLayout.GamepadInputEnabled = false
        UIPageLayout.ScrollWheelInputEnabled = false
        UIPageLayout.TouchInputEnabled = false

        local UIPageLayout1 = Instance.new("UIPageLayout")

        UIPageLayout1.Parent = PageList1
        UIPageLayout1.SortOrder = Enum.SortOrder.LayoutOrder
        UIPageLayout1.EasingDirection = Enum.EasingDirection.InOut
        UIPageLayout1.EasingStyle = Enum.EasingStyle.Quad
        UIPageLayout1.FillDirection = Enum.FillDirection.Vertical
        UIPageLayout1.Padding = UDim.new(0, 15)
        UIPageLayout1.TweenTime = 0.400
        UIPageLayout1.GamepadInputEnabled = false
        UIPageLayout1.ScrollWheelInputEnabled = false
        UIPageLayout1.TouchInputEnabled = false

        local UIPageLayout2 = Instance.new("UIPageLayout")

        UIPageLayout2.Parent = PageList2
        UIPageLayout2.SortOrder = Enum.SortOrder.LayoutOrder
        UIPageLayout2.EasingDirection = Enum.EasingDirection.InOut
        UIPageLayout2.EasingStyle = Enum.EasingStyle.Quad
        UIPageLayout2.FillDirection = Enum.FillDirection.Vertical
        UIPageLayout2.Padding = UDim.new(0, 15)
        UIPageLayout2.TweenTime = 0.400
        UIPageLayout2.GamepadInputEnabled = false
        UIPageLayout2.ScrollWheelInputEnabled = false
        UIPageLayout2.TouchInputEnabled = false

        local UIPageLayout3 = Instance.new("UIPageLayout")

        UIPageLayout3.Parent = PageList3
        UIPageLayout3.SortOrder = Enum.SortOrder.LayoutOrder
        UIPageLayout3.EasingDirection = Enum.EasingDirection.InOut
        UIPageLayout3.EasingStyle = Enum.EasingStyle.Quad
        UIPageLayout3.FillDirection = Enum.FillDirection.Vertical
        UIPageLayout3.Padding = UDim.new(0, 15)
        UIPageLayout3.TweenTime = 0.400
        UIPageLayout3.GamepadInputEnabled = false
        UIPageLayout3.ScrollWheelInputEnabled = false
        UIPageLayout3.TouchInputEnabled = false
        
        MakeDraggable(Top,Main)

        UserInputService.InputBegan:Connect(function(input)
            if input.KeyCode == Enum.KeyCode[yoo] then
                if uihide == false then
                    uihide = true
                    Main:TweenSize(UDim2.new(0, 0, 0, 0),"In","Quad",0.4,true)
                else
                    uihide = false
                    Main:TweenSize(UDim2.new(0, 803, 0, 590),"Out","Quad",0.4,true)
                end
            end
        end)
        
        local uitab = {}
        
        function uitab:TabRight(text)
            local TabButton = Instance.new("TextButton")
            TabButton.Parent = ScrollTab
            TabButton.Name = text.."Server"
            TabButton.Text = text
            TabButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            TabButton.BackgroundTransparency = 1
            TabButton.Size = UDim2.new(0, 50, 0, 23)
            TabButton.Font = Enum.Font.GothamSemibold
            TabButton.TextColor3 = Color3.fromRGB(225, 225, 225)
            TabButton.TextSize = 11.000
            TabButton.TextTransparency = 0.500

            local MainFramePage = Instance.new("ScrollingFrame")
            MainFramePage.Name = text.."_Page"
            MainFramePage.Parent = PageList3
            MainFramePage.Active = true
            MainFramePage.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
            MainFramePage.BackgroundTransparency = 1.000
            MainFramePage.BorderSizePixel = 0
            MainFramePage.Size = UDim2.new(0, 200, 0, 554)
            MainFramePage.CanvasSize = UDim2.new(0, 0, 0, 0)
            MainFramePage.ScrollBarThickness = 0
            
            local UIPadding = Instance.new("UIPadding")
            local UIListLayout = Instance.new("UIListLayout")
            
            UIPadding.Parent = MainFramePage
            UIPadding.PaddingLeft = UDim.new(0, 10)
            UIPadding.PaddingTop = UDim.new(0, 10)

            UIListLayout.Padding = UDim.new(0, 0)
            UIListLayout.Parent = MainFramePage
            UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
            
            TabButton.MouseButton1Click:Connect(function()
                for i,v in next, ScrollTab:GetChildren() do
                    if v:IsA("TextButton") then
                        TweenService:Create(
                            v,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {TextTransparency = 0}
                        ):Play()
                        v.TextColor3 = Color3.fromRGB(150, 150, 150)
                    end
                    TweenService:Create(
                        TabButton,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {TextTransparency = 0}
                    ):Play()
                    TabButton.TextColor3 = _G.Color
                end
                for i,v in next, PageList3:GetChildren() do
                    currentpage = string.gsub(TabButton.Name,"Server","").."_Page"
                    if v.Name == currentpage then
                        UIPageLayout3:JumpTo(v)
                    end
                end
            end)

            if abc == false then
                for i,v in next, ScrollTab:GetChildren() do
                    if v:IsA("TextButton") then
                        TweenService:Create(
                            v,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {TextTransparency = 0.5}
                        ):Play()
                        v.TextColor3 = Color3.fromRGB(150, 150, 150)
                    end
                    TweenService:Create(
                        TabButton,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {TextTransparency = 0}
                    ):Play()
                    TabButton.TextColor3 = _G.Color
                end
                UIPageLayout3:JumpToIndex(1)
                abc = true
            end
            
            game:GetService("RunService").Stepped:Connect(function()
                pcall(function()
                    MainFramePage.CanvasSize = UDim2.new(0,0,0,UIListLayout.AbsoluteContentSize.Y + 20)
                    ScrollTab.CanvasSize = UDim2.new(0,0,0,PLL.AbsoluteContentSize.Y + 20)
                end)
            end)
            
            local main = {}
            local main1 = {}
            function main:Button(text,callback)
                local Button = Instance.new("Frame")
                local UICorner = Instance.new("UICorner")
                local TextBtn = Instance.new("TextButton")
                local UICorner_2 = Instance.new("UICorner")
                local Black = Instance.new("Frame")
                local UICorner_3 = Instance.new("UICorner")
                
                Button.Name = "Button"
                Button.Parent = MainFramePage
                Button.BackgroundColor3 = _G.Color
                Button.Size = UDim2.new(0, 180, 0, 20)
                
                UICorner.CornerRadius = UDim.new(0, 5)
                UICorner.Parent = Button
                
                TextBtn.Name = "TextBtn"
                TextBtn.Parent = Button
                TextBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
                TextBtn.Position = UDim2.new(0, 0, 0, 1)
                TextBtn.Size = UDim2.new(0, 180, 0, 20)
                TextBtn.AutoButtonColor = true
                TextBtn.Font = Enum.Font.GothamSemibold
                TextBtn.Text = text
                TextBtn.TextColor3 = Color3.fromRGB(225, 225, 225)
                TextBtn.TextSize = 10.000
                
                UICorner_2.CornerRadius = UDim.new(0, 5)
                UICorner_2.Parent = TextBtn
                
                Black.Name = "Black"
                Black.Parent = Button
                Black.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                Black.BackgroundTransparency = 1.000
                Black.BorderSizePixel = 0
                Black.Position = UDim2.new(0, 0, 0, 1)
                Black.Size = UDim2.new(0, 180, 0, 20)
                
                UICorner_3.CornerRadius = UDim.new(0, 5)
                UICorner_3.Parent = Black

                TextBtn.MouseEnter:Connect(function()
                    TweenService:Create(
                        Black,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {BackgroundTransparency = 0.7}
                    ):Play()
                end)
                TextBtn.MouseLeave:Connect(function()
                    TweenService:Create(
                        Black,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {BackgroundTransparency = 1}
                    ):Play()
                end)
                TextBtn.MouseButton1Click:Connect(function()
                    TextBtn.TextSize = 0
                    TweenService:Create(
                        TextBtn,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {TextSize = 10}
                    ):Play()
                    callback()
                end)
            end
            function main:Toggle(text,config,callback)
                config = config or false
                local toggled = config
                local Toggle = Instance.new("Frame")
                local UICorner = Instance.new("UICorner")
                local Button = Instance.new("TextButton")
                local UICorner_2 = Instance.new("UICorner")
                local Label = Instance.new("TextLabel")
                local ToggleImage = Instance.new("Frame")
                local UICorner_3 = Instance.new("UICorner")
                local Circle = Instance.new("Frame")
                local UICorner_4 = Instance.new("UICorner")

                Toggle.Name = "Toggle"
                Toggle.Parent = MainFramePage
                Toggle.BackgroundColor3 = _G.Color
                Toggle.Size = UDim2.new(0, 180, 0, 20)

                UICorner.CornerRadius = UDim.new(0, 5)
                UICorner.Parent = Toggle

                Button.Name = "Button"
                Button.Parent = Toggle
                Button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
                Button.Position = UDim2.new(0, 0, 0, 1)
                Button.Size = UDim2.new(0, 180, 0, 20)
                Button.AutoButtonColor = false
                Button.Font = Enum.Font.SourceSans
                Button.Text = ""
                Button.TextColor3 = Color3.fromRGB(0, 0, 0)
                Button.TextSize = 11.000

                UICorner_2.CornerRadius = UDim.new(0, 5)
                UICorner_2.Parent = Button

                Label.Name = "Label"
                Label.Parent = Toggle
                Label.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Label.BackgroundTransparency = 1.000
                Label.Position = UDim2.new(0, 1, 0, 1)
                Label.Size = UDim2.new(0, 180, 0, 20)
                Label.Font = Enum.Font.GothamSemibold
                Label.Text = text
                Label.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label.TextSize = 10.000

                ToggleImage.Name = "ToggleImage"
                ToggleImage.Parent = Toggle
                ToggleImage.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                ToggleImage.Position = UDim2.new(0, 140, 0, 5)
                ToggleImage.Size = UDim2.new(0, 30, 0, 10)

                UICorner_3.CornerRadius = UDim.new(0, 10)
                UICorner_3.Parent = ToggleImage

                Circle.Name = "Circle"
                Circle.Parent = ToggleImage
                Circle.BackgroundColor3 = Color3.fromRGB(227, 60, 60)
                Circle.Position = UDim2.new(0, 0, 0, 0)
                Circle.Size = UDim2.new(0, 13, 0, 13)

                UICorner_4.CornerRadius = UDim.new(0, 10)
                UICorner_4.Parent = Circle

                Button.MouseButton1Click:Connect(function()
                    if toggled == false then
                        toggled = true
                        Circle:TweenPosition(UDim2.new(0,20,0,-1),"Out","Sine",0.2,true)
                        TweenService:Create(
                            Circle,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {BackgroundColor3 = _G.Color}
                        ):Play()
                    else
                        toggled = false
                        Circle:TweenPosition(UDim2.new(0,0,0,-1),"Out","Sine",0.2,true)
                        TweenService:Create(
                            Circle,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {BackgroundColor3 = Color3.fromRGB(227, 60, 110)}
                        ):Play()
                    end
                    pcall(callback,toggled)
                end)

                if config == true then
                    toggled = true
                    Circle:TweenPosition(UDim2.new(0,20,0,-1),"Out","Sine",0.4,true)
                    TweenService:Create(
                        Circle,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {BackgroundColor3 = _G.Color}
                    ):Play()
                    pcall(callback,toggled)
                end
            end
            function main:Dropdown(text,option,callback)
                local isdropping = false
                local Dropdown = Instance.new("Frame")
                local UICorner = Instance.new("UICorner")
                local DropTitle = Instance.new("TextLabel")
                local DropScroll = Instance.new("ScrollingFrame")
                local UIListLayout = Instance.new("UIListLayout")
                local UIPadding = Instance.new("UIPadding")
                local DropButton = Instance.new("TextButton")
                local DropImage = Instance.new("ImageLabel")
                
                Dropdown.Name = "Dropdown"
                Dropdown.Parent = MainFramePage
                Dropdown.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
                Dropdown.ClipsDescendants = true
                Dropdown.Size = UDim2.new(0, 180, 0, 20)
                
                UICorner.CornerRadius = UDim.new(0, 5)
                UICorner.Parent = Dropdown
                
                DropTitle.Name = "DropTitle"
                DropTitle.Parent = Dropdown
                DropTitle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                DropTitle.BackgroundTransparency = 1.000
                DropTitle.Size = UDim2.new(0, 180, 0, 20)
                DropTitle.Font = Enum.Font.GothamSemibold
                DropTitle.Text = text.. " : "
                DropTitle.TextColor3 = Color3.fromRGB(225, 225, 225)
                DropTitle.TextSize = 10.000
                
                DropScroll.Name = "DropScroll"
                DropScroll.Parent = DropTitle
                DropScroll.Active = true
                DropScroll.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                DropScroll.BackgroundTransparency = 1.000
                DropScroll.BorderSizePixel = 0
                DropScroll.Position = UDim2.new(0, 0, 0, 20)
                DropScroll.Size = UDim2.new(0, 180, 0, 100)
                DropScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
                DropScroll.ScrollBarThickness = 3
                
                UIListLayout.Parent = DropScroll
                UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
                UIListLayout.Padding = UDim.new(0, 5)
                
                UIPadding.Parent = DropScroll
                UIPadding.PaddingLeft = UDim.new(0, 5)
                UIPadding.PaddingTop = UDim.new(0, 5)
                
                DropImage.Name = "DropImage"
                DropImage.Parent = Dropdown
                DropImage.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                DropImage.BackgroundTransparency = 1.000
                DropImage.Position = UDim2.new(0, 160, 0, 4)
                DropImage.Rotation = 180.000
                DropImage.Size = UDim2.new(0, 15, 0, 15)
                DropImage.Image = "rbxassetid://6031090990"
                
                DropButton.Name = "DropButton"
                DropButton.Parent = Dropdown
                DropButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                DropButton.BackgroundTransparency = 1.000
                DropButton.Size = UDim2.new(0, 180, 0, 20)
                DropButton.Font = Enum.Font.SourceSans
                DropButton.Text = ""
                DropButton.TextColor3 = Color3.fromRGB(0, 0, 0)
                DropButton.TextSize = 10.000

                for i,v in next,option do
                    local Item = Instance.new("TextButton")

                    Item.Name = "Item"
                    Item.Parent = DropScroll
                    Item.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    Item.BackgroundTransparency = 1.000
                    Item.Size = UDim2.new(0, 180, 0, 20)
                    Item.Font = Enum.Font.GothamSemibold
                    Item.Text = tostring(v)
                    Item.TextColor3 = Color3.fromRGB(225, 225, 225)
                    Item.TextSize = 10.000
                    Item.TextTransparency = 0.500

                    Item.MouseEnter:Connect(function()
                        TweenService:Create(
                            Item,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {TextTransparency = 0}
                        ):Play()
                    end)

                    Item.MouseLeave:Connect(function()
                        TweenService:Create(
                            Item,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {TextTransparency = 0.5}
                        ):Play()
                    end)

                    Item.MouseButton1Click:Connect(function()
                        isdropping = false
                        Dropdown:TweenSize(UDim2.new(0,180,0,20),"Out","Quad",0.3,true)
                        TweenService:Create(
                            DropImage,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {Rotation = 180}
                        ):Play()
                        callback(Item.Text)
                        DropTitle.Text = Item.Text
                    end)
                end

                DropScroll.CanvasSize = UDim2.new(0,0,0,UIListLayout.AbsoluteContentSize.Y + 10)

                DropButton.MouseButton1Click:Connect(function()
                    if isdropping == false then
                        isdropping = true
                        Dropdown:TweenSize(UDim2.new(0,180,0,120),"Out","Quad",0.3,true)
                        TweenService:Create(
                            DropImage,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {Rotation = 0}
                        ):Play()
                    else
                        isdropping = false
                        Dropdown:TweenSize(UDim2.new(0,180,0,20),"Out","Quad",0.3,true)
                        TweenService:Create(
                            DropImage,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {Rotation = 180}
                        ):Play()
                    end
                end)

                local dropfunc = {}
                function dropfunc:Add(t)
                    local Item = Instance.new("TextButton")
                    Item.Name = "Item"
                    Item.Parent = DropScroll
                    Item.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    Item.BackgroundTransparency = 1.000
                    Item.Size = UDim2.new(0, 180, 0, 20)
                    Item.Font = Enum.Font.GothamSemibold
                    Item.Text = tostring(t)
                    Item.TextColor3 = Color3.fromRGB(225, 225, 225)
                    Item.TextSize = 10.000
                    Item.TextTransparency = 0.500

                    Item.MouseEnter:Connect(function()
                        TweenService:Create(
                            Item,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {TextTransparency = 0}
                        ):Play()
                    end)

                    Item.MouseLeave:Connect(function()
                        TweenService:Create(
                            Item,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {TextTransparency = 0.5}
                        ):Play()
                    end)

                    Item.MouseButton1Click:Connect(function()
                        isdropping = false
                        Dropdown:TweenSize(UDim2.new(0,180,0,20),"Out","Quad",0.3,true)
                        TweenService:Create(
                            DropImage,
                            TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                            {Rotation = 180}
                        ):Play()
                        callback(Item.Text)
                        DropTitle.Text = Item.Text
                    end)
                end
                function dropfunc:Clear()
                    DropTitle.Text = tostring(text).." : "
                    isdropping = false
                    Dropdown:TweenSize(UDim2.new(0,180,0,20),"Out","Quad",0.3,true)
                    TweenService:Create(
                        DropImage,
                        TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),
                        {Rotation = 180}
                    ):Play()
                    for i,v in next, DropScroll:GetChildren() do
                        if v:IsA("TextButton") then
                            v:Destroy()
                        end
                    end
                end
                return dropfunc
            end

            function main:Slider(text,min,max,set,callback)
                local Slider = Instance.new("Frame")
                local slidercorner = Instance.new("UICorner")
                local sliderr = Instance.new("Frame")
                local sliderrcorner = Instance.new("UICorner")
                local SliderLabel = Instance.new("TextLabel")
                local HAHA = Instance.new("Frame")
                local AHEHE = Instance.new("TextButton")
                local bar = Instance.new("Frame")
                local bar1 = Instance.new("Frame")
                local bar1corner = Instance.new("UICorner")
                local barcorner = Instance.new("UICorner")
                local circlebar = Instance.new("Frame")
                local UICorner = Instance.new("UICorner")
                local slidervalue = Instance.new("Frame")
                local valuecorner = Instance.new("UICorner")
                local TextBox = Instance.new("TextBox")
                local UICorner_2 = Instance.new("UICorner")

                Slider.Name = "Slider"
                Slider.Parent = MainFramePage
                Slider.BackgroundColor3 = _G.Color
                Slider.BackgroundTransparency = 0
                Slider.Size = UDim2.new(0, 180, 0, 35)

                slidercorner.CornerRadius = UDim.new(0, 5)
                slidercorner.Name = "slidercorner"
                slidercorner.Parent = Slider

                sliderr.Name = "sliderr"
                sliderr.Parent = Slider
                sliderr.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
                sliderr.Position = UDim2.new(0, 0, 0, 1)
                sliderr.Size = UDim2.new(0, 180, 0, 35)

                sliderrcorner.CornerRadius = UDim.new(0, 5)
                sliderrcorner.Name = "sliderrcorner"
                sliderrcorner.Parent = sliderr

                SliderLabel.Name = "SliderLabel"
                SliderLabel.Parent = sliderr
                SliderLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                SliderLabel.BackgroundTransparency = 1.000
                SliderLabel.Position = UDim2.new(0, 15, 0, 0)
                SliderLabel.Size = UDim2.new(0, 160, 0, 26)
                SliderLabel.Font = Enum.Font.GothamSemibold
                SliderLabel.Text = text
                SliderLabel.TextColor3 = Color3.fromRGB(225, 225, 225)
                SliderLabel.TextSize = 10.000
                SliderLabel.TextTransparency = 0
                SliderLabel.TextXAlignment = Enum.TextXAlignment.Left

                HAHA.Name = "HAHA"
                HAHA.Parent = sliderr
                HAHA.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                HAHA.BackgroundTransparency = 1.000
                HAHA.Size = UDim2.new(0, 180, 0, 29)

                AHEHE.Name = "AHEHE"
                AHEHE.Parent = sliderr
                AHEHE.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                AHEHE.BackgroundTransparency = 1.000
                AHEHE.Position = UDim2.new(0, 10, 0, 22)
                AHEHE.Size = UDim2.new(0, 160, 0, 5)
                AHEHE.Font = Enum.Font.SourceSans
                AHEHE.Text = ""
                AHEHE.TextColor3 = Color3.fromRGB(0, 0, 0)
                AHEHE.TextSize = 14.000

                bar.Name = "bar"
                bar.Parent = AHEHE
                bar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
                bar.Size = UDim2.new(0, 160, 0, 5)

                bar1.Name = "bar1"
                bar1.Parent = bar
                bar1.BackgroundColor3 = _G.Color
                bar1.BackgroundTransparency = 0
                bar1.Size = UDim2.new(set/max, 0, 0, 5)

                bar1corner.CornerRadius = UDim.new(0, 5)
                bar1corner.Name = "bar1corner"
                bar1corner.Parent = bar1

                barcorner.CornerRadius = UDim.new(0, 5)
                barcorner.Name = "barcorner"
                barcorner.Parent = bar

                circlebar.Name = "circlebar"
                circlebar.Parent = bar1
                circlebar.BackgroundColor3 = Color3.fromRGB(225, 225, 225)
                circlebar.Position = UDim2.new(1, -2, 0, -3)
                circlebar.Size = UDim2.new(0, 10, 0, 10)

                UICorner.CornerRadius = UDim.new(0, 100)
                UICorner.Parent = circlebar

                slidervalue.Name = "slidervalue"
                slidervalue.Parent = sliderr
                slidervalue.BackgroundColor3 = _G.Color
                slidervalue.BackgroundTransparency = 0
                slidervalue.Position = UDim2.new(0, 395, 0, 5)
                slidervalue.Size = UDim2.new(0, 65, 0, 18)

                valuecorner.CornerRadius = UDim.new(0, 5)
                valuecorner.Name = "valuecorner"
                valuecorner.Parent = slidervalue

                TextBox.Parent = slidervalue
                TextBox.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
                TextBox.Position = UDim2.new(0, 1, 0, 1)
                TextBox.Size = UDim2.new(0, 63, 0, 16)
                TextBox.Font = Enum.Font.GothamSemibold
                TextBox.TextColor3 = Color3.fromRGB(225, 225, 225)
                TextBox.TextSize = 9.000
                TextBox.Text = set
                TextBox.TextTransparency = 0

                UICorner_2.CornerRadius = UDim.new(0, 5)
                UICorner_2.Parent = TextBox

                local mouse = game.Players.LocalPlayer:GetMouse()
                local uis = game:GetService("UserInputService")

                if Value == nil then
                    Value = set
                    pcall(function()
                        callback(Value)
                    end)
                end
                
                AHEHE.MouseButton1Down:Connect(function()
                    Value = math.floor((((tonumber(max) - tonumber(min)) / 448) * bar1.AbsoluteSize.X) + tonumber(min)) or 0
                    pcall(function()
                        callback(Value)
                    end)
                    bar1.Size = UDim2.new(0, math.clamp(mouse.X - bar1.AbsolutePosition.X, 0, 448), 0, 5)
                    circlebar.Position = UDim2.new(0, math.clamp(mouse.X - bar1.AbsolutePosition.X - 2, 0, 438), 0, -3)
                    moveconnection = mouse.Move:Connect(function()
                        TextBox.Text = Value
                        Value = math.floor((((tonumber(max) - tonumber(min)) / 448) * bar1.AbsoluteSize.X) + tonumber(min))
                        pcall(function()
                            callback(Value)
                        end)
                        bar1.Size = UDim2.new(0, math.clamp(mouse.X - bar1.AbsolutePosition.X, 0, 448), 0, 5)
                        circlebar.Position = UDim2.new(0, math.clamp(mouse.X - bar1.AbsolutePosition.X - 2, 0, 438), 0, -3)
                    end)
                    releaseconnection = uis.InputEnded:Connect(function(Mouse)
                        if Mouse.UserInputType == Enum.UserInputType.MouseButton1 then
                            Value = math.floor((((tonumber(max) - tonumber(min)) / 448) * bar1.AbsoluteSize.X) + tonumber(min))
                            pcall(function()
                                callback(Value)
                            end)
                            bar1.Size = UDim2.new(0, math.clamp(mouse.X - bar1.AbsolutePosition.X, 0, 448), 0, 5)
                            circlebar.Position = UDim2.new(0, math.clamp(mouse.X - bar1.AbsolutePosition.X - 2, 0, 438), 0, -3)
                            moveconnection:Disconnect()
                            releaseconnection:Disconnect()
                        end
                    end)
                end)
                releaseconnection = uis.InputEnded:Connect(function(Mouse)
                    if Mouse.UserInputType == Enum.UserInputType.MouseButton1 then
                        Value = math.floor((((tonumber(max) - tonumber(min)) / 448) * bar1.AbsoluteSize.X) + tonumber(min))
                        TextBox.Text = Value
                    end
                end)

                TextBox.FocusLost:Connect(function()
                    if tonumber(TextBox.Text) > max then
                        TextBox.Text  = max
                    end
                    bar1.Size = UDim2.new((TextBox.Text or 0) / max, 0, 0, 5)
                    circlebar.Position = UDim2.new(1, -2, 0, -3)
                    TextBox.Text = tostring(TextBox.Text and math.floor( (TextBox.Text / max) * (max - min) + min) )
                    pcall(callback, TextBox.Text)
                end)
            end

            function main:Textbox(text,disappear,callback)
                local Textbox = Instance.new("Frame")
                local TextboxCorner = Instance.new("UICorner")
                local Textboxx = Instance.new("Frame")
                local TextboxxCorner = Instance.new("UICorner")
                local TextboxLabel = Instance.new("TextLabel")
                local txtbtn = Instance.new("TextButton")
                local RealTextbox = Instance.new("TextBox")
                local UICorner = Instance.new("UICorner")

                Textbox.Name = "Textbox"
                Textbox.Parent = MainFramePage
                Textbox.BackgroundColor3 = _G.Color
                Textbox.BackgroundTransparency = 0
                Textbox.Size = UDim2.new(0, 180, 0, 20)

                TextboxCorner.CornerRadius = UDim.new(0, 5)
                TextboxCorner.Name = "TextboxCorner"
                TextboxCorner.Parent = Textbox

                Textboxx.Name = "Textboxx"
                Textboxx.Parent = Textbox
                Textboxx.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
                Textboxx.Position = UDim2.new(0, 0, 0, 1)
                Textboxx.Size = UDim2.new(0, 180, 0, 20)

                TextboxxCorner.CornerRadius = UDim.new(0, 5)
                TextboxxCorner.Name = "TextboxxCorner"
                TextboxxCorner.Parent = Textboxx

                TextboxLabel.Name = "TextboxLabel"
                TextboxLabel.Parent = Textbox
                TextboxLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                TextboxLabel.BackgroundTransparency = 1.000
                TextboxLabel.Position = UDim2.new(0, 5, 0, 0)
                TextboxLabel.Text = text
                TextboxLabel.Size = UDim2.new(0, 130, 0, 20)
                TextboxLabel.Font = Enum.Font.GothamSemibold
                TextboxLabel.TextColor3 = Color3.fromRGB(225, 225, 225)
                TextboxLabel.TextSize = 10.000
                TextboxLabel.TextTransparency = 0
                TextboxLabel.TextXAlignment = Enum.TextXAlignment.Left

                txtbtn.Name = "txtbtn"
                txtbtn.Parent = Textbox
                txtbtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                txtbtn.BackgroundTransparency = 1.000
                txtbtn.Position = UDim2.new(0, 1, 0, 1)
                txtbtn.Size = UDim2.new(0, 180, 0, 20)
                txtbtn.Font = Enum.Font.SourceSans
                txtbtn.Text = ""
                txtbtn.TextColor3 = Color3.fromRGB(0, 0, 0)
                txtbtn.TextSize = 10.000

                RealTextbox.Name = "RealTextbox"
                RealTextbox.Parent = Textbox
                RealTextbox.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
                RealTextbox.BackgroundTransparency = 0
                RealTextbox.Position = UDim2.new(0, 130, 0, 4)
                RealTextbox.Size = UDim2.new(0, 50, 0, 15)
                RealTextbox.Font = Enum.Font.GothamSemibold
                RealTextbox.Text = ""
                RealTextbox.TextColor3 = Color3.fromRGB(225, 225, 225)
                RealTextbox.TextSize = 10.000
                RealTextbox.TextTransparency = 0

                RealTextbox.FocusLost:Connect(function()
                    callback(RealTextbox.Text)
                    if disappear then
                        RealTextbox.Text = ""
                    end
                end)

                UICorner.CornerRadius = UDim.new(0, 5)
                UICorner.Parent = RealTextbox
            end
            function main:Title(text)
                local Label = Instance.new("TextLabel")
                local PaddingLabel = Instance.new("UIPadding")
                local labelfunc = {}
        
                Label.Name = "Label"
                Label.Parent = MainFramePage
                Label.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                Label.BackgroundTransparency = 1.000
                Label.Size = UDim2.new(0, 200, 0, 15)
                Label.Font = Enum.Font.GothamSemibold
                Label.TextColor3 = _G.Color
                Label.TextSize = 12.000
                Label.Text = text
                Label.TextXAlignment = Enum.TextXAlignment.Center

                PaddingLabel.PaddingLeft = UDim.new(0,-20)
                PaddingLabel.Parent = Label
                PaddingLabel.Name = "PaddingLabel"
        
                function labelfunc:Set(newtext)
                    Label.Text = newtext
                end
                return labelfunc
            end
            function main:Label(text)
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 15)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = text
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,0)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            function main:NoLabel()
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 5)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = " "
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,15)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end

            function main:Seperator(text)
                local Seperator = Instance.new("Frame")
                local Sep1 = Instance.new("Frame")
                local Sep2 = Instance.new("TextLabel")
                local Sep3 = Instance.new("Frame")
                
                Seperator.Name = "Seperator"
                Seperator.Parent = MainFramePage
                Seperator.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Seperator.BackgroundTransparency = 1.000
                Seperator.Size = UDim2.new(0, 180, 0, 20)
                
                Sep1.Name = "Sep1"
                Sep1.Parent = Seperator
                Sep1.BackgroundColor3 = _G.Color
                Sep1.BorderSizePixel = 0
                Sep1.Position = UDim2.new(0, 0, 0, 10)
                Sep1.Size = UDim2.new(0, 20, 0, 1)
                
                Sep2.Name = "Sep2"
                Sep2.Parent = Seperator
                Sep2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Sep2.BackgroundTransparency = 1.000
                Sep2.Position = UDim2.new(0, 80, 0, 0)
                Sep2.Size = UDim2.new(0, 20, 0, 20)
                Sep2.Font = Enum.Font.GothamSemibold
                Sep2.Text = text
                Sep2.TextColor3 = _G.Color
                Sep2.TextSize = 12.000
                
                Sep3.Name = "Sep3"
                Sep3.Parent = Seperator
                Sep3.BackgroundColor3 = _G.Color
                Sep3.BorderSizePixel = 0
                Sep3.Position = UDim2.new(0, 160, 0, 10)
                Sep3.Size = UDim2.new(0, 20, 0, 1)
            end

            function main:Line()
                local Linee = Instance.new("Frame")
                local Line = Instance.new("Frame")
                
                Linee.Name = "Linee"
                Linee.Parent = MainFramePage
                Linee.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Linee.BackgroundTransparency = 1.000
                Linee.Position = UDim2.new(0, 0, 0.119999997, 0)
                Linee.Size = UDim2.new(0, 180, 0, 20)
                
                Line.Name = "Line"
                Line.Parent = Linee
                Line.BackgroundColor3 = _G.Color
                Line.BorderSizePixel = 0
                Line.Position = UDim2.new(0, 0, 0, 10)
                Line.Size = UDim2.new(0, 180, 0, 1)
            end
            return main, main1
        end
        function uitab:TabRightCenter(text)

            local MainFramePage = Instance.new("ScrollingFrame")
            MainFramePage.Name = text.."_Page"
            MainFramePage.Parent = PageList2
            MainFramePage.Active = true
            MainFramePage.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
            MainFramePage.BackgroundTransparency = 1.000
            MainFramePage.BorderSizePixel = 0
            MainFramePage.Size = UDim2.new(0, 200, 0, 554)
            MainFramePage.CanvasSize = UDim2.new(0, 0, 0, 0)
            MainFramePage.ScrollBarThickness = 0
            
            local UIPadding = Instance.new("UIPadding")
            local UIListLayout = Instance.new("UIListLayout")
            
            UIPadding.Parent = MainFramePage
            UIPadding.PaddingLeft = UDim.new(0, 0)
            UIPadding.PaddingTop = UDim.new(0, 10)

            UIListLayout.Padding = UDim.new(0,0)
            UIListLayout.Parent = MainFramePage
            UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

            if abc == false then
                UIPageLayout2:JumpToIndex(1)
                abc = true
            end
            
            local main = {}
            function main:Title(text)
                local Label = Instance.new("TextLabel")
                local PaddingLabel = Instance.new("UIPadding")
                local labelfunc = {}
        
                Label.Name = "Label"
                Label.Parent = MainFramePage
                Label.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                Label.BackgroundTransparency = 1.000
                Label.Size = UDim2.new(0, 200, 0, 15)
                Label.Font = Enum.Font.GothamSemibold
                Label.TextColor3 = _G.Color
                Label.TextSize = 12.000
                Label.Text = text
                Label.TextXAlignment = Enum.TextXAlignment.Center

                PaddingLabel.PaddingLeft = UDim.new(0,0)
                PaddingLabel.Parent = Label
                PaddingLabel.Name = "PaddingLabel"
        
                function labelfunc:Set(newtext)
                    Label.Text = newtext
                end
                return labelfunc
            end
            function main:Label(text)
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 15)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = text
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,10)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            function main:NoLabel()
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 5)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = " "
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,15)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            return main
        end
        function uitab:TabLeftCenter(text)

            local MainFramePage = Instance.new("ScrollingFrame")
            MainFramePage.Name = text.."_Page"
            MainFramePage.Parent = PageList1
            MainFramePage.Active = true
            MainFramePage.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
            MainFramePage.BackgroundTransparency = 1.000
            MainFramePage.BorderSizePixel = 0
            MainFramePage.Size = UDim2.new(0, 200, 0, 554)
            MainFramePage.CanvasSize = UDim2.new(0, 0, 0, 0)
            MainFramePage.ScrollBarThickness = 0
            
            local UIPadding = Instance.new("UIPadding")
            local UIListLayout = Instance.new("UIListLayout")
            
            UIPadding.Parent = MainFramePage
            UIPadding.PaddingLeft = UDim.new(0, 0)
            UIPadding.PaddingTop = UDim.new(0, 10)

            UIListLayout.Padding = UDim.new(0,0)
            UIListLayout.Parent = MainFramePage
            UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

            if abc == false then
                UIPageLayout1:JumpToIndex(1)
                abc = true
            end
            
            local main = {}
            function main:Title(text)
                local Label = Instance.new("TextLabel")
                local PaddingLabel = Instance.new("UIPadding")
                local labelfunc = {}
        
                Label.Name = "Label"
                Label.Parent = MainFramePage
                Label.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                Label.BackgroundTransparency = 1.000
                Label.Size = UDim2.new(0, 200, 0, 15)
                Label.Font = Enum.Font.GothamSemibold
                Label.TextColor3 = _G.Color
                Label.TextSize = 12.000
                Label.Text = text
                Label.TextXAlignment = Enum.TextXAlignment.Center

                PaddingLabel.PaddingLeft = UDim.new(0,0)
                PaddingLabel.Parent = Label
                PaddingLabel.Name = "PaddingLabel"
        
                function labelfunc:Set(newtext)
                    Label.Text = newtext
                end
                return labelfunc
            end
            function main:Label(text)
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 15)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = text
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,10)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            function main:NoLabel()
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 5)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = " "
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,15)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            return main
        end
        function uitab:TabLeft(text)

            local MainFramePage = Instance.new("ScrollingFrame")
            MainFramePage.Name = text.."_Page"
            MainFramePage.Parent = PageList
            MainFramePage.Active = true
            MainFramePage.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
            MainFramePage.BackgroundTransparency = 1.000
            MainFramePage.BorderSizePixel = 0
            MainFramePage.Size = UDim2.new(0, 200, 0, 554)
            MainFramePage.CanvasSize = UDim2.new(0, 0, 0, 0)
            MainFramePage.ScrollBarThickness = 0
            
            local UIPadding = Instance.new("UIPadding")
            local UIListLayout = Instance.new("UIListLayout")
            
            UIPadding.Parent = MainFramePage
            UIPadding.PaddingLeft = UDim.new(0, 0)
            UIPadding.PaddingTop = UDim.new(0, 10)

            UIListLayout.Padding = UDim.new(0,0)
            UIListLayout.Parent = MainFramePage
            UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

            if abc == false then
                UIPageLayout:JumpToIndex(1)
                abc = true
            end
            
            local main = {}
            function main:Title(text)
                local Label = Instance.new("TextLabel")
                local PaddingLabel = Instance.new("UIPadding")
                local labelfunc = {}
        
                Label.Name = "Label"
                Label.Parent = MainFramePage
                Label.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                Label.BackgroundTransparency = 1.000
                Label.Size = UDim2.new(0, 200, 0, 15)
                Label.Font = Enum.Font.GothamSemibold
                Label.TextColor3 = _G.Color
                Label.TextSize = 12.000
                Label.Text = text
                Label.TextXAlignment = Enum.TextXAlignment.Center

                PaddingLabel.PaddingLeft = UDim.new(0,0)
                PaddingLabel.Parent = Label
                PaddingLabel.Name = "PaddingLabel"
        
                function labelfunc:Set(newtext)
                    Label.Text = newtext
                end
                return labelfunc
            end
            function main:Label(text)
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 15)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = text
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,10)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            function main:NoLabel()
                local Label1 = Instance.new("TextLabel")
                local PaddingLabel1 = Instance.new("UIPadding")
                local labelfunc1 = {}
        
                Label1.Name = "Label1"
                Label1.Parent = MainFramePage
                Label1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                Label1.BackgroundTransparency = 1.000
                Label1.Size = UDim2.new(0, 200, 0, 5)
                Label1.Font = Enum.Font.GothamSemibold
                Label1.TextColor3 = Color3.fromRGB(225, 225, 225)
                Label1.TextSize = 10.000
                Label1.Text = " "
                Label1.TextXAlignment = Enum.TextXAlignment.Left

                PaddingLabel1.PaddingLeft = UDim.new(0,15)
                PaddingLabel1.Parent = Label1
                PaddingLabel1.Name = "PaddingLabel1"
        
                function labelfunc1:Set(newtext)
                    Label1.Text = newtext
                end
                return labelfunc1
            end
            return main
        end
        return uitab
    end

-- UI

-- AFK
    spawn(function()
        print("Anti AFK Dev.Exceed")
        while wait() do 
            local VirtualUser=game:service'VirtualUser'
            game:service'Players'.LocalPlayer.Idled:connect(function()
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.new())
            end)
            wait(60)
        end 
    end)
-- AFK

-- _G.Team
    repeat wait(1)
        pcall(function()
            if game:GetService("Players").LocalPlayer.PlayerGui.Main:FindFirstChild("ChooseTeam") then
                if game:GetService("Players").LocalPlayer.PlayerGui.Main.ChooseTeam.Visible == true then
                    if _G.Team == "Marines" then
                        for i,signal in pairs(getconnections(game:GetService("Players").LocalPlayer.PlayerGui.Main.ChooseTeam.Container.Marines.Frame.ViewportFrame.TextButton.Activated)) do
                            signal:Function()
                        end
                    else
                        for i,signal in pairs(getconnections(game:GetService("Players").LocalPlayer.PlayerGui.Main.ChooseTeam.Container.Pirates.Frame.ViewportFrame.TextButton.Activated)) do
                            signal:Function()
                        end
                    end
                end
            end
        end)
    until game.Players.localPlayer.Neutral == false
-- _G.Team

-- ปิด UI 
    game:GetService("Players").LocalPlayer.PlayerGui.Main.MenuButton.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Beli.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Fragments.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Level.Bar.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Level.Black.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Level.Exp.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.HP.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Energy.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.AlliesButton.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Code.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.CrewButton.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.HomeButton.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Mute.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Settings.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Version.Visible = false
    game:GetService("CoreGui").PlayerList.PlayerListMaster.Visible = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.DmgCounter.Visible = false
    game:GetService("CoreGui").ThemeProvider.Enabled = false
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Title.TextTransparency = 1
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.TextTransparency = 1
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestReward.Title.TextTransparency = 1
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Expand.TextTransparency = 1
    game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.BackgroundTransparency = 1
    game:GetService("Players").LocalPlayer.PlayerGui.TopbarPlus.Enabled = false
    game:GetService("Players").LocalPlayer.PlayerGui.Notifications.Enabled = false
    spawn(function()
        while wait(1) do
            game:GetService("Players").LocalPlayer.PlayerGui.Main.Compass.Visible = false
        end
    end)
-- ปิด UI 

-- Exceed Hub
    local win = Update:Window("Exceed Hub","6023426909",Enum.KeyCode.RightControl);
    StatusLeft_Tab        = win:TabLeft("StatusLeft","9613645002")
    StatusLeftCenter_Tab  = win:TabLeftCenter("StatusLeftCenter","9613645002")
    StatusRightCenter_Tab = win:TabRightCenter("StatusRightCenter","9613645002")
    Status                = win:TabRight("Status","9608089732")
    Setting               = win:TabRight("Setting","9608089732")
-- Exceed Hub

-- World
    Old_World = false
    New_World = false
    Three_World = false
    local placeId = game.PlaceId
    if placeId == 2753915549 then
        Old_World = true
        _G.WorldName = "1st"
    elseif placeId == 4442272183 then
        New_World = true
        _G.WorldName = "2nd"
    elseif placeId == 7449423635 then
        Three_World = true
        _G.WorldName = "3rd"
    end
-- World

-- RightControl
    local N=game:GetService("VirtualInputManager")
    if _G.Close_UI then
        N:SendKeyEvent(true,"RightControl",false,game)
        wait(0.1)
        N:SendKeyEvent(false,"RightControl",false,game)
        wait(1)
    end
-- RightControl

-- Monster
    function CheckLevel()
        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
        if Old_World then
            if Lv == 1 or Lv <= 9 or SelectMonster == "Bandit [Lv. 5]" then -- Bandit
                Ms = "Bandit [Lv. 5]"
                NameQuest = "BanditQuest1"
                QuestLv = 1
                NameMon = "Bandit"
                CFrameQ = CFrame.new(1060.9383544922, 16.455066680908, 1547.7841796875)
                CFrameMon = CFrame.new(1038.5533447266, 41.296249389648, 1576.5098876953)
            elseif Lv == 10 or Lv <= 99 or SelectMonster == "" then
                Ms = "God's Guard [Lv. 450]"
                NameQuest = "JungleQuest"
                QuestLv = 1
                NameMon = "God's Guard"
                CFrameQ = CFrame.new(-1601.6553955078, 36.85213470459, 153.38809204102)
                CFrameMon = CFrame.new(-4628.0498046875, 866.92877197266, -1931.2352294922)
                -- elseif Lv >= 10 or Lv <= 14 or SelectMonster == "Monkey [Lv. 14]" then -- Monkey
                --     Ms = "Monkey [Lv. 14]"
                --     NameQuest = "JungleQuest"
                --     QuestLv = 1
                --     NameMon = "Monkey"
                --     CFrameQ = CFrame.new(-1601.6553955078, 36.85213470459, 153.38809204102)
                --     CFrameMon = CFrame.new(-1448.1446533203, 50.851993560791, 63.60718536377)
                -- elseif Lv == 15 or Lv <= 29 or SelectMonster == "Gorilla [Lv. 20]" then -- Gorilla
                --     Ms = "Gorilla [Lv. 20]"
                --     NameQuest = "JungleQuest"
                --     QuestLv = 2
                --     NameMon = "Gorilla"
                --     CFrameQ = CFrame.new(-1601.6553955078, 36.85213470459, 153.38809204102)
                --     CFrameMon = CFrame.new(-1142.6488037109, 40.462348937988, -515.39227294922)
                --     SelectMonster = "Monkey [Lv. 14]"
                --     if Lv >= 25 then
                --         _G.SelectBoss = "The Gorilla King [Lv. 25] [Boss]" 
                --     end
                -- elseif Lv == 30 or Lv <= 39 or SelectMonster == "Pirate [Lv. 35]" then -- Pirate
                --     Ms = "Pirate [Lv. 35]"
                --     NameQuest = "BuggyQuest1"
                --     QuestLv = 1
                --     NameMon = "Pirate"
                --     CFrameQ = CFrame.new(-1140.1761474609, 4.752049446106, 3827.4057617188)
                --     CFrameMon = CFrame.new(-1201.0881347656, 40.628940582275, 3857.5966796875)
                -- elseif Lv == 40 or Lv <= 59 or SelectMonster == "Brute [Lv. 45]" then -- Brute
                --     Ms = "Brute [Lv. 45]"
                --     NameQuest = "BuggyQuest1"
                --     QuestLv = 2
                --     NameMon = "Brute"
                --     CFrameQ = CFrame.new(-1140.1761474609, 4.752049446106, 3827.4057617188)
                --     CFrameMon = CFrame.new(-1387.5324707031, 24.592035293579, 4100.9575195313)
                --     SelectMonster = "Pirate [Lv. 35]"
                --     if Lv >= 55 then
                --         _G.SelectBoss = "Bobby [Lv. 55] [Boss]"
                --     end
                -- elseif Lv == 60 or Lv <= 74 or SelectMonster == "Desert Bandit [Lv. 60]" then -- Desert Bandit
                --     Ms = "Desert Bandit [Lv. 60]"
                --     NameQuest = "DesertQuest"
                --     QuestLv = 1
                --     NameMon = "Desert Bandit"
                --     CFrameQ = CFrame.new(896.51721191406, 6.4384617805481, 4390.1494140625)
                --     CFrameMon = CFrame.new(984.99896240234, 16.109552383423, 4417.91015625)
                -- elseif Lv == 75 or Lv <= 89 or SelectMonster == "Desert Officer [Lv. 70]" then -- Desert Officer
                --     Ms = "Desert Officer [Lv. 70]"
                --     NameQuest = "DesertQuest"
                --     QuestLv = 2
                --     NameMon = "Desert Officer"
                --     CFrameQ = CFrame.new(896.51721191406, 6.4384617805481, 4390.1494140625)
                --     CFrameMon = CFrame.new(1547.1510009766, 14.452038764954, 4381.8002929688)
                --     SelectMonster = "Desert Bandit [Lv. 60]"
                -- elseif Lv == 90 or Lv <= 99 or SelectMonster == "Snow Bandit [Lv. 90]" then -- Snow Bandit
                --     Ms = "Snow Bandit [Lv. 90]"
                --     NameQuest = "SnowQuest"
                --     QuestLv = 1
                --     NameMon = "Snow Bandit"
                --     CFrameQ = CFrame.new(1386.8073730469, 87.272789001465, -1298.3576660156)
                --     CFrameMon = CFrame.new(1356.3028564453, 105.76865386963, -1328.2418212891)
            elseif Lv == 100 or Lv <= 119 or SelectMonster == "Snowman [Lv. 100]" then -- Snowman
                Ms = "Snowman [Lv. 100]"
                NameQuest = "SnowQuest"
                QuestLv = 2
                NameMon = "Snowman"
                CFrameQ = CFrame.new(1386.8073730469, 87.272789001465, -1298.3576660156)
                CFrameMon = CFrame.new(1218.7956542969, 138.01184082031, -1488.0262451172)
                SelectMonster = "Snow Bandit [Lv. 90]"
                if Lv >= 110 then
                    _G.SelectBoss = "Yeti [Lv. 110] [Boss]"
                end
            elseif Lv == 120 or Lv <= 149 or SelectMonster == "Chief Petty Officer [Lv. 120]" then -- Chief Petty Officer
                Ms = "Chief Petty Officer [Lv. 120]"
                NameQuest = "MarineQuest2"
                QuestLv = 1
                NameMon = "Chief Petty Officer"
                CFrameQ = CFrame.new(-5035.49609375, 28.677835464478, 4324.1840820313)
                CFrameMon = CFrame.new(-4931.1552734375, 65.793113708496, 4121.8393554688)
                if Lv >= 130 then
                    _G.SelectBoss = "Vice Admiral [Lv. 130] [Boss]"
                end
            elseif Lv == 150 or Lv <= 174 or SelectMonster == "Sky Bandit [Lv. 150]" then -- Sky Bandit
                Ms = "Sky Bandit [Lv. 150]"
                NameQuest = "SkyQuest"
                QuestLv = 1
                NameMon = "Sky Bandit"
                CFrameQ = CFrame.new(-4842.1372070313, 717.69543457031, -2623.0483398438)
                CFrameMon = CFrame.new(-4955.6411132813, 365.46365356445, -2908.1865234375)
            elseif Lv == 175 or Lv <= 189 or SelectMonster == "Dark Master [Lv. 175]" then -- Dark Master
                Ms = "Dark Master [Lv. 175]"
                NameQuest = "SkyQuest"
                QuestLv = 2
                NameMon = "Dark Master"
                CFrameQ = CFrame.new(-4842.1372070313, 717.69543457031, -2623.0483398438)
                CFrameMon = CFrame.new(-5148.1650390625, 439.04571533203, -2332.9611816406)
                SelectMonster = "Sky Bandit [Lv. 150]"
            elseif Lv == 190 or Lv <= 209 or SelectMonster == "Prisoner [Lv. 190]" then
                Ms = "Prisoner [Lv. 190]"
                NameQuest = "PrisonerQuest"
                QuestLv = 1
                NameMon = "Prisoner"
                CFrameQ = CFrame.new(5308.93115, 1.65517521, 475.120514)
                CFrameMon = CFrame.new(5433.39307, 88.678093, 514.986877)
            elseif Lv == 210 or Lv <= 249 or SelectMonster == "Dangerous Prisoner [Lv. 210]" then
                Ms = "Dangerous Prisoner [Lv. 210]"
                NameQuest = "PrisonerQuest"
                QuestLv = 2
                NameMon = "Dangerous Prisoner"
                CFrameQ = CFrame.new(5308.93115, 1.65517521, 475.120514)
                CFrameMon = CFrame.new(5433.39307, 88.678093, 514.986877)
                if Lv >= 240 then
                    _G.SelectBoss = "Swan [Lv. 240] [Boss]"
                    
                elseif Lv >= 230 then
                    _G.SelectBoss = "Chief Warden [Lv. 230] [Boss]"
                    
                elseif Lv >= 220 then
                    _G.SelectBoss = "Warden [Lv. 220] [Boss]"
                end
                SelectMonster = "Prisoner [Lv. 190]"
            elseif Lv == 250 or Lv <= 299 or SelectMonster == "Toga Warrior [Lv. 250]" then -- Toga Warrior
                Ms = "Toga Warrior [Lv. 250]"
                NameQuest = "ColosseumQuest"
                QuestLv = 1
                NameMon = "Toga Warrior"
                CFrameQ = CFrame.new(-1577.7890625, 7.4151420593262, -2984.4838867188)
                CFrameMon = CFrame.new(-1872.5166015625, 49.080215454102, -2913.810546875)
                -- elseif Lv == 275 or Lv <= 299 or SelectMonster == "Gladiator [Lv. 275]" then -- Gladiator
                --     Ms = "Gladiator [Lv. 275]"
                --     NameQuest = "ColosseumQuest"
                --     QuestLv = 2
                --     NameMon = "Gladiator"
                --     CFrameQ = CFrame.new(-1577.7890625, 7.4151420593262, -2984.4838867188)
                --     CFrameMon = CFrame.new(-1272.71, 7.7858, -3251.91)
            elseif Lv == 300 or Lv <= 329 or SelectMonster == "Military Soldier [Lv. 300]" then -- Military Soldier
                Ms = "Military Soldier [Lv. 300]"
                NameQuest = "MagmaQuest"
                QuestLv = 1
                NameMon = "Military Soldier"
                CFrameQ = CFrame.new(-5316.1157226563, 12.262831687927, 8517.00390625)
                CFrameMon = CFrame.new(-5369.0004882813, 61.24352645874, 8556.4921875)
            elseif Lv == 330 or Lv <= 374 or SelectMonster == "Military Spy [Lv. 325]" then -- Military Spy
                Ms = "Military Spy [Lv. 325]"
                NameQuest = "MagmaQuest"
                QuestLv = 2
                NameMon = "Military Spy"
                CFrameQ = CFrame.new(-5316.1157226563, 12.262831687927, 8517.00390625)
                CFrameMon = CFrame.new(-5984.0532226563, 82.14656829834, 8753.326171875)
                if Lv >= 350 then
                    _G.SelectBoss = "Magma Admiral [Lv. 350] [Boss]"
                end
                SelectMonster = "Military Soldier [Lv. 300]"
            elseif Lv == 375 or Lv <= 399 or SelectMonster == "Fishman Warrior [Lv. 375]" then -- Fishman Warrior 
                Ms = "Fishman Warrior [Lv. 375]"
                NameQuest = "FishmanQuest"
                QuestLv = 1
                NameMon = "Fishman Warrior"
                CFrameQ = CFrame.new(61122.65234375, 18.497442245483, 1569.3997802734)
                CFrameMon = CFrame.new(60844.10546875, 98.462875366211, 1298.3985595703)
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 3000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
                end
            elseif Lv == 400 or Lv <= 449 or SelectMonster == "Fishman Commando [Lv. 400]" then -- Fishman Commando
                Ms = "Fishman Commando [Lv. 400]"
                NameQuest = "FishmanQuest"
                QuestLv = 2
                NameMon = "Fishman Commando"
                CFrameQ = CFrame.new(61122.65234375, 18.497442245483, 1569.3997802734)
                CFrameMon = CFrame.new(61738.3984375, 64.207321166992, 1433.8375244141)
                SelectMonster = "Fishman Warrior [Lv. 375]"
                if Lv >= 425 then
                    _G.SelectBoss = "Fishman Lord [Lv. 425] [Boss]"
                end
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 3000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
                end
            elseif Lv == 450 or Lv <= 474 or SelectMonster == "God's Guard [Lv. 450]" then -- God's Guard
                Ms = "God's Guard [Lv. 450]"
                NameQuest = "SkyExp1Quest"
                QuestLv = 1
                NameMon = "God's Guard"
                CFrameQ = CFrame.new(-4721.8603515625, 845.30297851563, -1953.8489990234)
                CFrameMon = CFrame.new(-4628.0498046875, 866.92877197266, -1931.2352294922)
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 3000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-4607.82275, 872.54248, -1667.55688))
                end
                if Lv >= 425 then
                    _G.SelectBoss = "Fishman Lord [Lv. 425] [Boss]"
                end
            elseif Lv == 475 or Lv <= 524 or SelectMonster == "Shanda [Lv. 475]" then -- Shanda
                Ms = "Shanda [Lv. 475]"
                NameQuest = "SkyExp1Quest"
                QuestLv = 2
                NameMon = "Shanda"
                CFrameQ = CFrame.new(-7863.1596679688, 5545.5190429688, -378.42266845703)
                CFrameMon = CFrame.new(-7685.1474609375, 5601.0751953125, -441.38876342773)
                SelectMonster = "God's Guard [Lv. 450]"
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 3000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-7894.6176757813, 5547.1416015625, -380.29119873047))
                end
                if Lv >= 500 then
                    _G.SelectBoss = "Wysper [Lv. 500] [Boss]"
                end
            elseif Lv == 525 or Lv <= 549 or SelectMonster == "Royal Squad [Lv. 525]" then -- Royal Squad
                Ms = "Royal Squad [Lv. 525]"
                NameQuest = "SkyExp2Quest"
                QuestLv = 1
                NameMon = "Royal Squad"
                CFrameQ = CFrame.new(-7903.3828125, 5635.9897460938, -1410.923828125)
                CFrameMon = CFrame.new(-7654.2514648438, 5637.1079101563, -1407.7550048828)
            elseif Lv == 550 or Lv <= 624 or SelectMonster == "Royal Soldier [Lv. 550]" then -- Royal Soldier
                Ms = "Royal Soldier [Lv. 550]"
                NameQuest = "SkyExp2Quest"
                QuestLv = 2
                NameMon = "Royal Soldier"
                CFrameQ = CFrame.new(-7903.3828125, 5635.9897460938, -1410.923828125)
                CFrameMon = CFrame.new(-7760.4106445313, 5679.9077148438, -1884.8112792969)
                SelectMonster = "Royal Squad [Lv. 525]"
                if Lv >= 575 then
                    _G.SelectBoss = "Thunder God [Lv. 575] [Boss]"
                end
            elseif Lv == 625 or Lv <= 649 or SelectMonster == "Galley Pirate [Lv. 625]" then -- Galley Pirate
                Ms = "Galley Pirate [Lv. 625]"
                NameQuest = "FountainQuest"
                QuestLv = 1
                NameMon = "Galley Pirate"
                CFrameQ = CFrame.new(5258.2788085938, 38.526931762695, 4050.044921875)
                CFrameMon = CFrame.new(5557.1684570313, 152.32717895508, 3998.7758789063)
            elseif Lv >= 650 or SelectMonster == "Galley Captain [Lv. 650]" then -- Galley Captain
                Ms = "Galley Captain [Lv. 650]"
                NameQuest = "FountainQuest"
                QuestLv = 2
                NameMon = "Galley Captain"
                CFrameQ = CFrame.new(5258.2788085938, 38.526931762695, 4050.044921875)
                CFrameMon = CFrame.new(5677.6772460938, 92.786109924316, 4966.6323242188)
                SelectMonster = "Galley Pirate [Lv. 625]"
                if Lv >= 675 then
                    _G.SelectBoss = "Cyborg [Lv. 675] [Boss]"
                end
            end
        end
        if New_World then
            if Lv == 700 or Lv <= 724 or SelectMonster == "Raider [Lv. 700]" then -- Raider
                Ms = "Raider [Lv. 700]"
                NameQuest = "Area1Quest"
                QuestLv = 1
                NameMon = "Raider"
                CFrameQ = CFrame.new(-427.72567749023, 72.99634552002, 1835.9426269531)
                CFrameMon = CFrame.new(68.874565124512, 93.635643005371, 2429.6752929688)
            elseif Lv == 725 or Lv <= 774 or SelectMonster == "Mercenary [Lv. 725]" then -- Mercenary
                Ms = "Mercenary [Lv. 725]"
                NameQuest = "Area1Quest"
                QuestLv = 2
                NameMon = "Mercenary"
                CFrameQ = CFrame.new(-427.72567749023, 72.99634552002, 1835.9426269531)
                CFrameMon = CFrame.new(-864.85009765625, 122.47104644775, 1453.1505126953)
                SelectMonster = "Raider [Lv. 700]"
            elseif Lv == 775 or Lv <= 799 or SelectMonster == "Swan Pirate [Lv. 775]" then -- Swan Pirate
                Ms = "Swan Pirate [Lv. 775]"
                NameQuest = "Area2Quest"
                QuestLv = 1
                NameMon = "Swan Pirate"
                CFrameQ = CFrame.new(635.61151123047, 73.096351623535, 917.81298828125)
                CFrameMon = CFrame.new(1065.3669433594, 137.64012145996, 1324.3798828125)
            elseif Lv == 800 or Lv <= 874 or SelectMonster == "Factory Staff [Lv. 800]" then -- Factory Staff
                Ms = "Factory Staff [Lv. 800]"
                NameQuest = "Area2Quest"
                QuestLv = 2
                NameMon = "Factory Staff"
                CFrameQ = CFrame.new(635.61151123047, 73.096351623535, 917.81298828125)
                CFrameMon = CFrame.new(533.22045898438, 128.46876525879, 355.62615966797)
                SelectMonster = "Swan Pirate [Lv. 775]"
            elseif Lv == 875 or Lv <= 899 or SelectMonster == "Marine Lieutenant [Lv. 875]" then -- Marine Lieutenant
                Ms = "Marine Lieutenant [Lv. 875]"
                NameQuest = "MarineQuest3"
                QuestLv = 1
                NameMon = "Marine Lieutenant"
                CFrameQ = CFrame.new(-2440.9934082031, 73.04190826416, -3217.7082519531)
                CFrameMon = CFrame.new(-2489.2622070313, 84.613594055176, -3151.8830566406)
            elseif Lv == 900 or Lv <= 949 or SelectMonster == "Marine Captain [Lv. 900]" then -- Marine Captain
                Ms = "Marine Captain [Lv. 900]"
                NameQuest = "MarineQuest3"
                QuestLv = 2
                NameMon = "Marine Captain"
                CFrameQ = CFrame.new(-2440.9934082031, 73.04190826416, -3217.7082519531)
                CFrameMon = CFrame.new(-2335.2026367188, 79.786659240723, -3245.8674316406)
                SelectMonster = "Marine Lieutenant [Lv. 875]"
                if Lv >= 925 then
                    _G.SelectBoss = "Fajita [Lv. 925] [Boss]"
                end
            elseif Lv == 950 or Lv <= 974 or SelectMonster == "Zombie [Lv. 950]" then -- Zombie
                Ms = "Zombie [Lv. 950]"
                NameQuest = "ZombieQuest"
                QuestLv = 1
                NameMon = "Zombie"
                CFrameQ = CFrame.new(-5494.3413085938, 48.505931854248, -794.59094238281)
                CFrameMon = CFrame.new(-5536.4970703125, 101.08577728271, -835.59075927734)
            elseif Lv == 975 or Lv <= 999 or SelectMonster == "Vampire [Lv. 975]" then -- Vampire
                Ms = "Vampire [Lv. 975]"
                NameQuest = "ZombieQuest"
                QuestLv = 2
                NameMon = "Vampire"
                CFrameQ = CFrame.new(-5494.3413085938, 48.505931854248, -794.59094238281)
                CFrameMon = CFrame.new(-5806.1098632813, 16.722528457642, -1164.4384765625)
                SelectMonster = "Zombie [Lv. 950]"
            elseif Lv == 1000 or Lv <= 1049 or SelectMonster == "Snow Trooper [Lv. 1000]" then -- Snow Trooper
                Ms = "Snow Trooper [Lv. 1000]"
                NameQuest = "SnowMountainQuest"
                QuestLv = 1
                NameMon = "Snow Trooper"
                CFrameQ = CFrame.new(607.05963134766, 401.44781494141, -5370.5546875)
                CFrameMon = CFrame.new(535.21051025391, 432.74209594727, -5484.9165039063)
            elseif Lv == 1050 or Lv <= 1099 or SelectMonster == "Winter Warrior [Lv. 1050]" then -- Winter Warrior
                Ms = "Winter Warrior [Lv. 1050]"
                NameQuest = "SnowMountainQuest"
                QuestLv = 2
                NameMon = "Winter Warrior"
                CFrameQ = CFrame.new(607.05963134766, 401.44781494141, -5370.5546875)
                CFrameMon = CFrame.new(1234.4449462891, 456.95419311523, -5174.130859375)
                SelectMonster = "Snow Trooper [Lv. 1000]"
            elseif Lv == 1100 or Lv <= 1124 or SelectMonster == "Lab Subordinate [Lv. 1100]" then -- Lab Subordinate
                Ms = "Lab Subordinate [Lv. 1100]"
                NameQuest = "IceSideQuest"
                QuestLv = 1
                NameMon = "Lab Subordinate"
                CFrameQ = CFrame.new(-6061.841796875, 15.926671981812, -4902.0385742188)
                CFrameMon = CFrame.new(-5720.5576171875, 63.309471130371, -4784.6103515625)
            elseif Lv == 1125 or Lv <= 1174 or SelectMonster == "Horned Warrior [Lv. 1125]" then -- Horned Warrior
                Ms = "Horned Warrior [Lv. 1125]"
                NameQuest = "IceSideQuest"
                QuestLv = 2
                NameMon = "Horned Warrior"
                CFrameQ = CFrame.new(-6061.841796875, 15.926671981812, -4902.0385742188)
                CFrameMon = CFrame.new(-6292.751953125, 91.181983947754, -5502.6499023438)
                SelectMonster = "Lab Subordinate [Lv. 1100]"
            elseif Lv == 1175 or Lv <= 1199 or SelectMonster == "Magma Ninja [Lv. 1175]" then -- Magma Ninja
                Ms = "Magma Ninja [Lv. 1175]"
                NameQuest = "FireSideQuest"
                QuestLv = 1
                NameMon = "Magma Ninja"
                CFrameQ = CFrame.new(-5429.0473632813, 15.977565765381, -5297.9614257813)
                CFrameMon = CFrame.new(-5461.8388671875, 130.36347961426, -5836.4702148438)
            elseif Lv == 1200 or Lv <= 1249 or SelectMonster == "Lava Pirate [Lv. 1200]" then -- Lava Pirate
                Ms = "Lava Pirate [Lv. 1200]"
                NameQuest = "FireSideQuest"
                QuestLv = 2
                NameMon = "Lava Pirate"
                CFrameQ = CFrame.new(-5429.0473632813, 15.977565765381, -5297.9614257813)
                CFrameMon = CFrame.new(-5251.1889648438, 55.164535522461, -4774.4096679688)
                SelectMonster = "Magma Ninja [Lv. 1175]"
            elseif Lv == 1250 or Lv <= 1274 or SelectMonster == "Ship Deckhand [Lv. 1250]" then -- Ship Deckhand
                Ms = "Ship Deckhand [Lv. 1250]"
                NameQuest = "ShipQuest1"
                QuestLv = 1
                NameMon = "Ship Deckhand"
                CFrameQ = CFrame.new(1040.2927246094, 125.08293151855, 32911.0390625)
                CFrameMon = CFrame.new(921.12365722656, 125.9839553833, 33088.328125)
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 20000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
                end
            elseif Lv == 1275 or Lv <= 1299 or SelectMonster == "Ship Engineer [Lv. 1275]" then -- Ship Engineer
                Ms = "Ship Engineer [Lv. 1275]"
                NameQuest = "ShipQuest1"
                QuestLv = 2
                NameMon = "Ship Engineer"
                CFrameQ = CFrame.new(1040.2927246094, 125.08293151855, 32911.0390625)
                CFrameMon = CFrame.new(886.28179931641, 40.47790145874, 32800.83203125)
                SelectMonster = "Ship Deckhand [Lv. 1250]"
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 20000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
                end
            elseif Lv == 1300 or Lv <= 1324 or SelectMonster == "Ship Steward [Lv. 1300]" then -- Ship Steward
                Ms = "Ship Steward [Lv. 1300]"
                NameQuest = "ShipQuest2"
                QuestLv = 1
                NameMon = "Ship Steward"
                CFrameQ = CFrame.new(971.42065429688, 125.08293151855, 33245.54296875)
                CFrameMon = CFrame.new(943.85504150391, 129.58183288574, 33444.3671875)
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 20000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
                end
            elseif Lv == 1325 or Lv <= 1349 or SelectMonster == "Ship Officer [Lv. 1325]" then -- Ship Officer
                Ms = "Ship Officer [Lv. 1325]"
                NameQuest = "ShipQuest2"
                QuestLv = 2
                NameMon = "Ship Officer"
                CFrameQ = CFrame.new(971.42065429688, 125.08293151855, 33245.54296875)
                CFrameMon = CFrame.new(955.38458251953, 181.08335876465, 33331.890625)
                SelectMonster = "Ship Steward [Lv. 1300]"
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 20000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
                end
            elseif Lv == 1350 or Lv <= 1374 or SelectMonster == "Arctic Warrior [Lv. 1350]" then -- Arctic Warrior
                Ms = "Arctic Warrior [Lv. 1350]"
                NameQuest = "FrostQuest"
                QuestLv = 1
                NameMon = "Arctic Warrior"
                CFrameQ = CFrame.new(5668.1372070313, 28.202531814575, -6484.6005859375)
                CFrameMon = CFrame.new(5935.4541015625, 77.26016998291, -6472.7568359375)
                if Auto_Farm and (CFrameMon.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 20000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-6508.5581054688, 89.034996032715, -132.83953857422))
                end
            elseif Lv == 1375 or Lv <= 1424 or SelectMonster == "Snow Lurker [Lv. 1375]" then -- Snow Lurker
                Ms = "Snow Lurker [Lv. 1375]"
                NameQuest = "FrostQuest"
                QuestLv = 2
                NameMon = "Snow Lurker"
                CFrameQ = CFrame.new(5668.1372070313, 28.202531814575, -6484.6005859375)
                CFrameMon = CFrame.new(5628.482421875, 57.574996948242, -6618.3481445313)
                SelectMonster = "Arctic Warrior [Lv. 1350]"
            elseif Lv == 1425 or Lv <= 1449 or SelectMonster == "Sea Soldier [Lv. 1425]" then -- Sea Soldier
                Ms = "Sea Soldier [Lv. 1425]"
                NameQuest = "ForgottenQuest"
                QuestLv = 1
                NameMon = "Sea Soldier"
                CFrameQ = CFrame.new(-3054.5827636719, 236.87213134766, -10147.790039063)
                CFrameMon = CFrame.new(-3185.0153808594, 58.789089202881, -9663.6064453125)
            elseif Lv >= 1450 or SelectMonster == "Water Fighter [Lv. 1450]" then -- Water Fighter
                Ms = "Water Fighter [Lv. 1450]"
                NameQuest = "ForgottenQuest"
                QuestLv = 2
                NameMon = "Water Fighter"
                CFrameQ = CFrame.new(-3054.5827636719, 236.87213134766, -10147.790039063)
                CFrameMon = CFrame.new(-3262.9301757813, 298.69036865234, -10552.529296875)
                SelectMonster = "Sea Soldier [Lv. 1425]"
                if Lv >= 1475 then
                    _G.SelectBoss = "Tide Keeper [Lv. 1475] [Boss]"
                end
            end
        end
        if Three_World then
            if Lv == 1500 or Lv <= 1524 or SelectMonster == "Pirate Millionaire [Lv. 1500]" then -- Pirate Millionaire
                Ms = "Pirate Millionaire [Lv. 1500]"
                NameQuest = "PiratePortQuest"
                QuestLv = 1
                NameMon = "Pirate Millionaire"
                CFrameQ = CFrame.new(-289.61752319336, 43.819011688232, 5580.0903320313)
                CFrameMon = CFrame.new(-435.68109130859, 189.69866943359, 5551.0756835938)
            elseif Lv == 1525 or Lv <= 1574 or SelectMonster == "Pistol Billionaire [Lv. 1525]" then -- Pistol Billoonaire
                Ms = "Pistol Billionaire [Lv. 1525]"
                NameQuest = "PiratePortQuest"
                QuestLv = 2
                NameMon = "Pistol Billionaire"
                CFrameQ = CFrame.new(-289.61752319336, 43.819011688232, 5580.0903320313)
                CFrameMon = CFrame.new(-236.53652954102, 217.46676635742, 6006.0883789063)
                SelectMonster = "Pirate Millionaire [Lv. 1500]"
            elseif Lv == 1575 or Lv <= 1599 or SelectMonster == "Dragon Crew Warrior [Lv. 1575]" then -- Dragon Crew Warrior
                Ms = "Dragon Crew Warrior [Lv. 1575]"
                NameQuest = "AmazonQuest"
                QuestLv = 1
                NameMon = "Dragon Crew Warrior"
                CFrameQ = CFrame.new(5833.1147460938, 51.60498046875, -1103.0693359375)
                CFrameMon = CFrame.new(6301.9975585938, 104.77153015137, -1082.6075439453)
            elseif Lv == 1600 or Lv <= 1624 or SelectMonster == "Dragon Crew Archer [Lv. 1600]" then -- Dragon Crew Archer
                Ms = "Dragon Crew Archer [Lv. 1600]"
                NameQuest = "AmazonQuest"
                QuestLv = 2
                NameMon = "Dragon Crew Archer"
                CFrameQ = CFrame.new(5833.1147460938, 51.60498046875, -1103.0693359375)
                CFrameMon = CFrame.new(6831.1171875, 441.76708984375, 446.58615112305)
                SelectMonster = "Dragon Crew Warrior [Lv. 1575]"
            elseif Lv == 1625 or Lv <= 1649 or SelectMonster == "Female Islander [Lv. 1625]" then -- Female Islander
                Ms = "Female Islander [Lv. 1625]"
                NameQuest = "AmazonQuest2"
                QuestLv = 1
                NameMon = "Female Islander"
                CFrameQ = CFrame.new(5446.8793945313, 601.62945556641, 749.45672607422)
                CFrameMon = CFrame.new(5792.5166015625, 848.14392089844, 1084.1818847656)
            elseif Lv == 1650 or Lv <= 1699 or SelectMonster == "Giant Islander [Lv. 1650]" then -- Giant Islander
                Ms = "Giant Islander [Lv. 1650]"
                NameQuest = "AmazonQuest2"
                QuestLv = 2
                NameMon = "Giant Islander"
                CFrameQ = CFrame.new(5446.8793945313, 601.62945556641, 749.45672607422)
                CFrameMon = CFrame.new(5009.5068359375, 664.11071777344, -40.960144042969)
                SelectMonster = "Female Islander [Lv. 1625]"
            elseif Lv == 1700 or Lv <= 1724 or SelectMonster == "Marine Commodore [Lv. 1700]" then -- Marine Commodore
                Ms = "Marine Commodore [Lv. 1700]"
                NameQuest = "MarineTreeIsland"
                QuestLv = 1
                NameMon = "Marine Commodore"
                CFrameQ = CFrame.new(2179.98828125, 28.731239318848, -6740.0551757813)
                CFrameMon = CFrame.new(2198.0063476563, 128.71075439453, -7109.5043945313)
            elseif Lv == 1725 or Lv <= 1774 or SelectMonster == "Marine Rear Admiral [Lv. 1725]" then -- Marine Rear Admiral
                Ms = "Marine Rear Admiral [Lv. 1725]"
                NameQuest = "MarineTreeIsland"
                QuestLv = 2
                NameMon = "Marine Rear Admiral"
                CFrameQ = CFrame.new(2179.98828125, 28.731239318848, -6740.0551757813)
                CFrameMon = CFrame.new(3294.3142089844, 385.41125488281, -7048.6342773438)
                SelectMonster = "Marine Commodore [Lv. 1700]"
            elseif Lv == 1775 or Lv <= 1799 or SelectMonster == "Fishman Raider [Lv. 1775]" then -- Fishman Raide
                Ms = "Fishman Raider [Lv. 1775]"
                NameQuest = "DeepForestIsland3"
                QuestLv = 1
                NameMon = "Fishman Raider"
                CFrameQ = CFrame.new(-10582.759765625, 331.78845214844, -8757.666015625)
                CFrameMon = CFrame.new(-10553.268554688, 521.38439941406, -8176.9458007813)
            elseif Lv == 1800 or Lv <= 1824 or SelectMonster == "Fishman Captain [Lv. 1800]" then -- Fishman Captain
                Ms = "Fishman Captain [Lv. 1800]"
                NameQuest = "DeepForestIsland3"
                QuestLv = 2
                NameMon = "Fishman Captain"
                CFrameQ = CFrame.new(-10583.099609375, 331.78845214844, -8759.4638671875)
                CFrameMon = CFrame.new(-10789.401367188, 427.18637084961, -9131.4423828125)
                SelectMonster = "Fishman Raider [Lv. 1775]"
            elseif Lv == 1825 or Lv <= 1849 or SelectMonster == "Forest Pirate [Lv. 1825]" then -- Forest Pirate
                Ms = "Forest Pirate [Lv. 1825]"
                NameQuest = "DeepForestIsland"
                QuestLv = 1
                NameMon = "Forest Pirate"
                CFrameQ = CFrame.new(-13232.662109375, 332.40396118164, -7626.4819335938)
                CFrameMon = CFrame.new(-13489.397460938, 400.30349731445, -7770.251953125)
            elseif Lv == 1850 or Lv <= 1899 or SelectMonster == "Mythological Pirate [Lv. 1850]" then -- Mythological Pirate
                Ms = "Mythological Pirate [Lv. 1850]"
                NameQuest = "DeepForestIsland"
                QuestLv = 2
                NameMon = "Mythological Pirate"
                CFrameQ = CFrame.new(-13232.662109375, 332.40396118164, -7626.4819335938)
                CFrameMon = CFrame.new(-13508.616210938, 582.46228027344, -6985.3037109375)
                SelectMonster = "Forest Pirate [Lv. 1825]"
            elseif Lv == 1900 or Lv <= 1924 or SelectMonster == "Jungle Pirate [Lv. 1900]" then -- Jungle Pirate
                Ms = "Jungle Pirate [Lv. 1900]"
                NameQuest = "DeepForestIsland2"
                QuestLv = 1
                NameMon = "Jungle Pirate"
                CFrameQ = CFrame.new(-12682.096679688, 390.88653564453, -9902.1240234375)
                CFrameMon = CFrame.new(-12267.103515625, 459.75262451172, -10277.200195313)
            elseif Lv == 1925 or Lv <= 1974 or SelectMonster == "Musketeer Pirate [Lv. 1925]" then -- Musketeer Pirate
                Ms = "Musketeer Pirate [Lv. 1925]"
                NameQuest = "DeepForestIsland2"
                QuestLv = 2
                NameMon = "Musketeer Pirate"
                CFrameQ = CFrame.new(-12682.096679688, 390.88653564453, -9902.1240234375)
                CFrameMon = CFrame.new(-13291.5078125, 520.47338867188, -9904.638671875)
                SelectMonster = "Jungle Pirate [Lv. 1900]"
            elseif Lv == 1975 or Lv <= 1999 or SelectMonster == "Reborn Skeleton [Lv. 1975]" then -- Reborn Skeleton
                Ms = "Reborn Skeleton [Lv. 1975]"
                NameQuest = "HauntedQuest1"
                QuestLv = 1
                NameMon = "Reborn Skeleton"
                CFrameQ = CFrame.new(-9480.80762, 142.130661, 5566.37305)
                CFrameMon = CFrame.new(-8760.26, 142.105, 5979.31)
            elseif Lv == 2000 or Lv <= 2024 or SelectMonster == "Living Zombie [Lv. 2000]" then -- Living Zombie
                Ms = "Living Zombie [Lv. 2000]"
                NameQuest = "HauntedQuest1"
                QuestLv = 2
                NameMon = "Living Zombie"
                CFrameQ = CFrame.new(-9480.80762, 142.130661, 5566.37305)
                CFrameMon = CFrame.new(-10103.7529, 238.565979, 6179.75977)
                SelectMonster = "Reborn Skeleton [Lv. 1975]"
            elseif Lv == 2025 or Lv <= 2049 or SelectMonster == "Demonic Soul [Lv. 2025]" then -- Demonic Soul
                Ms = "Demonic Soul [Lv. 2025]"
                NameQuest = "HauntedQuest2"
                QuestLv = 1
                NameMon = "Demonic Souls"
                CFrameQ = CFrame.new(-9515.39551, 172.266037, 6078.89746)
                CFrameMon = CFrame.new(-9709.30762, 204.695892, 6044.04688)
            elseif Lv == 2050 or Lv <= 2074 or SelectMonster == "Posessed Mummy [Lv. 2050]" then -- Posessed Mummy
                Ms = "Posessed Mummy [Lv. 2050]"
                NameQuest = "HauntedQuest2"
                QuestLv = 2
                NameMon = "Posessed Mummys"
                CFrameQ = CFrame.new(-9515.39551, 172.266037, 6078.89746)
                CFrameMon = CFrame.new(-9554.11035, 65.6141663, 6041.73584)
                SelectMonster = "Demonic Soul [Lv. 2025]"
            elseif Lv == 2075 or Lv <= 2099 or SelectMonster == "Peanut Scout [Lv. 2075]" then -- Peanut Scout
                Ms = "Peanut Scout [Lv. 2075]"
                NameQuest = "NutsIslandQuest"
                QuestLv = 1
                NameMon = "Peanut Scout"
                CFrameQ = CFrame.new(-2104.453125, 38.129974365234, -10194.0078125)
                CFrameMon = CFrame.new(-2068.0949707031, 76.512603759766, -10117.150390625)
            elseif Lv == 2100 or Lv <= 2124 or SelectMonster == "Peanut President [Lv. 2100]" then -- Peanut President
                Ms = "Peanut President [Lv. 2100]"
                NameQuest = "NutsIslandQuest"
                QuestLv = 2
                NameMon = "Peanut Presidents"
                CFrameQ = CFrame.new(-2104.453125, 38.129974365234, -10194.0078125)
                CFrameMon = CFrame.new(-2067.33203125, 90.557350158691, -10552.051757812)
                SelectMonster = "Peanut Scout [Lv. 2075]"
            elseif Lv == 2125 or Lv <= 2149 or SelectMonster == "Ice Cream Chef [Lv. 2125]" then --Ice Cream Chef
                Ms = "Ice Cream Chef [Lv. 2125]"
                NameQuest = "IceCreamIslandQuest"
                QuestLv = 1
                NameMon = "Ice Cream Chef"
                CFrameQ = CFrame.new(-821.35913085938, 65.845329284668, -10965.2578125)
                CFrameMon = CFrame.new(-796.37261962891, 110.95275878906, -10847.473632812)
            elseif Lv == 2150 or Lv <= 2200 or SelectMonster == "Ice Cream Commander [Lv. 2150]" then -- Ice Cream Commander
                Ms = "Ice Cream Commander [Lv. 2150]"
                NameQuest = "IceCreamIslandQuest"
                QuestLv = 2
                NameMon = "Ice Cream Commander"
                CFrameQ = CFrame.new(-821.35913085938, 65.845329284668, -10965.2578125)
                CFrameMon = CFrame.new(-697.65338134766, 174.48368835449, -11218.38671875)
                SelectMonster = "Ice Cream Chef [Lv. 2125]"
            elseif Lv == 2200 or Lv <= 2224 or SelectMonster == "Cookie Crafter [Lv. 2200]" then -- Ice Cream Commander
                Ms = "Cookie Crafter [Lv. 2200]"
                NameQuest = "CakeQuest1"
                QuestLv = 1
                NameMon = "Cookie Crafter"
                CFrameQ = CFrame.new(-2017.4874267578125, 36.85276412963867, -12027.53515625)
                CFrameMon = CFrame.new(-2358.5791015625, 36.85615539550781, -12111.052734375)
            elseif Lv == 2225 or Lv <= 2249 or SelectMonster == "Cake Guard [Lv. 2225]" then
                Ms = "Cake Guard [Lv. 2225]"
                NameQuest = "CakeQuest1"
                QuestLv = 2
                NameMon = "Cake Guard"
                CFrameMon = CFrame.new(-1430.4925537109375, 36.85621643066406, -12322.162109375)
                CFrameQ = CFrame.new(-2017.4874267578125, 36.85276412963867, -12027.53515625)
                SelectMonster = "Cookie Crafter [Lv. 2200]"
            elseif Lv == 2250 or Lv <= 2274 or SelectMonster == "Baking Staff [Lv. 2250]" then
                Ms = "Baking Staff [Lv. 2250]"
                NameQuest = "CakeQuest2"
                QuestLv = 1
                NameMon = "Baking Staff"
                CFrameMon = CFrame.new(-1836.11, 37.7982, -12969.7)
                CFrameQ = CFrame.new(-1926.01, 40.2479, -12841.1)
            elseif Lv == 2275 or Lv <= 2299 or SelectMonster == "Head Baker [Lv. 2275]" then
                Ms = "Head Baker [Lv. 2275]"
                NameQuest = "CakeQuest2"
                QuestLv = 2
                NameMon = "Head Baker"
                CFrameMon = CFrame.new(-2199.69, 53.3981, -12884.7)
                CFrameQ = CFrame.new(-1926.01, 40.2479, -12841.1)
                SelectMonster = "Baking Staff [Lv. 2250]"
            elseif Lv == 2300 or Lv <= 2324 or SelectMonster == "Cocoa Warrior [Lv. 2300]" then
                Ms = "Cocoa Warrior [Lv. 2300]"
                NameQuest = "ChocQuest1"
                QuestLv = 1
                NameMon = "Cocoa Warrior"
                CFrameMon = CFrame.new(-27.7522, 24.7344, -12240.2)
                CFrameQ = CFrame.new(237.611, 24.7343, -12200.4)
            elseif Lv == 2325 or Lv <= 2349 or SelectMonster == "Chocolate Bar Battler [Lv. 2325]" then
                Ms = "Chocolate Bar Battler [Lv. 2325]"
                NameQuest = "ChocQuest1"
                QuestLv = 2
                NameMon = "Chocolate Bar Battler"
                CFrameMon = CFrame.new(739.411, 24.7343, -12670.4)
                CFrameQ = CFrame.new(237.611, 24.7343, -12200.4)
                SelectMonster = "Cocoa Warrior [Lv. 2300]"
            elseif Lv == 2350 or Lv <= 2374 or SelectMonster == "Sweet Thief [Lv. 2350]" then
                Ms = "Sweet Thief [Lv. 2350]"
                NameQuest = "ChocQuest2"
                QuestLv = 1
                NameMon = "Sweet Thief"
                CFrameMon = CFrame.new(13.7365, 24.7939, -12606.7)
                CFrameQ = CFrame.new(149.998, 24.7938, -12769.9)
            elseif Lv >= 2375 or SelectMonster == "Candy Rebel [Lv. 2375]" then
                Ms = "Candy Rebel [Lv. 2375]"
                NameQuest = "ChocQuest2"
                QuestLv = 2
                NameMon = "Candy Rebel"
                CFrameMon = CFrame.new(87.8577, 24.7938, -12932.8)
                CFrameQ = CFrame.new(149.998, 24.7938, -12769.9)
                SelectMonster = "Sweet Thief [Lv. 2350]"
            end
        end
    end
-- Monster

-- TP
    function TP(P1)
        if not _G.Stop_Tween then
            local Distance = (P1.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if Distance < 150 then
                Speed = 2000
            elseif Distance < 200 then
                Speed = 1500
            elseif Distance < 300 then
                Speed = 800
            elseif Distance < 500 then
                Speed = 500
            elseif Distance < 1000 then
                Speed = 250
            elseif Distance >= 1000 then
                Speed = 250
            end
            Tween = game:GetService("TweenService"):Create(game.Players.LocalPlayer.Character.HumanoidRootPart,TweenInfo.new(Distance/Speed, Enum.EasingStyle.Linear),{CFrame = P1})
            if _G.Stop_Tween then
                Tween:Cancel()
            elseif game.Players.LocalPlayer.Character.Humanoid.Health > 0 then
                Tween:Play()
            end
        end
    end
-- TP

-- EquipWeapon
    function weapon()
        for i2,v2 in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do  
            if tostring(v2.ToolTip) == "Melee" then
                _G.Weapon = v2.Name
            end
        end
    end
    function EquipWeapon(ToolSe)
        if game.Players.LocalPlayer.Backpack:FindFirstChild(ToolSe) then
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild(ToolSe)
            wait(1)
            game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
        end
    end
    function UnEquipweapon()
        for i,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do 
            if v["ClassName"] == "Tool" then
                if string.find(v.Name,"Fruit") then
                    game.Players.LocalPlayer.Character:FindFirstChild(v.Name).Parent = game.Players.LocalPlayer.Backpack
                end    
            end   
        end
    end
-- EquipWeapon

-- FastAttack_Mode
    if _G.FastAttack_Mode == "Fast" then
        _G.Fast_Delay = 0.1
    elseif _G.FastAttack_Mode == "Smooth" then
        _G.Fast_Delay = 0.2
    elseif _G.FastAttack_Mode == "Super Fast" then
        _G.Fast_Delay = 0.05
    end
-- FastAttack_Mode

-- CanCollide
    local function NoclipLoop()
        for _, child in pairs(game:GetService("Players").LocalPlayer.Character:GetDescendants()) do
            if child:IsA("BasePart") and child.CanCollide == true then
                child.CanCollide = false
            end
        end
    end
    Noclipping = game:GetService('RunService').Stepped:Connect(NoclipLoop)
-- CanCollide

-- Function 
    for i2,v2 in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do  
        if tostring(v2.ToolTip) == "Melee" then
            _G.Weapon = v2.Name
        end
    end
    function CheckBackPack(Backpack)
        return game.Players.LocalPlayer.Backpack:FindFirstChild(Backpack)
    end
    function CheckCharacter(Character)
        return game.Players.LocalPlayer.Character:FindFirstChild(Character)
    end
    function CheckWeaponBC(BC)
        return game.Players.LocalPlayer.Character:FindFirstChild(BC) or game.Players.LocalPlayer.Backpack:FindFirstChild(BC)
    end
    function CheckSword(Sword)
        for i,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
            if v:IsA("Tool") then
                if v.Name == Sword then 
                    return true
                end
            end
        end
        for i,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
            if v:IsA("Tool") then
                if v.Name == Sword then 
                    return true
                end
            end
        end
        local CS = game:GetService("ReplicatedStorage").Remotes["CommF_"]:InvokeServer("getInventoryWeapons")
        for i,v in pairs(CS) do
            if v.Name == Sword then 
                return true
            end
        end
    end 
    function CheckLevelSword(Sword)
        local CheckSword = game:GetService("ReplicatedStorage").Remotes["CommF_"]:InvokeServer("getInventory")
        for i,v in pairs(CheckSword) do
            if v.Type == "Sword" then
                if v.Name == Sword then 
                    return v.Mastery
                end
            end
        end
    end
    function CheckLevelGun(Gun)
        local CheckGun = game:GetService("ReplicatedStorage").Remotes["CommF_"]:InvokeServer("getInventory")
        for i,v in pairs(CheckGun) do
            if v.Type == "Gun" then
                if v.Name == Gun then 
                    return v.Mastery
                end
            end
        end
    end
    function Check_Fruit_BackPack(FruitBP)
        Nameruits = game:GetService("Players").localPlayer.Data.DevilFruit.Value
        return Nameruits == FruitBP 
    end
    function Check_Fruit_CB()
        for u,y in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
            if string.find(y.Name, "Fruit") then 
                return true
            end
        end
        for i,v in pairs(game:GetService("Players").LocalPlayer.Character:GetChildren()) do
            if string.find(v.Name, "Fruit") then 
                return true
            end
        end
    end
    function Check_Fruit_CBI()
        local fruit = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getInventoryFruits")
        for i,v in pairs(fruit) do
            if v["Price"] < 1000000 then
                return true
            end
        end
        for u,y in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
            if string.find(y.Name, "Fruit") then 
                return true
            end
        end
        for i,v in pairs(game:GetService("Players").LocalPlayer.Character:GetChildren()) do
            if string.find(v.Name, "Fruit") then 
                return true
            end
        end
    end
    function Check_Fruit_CBIPrice1M()
        local fruit = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getInventoryFruits")
        for i,v in pairs(fruit) do
            if v["Price"] >= 1000000 then
                return true
            end
        end
    end
    function Check_Inventory_Item(CName, CNumber)
        local CheckInventory = game:GetService("ReplicatedStorage").Remotes["CommF_"]:InvokeServer("getInventory")
        for i,v in pairs(CheckInventory) do
            if v.Type == "Material" then
                if v.Name == CName and v.Count >= CNumber then 
                    return true
                elseif v.Name == CName and v.Count < CNumber then 
                    return false
                end
            end
        end
    end
    function Check_Inventory_Item_Name(CName)
        local CheckInventory = game:GetService("ReplicatedStorage").Remotes["CommF_"]:InvokeServer("getInventory")
        for i,v in pairs(CheckInventory) do
            if v.Type == "Material" then
                if v.Name == CName then 
                    if v.Count > 0 then 
                        return v.Count
                    else
                        return "0"
                    end
                end
            end
        end
    end
    function CheckStatusRace()
        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Wenlocktoad","1") == -2 then
            return "✅ EvoRaceSuccess"
        elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Wenlocktoad","1") ~= -2 then
            return "❌ Not EvoRace"
        end
    end
    function CheckStatusColorHaki(Name)
        CheckColorHaki = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getColors")
        for i,v in pairs(CheckColorHaki) do
            if v.Name == Name then --"Yellow Sunshine" then 
                return true
            end
        end
    end
    function StatusFruitAwakened()
        -- pcall(function()
            Check_Fruit = game:GetService("Players").LocalPlayer.Data.DevilFruit.Value
            if Check_Fruit == "" then
                return "❌ Not Fruit"
            else
                if CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "✅ Z, X, C, V, F"
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "✅ Z, X, C, V" 
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "🔁 Z, X, C, F"
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "🔁 Z, X, C"
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "🔁 Z, X, F"
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "🔁 Z, X"
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "🔁 Z, F"
                elseif CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "🔁 Z"
                elseif not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("Z") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("X") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("F") and
                    not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("C") and not CheckWeaponBC(Check_Fruit).AwakenedMoves:FindFirstChild("V") then 
                    return "❌ Not Awakened"
                else
                    return "❌ Not Awakened"
                end
            end
        -- end)
    end
    function CheckHaki()
        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk", "Start") ~= 0 then
            return "0"
        else
            local test = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk", "Status")
            local Lpp = string.match(tostring(test), "%d+")
            return tonumber(Lpp)
        end
    end
-- Function 

-- Attack
    local CameraShaker = require(game.ReplicatedStorage.Util.CameraShaker)
    CombatFrameworkR = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework)
    y = debug.getupvalues(CombatFrameworkR)[2]
    spawn(function()
        game:GetService("RunService").RenderStepped:Connect(function()
            if typeof(y) == "table" then
                pcall(function()
                    CameraShaker:Stop()
                    y.activeController.timeToNextAttack = (math.huge^math.huge^math.huge)
                    y.activeController.timeToNextAttack = 0
                    y.activeController.hitboxMagnitude = 60
                    y.activeController.active = false
                    y.activeController.timeToNextBlock = 0
                    y.activeController.focusStart = 1655503339.0980349
                    y.activeController.increment = 3
                    y.activeController.blocking = false
                    y.activeController.attacking = false
                    y.activeController.humanoid.AutoRotate = true
                end)
            end
        end)
    end)
    local plr = game.Players.LocalPlayer

    local CbFw = debug.getupvalues(require(plr.PlayerScripts.CombatFramework))
    local CbFw2 = CbFw[2]

    function GetCurrentBlade() 
        local p13 = CbFw2.activeController
        local ret = p13.blades[1]
        if not ret then return end
        while ret.Parent~=game.Players.LocalPlayer.Character do ret=ret.Parent end
        return ret
    end
    function AttackNoCD() 
        local AC = CbFw2.activeController
        for i = 1, 1 do 
            local bladehit = require(game.ReplicatedStorage.CombatFramework.RigLib).getBladeHits(
                plr.Character,
                {plr.Character.HumanoidRootPart},
                60
            )
            local cac = {}
            local hash = {}
            for k, v in pairs(bladehit) do
                if v.Parent:FindFirstChild("HumanoidRootPart") and not hash[v.Parent] then
                    table.insert(cac, v.Parent.HumanoidRootPart)
                    hash[v.Parent] = true
                end
            end
            bladehit = cac
            if #bladehit > 0 then
                pcall(function()
                    local u8 = debug.getupvalue(AC.attack, 5)
                    local u9 = debug.getupvalue(AC.attack, 6)
                    local u7 = debug.getupvalue(AC.attack, 4)
                    local u10 = debug.getupvalue(AC.attack, 7)
                    local u12 = (u8 * 798405 + u7 * 727595) % u9
                    local u13 = u7 * 798405
                    (function()
                        u12 = (u12 * u9 + u13) % 1099511627776
                        u8 = math.floor(u12 / u9)
                        u7 = u12 - u8 * u9
                    end)()
                    u10 = u10 + 1
                    debug.setupvalue(AC.attack, 5, u8)
                    debug.setupvalue(AC.attack, 6, u9)
                    debug.setupvalue(AC.attack, 4, u7)
                    debug.setupvalue(AC.attack, 7, u10)
                        for k, v in pairs(AC.animator.anims.basic) do
                            v:Play(0.01,0.01,0.01)
                        end                  
                    if plr.Character:FindFirstChildOfClass("Tool") and AC.blades and AC.blades[1] then 
                        game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("weaponChange",tostring(GetCurrentBlade()))
                        game.ReplicatedStorage.Remotes.Validator:FireServer(math.floor(u12 / 1099511627776 * 16777215), u10)
                        game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("hit", bladehit, i, "") 
                    end
                end)
            end
        end
    end

-- Attack

-- Farm 

    Setting:Seperator("All Farm KaiTun")

    Setting:Toggle("Farm Level Tun", _G.KaiTun,function(vu)
        Auto_Farm = vu
        HopGoWorld3_Core = vu
        NoClip = vu
        World2 = vu
        Saber_Farm = vu
        Bartlio_Farm = vu
        Don_Swan_Quest = vu
        Auto_Three_World = vu
        Inventory_Fruit = vu
        BringFruit = vu
        RedeemCode = vu
        AutoHaki = vu
        Attack_Death_Step = vu
        Attack_Shakman_Karate = vu
        Attack_Electric_Claw = vu
        Attack_Dragon_Talon = vu
        Buy_Superhuman = vu
        Buy_Fruit_Awakener = vu
        Farm_Bones = vu
        FarmItemInventroy = vu
        _G.Hop_Saber = vu
        _G.KaiTun = vu
    end)

    -- Setting:Toggle("AutoRaid", false,function(vu)
    --     NoClip = vu
    --     BringFruit = vu
    --     Auto_Raid = vu
    -- end)

    Setting:Toggle("Test Function", _G.TestFunction,function(vu)
        Attack_Auto_Farm_Mastery = vu
        NoClip = vu
    end)
    
    -- FarmLevel
        spawn(function()
            while wait(1) do 
                pcall(function()
                    if Auto_Farm and not Raid_Fruit_Fragments and not Auto_Raid and not Farm_Item_Inventroy_Magma_Ore and not Farm_Item_Inventroy_Fish_Tail and not Farm_Item_Inventroy_Mystic_Droplet and  
                        not Farm_Item_Inventroy_Dragon_Scale and not Attack_Boss_Dragon_Talon and not Attack_Boss_Electric_Claw and not Attack_Boss_Shakman_Karate and not Attack_Boss_Death_Step and 
                        not HopGoWorld3_Core_Attack and not Farm_Sword_Saber and not Farm_Bones_UI then 
                        if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                            if SelectMonster ~= nil then
                                StatrMagnet = false
                                CheckLevel()
                                repeat wait(1)
                                    TP(CFrameQ)
                                until (CFrameQ.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 or not Auto_Farm or Raid_Fruit_Fragments or Auto_Raid or Farm_Item_Inventroy_Magma_Ore or 
                                Farm_Item_Inventroy_Fish_Tail or Farm_Item_Inventroy_Mystic_Droplet or Farm_Item_Inventroy_Dragon_Scale or Attack_Boss_Dragon_Talon or Attack_Boss_Electric_Claw or 
                                Attack_Boss_Shakman_Karate or Attack_Boss_Death_Step or HopGoWorld3_Core_Attack or Farm_Sword_Saber or Farm_Bones_UI


                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, QuestLv)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                SelectMonster = nil
                            else
                                StatrMagnet = false
                                CheckLevel()
                                repeat wait(1)
                                    TP(CFrameQ)
                                until (CFrameQ.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 or not Auto_Farm or Raid_Fruit_Fragments or Auto_Raid or Farm_Item_Inventroy_Magma_Ore or 
                                Farm_Item_Inventroy_Fish_Tail or Farm_Item_Inventroy_Mystic_Droplet or Farm_Item_Inventroy_Dragon_Scale or Attack_Boss_Dragon_Talon or Attack_Boss_Electric_Claw or 
                                Attack_Boss_Shakman_Karate or Attack_Boss_Death_Step or HopGoWorld3_Core_Attack or Farm_Sword_Saber or Farm_Bones_UI


                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, QuestLv)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                            end
                        elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                            -- CheckLevel()
                            if game.Workspace.Enemies:FindFirstChild(Ms) then
                                local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                                for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                    if v.Name == Ms and v.Humanoid.Health > 0 then
                                        StatrMagnet = true
                                        if v.Humanoid:FindFirstChild("Animator") then
                                            v.Humanoid.Animator:Destroy()
                                        end
                                        weapon()
                                        repeat wait(_G.Fast_Delay)
                                            EquipWeapon(_G.Weapon)
                                            _G.PosMon = v.HumanoidRootPart.CFrame
                                            v.Humanoid:ChangeState(11)
                                            v.Humanoid.JumpPower = 0
                                            v.Humanoid.WalkSpeed = 0
                                            v.HumanoidRootPart.CanCollide = false
                                            if not game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then 
                                                TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,10))
                                            elseif game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then 
                                                TP(v.HumanoidRootPart.CFrame*CFrame.new(0,0,-5))
                                            end
                                            AttackNoCD()
                                        until game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false or not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) or not v.Parent or v.Humanoid.Health <= 0 or not Auto_Farm
                                        if Lv >= 100 and not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) then
                                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                        end
                                    end
                                end
                            else
                                TP(CFrameMon)
                            end
                        end
                    end
                end)
            end
        end)
    -- FarmLevel
    
    -- FarmItemInventroy
        spawn(function()
            while wait(1) do
                if FarmItemInventroy then
                    pcall(function()
                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) ~= 0 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) ~= 1 then
                            -- Magma Ore
                                if Lv >= 330 and not Check_Inventory_Item("Magma Ore", 20) and Old_World and not Farm_Item_Inventroy_Fish_Tail and not Farm_Sword_Saber then 
                                    Farm_Item_Inventroy_Magma_Ore = true
                                    if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                                        StatrMagnet = false
                                        repeat wait(1)
                                            TP(CFrame.new(-5316.1157226563, 12.262831687927, 8517.00390625))
                                        until (CFrame.new(-5316.1157226563, 12.262831687927, 8517.00390625).Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", "MagmaQuest", 2)
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                        SelectMonster = nil
                                    elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                                        if game.Workspace.Enemies:FindFirstChild("Military Spy [Lv. 325]") then
                                            for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                                if v.Name == "Military Spy [Lv. 325]" and v.Humanoid.Health > 0 then
                                                    StatrMagnet = true
                                                    if v.Humanoid:FindFirstChild("Animator") then
                                                        v.Humanoid.Animator:Destroy()
                                                    end
                                                    weapon()
                                                    Ms = "Military Spy [Lv. 325]"
                                                    _G.PosMon = v.HumanoidRootPart.CFrame
                                                    v.Humanoid:ChangeState(11)
                                                    v.Humanoid.JumpPower = 0
                                                    v.Humanoid.WalkSpeed = 0
                                                    v.HumanoidRootPart.CanCollide = false
                                                    repeat wait(_G.Fast_Delay)
                                                        EquipWeapon(_G.Weapon)
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,10))
                                                        AttackNoCD()
                                                    until not v.Parent or v.Humanoid.Health <= 0 or Check_Inventory_Item("Magma Ore", 20) or game:GetService('Players').LocalPlayer.Character.Humanoid.Health == 0
                                                    if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Military Spy") then
                                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                                    end
                                                    if Check_Inventory_Item("Magma Ore", 20) then
                                                        StatrMagnet = false
                                                        Farm_Item_Inventroy_Magma_Ore = false
                                                    end
                                                end
                                            end
                                        else
                                            TP(CFrame.new(-5984.0532226563, 82.14656829834, 8753.326171875))
                                        end
                                    end
                                elseif Lv >= 330 and not Old_World and not Check_Inventory_Item("Magma Ore", 20) then
                                    TP_World1()
                                elseif Check_Inventory_Item("Magma Ore", 20) and Farm_Item_Inventroy_Magma_Ore then
                                    Farm_Item_Inventroy_Magma_Ore = false
                                end
                            -- Magma Ore

                            -- Fish Tail   
                                if Lv >= 400 and not Check_Inventory_Item("Fish Tail", 20) and Old_World and not Farm_Item_Inventroy_Magma_Ore and not Farm_Sword_Saber then 
                                    Farm_Item_Inventroy_Fish_Tail = true
                                    if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                                        Farm_Item_Inventroy_Fish_Tail = true
                                        StatrMagnet = false
                                        repeat wait(1)
                                            if (CFrame.new(60844.10546875, 98.462875366211, 1298.3985595703).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 3000 then
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
                                            end
                                            TP(CFrame.new(61122.65234375, 18.497442245483, 1569.3997802734))
                                        until (CFrame.new(61122.65234375, 18.497442245483, 1569.3997802734).Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", "FishmanQuest", 1)
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                        SelectMonster = nil
                                    elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                                        if game.Workspace.Enemies:FindFirstChild("Fishman Warrior [Lv. 375]") then
                                            for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                                if v.Name == "Fishman Warrior [Lv. 375]" and v.Humanoid.Health > 0 then
                                                    StatrMagnet = true
                                                    if v.Humanoid:FindFirstChild("Animator") then
                                                        v.Humanoid.Animator:Destroy()
                                                    end
                                                    weapon()
                                                    Ms = "Fishman Warrior [Lv. 375]"
                                                    _G.PosMon = v.HumanoidRootPart.CFrame
                                                    v.Humanoid:ChangeState(11)
                                                    v.Humanoid.JumpPower = 0
                                                    v.Humanoid.WalkSpeed = 0
                                                    v.HumanoidRootPart.CanCollide = false
                                                    repeat wait(_G.Fast_Delay)
                                                        EquipWeapon(_G.Weapon)
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,10))
                                                        AttackNoCD()
                                                    until not v.Parent or v.Humanoid.Health <= 0 or Check_Inventory_Item("Fish Tail", 20) or game:GetService('Players').LocalPlayer.Character.Humanoid.Health == 0
                                                    if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Fishman Warrior") then
                                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                                    end
                                                    if Check_Inventory_Item("Fish Tail", 20) then
                                                        StatrMagnet = false
                                                        Auto_Farm = true
                                                        Farm_Item_Inventroy_Fish_Tail = false
                                                    end
                                                end
                                            end
                                        else
                                            if (CFrame.new(60844.10546875, 98.462875366211, 1298.3985595703).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 3000 then
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
                                            end
                                            TP(CFrame.new(60844.10546875, 98.462875366211, 1298.3985595703))
                                        end
                                    end
                                elseif Lv >= 400 and not Old_World and not Check_Inventory_Item("Fish Tail", 20) then
                                    TP_World1()
                                elseif Check_Inventory_Item("Fish Tail", 20) and Farm_Item_Inventroy_Fish_Tail then
                                    Farm_Item_Inventroy_Fish_Tail = false
                                end
                            -- Fish Tail

                            -- Mystic Droplet
                                if Lv >= 1450 and not Check_Inventory_Item("Mystic Droplet", 10) and New_World and Check_Inventory_Item("Magma Ore", 20) and Check_Inventory_Item("Fish Tail", 20) then
                                    Farm_Item_Inventroy_Mystic_Droplet = true
                                    if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                                        StatrMagnet = false
                                        repeat wait(1)
                                            TP(CFrame.new(-3054.5827636719, 236.87213134766, -10147.790039063))
                                        until (CFrame.new(-3054.5827636719, 236.87213134766, -10147.790039063).Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", "ForgottenQuest", 2)
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                        SelectMonster = nil
                                    elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                                        if game.Workspace.Enemies:FindFirstChild("Water Fighter [Lv. 1450]") then
                                            for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                                if v.Name == "Water Fighter [Lv. 1450]" and v.Humanoid.Health > 0 then
                                                    StatrMagnet = true
                                                    if v.Humanoid:FindFirstChild("Animator") then
                                                        v.Humanoid.Animator:Destroy()
                                                    end
                                                    
                                                    weapon()
                                                    Ms = "Water Fighter [Lv. 1450]"
                                                    _G.PosMon = v.HumanoidRootPart.CFrame
                                                    v.Humanoid:ChangeState(11)
                                                    v.Humanoid.JumpPower = 0
                                                    v.Humanoid.WalkSpeed = 0
                                                    v.HumanoidRootPart.CanCollide = false
                                                    repeat wait(_G.Fast_Delay)
                                                        EquipWeapon(_G.Weapon)
                                                        
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,10))
                                                        AttackNoCD()
                                                    until not v.Parent or v.Humanoid.Health <= 0 or Check_Inventory_Item("Mystic Droplet", 10)
                                                    if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Water Fighter") then
                                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                                    end
                                                    if Check_Inventory_Item("Mystic Droplet", 10) then
                                                        StatrMagnet = false
                                                        Farm_Item_Inventroy_Mystic_Droplet = false
                                                    end
                                                end
                                            end
                                        else
                                            TP(CFrame.new(-3262.9301757813, 298.69036865234, -10552.529296875))
                                        end
                                    end
                                elseif Lv >= 1450 and not New_World and not Check_Inventory_Item("Mystic Droplet", 10) and Check_Inventory_Item("Magma Ore", 20) and Check_Inventory_Item("Fish Tail", 20)  then
                                    TP_World2()
                                elseif Check_Inventory_Item("Mystic Droplet", 10) and Farm_Item_Inventroy_Mystic_Droplet then
                                    Farm_Item_Inventroy_Mystic_Droplet = false
                                end
                            -- Mystic Droplet

                            -- Dragon Scale
                                if Lv >= 1600 and not Check_Inventory_Item("Dragon Scale", 10) and Three_World then 
                                    Farm_Item_Inventroy_Dragon_Scale = true
                                    if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                                        StatrMagnet = false
                                        repeat wait(1)
                                            TP(CFrame.new(5833.1147460938, 51.60498046875, -1103.0693359375))
                                        until (CFrame.new(5833.1147460938, 51.60498046875, -1103.0693359375).Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", "AmazonQuest", 2)
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                        SelectMonster = nil
                                    elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                                        if game.Workspace.Enemies:FindFirstChild("Dragon Crew Archer [Lv. 1600]") then
                                            for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                                if v.Name == "Dragon Crew Archer [Lv. 1600]" and v.Humanoid.Health > 0 then
                                                    StatrMagnet = true
                                                    if v.Humanoid:FindFirstChild("Animator") then
                                                        v.Humanoid.Animator:Destroy()
                                                    end
                                                    
                                                    weapon()
                                                    Ms = "Dragon Crew Archer [Lv. 1600]"
                                                    _G.PosMon = v.HumanoidRootPart.CFrame
                                                    v.Humanoid:ChangeState(11)
                                                    v.Humanoid.JumpPower = 0
                                                    v.Humanoid.WalkSpeed = 0
                                                    v.HumanoidRootPart.CanCollide = false
                                                    repeat wait(_G.Fast_Delay)
                                                        EquipWeapon(_G.Weapon)
                                                        
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,10))
                                                        AttackNoCD()
                                                    until not v.Parent or v.Humanoid.Health <= 0 or Check_Inventory_Item("Dragon Scale", 10)
                                                    if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Dragon Crew Archer") then
                                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                                    end
                                                    if Check_Inventory_Item("Dragon Scale", 10) then
                                                        StatrMagnet = false
                                                        Farm_Item_Inventroy_Dragon_Scale = false
                                                    end
                                                end
                                            end
                                        else
                                            TP(CFrame.new(6831.1171875, 441.76708984375, 446.58615112305))
                                        end
                                    end
                                elseif Lv >= 1600 and not Three_World and not Check_Inventory_Item("Dragon Scale", 10) and Check_Inventory_Item("Mystic Droplet", 10) and Check_Inventory_Item("Magma Ore", 20) and Check_Inventory_Item("Fish Tail", 20) and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check") == 1 then
                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou")
                                elseif Check_Inventory_Item("Dragon Scale", 10) and Farm_Item_Inventroy_Dragon_Scale then
                                    Farm_Item_Inventroy_Dragon_Scale = false
                                end
                            -- Dragon Scale
                        end
                    end)
                end
            end
        end)
    -- FarmItemInventroy

    -- FarmHakiV2
        -- Farm Haki    
            spawn(function()
                while wait(2) do
                    if Farm_Haki_Max5K and Three_World then
                        if CheckHaki() < 5000 then
                            if game.Workspace.Enemies:FindFirstChild("Pirate Millionaire [Lv. 1500]") then
                                for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                    if v.Name == "Pirate Millionaire [Lv. 1500]" and v.Humanoid.Health > 0 then
                                        if v.Humanoid:FindFirstChild("Animator") then
                                            v.Humanoid.Animator:Destroy()
                                        end
                                        repeat wait(_G.Fast_Delay)
                                            
                                            TP(v.HumanoidRootPart.CFrame*CFrame.new(0,0,-2))
                                        until not v.Parent or v.Humanoid.Health <= 0 or not Farm_Haki_Max5K or not game.Players.LocalPlayer.Character:FindFirstChild("Highlight")
                                        if not game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then 
                                            HopServer()
                                        end
                                    end
                                end
                            else
                                TP(CFrame.new(-435.68109130859, 189.69866943359, 5551.0756835938))
                            end
                        elseif CheckHaki() >= 5000 then
                            Auto_Quest_HakiV2 = true
                            Farm_Haki_Max5K = false
                        end
                    end
                end
            end)
        -- Farm Haki

        -- Quest Haki V2
            spawn(function()
                while wait(1) do
                    if Auto_Quest_HakiV2 and Three_World then
                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                        if Lv >= 1800 then
                            if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Start") ~= 0 then
                                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen") == 3 then
                                    if not game.Players.LocalPlayer.Backpack:FindFirstChild("Pineapple") and not game.Players.LocalPlayer.Character:FindFirstChild("Pineapple") then
                                        for i,v in pairs(game.Workspace.PineappleSpawner:GetChildren()) do
                                            if v.Name == "Pineapple" then
                                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                                                wait(1)
                                            end
                                        end
                                    elseif not game.Players.LocalPlayer.Backpack:FindFirstChild("Banana") and not game.Players.LocalPlayer.Character:FindFirstChild("Banana") then
                                        for i,v in pairs(game.Workspace.BananaSpawner:GetChildren()) do
                                            if v.Name == "Banana" then
                                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                                                wait(1)
                                            end
                                        end
                                    elseif not game.Players.LocalPlayer.Backpack:FindFirstChild("Apple") and not game.Players.LocalPlayer.Character:FindFirstChild("Apple") then
                                        for i,v in pairs(game.Workspace.AppleSpawner:GetChildren()) do
                                            if v.Name == "Apple" then
                                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                                                wait(1)
                                            end
                                        end
                                    end
                                    
                                    if not game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Fruit Bowl") and not game:GetService("Players").LocalPlayer.Character:FindFirstChild("Fruit Bowl") then
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Start")
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Buy")
                                    end

                                    if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Fruit Bowl") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Fruit Bowl") then
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Start")
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Buy")
                                    end  
                                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen") == 2 then
                                    print(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen"))
                                    TP(CFrame.new(-12512.6533203125, 336.21612548828125, -9872.841796875))
                                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen") == 1 then
                                    print(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen"), " Captain Elephant [Lv. 1875] [Boss]")
                                    if game.Workspace.Enemies:FindFirstChild("Captain Elephant [Lv. 1875] [Boss]") then
                                        for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                            if v.Name == "Captain Elephant [Lv. 1875] [Boss]" and v.Humanoid.Health > 0 then
                                                weapon()
                                                repeat wait(_G.Fast_Delay)
                                                    EquipWeapon(_G.Weapon)
                                                    TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,0))
                                                    AttackNoCD()
                                                until v.Humanoid.Health <= 0 or not v.Parent or Auto_Quest_HakiV2 == false
                                            end
                                        end
                                    else
                                        TP(CFrame.new(-13400.6103515625, 317.9277648925781, -8409.9853515625)*CFrame.new(0,200,0))
                                    end
                                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen") == 0 then
                                    print(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen"))
                                    if LIPO == nil then
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest","CitizenQuest",1)
                                        LIPO = true
                                    end
                                    if game.Workspace.Enemies:FindFirstChild("Forest Pirate [Lv. 1825]") then
                                        for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                            if v.Name == "Forest Pirate [Lv. 1825]" and v.Humanoid.Health > 0 then
                                                if v.Humanoid:FindFirstChild("Animator") then
                                                    v.Humanoid.Animator:Destroy()
                                                end
                                                weapon()
                                                StatrMagnet = true
                                                repeat wait(_G.Fast_Delay)
                                                    _G.PosMon = v.HumanoidRootPart.CFrame
                                                    EquipWeapon(_G.Weapon)
                                                    v.Humanoid:ChangeState(11)
                                                    v.Humanoid.JumpPower = 0
                                                    v.Humanoid.WalkSpeed = 0
                                                    v.HumanoidRootPart.CanCollide = false
                                                    
                                                    TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,0))
                                                    AttackNoCD()
                                                until v.Humanoid.Health <= 0 or not v.Parent or Auto_Quest_HakiV2 == false
                                                StatrMagnet = false
                                            end
                                        end
                                    else
                                        TP(CFrame.new(-13351.8, 332.378, -7784.83)*CFrame.new(0,30,0))
                                    end
                                end
                            elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Start") == 0 then
                                Auto_Quest_HakiV2 = false
                            end
                        end
                    end
                end
            end)
        -- Quest Haki V2
    -- FarmHakiV2
    
    -- FarmBones
        spawn(function()
            while wait(1) do
                if Farm_Bones and Three_World then
                    pcall(function()
                        local CheckBones = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Bones","Check")
                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                        if Lv >= 1975 and tonumber(CheckBones) < 5000 and not Raid_Fruit_Fragments and not Auto_Raid and not Farm_Item_Inventroy_Magma_Ore and not Farm_Item_Inventroy_Fish_Tail and not Farm_Item_Inventroy_Mystic_Droplet and  
                        not Farm_Item_Inventroy_Dragon_Scale and not Attack_Boss_Dragon_Talon and not Attack_Boss_Electric_Claw and not Attack_Boss_Shakman_Karate and not Attack_Boss_Death_Step and not HopGoWorld3_Core_Attack and
                        not Farm_Sword_Saber  then 
                            if not CheckSword("Hallow Scythe") and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) ~= 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) ~= 1 then
                                Farm_Bones_UI = true
                                if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                                    StatrMagnet = false
                                    repeat wait(1)
                                        TP(CFrame.new(-9480.80762, 142.130661, 5566.37305))
                                    until (CFrame.new(-9480.80762, 142.130661, 5566.37305).Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 or not Farm_Bones or not Auto_Farm or Raid_Fruit_Fragments or Auto_Raid or 
                                    Farm_Item_Inventroy_Magma_Ore or Farm_Item_Inventroy_Fish_Tail or Farm_Item_Inventroy_Mystic_Droplet or Farm_Item_Inventroy_Dragon_Scale or Attack_Boss_Dragon_Talon or Attack_Boss_Electric_Claw or 
                                    Attack_Boss_Shakman_Karate or Attack_Boss_Death_Step or HopGoWorld3_Core_Attack or Farm_Sword_Saber

                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", "HauntedQuest1", 1)
                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                    SelectMonster = nil
                                elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                                    -- CheckLevel()
                                    if game.Workspace.Enemies:FindFirstChild("Reborn Skeleton [Lv. 1975]") then
                                        for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                            if v.Name == "Reborn Skeleton [Lv. 1975]" and v.Humanoid.Health > 0 then
                                                StatrMagnet = true
                                                if v.Humanoid:FindFirstChild("Animator") then
                                                    v.Humanoid.Animator:Destroy()
                                                end
                                                Ms = "Reborn Skeleton [Lv. 1975]"
                                                _G.PosMon = v.HumanoidRootPart.CFrame
                                                v.Humanoid:ChangeState(11)
                                                v.Humanoid.JumpPower = 0
                                                v.Humanoid.WalkSpeed = 0
                                                v.HumanoidRootPart.CanCollide = false
                                                weapon()
                                                
                                                repeat wait(_G.Fast_Delay)
                                                    EquipWeapon(_G.Weapon)
                                                    if not game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then 
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,10))
                                                    elseif game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then 
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,0,-5))
                                                    end
                                                    AttackNoCD()
                                                until game:GetService('Players').LocalPlayer.Character.Humanoid.Health <= 0 or game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false or not v.Parent or v.Humanoid.Health <= 0 or not Farm_Bones
                                                if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Reborn Skeleton") then
                                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                                end
                                            end
                                        end
                                    else
                                        TP(CFrame.new(-8760.26, 142.105, 5979.31))
                                    end
                                end
                            else
                                Farm_Bones_UI = false
                                Farm_Bones = false
                            end
                        end
                    end)
                end
            end
        end)
    -- FarmBones

    -- Attack Core Fruit
        spawn(function()
            while wait(1) do
                if HopGoWorld3_Core and New_World then
                    local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                    if Lv >= 700 then
                        if game.Workspace.Enemies:FindFirstChild("Core") or game.ReplicatedStorage:FindFirstChild("Core") then
                            HopGoWorld3_Core_Attack = true
                            if game.Workspace.Enemies:FindFirstChild("Core") then
                                for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                    if v.Name == "Core" and v.Humanoid.Health > 0 then
                                        weapon()
                                        repeat wait(_G.Fast_Delay)
                                            EquipWeapon(_G.Weapon)
                                            TP(CFrame.new(402.404296875, 182.53373718262, -414.73394775391))
                                            AttackNoCD()
                                        until game:GetService('Players').LocalPlayer.Character.Humanoid.Health <= 0 or v.Humanoid.Health <= 0 or not game.Workspace.Enemies:FindFirstChild("Core") or not game.ReplicatedStorage:FindFirstChild("Core") or not HopGoWorld3_Core or not HopGoWorld3_Core_Attack
                                    end
                                end
                            else
                                TP(CFrame.new(402.404296875, 182.53373718262, -414.73394775391))
                            end
                        elseif not game.Workspace.Enemies:FindFirstChild("Core") and not game.ReplicatedStorage:FindFirstChild("Core") then
                            HopGoWorld3_Core_Attack = false
                        end
                    end
                end 
            end
        end)
    -- Attack Core Fruit

    -- FarmMastery
        -- ตัวแปลสำคัญ Mastery
            spawn(function()
                local gg = getrawmetatable(game)
                local old = gg.__namecall
                setreadonly(gg,false)
                gg.__namecall = newcclosure(function(...)
                    local method = getnamecallmethod()
                    local args = {...}
                    if tostring(method) == "FireServer" then
                        if tostring(args[1]) == "RemoteEvent" then
                            if tostring(args[2]) ~= "true" and tostring(args[2]) ~= "false" then
                                if UseSkillMasteryDevilFruit then
                                    args[2] = PositionSkillMasteryDevilFruit
                                    return old(unpack(args))
                                end
                            end
                        end
                    end
                    return old(...)
                end)
            end)
        -- ตัวแปลสำคัญ Mastery

        -- Auto Farm Mastery
            spawn(function()
                while wait(1) do
                    if Attack_Auto_Farm_Mastery then
                        if game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                            UseSkillMasteryDevilFruit = false
                            if SelectMonster ~= nil then
                                StatrMagnet = false
                                CheckLevel()
                                repeat wait(1)
                                    TP(CFrameQ)
                                until (CFrameQ.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 or not Attack_Auto_Farm_Mastery
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, QuestLv)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                SelectMonster = nil
                            else
                                StatrMagnet = false
                                CheckLevel()
                                repeat wait(1)
                                    TP(CFrameQ)
                                until (CFrameQ.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 or not Attack_Auto_Farm_Mastery
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, QuestLv)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                            end
                        elseif game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                            if game.Workspace.Enemies:FindFirstChild(Ms) then
                                for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                    if v.Name == Ms and v.Humanoid.Health > 0 then
                                        StatrMagnet = true
                                        if v.Humanoid:FindFirstChild("Animator") then
                                            v.Humanoid.Animator:Destroy()
                                        end
                                        weapon()
                                        _G.PosMon = v.HumanoidRootPart.CFrame
                                        v.Humanoid:ChangeState(11)
                                        v.Humanoid.JumpPower = 0
                                        v.Humanoid.WalkSpeed = 0
                                        v.HumanoidRootPart.CanCollide = false
                                        repeat wait(_G.Fast_Delay)
                                            TP(v.HumanoidRootPart.CFrame*CFrame.new(0,30,30))
                                            local health = v.Humanoid.Health
                                            local maxhealth = v.Humanoid.MaxHealth
                                            local percent = (health / maxhealth) * 100
                                            if percent > 11 and v.Humanoid.Health > 0 then
                                                UseSkillMasteryDevilFruit = false
                                                EquipWeapon(_G.Weapon)
                                                game:GetService'VirtualUser':Button1Down(Vector2.new(1280,600))
                                            else
                                                UseSkillMasteryDevilFruit = true
                                                EquipWeapon(game.Players.LocalPlayer.Data.DevilFruit.Value)
                                            end
                                        until game:GetService('Players').LocalPlayer.Character.Humanoid.Health <= 0 or game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == false or not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) or not v.Parent or v.Humanoid.Health <= 0 or not Attack_Auto_Farm_Mastery
                                        if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) then
                                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                        end
                                    end
                                end
                            else
                                UseSkillMasteryDevilFruit = false
                                TP(CFrameMon*CFrame.new(0,30,10))
                            end
                        end
                    end
                end
            end)
        -- Auto Farm Mastery

        -- Auto Skill
            spawn(function()
                while wait(0.05) do
                    if Attack_Auto_Farm_Mastery then
                        pcall(function()
                            CheckLevel()
                            for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                                if v.Name == Ms then
                                    local health = v.Humanoid.Health
                                    local maxhealth = v.Humanoid.MaxHealth
                                    local percent = (health / maxhealth) * 100
                                    if percent <= 11 then
                                        wait(2)
                                        PositionSkillMasteryDevilFruit = v.HumanoidRootPart.Position
                                        if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dragon-Dragon") then      
                                            if v.Humanoid.Health > 0 then                     
                                                game:GetService("Players").LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value).MousePos.Value = v.HumanoidRootPart.Position  
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                                            end

                                            if v.Humanoid.Health > 0 then
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                                            end
                                        
                                        elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Venom-Venom") then   
                                            game:GetService("Players").LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value).MousePos.Value = v.HumanoidRootPart.Position  
                                            if v.Humanoid.Health > 0 then
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then            
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then                        
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                                            end
                                        elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Human-Human: Buddha") then
                                            game:GetService("Players").LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value).MousePos.Value = v.HumanoidRootPart.Position 
                                            if game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Size == Vector3.new(2, 2.02, 1) then                       
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                                            end
                                            if v.Humanoid.Health > 0 then                       
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then                         
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"V",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"V",false,game)
                                            end
                                        elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild(game:GetService("Players").LocalPlayer.Data.DevilFruit.Value) then
                                            game:GetService("Players").LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value).MousePos.Value = v.HumanoidRootPart.Position 
                                            if v.Humanoid.Health > 0 then                       
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then                         
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then                          
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                                            end
                                            
                                            if v.Humanoid.Health > 0 then
                                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"V",false,game)
                                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"V",false,game)
                                            end
                                        end
                                    end
                                end
                            end
                        end)
                    end
                end
            end)
        -- Auto Skill
    -- FarmMastery

    -- BringMonster
        spawn(function()
            while wait(1) do
                pcall(function()
                    if StatrMagnet then
                        for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                            if v.Name == Ms and (v.HumanoidRootPart.Position-_G.PosMon.Position).Magnitude <= 350 then
                                if v.Humanoid:FindFirstChild("Animator") then
                                    v.Humanoid.Animator:Destroy()
                                end
                                v.Humanoid:ChangeState(11)
                                v.Humanoid.JumpPower = 0
                                v.Humanoid.WalkSpeed = 0
                                v.HumanoidRootPart.CanCollide = false
                                
                                v.HumanoidRootPart.CFrame = _G.PosMon
                                sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius",  math.huge)
                                -- sethiddenproperty(game.Players.LocalPlayer, "MaximumSimulationRadius",  math.huge)
                                -- sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius",  9e20)
                            end
                        end
                    end
                end)
            end
        end)
    -- BringMonster

    -- AutoStatus    
        spawn(function()
            while wait(0) do 
                if Auto_Farm then
                    if game:GetService("Players").LocalPlayer.Data.Points.Value > 0 then
                        local StatsMelee = game.Players.LocalPlayer.Data.Stats.Melee.Level.Value
                        local StatsDefense = game.Players.LocalPlayer.Data.Stats.Defense.Level.Value
                        local StatsSword = game.Players.LocalPlayer.Data.Stats.Sword.Level.Value
                        local StatsGun = game.Players.LocalPlayer.Data.Stats.Gun.Level.Value
                        local StatsFruit = game.Players.LocalPlayer.Data.Stats["Demon Fruit"].Level.Value

                        if StatsMelee < 2400 then
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Melee", 1)
                        elseif StatsMelee == 2400 and StatsDefense < 2400 then 
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Defense", 1)
                        elseif StatsMelee == 2400 and StatsDefense == 2400 and StatsFruit < 800 then 
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Demon Fruit", 1)
                        elseif StatsMelee == 2400 and StatsDefense == 2400 and StatsFruit == 800 and StatsSword < 800 then 
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Sword", 1)
                        elseif StatsMelee == 2400 and StatsDefense == 2400 and StatsFruit == 800 and StatsSword == 800 and StatsGun < 800 then 
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Gun", 1)
                        end
                    else
                        wait(1)
                    end
                end
            end
        end)
    -- AutoStatus

    -- Open Haki
        spawn(function()
            while wait(1) do
                if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
                else
                    wait(7)
                end
                if not game.Players.LocalPlayer.Character:FindFirstChild("Highlight") then 
                    wait(300)
                    N:SendKeyEvent(true,"K",false,game)
                else
                    wait(7)
                end
            end
        end)
    -- Open Haki

    -- BuyWeapon
        spawn(function()
            while wait(5) do
                pcall(function()
                    if Buy_Superhuman then
                        local Check_Level = game.Players.LocalPlayer.Data.Level.Value
                        local Check_Beli = game.Players.LocalPlayer.Data.Beli.Value 
                        local Check_Fragments = game.Players.LocalPlayer.Data.Fragments.Value  
                        if CheckWeaponBC("Combat") then
                            if Check_Beli >= 150000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyBlackLeg")
                                if CheckWeaponBC("Black Leg") then
                                    _G.Weapon = "Black Leg"
                                end
                            end
                        end
                        if CheckWeaponBC("Black Leg") and CheckWeaponBC("Black Leg").Level.Value >= 400 then 
                            if Check_Beli >= 500000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectro")
                                if CheckWeaponBC("Electro") then
                                    _G.Weapon = "Electro"
                                end
                            end
                        end
                        if CheckWeaponBC("Electro") and CheckWeaponBC("Electro").Level.Value >= 400 then
                            if Check_Beli >= 750000 then 
                                print("Fishman Karate")
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyFishmanKarate")
                                if CheckWeaponBC("Fishman Karate") then
                                    _G.Weapon = "Fishman Karate"
                                end
                            end
                        end
                        if CheckWeaponBC("Fishman Karate") and CheckWeaponBC("Fishman Karate").Level.Value >= 400 then
                            if Check_Fragments >= 1500 then
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "1")
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "2")
                                if CheckWeaponBC("Dragon Claw") then
                                    _G.Weapon = "Dragon Claw"
                                end
                            else
                                if Check_Fruit_CBI() then
                                    Raid_Fruit_Fragments = true
                                elseif not Check_Fruit_CBI() and not Raid_Fruit_Fragments and not Auto_Raid  then
                                    HopServer()
                                end
                            end
                        end
                        if CheckWeaponBC("Dragon Claw") and CheckWeaponBC("Dragon Claw").Level.Value >= 400 then
                            if Check_Beli >= 300000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman")
                                CheckMelee_Superhuman = true
                                if CheckWeaponBC("Superhuman") then
                                    _G.Weapon = "Superhuman"
                                end
                            end
                        end
                        if CheckWeaponBC("Superhuman") and CheckWeaponBC("Superhuman").Level.Value >= 400 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep",true) == 0 then
                            if Check_Beli >= 3000000 and Check_Fragments >= 5000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep")
                                CheckMelee_DeathStep = true
                                if CheckWeaponBC("Death Step") then
                                     _G.Weapon = "Death Step"
                                end
                            elseif Check_Fragments < 5000 then 
                                if Check_Fruit_CBI() then
                                    Raid_Fruit_Fragments = true
                                elseif not Check_Fruit_CBI() and not Raid_Fruit_Fragments and not Auto_Raid  then
                                    HopServer()
                                end
                            end
                        end
                        if CheckWeaponBC("Death Step") and CheckWeaponBC("Death Step").Level.Value >= 400 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate",true) == 0 then
                            if Check_Beli >= 3000000 and Check_Fragments >= 5000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate")
                                CheckMelee_ShakmanKarate = true
                                if CheckWeaponBC("Sharkman Karate") then
                                    _G.Weapon = "Sharkman Karate"
                                end
                            elseif Check_Fragments < 5000 then 
                                if Check_Fruit_CBI() then
                                    Raid_Fruit_Fragments = true
                                elseif not Check_Fruit_CBI() and not Raid_Fruit_Fragments and not Auto_Raid  then
                                    HopServer()
                                end
                            end
                        end
                        if CheckWeaponBC("Sharkman Karate") and CheckWeaponBC("Sharkman Karate").Level.Value >= 400 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw",true) == 0 then
                            if Check_Beli >= 3000000 and Check_Fragments >= 5000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
                                CheckMelee_ElectricClaw = true
                                if CheckWeaponBC("Electric Claw") then
                                    _G.Weapon = "Electric Claw"
                                end
                            elseif Check_Fragments < 5000 then 
                                if Check_Fruit_CBI() then
                                    Raid_Fruit_Fragments = true
                                elseif not Check_Fruit_CBI() and not Raid_Fruit_Fragments and not Auto_Raid  then
                                    HopServer()
                                end
                            end
                        end
                        if CheckWeaponBC("Electric Claw") and CheckWeaponBC("Electric Claw").Level.Value >= 400 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon",true) == 0 then
                            if Check_Beli >= 3000000 and Check_Fragments >= 5000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon")
                                CheckMelee_DragonTalon = true
                                if CheckWeaponBC("Dragon Talon") then
                                    _G.Weapon = "Dragon Talon"
                                end
                            elseif Check_Fragments < 5000 then 
                                if Check_Fruit_CBI() then
                                    Raid_Fruit_Fragments = true
                                elseif not Check_Fruit_CBI() and not Raid_Fruit_Fragments and not Auto_Raid then
                                    HopServer()
                                end
                            end
                        end
                        if CheckWeaponBC("Dragon Talon") and CheckWeaponBC("Dragon Talon").Level.Value >= 400 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman",true) == 0 then
                            if Check_Beli >= 3000000 and Check_Fragments >= 5000 then 
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman")
                                CheckMelee_Godhuman = true
                                if CheckWeaponBC("Godhuman") then
                                    _G.Weapon = "Godhuman"
                                end
                            elseif Check_Fragments < 5000 then 
                                if Check_Fruit_CBI() then
                                    Raid_Fruit_Fragments = true
                                elseif not Check_Fruit_CBI() and not Raid_Fruit_Fragments and not Auto_Raid then
                                    HopServer()
                                end
                            end
                        end
                        if CheckWeaponBC("Godhuman") and CheckWeaponBC("Godhuman").Level.Value >= 400 then
                            wait(15)
                            if StatusFruitAwakened() == "✅ Z, X, C, V, F" or StatusFruitAwakened() == "✅ Z, X, C, V" then
                                Auto_Raid = false
                            else
                                if Check_Fruit_CBI() then 
                                    Auto_Raid = true
                                elseif game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false then
                                    HopServer()
                                end
                            end
                        end
                    end
                end)
            end
        end)
    -- BuyWeapon

    -- Attack_WeaponV2
        -- Attack_Death_Step
            spawn(function()
                while true do 
                    wait(10)
                    if Attack_Death_Step and New_World then 
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 3 then
                            if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep") ~= 1 then
                                if game.Players.LocalPlayer.Backpack:FindFirstChild("Library Key") or game.Players.LocalPlayer.Character:FindFirstChild("Library Key") and game:GetService("Workspace").Map.IceCastle.Hall.LibraryDoor:FindFirstChild("PhoeyuDoor") then
                                    Attack_Boss_Death_Step = true
                                    EquipWeapon("Library Key")
                                    repeat wait(1)
                                        TP(CFrame.new(6377.12549, 296.634735, -6843.76025))
                                    until (CFrame.new(6377.12549, 296.634735, -6843.76025).Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 or not Attack_Boss_Death_Step 
                                    wait(3)
                                    Attack_Boss_Death_Step = false
                                else
                                    if game:GetService("ReplicatedStorage"):FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]") then
                                        if not Raid_Fruit_Fragments and not Attack_Boss_Shakman_Karate and not Farm_Item_Inventroy_Mystic_Droplet and not HopGoWorld3_Core_Attack then
                                            Attack_Boss_Death_Step = true 
                                            if game:GetService("Workspace").Enemies:FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]") then
                                                for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                                                    if v.Name == "Awakened Ice Admiral [Lv. 1400] [Boss]" and game:GetService('Players').LocalPlayer.Character.Humanoid.Health > 0  then
                                                        weapon()
                                                        repeat wait(_G.Fast_Delay)
                                                            EquipWeapon(_G.Weapon)
                                                            TP(v.HumanoidRootPart.CFrame * CFrame.new(0,25,15))
                                                            AttackNoCD()
                                                        until not Attack_Boss_Death_Step or game:GetService('Players').LocalPlayer.Character.Humanoid.Health <= 0 or not game:GetService("Workspace").Enemies:FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]").Humanoid.Health <= 0
                                                        Attack_Boss_Death_Step = false
                                                    end
                                                end
                                            else
                                                TP(CFrame.new(6421.53, 295.764, -6924.07)*CFrame.new(0,50,0))
                                            end
                                        end
                                    elseif not game:GetService("ReplicatedStorage"):FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]") and not game:GetService("Workspace").Enemies:FindFirstChild("Awakened Ice Admiral [Lv. 1400] [Boss]") then 
                                        Attack_Boss_Death_Step = false
                                    end
                                end
                            end
                        elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) ~= 3 then
                            Attack_Death_Step = false
                            Attack_Boss_Death_Step = false
                        end
                    end
                end
            end)
        -- Attack_Death_Step

        -- Attack_Shakman_Karate
            spawn(function()
                while true do 
                    wait(15)
                    if Attack_Shakman_Karate and New_World then 
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) ~= 0 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) ~= 1 then
                            if game.Players.LocalPlayer.Character:FindFirstChild("Water Key") or game.Players.LocalPlayer.Backpack:FindFirstChild("Water Key") then
                                Attack_Boss_Shakman_Karate = true 
                                EquipWeapon("Water Key")
                                wait(0.5)
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate",true)
                                wait(2)
                                Attack_Boss_Shakman_Karate = false 
                                Attack_Shakman_Karate = false
                            else
                                if game:GetService("ReplicatedStorage"):FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Tide Keeper [Lv. 1475] [Boss]")  then
                                    if not Attack_Boss_Death_Step and not Auto_Raid and not Raid_Fruit_Fragments and not Farm_Item_Inventroy_Mystic_Droplet and not HopGoWorld3_Core_Attack then
                                        Attack_Boss_Shakman_Karate = true 
                                        if game:GetService("Workspace").Enemies:FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") then
                                            for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                                                if v.Name == "Tide Keeper [Lv. 1475] [Boss]" and game:GetService('Players').LocalPlayer.Character.Humanoid.Health > 0 then
                                                    weapon()
                                                    repeat wait(_G.Fast_Delay)
                                                        EquipWeapon(_G.Weapon)
                                                        TP(v.HumanoidRootPart.CFrame * CFrame.new(0,25,15))
                                                        AttackNoCD()
                                                    until game:GetService('Players').LocalPlayer.Character.Humanoid.Health <= 0 or not game:GetService("Workspace").Enemies:FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Tide Keeper [Lv. 1475] [Boss]").Humanoid.Health <= 0
                                                    Attack_Boss_Shakman_Karate = false 
                                                end
                                            end
                                        else
                                            TP(CFrame.new(-3570.18652, 123.328949, -11555.9072)*CFrame.new(0,50,0))
                                        end
                                    end
                                elseif not game:GetService("ReplicatedStorage"):FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") or not game:GetService("Workspace").Enemies:FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") then
                                    Attack_Boss_Shakman_Karate = false 
                                end
                            end
                        elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate",true) == 1 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate",true) == 0 then
                            Attack_Shakman_Karate = false
                            Attack_Boss_Shakman_Karate = false
                        end
                    end
                end
            end)
        -- Attack_Shakman_Karate

        -- Attack_Electric_Claw
            spawn(function()
                while true do 
                    wait(10)
                    if Attack_Electric_Claw and Three_World then
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) ~= 0 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) ~= 1 then
                            if not Attack_Boss_Dragon_Talon and not Auto_Raid and not Raid_Fruit_Fragments and not Farm_Item_Inventroy_Dragon_Scale then
                                Attack_Boss_Electric_Claw = true 
                                repeat wait(1)
                                    TP(CFrame.new(-10363, 331.654, -10122.6))
                                until (Vector3.new(-10363, 331.654, -10122.6)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 10
                                wait(1)
                                game.ReplicatedStorage.Remotes.CommF_:InvokeServer("BuyElectricClaw", "Start") 
                                wait(1)
                                repeat wait(1)
                                    TP(CFrame.new(-12548, 337, -7481))
                                until (Vector3.new(-12548, 337, -7481)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 10
                                wait(1)
                                Attack_Boss_Electric_Claw = false 
                            end
                        elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 1 then
                            Attack_Electric_Claw = false
                            Attack_Boss_Electric_Claw = false 
                        end
                    end
                end
            end)
        -- Attack_Electric_Claw
        
        -- Attack_Dragon_Talon
            spawn(function()
                while true do 
                    wait(1)
                    if Attack_Dragon_Talon and Three_World then
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 1 then
                            if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) ~= 0 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) ~= 1 then
                                if not Attack_Boss_Electric_Claw and not Auto_Raid and not Raid_Fruit_Fragments then
                                    if game.Players.LocalPlayer.Backpack:FindFirstChild("Fire Essence") or game.Players.LocalPlayer.Character:FindFirstChild("Fire Essence") then
                                        print("Fire Essence")
                                        Attack_Boss_Dragon_Talon = true 
                                        game.ReplicatedStorage.Remotes.CommF_:InvokeServer("BuyDragonTalon",true)
                                    end
                                    wait(1)
                                    Attack_Boss_Dragon_Talon = false 
                                end
                            elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 1 then
                                Attack_Boss_Dragon_Talon = false 
                                Attack_Dragon_Talon = false
                            end
                        end
                    end
                end
            end)
        -- Attack_Dragon_Talon
    -- Attack_WeaponV2
    
    -- AutoRaid
        -- Raid Fruit Fragments
            spawn(function()
                while wait(2) do
                    if Raid_Fruit_Fragments and not Auto_Raid and not HopGoWorld3_Core_Attack and not Attack_Boss_Shakman_Karate and not Attack_Boss_Death_Step and not Attack_Boss_Dragon_Talon and 
                    not Attack_Boss_Electric_Claw and not Attack_Boss_Three_World and not Farm_Item_Inventroy_Magma_Ore and not Farm_Item_Inventroy_Fish_Tail and not Farm_Item_Inventroy_Dragon_Scale and 
                    not Farm_Item_Inventroy_Mystic_Droplet then
                        if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false and Check_Fruit_CBI() then 
                            BuyChip()
							repeat wait(1)
							until game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true
                        elseif game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false and not Check_Fruit_CBI() then 
                            wait(10)
                            Inventory_Fruit = true
                            -- Auto_Farm = true
                            Raid_Fruit_Fragments = false
                        elseif game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true then
                            repeat wait(1)
                            until game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false
                        end
                    end
                end
            end)
        -- Raid Fruit Fragments

        -- Raid 
            spawn(function()
                while wait() do 
                    wait(20)
                    if Auto_Raid and not Raid_Fruit_Fragments and not Farm_Item_Inventroy_Magma_Ore and not Farm_Item_Inventroy_Fish_Tail and not Farm_Item_Inventroy_Mystic_Droplet and not Farm_Item_Inventroy_Dragon_Scale and
                     not Attack_Boss_Dragon_Talon and not Attack_Boss_Electric_Claw and not Attack_Boss_Shakman_Karate and not Attack_Boss_Death_Step and not HopGoWorld3_Core_Attack then 
                        if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false then 
                            BuyChip()
							repeat wait(1)
							until game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true
                        elseif game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true then 
                            repeat wait(1)
                            until game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false
                            wait(3)
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Awakener","Check")
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Awakener","Awaken")
                        end
                    end 
                end
            end)
        -- Raid

        -- AutoRaidSuperHuman 
            spawn(function()
                while wait(1) do 
                    if Auto_Raid then
                        pcall(function()
                            repeat wait()
                                if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true then 
                                    if game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1").CFrame * CFrame.new(0,70,0))
                                        end
                                    end
                                end
                            until not Auto_Raid
                        end)
                    end
                end
            end)
            spawn(function()
                while wait(1) do 
                    if Raid_Fruit_Fragments then
                        repeat wait()
                            pcall(function()
                                if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true then 
                                    if game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2").CFrame * CFrame.new(0,70,0))
                                        end
                                    elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1") then
                                        if (game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1").Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2300 then
                                            TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1").CFrame * CFrame.new(0,70,0))
                                        end
                                    end
                                end
                            end)
                        until not Raid_Fruit_Fragments 
                    end
                end
            end)
        -- AutoRaidSuperHuman 

        -- Kill All
            spawn(function()
                while wait() do 
                    if Auto_Raid or Raid_Fruit_Fragments then 
                        if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true then 
                            for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and (v.HumanoidRootPart.Position-game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 1500 then
                                    pcall(function()
                                        repeat wait()
                                            sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius", math.huge)
                                            v.HumanoidRootPart.Size = Vector3.new(50, 50, 50)
                                            v.HumanoidRootPart.CanCollide = true
                                            v.Humanoid.Health = 0
                                            v.HumanoidRootPart.CanCollide = false
                                            v.Humanoid.Health = 0
                                        until not v.Parent or v.Humanoid.Health <= 0
                                    end)
                                end
                            end
                        end
                    end
                end
            end)
        -- Kill All

        -- Buy Chip AutoRaid
            function BuyChip()
                if Inventory_Fruit then 
                    _G.Stop_Tween = true
                    wait(1)
                    TP(game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame)
                    _G.Stop_Tween = nil
                    Inventory_Fruit = false
                end
                if Check_Fruit_BackPack("Flame-Flame") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Flame")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Ice-Ice") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Ice")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Quake-Quake") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Quake")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Light-Light") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Light")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Dark-Dark") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Dark")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("String-String") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "String")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Rumble-Rumble") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Rumble")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Magma-Magma") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Magma")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                elseif Check_Fruit_BackPack("Human-Human: Buddha") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Human: Buddha")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                else
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                    PutFruitAutoRaid()
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", "Light")
                    wait(5)
                    if New_World then
                        fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
                    elseif Three_World then
                        fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
                    end
                    wait(15)
                end
                if not Inventory_Fruit then 
                    _G.Stop_Tween = true
                    wait(1)
                    TP(game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame)
                    _G.Stop_Tween = nil
                    Inventory_Fruit = true
                end
            end
        -- Buy Chip AutoRaid

        -- function put fruit autoraid
            function PutFruitAutoRaid()
                local fruit = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getInventoryFruits")
                for i,v in pairs(fruit) do
                    if v["Price"] < 1000000 then
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("LoadFruit",v["Name"])
                        _G.Have_Fruit = true
                        return
                    else 
                        _G.Have_Fruit = nil
                    end
                end
            end
        -- function put fruit autoraid
    -- AutoRaid

    -- NoClip
        spawn(function()
            while wait(.1) do
                pcall(function()
                    if Auto_Farm or NoClip then
                        local character = game.Players.LocalPlayer.Character
                        for _, v in pairs(character:GetChildren()) do
                            if v:IsA("BasePart") then
                                v.CanCollide = false
                            end
                        end
                        if game.Players.LocalPlayer.Character.Humanoid.Sit == true then
                            game.Players.LocalPlayer.Character.Humanoid:ChangeState(15)
                            wait(5)
                        end
                        PIO = false
                        if not game.Players.LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyVelocity") then 
                            local L_1 = Instance.new("BodyVelocity")
                            L_1.Name = "BodyGyrozz"
                            L_1.Parent = game.Players.LocalPlayer.Character.HumanoidRootPart 
                            L_1.MaxForce=Vector3.new(1000000000,1000000000,1000000000)
                            L_1.Velocity=Vector3.new(0,0,0) 
                        end
                    elseif PIO == false then
                        for i,v in pairs(game.Players.LocalPlayer.Character.HumanoidRootPart:GetChildren()) do
                            if v.Name == "BodyGyrozz" then
                                v:Destroy()
                                PIO = true
                            end
                        end
                    end
                end)
            end
        end)
    -- NoClip

    -- Random Bones
        spawn(function()
            while wait(5) do 
                if Three_World then
                    if Auto_Farm or Farm_Bones then
                        local B_H = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Bones","Check")
                        if tonumber(B_H) >= 49 then
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Bones","Check")
                            game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Bones", "Buy",1,1)
                        end
                    end
                end
            end
        end)
    -- Random Bones

    -- RedeemCode
        spawn(function()
            while wait(1) do
                if RedeemCode then
                    function UseCode(Text)
                        game:GetService("ReplicatedStorage").Remotes.Redeem:InvokeServer(Text)
                    end
                    UseCode("Sub2Fer999")
                    UseCode("Enyu_is_Pro")
                    UseCode("Magicbus")
                    UseCode("JCWK")
                    UseCode("Starcodeheo")
                    UseCode("Bluxxy")
                    UseCode("THEGREATACE")
                    UseCode("SUB2GAMERROBOT_EXP1")
                    UseCode("StrawHatMaine")
                    UseCode("Sub2OfficialNoobie")
                    UseCode("SUB2NOOBMASTER123")
                    UseCode("Sub2Daigrock")
                    UseCode("Axiore")
                    UseCode("TantaiGaming")
                    UseCode("STRAWHATMAINE")
                    RedeemCode = false
                end
            end
        end)
    -- RedeemCode
-- Farm

-- BuyFruit
    spawn(function()
        while wait(600) do
            if Buy_Fruit_Awakener then 
                local CheckLevel = game.Players.LocalPlayer.Data.Level.Value
                if CheckLevel >= 200 then 
                    local CheckFruits = game.Players.localPlayer.Data.DevilFruit.Value
                    if CheckFruits == "" then 
                        local fruits = {
                            [1] = "Ice-Ice",
                            [2] = "Dark-Dark",
                            [3] = "Light-Light",
                            [4] = "Quake-Quake",
                            [5] = "String-String",
                            [6] = "Rumble-Rumble",
                            [7] = "Human-Human: Buddha"
                        }
                        local Check_Sell_Fruit = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("GetFruits")
                        for i,v in pairs(Check_Sell_Fruit) do
                            for _,FruitBuy in pairs(fruits) do
                                if v.OnSale then
                                    if v.Name == FruitBuy then
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("PurchaseRawFruit", v.Name)
                                    end
                                end
                            end
                        end
                    else
                        Buy_Fruit_Awakener = false
                    end
                end
            end
        end
    end)
-- BuyFruit

-- World 2 - 3
    -- GoTo World 2
        -- Attack Boss World Two
            spawn(function()
                while wait(1) do
                    if World2 and Old_World then
                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                        local Beli = game:GetService("Players").LocalPlayer.Data.Beli.Value
                        local FG = game:GetService("Players").LocalPlayer.Data.Fragments.Value
                        if Lv >= 700 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("DressrosaQuestProgress","Detective") ~= 2 then
                            if Auto_Farm then
                                Auto_Farm = false
                            end
                            if not Auto_Farm then
                                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("DressrosaQuestProgress","Detective") == 1 then
                                else
                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("DressrosaQuestProgress","Detective")
                                    wait(2)
                                end
                                repeat wait()
                                    EquipWeapon("Key")
                                    TP( CFrame.new(1347.32947, 37.349369, -1325.44922))
                                until (Vector3.new(1347.32947, 37.349369, -1325.44922)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 1 or World2 == false or not game:GetService("Workspace").Map.Ice.Door
                                wait(3)
                                for i,v in pairs(game.workspace.Enemies:GetChildren()) do
                                    if v.Name == "Ice Admiral [Lv. 700] [Boss]" then
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                        weapon()
                                        repeat wait(_G.Fast_Delay)
                                            EquipWeapon(_G.Weapon)
                                            v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                            TP(v.HumanoidRootPart.CFrame*CFrame.new(0,25,15))
                                            AttackNoCD()
                                        until v.Humanoid.Health <= 0 or not v.Parent or World2 == false
                                        wait(2)
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")
                                        wait(25)
                                    end
                                end
                            end
                        end
                    end
                end
            end)
        -- Attack Boss World Two
        
        -- Auto_Two_World Go 
            spawn(function()
                while wait(60) do
                    if World2 and New_World then
                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("DressrosaQuestProgress","Detective") == 2 then
                            if Lv >= 700 and Check_Inventory_Item_Name("Fish Tail") >= 20 and Check_Inventory_Item_Name("Magma Ore") >= 20 then 
                                TP_World2()
                            end
                        end
                    end
                end
            end)
        -- Auto_Two_World Go
    -- GoTo World 2

    -- GoTo World 3
        -- Bartlio_Farm
            spawn(function()
                while wait(1) do
                    pcall(function()
                        if Bartlio_Farm and New_World then
                            local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                            if Lv >= 900 then
                                if Auto_Farm and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") ~= 3 then
                                    Auto_Farm = false
                                elseif Auto_Farm and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 3 then
                                    Bartlio_Farm = false
                                end
                                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 0 then
                                    if string.find(game.Players.LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Swan Pirates") and string.find(game.Players.LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "50") and game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                                        if game.workspace.Enemies:FindFirstChild("Swan Pirate [Lv. 775]") then
                                            for i,v in pairs(game.workspace.Enemies:GetChildren()) do
                                                if v.Name == "Swan Pirate [Lv. 775]" then
                                                    _G.PosMon = v.HumanoidRootPart.CFrame
                                                    StatrMagnet = true
                                                    weapon()
                                                    repeat wait(_G.Fast_Delay)
                                                        EquipWeapon(_G.Weapon)
                                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,25,0))
                                                        AttackNoCD()
                                                    until v.Humanoid.Health <= 0 or not v.Parent or Bartlio_Farm ==false
                                                    StatrMagnet = false
                                                end
                                            end
                                        else
                                            TP(CFrame.new(1065.3669433594, 137.64012145996, 1324.3798828125)*CFrame.new(0,20,0))
                                        end
                                    else
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest","BartiloQuest",1)
                                    end
                                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 1 then
                                    _G.SelectBoss = "Jeremy [Lv. 850] [Boss]"
                                    if game.workspace.Enemies:FindFirstChild(_G.SelectBoss) or game:GetService("ReplicatedStorage"):FindFirstChild(_G.SelectBoss) then
                                        for i,v in pairs(game.workspace.Enemies:GetChildren()) do
                                            if v.Name == _G.SelectBoss then
                                                weapon()
                                                repeat wait(_G.Fast_Delay)
                                                    EquipWeapon(_G.Weapon)
                                                    v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                                    TP(v.HumanoidRootPart.CFrame*CFrame.new(0,25,0))
                                                    AttackNoCD()
                                                until v.Humanoid.Health <= 0 or not v.Parent or Bartlio_Farm == false
                                            else 
                                                TP(game:GetService("ReplicatedStorage"):FindFirstChild(_G.SelectBoss).HumanoidRootPart.CFrame*CFrame.new(0,20,0))
                                            end
                                        end
                                    else
                                        HopServer()
                                    end
                                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 2 then
                                    repeat wait()
                                        TP(CFrame.new(-1836, 11, 1714))
                                    until (Vector3.new(-1836, 11, 1714)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1836, 11, 1714)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1850.49329, 13.1789551, 1750.89685)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1858.87305, 19.3777466, 1712.01807)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1803.94324, 16.5789185, 1750.89685)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1858.55835, 16.8604317, 1724.79541)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1869.54224, 15.987854, 1681.00659)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1800.0979, 16.4978027, 1684.52368)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1819.26343, 14.795166, 1717.90625)
                                    wait(1)
                                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1813.51843, 14.8604736, 1724.79541)
                                    wait(2)
                                    Auto_Farm = true
                                    Bartlio_Farm = false
                                end
                            end
                        end
                    end)
                end
            end)
        -- Bartlio_Farm
        
        -- ตัวแปร
            local function split(str, sep)
                local result = {}
                local regex = ("([^%s]+)"):format(sep)
                for each in str:gmatch(regex) do
                table.insert(result, each)
                end
                return result
            end
        -- ตัวแปร

        -- Don_Swan_Quest
            spawn(function()
                while wait(1) do
                    pcall(function()
                        if Don_Swan_Quest then
                            if New_World then
                                local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                                if Lv >= 1200 and Check_Fruit_CBIPrice1M() then
                                    if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TalkTrevor","1") ~= 0 then
                                        local fruit = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getInventoryFruits")
                                        for i,v in pairs(fruit) do
                                            if v["Price"] >= 1000000 then
                                                if Auto_Farm then
                                                    Auto_Farm = false
                                                end
                                                wait(2)
                                                Inventory_Fruit = false
                                                wait(1)
                                                local NameFruit = split(v["Name"], "-")[2] .. " Fruit"
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("LoadFruit",v["Name"])
                                                wait(2)
                                                EquipWeapon(NameFruit)
                                                wait(2)
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TalkTrevor","1")
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TalkTrevor","2")
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TalkTrevor","3")
                                                wait(2)
                                                Inventory_Fruit = true
                                                Auto_Farm = true
                                                Don_Swan_Quest = false
                                            end
                                        end
                                    end
                                end
                            end
                        else
                            wait(2)
                        end
                    end)
                end
            end)
        -- Don_Swan_Quest

        -- Auto_Three_World
            spawn(function()
                while wait(1) do
                    -- pcall(function()
                        if Auto_Three_World and New_World then
                            local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                            if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check") ~= 1 then
                                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TalkTrevor","1") == 0 and not Raid_Fruit_Fragments and not Auto_Raid and not Attack_Boss_Shakman_Karate and not Attack_Boss_Death_Step and not Farm_Item_Inventroy_Mystic_Droplet then
                                    if Lv >= 1500 then
                                        if Auto_Farm then
                                            Attack_Boss_Three_World = true
                                            Attack_Boss_Don_Swan = true
                                            Auto_Farm = false
                                            _G.Stop_Tween = true
                                            wait(0.1)
                                            _G.Stop_Tween = nil
                                        end
                                        if Attack_Boss_Don_Swan then
                                            if game.Workspace.Enemies:FindFirstChild("Don Swan [Lv. 1000] [Boss]") or game.ReplicatedStorage:FindFirstChild("Don Swan [Lv. 1000] [Boss]") then
                                                if game.Workspace.Enemies:FindFirstChild("Don Swan [Lv. 1000] [Boss]") then
                                                    for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                                        if v.Name == "Don Swan [Lv. 1000] [Boss]" and v.Humanoid.Health > 0 then
                                                            weapon()
                                                            repeat wait(_G.Fast_Delay)
                                                                EquipWeapon(_G.Weapon)
                                                                TP(v.HumanoidRootPart.CFrame*CFrame.new(0,20,10))
                                                                AttackNoCD()
                                                            until v.Humanoid.Health <= 0 or not v.Parent or not Attack_Boss_Don_Swan
                                                            Attack_Boss_Rip_Indra = true
                                                            Attack_Boss_Don_Swan = false
                                                        end
                                                    end
                                                else
                                                    TP(CFrame.new(2289.77, 15.152, 805.584)*CFrame.new(0,30,0))
                                                end
                                            else
                                                if not game.Workspace.Enemies:FindFirstChild("Don Swan [Lv. 1000] [Boss]") and not game.ReplicatedStorage:FindFirstChild("Don Swan [Lv. 1000] [Boss]") then
                                                    HopServer()
                                                    wait(50)
                                                end
                                            end
                                        end
                                        if Attack_Boss_Rip_Indra then
                                            if game:GetService("Workspace").Enemies:FindFirstChild("rip_indra [Lv. 1500] [Boss]") then
                                                for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                                    if v.Name == "rip_indra [Lv. 1500] [Boss]" and v.Humanoid.Health > 0 then
                                                        if v.Humanoid:FindFirstChild("Animator") then
                                                            v.Humanoid.Animator:Destroy()
                                                        end
                                                        weapon()
                                                        repeat wait(_G.Fast_Delay)
                                                            EquipWeapon(_G.Weapon)
                                                            v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                                            TP(v.HumanoidRootPart.CFrame*CFrame.new(0,20,10))
                                                            AttackNoCD()
                                                        until not Auto_Three_World or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check") == 1
                                                        if not Auto_Farm then
                                                            wait(30)
                                                            _G.Stop_Tween = true
                                                            wait(0.1)
                                                            _G.Stop_Tween = nil
                                                            Auto_Farm = true
                                                            Attack_Boss_Rip_Indra = false
                                                            Attack_Boss_Three_World = false
                                                            Attack_Boss_Don_Swan = false
                                                        end
                                                    end
                                                end
                                            else
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check")
                                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Begin")
                                                repeat wait(1)
                                                    TP(CFrame.new(-26952.3, 20.7341, 329.352))
                                                until (Vector3.new(-26952.3, 20.7341, 329.352)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2
                                            end
                                        end
                                    end-- 
                                end
                            end
                        else
                            wait(2)
                        end
                    -- end)
                end
            end)
        -- Auto_Three_World

        -- Auto_Three_World Go 
            spawn(function()
                while wait(60) do
                    if Auto_Three_World and New_World then
                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                        if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 1 then
                            if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 1 then
                                if Lv >= 1500 and not Farm_Item_Inventroy_Mystic_Droplet and Check_Inventory_Item_Name("Mystic Droplet") >= 10 then
                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou")
                                end
                            end
                        end
                    end
                end
            end)
        -- Auto_Three_World Go
    -- GoTo World 3
-- World 2 - 3

-- BringFruit GetFruit BuyFruit
    -- BringFruit
        spawn(function()
            while wait(1) do
                -- pcall(function()
                    if BringFruit then
                        for i,v6 in pairs(game:GetService("Workspace"):GetChildren()) do
                            if v6:IsA ("Tool") and (v6.Handle.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 13000 then
                                _G.Stop_Tween = true
                                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v6.Handle.CFrame
                                v6.Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
                                wait(2)
                                UnEquipweapon()
                                _G.Stop_Tween = nil
                            end
                        end
                    end
                -- end)
            end
        end)
    -- BringFruit

    -- GetFruit
        spawn(function()
            while wait(2) do
                pcall(function()
                    if Inventory_Fruit then
                        for u,y in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
                            if string.find(y.Name, "Fruit") then 
                                local Fruit = game:GetService("ReplicatedStorage").Remotes["CommF_"]:InvokeServer("getInventory")
                                for i,v in pairs(Fruit) do
                                    if v.Type == "Blox Fruit" then
                                        if 	   y.Name == "Human: Buddha Fruit" then
                                            itemfruit = string.gsub(y.Name, ": Buddha Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        elseif y.Name == "Bird: Falcon Fruit" then 
                                            itemfruit = string.gsub(y.Name, ": Falcon Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        elseif y.Name == "Bird: Phoenix Fruit" then 
                                            itemfruit = string.gsub(y.Name, ": Phoenix Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        else
                                            itemfruit = string.gsub(y.Name, " Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        end
                                        
                                        local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                                        if itemfruit == v.Name and Lv >= 1100 and not Farm_Item_Inventroy_Magma_Ore and not Farm_Item_Inventroy_Fish_Tail and not Farm_Item_Inventroy_Mystic_Droplet and  
                                        not Farm_Item_Inventroy_Dragon_Scale and not Attack_Boss_Dragon_Talon and not Attack_Boss_Electric_Claw and not Attack_Boss_Shakman_Karate and
                                        not Attack_Boss_Death_Step and not HopGoWorld3_Core_Attack then
                                            
                                            Raid_Fruit_Fragments = true
                                        elseif itemfruit ~= v.Name then
                                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit", itemfruit, game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(y.Name))
                                        end
                                    else
                                        if 	   y.Name == "Human: Buddha Fruit" then
                                            itemfruit = string.gsub(y.Name, ": Buddha Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        elseif y.Name == "Bird: Falcon Fruit" then 
                                            itemfruit = string.gsub(y.Name, ": Falcon Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        elseif y.Name == "Bird: Phoenix Fruit" then 
                                            itemfruit = string.gsub(y.Name, ": Phoenix Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        else
                                            itemfruit = string.gsub(y.Name, " Fruit","-"..string.gsub(y.Name, " Fruit",""))
                                        end
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit", itemfruit, game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(y.Name))
                                    end
                                end
                            end
                        end
                    end
                end)
            end
        end)
    -- GetFruit

    -- BuyFruit
        spawn(function()
            while true do
                if Inventory_Fruit then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "Buy")
                end
                wait(3600)
            end
        end)
    -- BuyFruit
-- BringFruit GetFruit BuyFruit

-- SwordFarm 
    -- Saber
        spawn(function()
            while wait(1) do
                local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                local Beli = game:GetService("Players").LocalPlayer.Data.Beli.Value
                local FG = game:GetService("Players").LocalPlayer.Data.Fragments.Value
                if Old_World and Saber_Farm and Lv > 300 and not CheckSword("Saber") and not Farm_Item_Inventroy_Magma_Ore and not Attack_Boss_Shakman_Karate then
                    if Auto_Farm then
                        Auto_Farm = false
                    end
                    Farm_Sword_Saber = true
                    if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","SickMan") ~= 0 then
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                        Auto_Farm = false
                        repeat wait()
                            TP(CFrame.new(-1609.29956, 40.0520697, 162.820404))
                        until (Vector3.new(-1609.29956, 40.0520697, 162.820404)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 10
                        wait(3)
                        for i,v in pairs(game:GetService("Workspace").Map.Jungle.QuestPlates:GetChildren()) do
                            if v.Name == "Plate1" then
                                repeat wait()
                                TP(v.Button.CFrame)
                                until (v.Button.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 1
                                wait(1)
                            end
                            if v.Name == "Plate2" then
                                repeat wait()
                                TP(v.Button.CFrame)
                                until (v.Button.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 1
                                wait(1)
                            end
                            if v.Name == "Plate3" then
                                repeat wait()
                                TP(v.Button.CFrame)
                                until (v.Button.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 1
                                wait(1)
                            end
                            if v.Name == "Plate4" then
                                repeat wait()
                                TP(v.Button.CFrame)
                                until (v.Button.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 1
                                wait(1)
                            end
                            if v.Name == "Plate5" then
                                repeat wait()
                                TP(v.Button.CFrame)
                                until (v.Button.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 1
                                wait(1)
                            end
                        end
                        wait(2)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1609.29956, 12.0520697, 162.820404, -0.993804812, 2.60902091e-08, 0.11113929, 3.47902116e-08, 1, 7.63408607e-08, -0.11113929, 7.97344768e-08, -0.993804812)
                        wait(2)
                        repeat wait()
                            EquipWeapon("Torch")
                            TP(CFrame.new(1113.9502, 4.92147732, 4350.91992, -0.60768199, 3.86533046e-08, 0.794180453, 6.00742425e-08, 1, -2.70375722e-09, -0.794180453, 4.60667628e-08, -0.60768199))
                        until (Vector3.new(1113.9502, 4.92147732, 4350.91992)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 1
                        wait(1)
                        --fireclickdetector(game:GetService("Workspace").Map.Desert.Burn.Fire.ClickDetector)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1113.9502, 4.92147732, 4350.91992, -0.60768199, 3.86533046e-08, 0.794180453, 6.00742425e-08, 1, -2.70375722e-09, -0.794180453, 4.60667628e-08, -0.60768199)
                        wait(1)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1113.9502, 4.92147732, 4350.91992, -0.60768199, 3.86533046e-08, 0.794180453, 6.00742425e-08, 1, -2.70375722e-09, -0.794180453, 4.60667628e-08, -0.60768199)
                        wait(1)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1114.87708, 4.9214654, 4349.8501, -0.612586915, -9.68697833e-08, 0.790403247, -1.2634203e-07, 1, 2.4638446e-08, -0.790403247, -8.47679615e-08, -0.612586915)
                        wait(10)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1113.33618, 7.5484705, 4365.56396, -0.618000686, 2.54492516e-08, -0.786177576, -8.16741874e-09, 1, 3.87911392e-08, 0.786177576, 3.03939913e-08, -0.618000686)
                        wait(2)
                        repeat wait()
                            EquipWeapon("Cup")
                            TP(CFrame.new(1397.73975, 37.3480263, -1321.54456, -0.170597032, -4.99889588e-08, 0.985340893, 2.99880867e-08, 1, 5.59246409e-08, -0.985340893, 3.90890662e-08, -0.170597032))
                        until (Vector3.new(1397.73975, 37.3480263, -1321.54456)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2
                        wait(1)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1397.73975, 37.3480263, -1321.54456, -0.170597032, -4.99889588e-08, 0.985340893, 2.99880867e-08, 1, 5.59246409e-08, -0.985340893, 3.90890662e-08, -0.170597032)
                        wait(3)
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","SickMan")
                        wait(1)
                        Mob_Boss = true
                        Saber_Farm = false
                        
                    else
                        Auto_Farm = false
                        Mob_Boss = true
                        Saber_Farm = false
                    end
                elseif not Old_World and Saber_Farm and not CheckSword("Saber")  then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelMain")
                elseif CheckSword("Saber")  then
                    Saber_Farm = false
                end
            end
        end)

        spawn(function()
            while wait(1) do
                if Mob_Boss and not Farm_Item_Inventroy_Magma_Ore and not Attack_Boss_Shakman_Karate then
                    if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","RichSon") == 1 then
                        Farm_Sword_Saber = true
                        if game.Players.LocalPlayer.Backpack:FindFirstChild("Relic") or game.Players.LocalPlayer.Character:FindFirstChild("Relic") then
                            EquipWeapon("Relic")
                            repeat wait()
                                EquipWeapon("Relic")
                                TP(CFrame.new(-1406.60925, 29.8520069, 4.5805192, 0.882920146, 3.62382622e-08, 0.469523162, -3.61928865e-09, 1, -7.03750587e-08, -0.469523162, 6.04362143e-08, 0.882920146))
                            until (Vector3.new(-1406.60925, 29.8520069, 4.5805192)-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 2
                            wait(1)
                            Saber_Kill = true
                            Mob_Boss = false
                        else
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","RichSon")
                        end
                    elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","RichSon") == 0 then
                        Farm_Sword_Saber = true
                        if game.Workspace.Enemies:FindFirstChild("Mob Leader [Lv. 120] [Boss]") then
                            for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                                if v.Name == "Mob Leader [Lv. 120] [Boss]" then
                                    weapon()
                                    repeat wait(_G.Fast_Delay)
                                        EquipWeapon(_G.Weapon)
                                        v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,20,0))
                                        AttackNoCD()
                                    until not v.Parent or v.Humanoid.Health <= 0 or Mob_Boss == false
                                end
                            end
                        else
                            TP(game:GetService("ReplicatedStorage"):FindFirstChild("Mob Leader [Lv. 120] [Boss]").HumanoidRootPart.CFrame*CFrame.new(0,20,0))
                        end
                    else
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","RichSon")
                    end
                end
            end
        end)

        spawn(function()
            while wait(1) do
                pcall(function()
                    if Saber_Kill and not Farm_Item_Inventroy_Magma_Ore and not Attack_Boss_Shakman_Karate then
                        if game.Workspace.Enemies:FindFirstChild("Saber Expert [Lv. 200] [Boss]") then
                            Farm_Sword_Saber = true
                            for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
                                if v.Name == "Saber Expert [Lv. 200] [Boss]" then
                                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                                    weapon()
                                    repeat wait()
                                        EquipWeapon(_G.Weapon)
                                        v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                        TP(v.HumanoidRootPart.CFrame*CFrame.new(0,25,15))
                                        AttackNoCD()
                                    until not v.Parent or v.Humanoid.Health <= 0 or Saber_Kill == false
                                    Farm_Sword_Saber = false
                                    Saber_Kill = false
                                    Auto_Farm = true
                                end
                            end
                        elseif _G.Hop_Saber and not game.Workspace.Enemies:FindFirstChild("Saber Expert [Lv. 200] [Boss]") and not game:GetService("ReplicatedStorage"):FindFirstChild("Saber Expert [Lv. 200] [Boss]") then
                            wait(5)
                            HopServer()
                        else
                            TP(CFrame.new(-1458.89502, 29.8870335, -50.633564)*CFrame.new(0,25,15))
                        end
                    end
                end)
            end
        end)
    -- Saber
-- SwordFarm

-- BuySword
    -- LegendarySword 3 and Rengoku
        spawn(function()
            while wait() do
                local args = {
                    [1] = "LegendarySwordDealer",
                    [2] = "1"
                }
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                local args = {
                    [1] = "LegendarySwordDealer",
                    [2] = "2"
                }
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                local args = {
                    [1] = "LegendarySwordDealer",
                    [2] = "3"
                }
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                local args = {
                    [1] = "OpenRengoku"
                }
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                local args = {
                    [1] = "Ectoplasm",
                    [2] = "Buy",
                    [3] = 3
                }
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                wait(5)
            end
        end)
    -- LegendarySword 3 and Rengoku

-- BuySword

-- AutoBuyHaki 
    spawn(function()
        while wait() do
            if AutoHaki then
                local Lv = game:GetService("Players").LocalPlayer.Data.Level.Value
                local CheckBeli = game.Players.LocalPlayer.Data.Beli.Value 
                local FG = game:GetService("Players").LocalPlayer.Data.Fragments.Value
                if CheckBeli >= 10000 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Geppo") ~= 2 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Geppo")
                end
                if CheckBeli >= 25000 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Buso") ~= 2 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Buso")
                end
                if CheckBeli >= 100000 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Soru") ~= 2 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Soru")
                end
                if Lv >= 400 and CheckBeli >= 750000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk", "Start")
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk", "Buy")
                end
                
                if Lv >= 2400 and FG >= 7500 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ColorsDealer", "2")
                end
                wait(5)
            end
        end
    end)
-- AutoBuyHaki

-- Hop_Tab
    Setting:Seperator("Server")
    Setting:Button("Hop Server",function()
        HopServer()
    end)
    function HopServer()
        if not _G.TP_Ser then
            _G.TP_Ser = true
            game.StarterGui:SetCore("SendNotification", {
                Title = "HopServer", 
                Text = "กำลังหาเซิฟ",
                Icon = "",
                Duration = 25
            })
            local PlaceID = game.PlaceId
            local AllIDs = {}
            local foundAnything = ""
            local actualHour = os.date("!*t").hour
            local Deleted = false
            function TPReturner()
                local Site;
                if foundAnything == "" then
                    Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
                else
                    Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
                end
                local ID = ""
                if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
                    foundAnything = Site.nextPageCursor
                end
                local num = 0;
                for i,v in pairs(Site.data) do
                    local Possible = true
                    ID = tostring(v.id)
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "HopServer", 
                        Text = "Players : " ..tonumber(v.playing),
                        Icon = "",
                        Duration = 1.5
                    })
                    if tonumber(v.maxPlayers) > tonumber(v.playing) then
                        for _,Existing in pairs(AllIDs) do
                            if num ~= 0 then
                                if ID == tostring(Existing) then
                                    Possible = false
                                end
                            else
                                if tonumber(actualHour) ~= tonumber(Existing) then
                                    local delFile = pcall(function()
                                        -- delfile("NotSameServers.json")
                                        AllIDs = {}
                                        table.insert(AllIDs, actualHour)
                                    end)
                                end
                            end
                            num = num + 1
                        end
                        if Possible == true then
                            table.insert(AllIDs, ID)
                            wait()
                            pcall(function()
                                _G.TP_Ser = true
                                -- writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
                                wait()
                                game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
                            end)
                            wait(.1)
                        end
                    end
                end
            end
            function Bring()
                while wait(1) do
                    pcall(function()
                        TPReturner()
                        if foundAnything ~= "" then
                            TPReturner()
                        end
                    end)
                end
            end
            Bring()
        end
    end

    Setting:Button("Hop Low Server",function()
        HopLowServer()
    end)
    function HopLowServer()
        if not _G.TP_Ser then
            _G.TP_Ser = true
            game.StarterGui:SetCore("SendNotification", {
                Title = "HopServerLow", 
                Text = "กำลังหาเซิฟ",
                Icon = "",
                Duration = 25
            })
            local PlaceID = game.PlaceId
            local AllIDs = {}
            local foundAnything = ""
            local actualHour = os.date("!*t").hour
            local Deleted = false
            function TPReturner()
                local Site;
                if foundAnything == "" then
                    Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
                else
                    Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
                end
                local ID = ""
                if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
                    foundAnything = Site.nextPageCursor
                end
                local num = 0;
                for i,v in pairs(Site.data) do
                    local Possible = true
                    ID = tostring(v.id)
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "HopServerLow", 
                        Text = "Players : " ..tonumber(v.playing),
                        Icon = "",
                        Duration = 1.5
                    })
                    if tonumber(v.maxPlayers) > tonumber(v.playing) then
                        for _,Existing in pairs(AllIDs) do
                            if num ~= 0 then
                                if ID == tostring(Existing) then
                                    Possible = false
                                end
                            else
                                if tonumber(actualHour) ~= tonumber(Existing) then
                                    local delFile = pcall(function()
                                        -- delfile("NotSameServers.json")
                                        AllIDs = {}
                                        table.insert(AllIDs, actualHour)
                                    end)
                                end
                            end
                            num = num + 1
                        end
                        if Possible == true then
                            table.insert(AllIDs, ID)
                            wait()
                            pcall(function()
                                _G.TP_Ser = true
                                -- writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
                                wait()
                                game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
                            end)
                            wait(.1)
                        end
                    end
                end
            end
            function Bring()
                while wait(1) do
                    pcall(function()
                        TPReturner()
                        if foundAnything ~= "" then
                            TPReturner()
                        end
                    end)
                end
            end
            Bring()
        end
    end

    Setting:Button("Re join Server",function()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Re Join Server", 
            Text = "Ready Go!",
            Icon = "",
            Duration = 10
        })
        _G.TP_Ser = true
        wait(1)
        game:GetService("TeleportService"):Teleport(game.PlaceId)
        wait(50)
    end)
-- Hop_Tab

-- World
    function TP_World1()
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelMain")
    end
    function TP_World2()
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")	
    end
    function TP_World3()
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou")
    end

    Setting:Seperator("World")
    Setting:Button("Teleport to World 1", function()
        TP_World1()
    end)
    Setting:Button("Teleport to World 2", function()
        TP_World2()
    end)

    Setting:Button("Teleport to World 3", function()
        TP_World3()
    end)
-- World

-- Open Fruit
    Setting:Seperator("Check Status")
    Setting:Button("Open Fruit Inventory",function()
        game:GetService("Players").LocalPlayer.PlayerGui.Main.FruitInventory.Visible = true
    end)
    Setting:Button("Open Fruit Awaken",function()
        game:GetService("Players").LocalPlayer.PlayerGui.Main.AwakeningToggler.Visible = true
    end)
    Setting:Button("Open Fruit Store",function()
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("GetFruits")
        game.Players.localPlayer.PlayerGui.Main.FruitShop.Visible = true
    end)
    Setting:Button("Open Inventory",function()
        game:GetService("Players").LocalPlayer.PlayerGui.Main.Inventory.Visible = true 
    end)
    Setting:Button("Open Color Haki",function()
        game.Players.localPlayer.PlayerGui.Main.Colors.Visible = true
    end)
-- Open Fruit

-- CheckStatus
    -- StatusLeft_Tab
        StatusLeft_Tab:Title("User")
        local CheckStatus_Username       = StatusLeft_Tab:Label("📺 Username : " .. game:GetService("Players").LocalPlayer.Name)
        local CheckStatus_World          = StatusLeft_Tab:Label("🗺 World : " .. _G.WorldName)
        local CheckStatus_Level          = StatusLeft_Tab:Label("📊 Level : " .. game:GetService("Players").LocalPlayer.Data.Level.Value)
        local CheckStatus_Bail           = StatusLeft_Tab:Label("💵 Bail : " .. game:GetService("Players").LocalPlayer.Data.Beli.Value)
        local CheckStatus_Fragments      = StatusLeft_Tab:Label("💷 Fragments : " .. game:GetService("Players").LocalPlayer.Data.Fragments.Value)
        local CheckStatus_Bones          = StatusLeft_Tab:Label("🟠 Bones : 0")
        local CheckStatus_Race           = StatusLeft_Tab:Label("🐰 Race : " .. game:GetService("Players").LocalPlayer.Data.Race.Value)
        local CheckStatus_StatusRace     = StatusLeft_Tab:Label("🐰 StatusRace : " .. CheckStatusRace())
        local CheckStatus_BloxFruit      = StatusLeft_Tab:Label("🍎 Fruit : ❌ NoFruit")
        local CheckStatus_LevelBloxFruit = StatusLeft_Tab:Label("🍎 LevelFruit : ❌ NoFruit")
        local CheckStatus_StatusFruit    = StatusLeft_Tab:Label("🍎 StatusFruit : ❌ NoFruit" )
        local CheckStatus_Haki           = StatusLeft_Tab:Label("👀 StatusHaki : " .. CheckHaki())
        StatusLeft_Tab:Title("Status")
        local CheckStatus_Melee       = StatusLeft_Tab:Label("📈 Melee : " .. game:GetService("Players").LocalPlayer.Data.Stats.Melee.Level.Value)
        local CheckStatus_Defense     = StatusLeft_Tab:Label("📈 Defense : " .. game:GetService("Players").LocalPlayer.Data.Stats.Defense.Level.Value)
        local CheckStatus_Sword       = StatusLeft_Tab:Label("📈 Sword : " .. game:GetService("Players").LocalPlayer.Data.Stats.Sword.Level.Value)
        local CheckStatus_Gun         = StatusLeft_Tab:Label("📈 Gun : " .. game:GetService("Players").LocalPlayer.Data.Stats.Gun.Level.Value)
        local CheckStatus_Fruit       = StatusLeft_Tab:Label("📈 Fruit : " .. game:GetService("Players").LocalPlayer.Data.Stats["Demon Fruit"].Level.Value)
        local CheckStatus_Points      = StatusLeft_Tab:Label("📊 Points : " .. game:GetService("Players").LocalPlayer.Data.Points.Value)
        StatusLeft_Tab:Title("Melee Check")
        local CheckStatus_Superhuman    = StatusLeft_Tab:Label("❌ : Superhuman")
        local CheckStatus_DeathStep     = StatusLeft_Tab:Label("❌ : Death Step")
        local CheckStatus_ShakmanKarate = StatusLeft_Tab:Label("❌ : Sharkman Karate")
        local CheckStatus_ElectricClaw  = StatusLeft_Tab:Label("❌ : ElectricClaw")
        local CheckStatus_DragonTalon   = StatusLeft_Tab:Label("❌ : Dragon Talon")
        local CheckStatus_Godhuman      = StatusLeft_Tab:Label("❌ : Godhuman")
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:NoLabel()
        StatusLeft_Tab:Title("Fruit Price < 1M")
        local CheckFruitPrice1M1        = StatusLeft_Tab:Title("❌ : 0 Fruit")
        StatusLeft_Tab:Title("Fruit Price > 1M")
        local CheckFruitPrice1M2        = StatusLeft_Tab:Title("❌ : 0 Fruit")
    -- StatusLeft_Tab

    -- StatusLeftCenter_Tab
        StatusLeftCenter_Tab:Title("Status Quest")
        local Kill_Elite_Hunter_ST   = StatusLeftCenter_Tab:Label("Kill Elite Hunter : 0")
        local Kill_Cake_Monster_ST   = StatusLeftCenter_Tab:Label("Kill Cake Monster : 0/500")
        local Bartlio_ST             = StatusLeftCenter_Tab:Label("❌ : Quest Bartlio")
        local Open_Don_Swan_ST       = StatusLeftCenter_Tab:Label("❌ : Quest Open Don Swan")
        local Open_Door_World3       = StatusLeftCenter_Tab:Label("❌ : Quest OpenDoor World 3")
        StatusLeftCenter_Tab:Title("Status Haki")
        local Observation_Haki_ST    = StatusLeftCenter_Tab:Label("❌ : Quest Observation Haki")
        local Observation_Haki_V2_ST = StatusLeftCenter_Tab:Label("❌ : Quest Observation Haki V2")
        local Check_Quest_Haki       = StatusLeftCenter_Tab:Label("❌ : Quest HakiV2 : 0 / 5")
        StatusLeftCenter_Tab:Title("Melee Quest")
        Check_Melee_DeathStep     = StatusLeftCenter_Tab:Label("❌ : Open Quest Buy Death Step")
        Check_Melee_ShakmanKarate = StatusLeftCenter_Tab:Label("❌ : Open Quest Buy Shakman Karate")
        Check_Melee_ElectricClaw  = StatusLeftCenter_Tab:Label("❌ : Open Quest Buy Electric Claw ")
        Check_Melee_DragonTalon   = StatusLeftCenter_Tab:Label("❌ : Open Quest Buy DragonTalon ")
        Check_Melee_Godhuman      = StatusLeftCenter_Tab:Label("❌ : Open Quest Buy Godhuman ")
        StatusLeftCenter_Tab:Title("Melee Quest GodHuman")
        Check_Item_FishTail_Godhuman      = StatusLeftCenter_Tab:Label("❌ : Fish Tail : 0 / 20")
        Check_Item_MagmaOre_Godhuman      = StatusLeftCenter_Tab:Label("❌ : Magma Ore : 0 / 20")
        Check_Item_MysticDroplet_Godhuman = StatusLeftCenter_Tab:Label("❌ : Mystic Droplet : 0 / 10")
        Check_Item_DragonScale_Godhuman   = StatusLeftCenter_Tab:Label("❌ : Dragon Scale : 0 / 10")

    -- StatusLeftCenter_Tab

    -- StatusRightCenter_Tab Sword  
        StatusRightCenter_Tab:Title("Status Sword")
        local ST_CursedDualKatana = StatusRightCenter_Tab:Label("❌ : Cursed Dual Katana")
        local ST_HallowScythe = StatusRightCenter_Tab:Label("❌ : Hallow Scythe")
        local ST_Shisui = StatusRightCenter_Tab:Label("❌ : Shisui")
        local ST_Saddi = StatusRightCenter_Tab:Label("❌ : Saddi")
        local ST_Wando = StatusRightCenter_Tab:Label("❌ : Wando")
        local ST_TrueTripleKatana = StatusRightCenter_Tab:Label("❌ : True Triple Katana")
        local ST_Yama = StatusRightCenter_Tab:Label("❌ : Yama")
        local ST_BuddySword = StatusRightCenter_Tab:Label("❌ : Buddy Sword")
        local ST_Canvander = StatusRightCenter_Tab:Label("❌ : Canvander")
        local ST_PoleV1 = StatusRightCenter_Tab:Label("❌ : Pole (1st Form)")
        local ST_PoleV2 = StatusRightCenter_Tab:Label("❌ : Pole (2nd Form)")
        local ST_SpikeyTrident = StatusRightCenter_Tab:Label("❌ : Spikey Trident")
        local ST_Tushita = StatusRightCenter_Tab:Label("❌ : Tushita")
        local ST_Saber = StatusRightCenter_Tab:Label("❌ : Saber")
        local ST_Rengoku = StatusRightCenter_Tab:Label("❌ : Rengoku")
        local ST_Bisento = StatusRightCenter_Tab:Label("❌ : Bisento")
        local ST_MidnightBlade = StatusRightCenter_Tab:Label("❌ : Midnight Blade")
    -- StatusRightCenter_Tab Sword
        
    -- StatusRightCenter_Tab Gun
        StatusRightCenter_Tab:Title("Status Gun")
        local ST_Kabucha     = StatusRightCenter_Tab:Label("❌ : Kabucha")
        local ST_AcidumRifle = StatusRightCenter_Tab:Label("❌ : Acidum Rifle")
        local ST_SerpentBow  = StatusRightCenter_Tab:Label("❌ : Serpent Bow")
        local ST_SoulGuitar  = StatusRightCenter_Tab:Label("❌ : Soul Guitar")

        StatusRightCenter_Tab:Title("Status Accessory")
        local ST_PaleScarf   = StatusRightCenter_Tab:Label("❌ : Pale Scarf")
    -- StatusRightCenter_Tab Gun
    
    -- Status
        Status:Title("StatusBot")
        local Check_AutoFarm_UI             = Status:Label("❌ : Attack Auto  Farm")
        local Check_AutoFarm_Bones_UI       = Status:Label("❌ : Attack Auto  FarmBones")
        local Check_Auto_Attack_Core        = Status:Label("❌ : Attack Auto  Core")
        local Check_Raid_UI                 = Status:Label("❌ : Attack Auto  Raid")
        local Attack_Auto_Farm_Mastery_UI   = Status:Label("❌ : Attack Auto Farm Mastery")
        Check_OpenDoor_World3               = Status:Label("❌ : Attack Quest OpenDoor World 3")
        Check_Death_Step                    = Status:Label("❌ : Attack Quest Boss Death Step ")
        Check_Shakman_Karate_UI             = Status:Label("❌ : Attack Quest Boss Shakman Karate")
        Check_Electric_Claw_UI              = Status:Label("❌ : Attack Quest Boss Electric Claw")
        Check_DragonTalon_UI                = Status:Label("❌ : Attack Quest Boss DragonTalon")
        Check_Godhuman_UI                   = Status:Label("❌ : Attack Quest Boss Godhuman")
        Farm_Item_Inventroy_Godhuman        = Status:Label("❌ : Attack Quest Godhuman")
        Status:Title("CheckColorHaki")
        local CheckColorHaki_OrangeSoda        = Status:Label("❌ : Orange Soda")
        local CheckColorHaki_BrightYellow      = Status:Label("❌ : Bright Yellow")
        local CheckColorHaki_YellowSunshine    = Status:Label("❌ : Yellow Sunshine")
        local CheckColorHaki_SlimyGreen        = Status:Label("❌ : Slimy Green")
        local CheckColorHaki_GreenLizard       = Status:Label("❌ : Green Lizard")
        local CheckColorHaki_BlueJeans         = Status:Label("❌ : Blue Jeans")
        local CheckColorHaki_PlumpPurple       = Status:Label("❌ : Plump Purple")
        local CheckColorHaki_FieryRose         = Status:Label("❌ : Fiery Rose")
        local CheckColorHaki_HeatWave          = Status:Label("❌ : Heat Wave")
        local CheckColorHaki_AbsoluteZero      = Status:Label("❌ : Absolute Zero")
        local CheckColorHaki_SnowWhite         = Status:Label("❌ : Snow White")
        local CheckColorHaki_PureRad           = Status:Label("❌ : Pure Red")
        local CheckColorHaki_WinterSky         = Status:Label("❌ : Winter Sky")
        local CheckColorHaki_RainbowSaviour    = Status:Label("❌ : Rainbow Saviour")
    -- Status
    spawn(function()
        while wait(1) do 
            -- Status 
                pcall(function()
                    CheckStatus_Username        :Set("📺 Username : " .. game:GetService("Players").LocalPlayer.Name)
                    CheckStatus_Username        :Set("📺 Username : " .. game:GetService("Players").LocalPlayer.Name)
                    CheckStatus_World           :Set("🗺 World : " .. _G.WorldName)
                    CheckStatus_Level           :Set("📊 Level : " .. game:GetService("Players").LocalPlayer.Data.Level.Value)
                    CheckStatus_Bail            :Set("💵 Bail : " .. game:GetService("Players").LocalPlayer.Data.Beli.Value)
                    CheckStatus_Fragments       :Set("💷 Fragments : " .. game:GetService("Players").LocalPlayer.Data.Fragments.Value)
                    
                    local B_H = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Bones","Check")
                    if tonumber(B_H) >= 1 then
                        CheckStatus_Bones  :Set("🟠 Bones : " .. tonumber(B_H))
                    end
                    CheckStatus_Race            :Set("🐰 Race : " .. game:GetService("Players").LocalPlayer.Data.Race.Value)
                    CheckStatus_StatusRace      :Set("🐰 StatusRace : " .. CheckStatusRace())
                    if game.Players.localPlayer.Data.DevilFruit.Value ~= "" then 
                        CheckStatus_BloxFruit       :Set("🍎 Fruit : " .. game:GetService("Players").LocalPlayer.Data.DevilFruit.Value)
                        CheckStatus_LevelBloxFruit  :Set("🍎 LevelFruit : " .. CheckWeaponBC(game.Players.LocalPlayer.Data.DevilFruit.Value).Level.Value)
                        CheckStatus_StatusFruit     :Set("🍎 StatusFruit : " .. StatusFruitAwakened())
                    end
                end)
                
                CheckStatus_Melee      :Set("📈 Melee : " .. game:GetService("Players").LocalPlayer.Data.Stats.Melee.Level.Value)
                CheckStatus_Defense    :Set("📈 Defense : " .. game:GetService("Players").LocalPlayer.Data.Stats.Defense.Level.Value)
                CheckStatus_Sword      :Set("📈 Sword : " .. game:GetService("Players").LocalPlayer.Data.Stats.Sword.Level.Value)
                CheckStatus_Gun        :Set("📈 Gun : " .. game:GetService("Players").LocalPlayer.Data.Stats.Gun.Level.Value)
                CheckStatus_Fruit      :Set("📈 Fruit : " .. game:GetService("Players").LocalPlayer.Data.Stats["Demon Fruit"].Level.Value)
                CheckStatus_Points     :Set("📊 Points : " .. game:GetService("Players").LocalPlayer.Data.Points.Value)

                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 1 then
                    Check_Item_FishTail_Godhuman     :Set("✅ : Fish Tail : Success")
                    Check_Item_MagmaOre_Godhuman     :Set("✅ : Magma Ore : Success")
                    Check_Item_MysticDroplet_Godhuman:Set("✅ : Mystic Droplet : Success")
                    Check_Item_DragonScale_Godhuman  :Set("✅ : Dragon Scale : Success")
                    Farm_Item_Inventroy_Godhuman     :Set("✅ : Attack Quest Godhuman")
                else
                    if Farm_Item_Inventroy_Magma_Ore or Farm_Item_Inventroy_Fish_Tail or Farm_Item_Inventroy_Dragon_Scale or Farm_Item_Inventroy_Mystic_Droplet then
                        Farm_Item_Inventroy_Godhuman:Set("🔄 : Attack Quest Godhuman")
                    else
                        Farm_Item_Inventroy_Godhuman:Set("❌ : Attack Quest Godhuman")
                    end

                    if Check_Inventory_Item_Name("Fish Tail") ~= nil and Check_Inventory_Item_Name("Fish Tail") >= 20 then
                        Check_Item_FishTail_Godhuman     :Set("✅ : Fish Tail : " .. Check_Inventory_Item_Name("Fish Tail") .. " / 20")
                    elseif Check_Inventory_Item_Name("Fish Tail") ~= nil and Check_Inventory_Item_Name("Fish Tail") < 20 then
                        Check_Item_FishTail_Godhuman     :Set("❌ : Fish Tail : " .. Check_Inventory_Item_Name("Fish Tail") .. " / 20")
                    else
                        Check_Item_FishTail_Godhuman     :Set("❌ : Fish Tail : 0 / 20")
                    end
                    
                    if Check_Inventory_Item_Name("Magma Ore") ~= nil and Check_Inventory_Item_Name("Magma Ore") >= 20 then
                        Check_Item_MagmaOre_Godhuman     :Set("✅ : Magma Ore : " .. Check_Inventory_Item_Name("Magma Ore") .. " / 20")
                    elseif Check_Inventory_Item_Name("Magma Ore") ~= nil and Check_Inventory_Item_Name("Magma Ore") < 20 then 
                        Check_Item_MagmaOre_Godhuman     :Set("❌ : Magma Ore : " .. Check_Inventory_Item_Name("Magma Ore") .. " / 20")
                    else
                        Check_Item_MagmaOre_Godhuman     :Set("❌ : Magma Ore : 0 / 20")
                    end

                    if Check_Inventory_Item_Name("Mystic Droplet") ~= nil and Check_Inventory_Item_Name("Mystic Droplet") >= 10 then 
                        Check_Item_MysticDroplet_Godhuman:Set("✅ : Mystic Droplet : " .. Check_Inventory_Item_Name("Mystic Droplet") .. " / 10")
                    elseif Check_Inventory_Item_Name("Mystic Droplet") ~= nil and Check_Inventory_Item_Name("Mystic Droplet") < 10 then 
                        Check_Item_MysticDroplet_Godhuman:Set("❌ : Mystic Droplet : " .. Check_Inventory_Item_Name("Mystic Droplet") .. " / 10")
                    else
                        Check_Item_MysticDroplet_Godhuman:Set("❌ : Mystic Droplet : 0 / 10")
                    end

                    if Check_Inventory_Item_Name("Dragon Scale") ~= nil and Check_Inventory_Item_Name("Dragon Scale") >= 10 then
                        Check_Item_DragonScale_Godhuman  :Set("✅ : Dragon Scale : " .. Check_Inventory_Item_Name("Dragon Scale") .. " / 10")
                    elseif Check_Inventory_Item_Name("Dragon Scale") ~= nil and Check_Inventory_Item_Name("Dragon Scale") < 10 then
                        Check_Item_DragonScale_Godhuman  :Set("❌ : Dragon Scale : " .. Check_Inventory_Item_Name("Dragon Scale") .. " / 10")
                    else
                        Check_Item_DragonScale_Godhuman  :Set("❌ : Dragon Scale : 0 / 10")
                    end
                end

                if Auto_Farm then
                    Check_AutoFarm_UI:Set("🔄 : Attack Auto  Farm")
                else
                    Check_AutoFarm_UI:Set("❌ : Attack Auto  Farm")
                end

                if Farm_Bones_UI then
                    Check_AutoFarm_Bones_UI:Set("🔄 : Attack Auto  FarmBones")
                else
                    Check_AutoFarm_Bones_UI:Set("❌ : Attack Auto  FarmBones")
                end

                if Auto_Raid or Raid_Fruit_Fragments then 
                    Check_Raid_UI:Set("🔄 : Attack Auto  Raid")
                else
                    Check_Raid_UI:Set("❌ : Attack Auto  Raid")
                end

                if HopGoWorld3_Core_Attack then 
                    Check_Auto_Attack_Core:Set("🔄 : Attack Auto  Core")
                else
                    Check_Auto_Attack_Core:Set("❌ : Attack Auto  Core")
                end

                if Attack_Auto_Farm_Mastery then
                    Attack_Auto_Farm_Mastery_UI:Set("🔄 : Attack Auto Farm Mastery")
                else
                    Attack_Auto_Farm_Mastery_UI:Set("❌ : Attack Auto Farm Mastery")
                end

                if Attack_Boss_Three_World then
                    Check_OpenDoor_World3:Set("🔄 : Attack Quest OpenDoor World 3")
                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check") == 1 then
                    Check_OpenDoor_World3:Set("✅ : Attack Quest OpenDoor World 3")
                else
                    Check_OpenDoor_World3:Set("❌ : Attack Quest OpenDoor World 3")
                end

                if Attack_Boss_Death_Step then
                    Check_Death_Step:Set("🔄 : Attack Quest Boss Death Step ")
                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 1 then
                    Check_Death_Step:Set("✅ : Attack Quest Boss Death Step ")
                else
                    Check_Death_Step:Set("❌ : Attack Quest Boss Death Step ")
                end

                if Attack_Boss_Shakman_Karate then
                    Check_Shakman_Karate_UI:Set("🔄 : Attack Quest Boss Shakman Karate")
                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 1 then
                    Check_Shakman_Karate_UI:Set("✅ : Attack Quest Boss Shakman Karate")
                else
                    Check_Shakman_Karate_UI:Set("❌ : Attack Quest Boss Shakman Karate")
                end

                if Attack_Boss_Electric_Claw then
                    Check_Electric_Claw_UI:Set("🔄 : Attack Quest Boss Electric Claw")
                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 1 then
                    Check_Electric_Claw_UI:Set("✅ : Attack Quest Boss Electric Claw")
                else
                    Check_Electric_Claw_UI:Set("❌ : Attack Quest Boss Electric Claw")
                end

                if Attack_Boss_Dragon_Talon then
                    Check_DragonTalon_UI:Set("🔄 : Attack Quest Boss DragonTalon")
                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 1 then
                    Check_DragonTalon_UI:Set("✅ : Attack Quest Boss DragonTalon")
                else
                    Check_DragonTalon_UI:Set("❌ : Attack Quest Boss DragonTalon")
                end

                if Attack_Boss_GodHuman then
                    Check_Godhuman_UI:Set("🔄 : Attack Quest Boss Godhuman")
                elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 1 then
                    Check_Godhuman_UI:Set("✅ : Attack Quest Boss Godhuman")
                else
                    Check_Godhuman_UI:Set("❌ : Attack Quest Boss Godhuman")
                end
            -- Status
        end
    end)
    CheckFruitPrice1 = {}
    CheckFruitPrice2 = {}
    CheckMelee_Superhuman = true
    CheckMelee_DeathStep = true
    CheckMelee_ShakmanKarate = true
    CheckMelee_ElectricClaw = true
    CheckMelee_DragonTalon = true
    CheckMelee_Godhuman = true
    spawn(function() -- CheckMelee
        while true do 
            -- CheckMelee
                if CheckMelee_Superhuman and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman", true) == 1 then 
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman")
                    wait(1)
                    if CheckWeaponBC("Superhuman") then
                        CheckStatus_Superhuman:Set("✅ : Superhuman | Lv : "..CheckWeaponBC("Superhuman").Level.Value)
                        _G.Weapon = "Superhuman"
                        EquipWeapon(_G.Weapon)
                        CheckMelee_Superhuman = false
                    end
                end
                if CheckMelee_DeathStep and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 1 and CheckWeaponBC("Superhuman") and CheckWeaponBC("Superhuman").Level.Value >= 400 then 
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep")
                    wait(1)
                    if CheckWeaponBC("Death Step") then
                        CheckStatus_DeathStep:Set("✅ : Death Step | Lv : "..CheckWeaponBC("Death Step").Level.Value)
                        _G.Weapon = "Death Step"
                        EquipWeapon(_G.Weapon)
                        CheckMelee_DeathStep = false
                    end
                end
                if CheckMelee_ShakmanKarate and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 1 and CheckWeaponBC("Death Step") and CheckWeaponBC("Death Step").Level.Value >= 400  then 
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true)
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate")
                    wait(1)
                    if CheckWeaponBC("Sharkman Karate") then
                        CheckStatus_ShakmanKarate:Set("✅ : Sharkman Karate | Lv : "..CheckWeaponBC("Sharkman Karate").Level.Value)
                        _G.Weapon = "Sharkman Karate"
                        EquipWeapon(_G.Weapon)
                        CheckMelee_ShakmanKarate = false
                    end
                end
                if CheckMelee_ElectricClaw and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 1 and CheckWeaponBC("Sharkman Karate") and CheckWeaponBC("Sharkman Karate").Level.Value >= 400 then 
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
                    wait(1)
                    if CheckWeaponBC("Electric Claw") then
                        CheckStatus_ElectricClaw:Set("✅ : Electric Claw | Lv : "..CheckWeaponBC("Electric Claw").Level.Value)
                        _G.Weapon = "Electric Claw"
                        EquipWeapon(_G.Weapon)
                        CheckMelee_ElectricClaw = false
                    end
                end
                if CheckMelee_DragonTalon and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 1 and CheckWeaponBC("Electric Claw") and CheckWeaponBC("Electric Claw").Level.Value >= 400 then 
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true)
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon")
                    wait(1)
                    if CheckWeaponBC("Dragon Talon") then
                        CheckStatus_DragonTalon:Set("✅ : Dragon Talon | Lv : "..CheckWeaponBC("Dragon Talon").Level.Value)
                        _G.Weapon = "Dragon Talon"
                        EquipWeapon(_G.Weapon)
                        CheckMelee_DragonTalon = false
                    end
                end
                if CheckMelee_Godhuman and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 1 and CheckWeaponBC("Dragon Talon") and CheckWeaponBC("Dragon Talon").Level.Value >= 400 then 
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true)
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman")
                    wait(1)
                    if CheckWeaponBC("Godhuman") then
                        CheckStatus_Godhuman:Set("✅ : Godhuman | Lv : "..CheckWeaponBC("Godhuman").Level.Value)
                        _G.Weapon = "Godhuman"
                        EquipWeapon(_G.Weapon)
                        CheckMelee_Godhuman = false
                    end
                end
            -- CheckMelee
            wait(30)
        end
    end)
    spawn(function() -- Quest
        while true do 
            -- Quest
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 3 then
                    Bartlio_ST:Set("✅ : Quest Bartilo") 
                end
                Kill_Elite_Hunter_ST:Set("Kill Elite Hunter : "..tostring(game.ReplicatedStorage.Remotes.CommF_:InvokeServer("EliteHunter", "Progress")))
                local OP = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("CakePrinceSpawner")
                local Lp = tonumber(string.match(tostring(OP), "%d+"))
                Kill_Cake_Monster_ST:Set("Kill Cake Monster : "..tostring(Lp).."/500")
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TalkTrevor","1") == 0 then
                    Open_Don_Swan_ST:Set("✅ : Quest Open Don Swan")
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SickScientist","Heal") == 1 then
                    Phoenix_Awaken_ST:Set('✅ : Quest Phoenix Awaken')
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","SickMan") == 0 then
                    Observation_Haki_ST:Set('✅ : Quest Observation Haki')
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen") == 3 then
                    Observation_Haki_V2_ST:Set('✅ : Quest Observation Haki V2')
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep", true) == 1 then
                    Check_Melee_DeathStep:Set("✅ : Open Quest Buy Death Step")
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate", true) == 1 then
                    Check_Melee_ShakmanKarate:Set("✅ : Open Quest Buy Shakman Karate")
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw", true) == 1 then
                    Check_Melee_ElectricClaw:Set("✅ : Open Quest Buy Electric Claw")
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon", true) == 1 then
                    Check_Melee_DragonTalon:Set("✅ : Open Quest Buy DragonTalon ")
                end
                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 0 or game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman", true) == 1 then
                    Check_Melee_Godhuman:Set("✅ : Open Quest Buy Godhuman ")
                end

                if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check") == 1 then
                    Open_Door_World3:Set("✅ : Quest OpenDoor World 3")
                end
            -- Quest
            wait(30)
        end
    end)
    spawn(function() -- Sword
        while true do  
            -- Sword
                if CheckSword("Cursed Dual Katana") then
                    ST_CursedDualKatana:Set("✅ : Cursed Dual Katana | Lv : "..CheckLevelSword("Cursed Dual Katana"))
                end
                if CheckSword("Hallow Scythe") then
                    ST_HallowScythe:Set("✅ : Hallow Scythe | Lv : "..CheckLevelSword("Hallow Scythe"))
                end
                if CheckSword("Shisui") then
                    ST_Shisui:Set("✅ : Shisui | Lv : "..CheckLevelSword("Shisui"))
                end
                if CheckSword("Saddi") then
                    ST_Saddi:Set("✅ : Saddi | Lv : "..CheckLevelSword("Saddi"))
                end
                if CheckSword("Wando") then
                    ST_Wando:Set("✅ : Wando | Lv : "..CheckLevelSword("Wando"))
                end
                if CheckSword("True Triple Katana") then
                    ST_TrueTripleKatana:Set("✅ : True Triple Katana | Lv : "..CheckLevelSword("True Triple Katana"))
                end
                if CheckSword("Yama") then
                    ST_Yama:Set("✅ : Yama | Lv : "..CheckLevelSword("Yama"))
                end
                if CheckSword("Buddy Sword") then
                    ST_BuddySword:Set("✅ : Buddy Sword | Lv : "..CheckLevelSword("Buddy Sword"))
                end
                if CheckSword("Canvander") then
                    ST_Canvander:Set("✅ : Canvander | Lv : "..CheckLevelSword("Canvander"))
                end
                if CheckSword("Pole (1st Form)") then
                    ST_PoleV1:Set("✅ : Pole (1st Form) | Lv : "..CheckLevelSword("Pole (1st Form)"))
                end
                if CheckSword("Pole (2nd Form)") then
                    ST_PoleV2:Set("✅ : Pole (2nd Form) | Lv : "..CheckLevelSword("Pole (2nd Form)"))
                end
                if CheckSword("Spikey Trident") then
                    ST_SpikeyTrident:Set("✅ : Spikey Trident | Lv : "..CheckLevelSword("Spikey Trident"))
                end
                if CheckSword("Tushita") then
                    ST_Tushita:Set("✅ : Tushita | Lv : "..CheckLevelSword("Tushita"))
                end
                if CheckSword("Saber") then
                    ST_Saber:Set("✅ : Saber | Lv : "..CheckLevelSword("Saber"))
                end
                if CheckSword("Rengoku") then
                    ST_Rengoku:Set("✅ : Rengoku | Lv : "..CheckLevelSword("Rengoku"))
                end
                if CheckSword("Bisento") then
                    ST_Bisento:Set("✅ : Bisento | Lv : "..CheckLevelSword("Bisento"))
                end
                if CheckSword("Midnight Blade") then
                    ST_MidnightBlade:Set("✅ : Midnight Blade | Lv : "..CheckLevelSword("Midnight Blade"))
                end
            -- Sword
            wait(30)
        end
    end)
    spawn(function() -- Gun
        while true do 
            -- Gun
                if CheckSword("Kabucha") then
                    ST_Kabucha:Set("✅ : Kabucha | Lv : "..CheckLevelGun("Kabucha"))
                end
                if CheckSword("Acidum Rifle") then
                    ST_AcidumRifle:Set("✅ : Acidum Rifle | Lv : "..CheckLevelGun("Acidum Rifle"))
                end
                if CheckSword("Serpent Bow") then
                    ST_SerpentBow:Set("✅ : Serpent Bow | Lv : "..CheckLevelGun("Serpent Bow"))
                end
                if CheckSword("Soul Guitar") then
                    ST_SoulGuitar:Set("✅ : Soul Guitar | Lv : "..CheckLevelGun("Soul Guitar"))
                end
            -- Gun
            wait(30)
        end
    end)
    spawn(function() -- Accessory
        while true do 
            -- Accessory
                if CheckSword("Pale Scarf") then
                    ST_PaleScarf:Set("✅ : Pale Scarf")
                end
            -- Accessory
            wait(30)
        end
    end)
    spawn(function() -- ColorHaki
        while true do 
            -- ColorHaki
                if CheckStatusColorHaki("Orange Soda") then
                    CheckColorHaki_OrangeSoda:Set("✅ : Orange Soda")
                end

                if CheckStatusColorHaki("Bright Yellow") then
                    CheckColorHaki_BrightYellow:Set("✅ : Bright Yellow")
                end


                if CheckStatusColorHaki("Yellow Sunshine") then
                    CheckColorHaki_YellowSunshine:Set("✅ : Yellow Sunshine")
                end


                if CheckStatusColorHaki("Slimy Green") then
                    CheckColorHaki_SlimyGreen:Set("✅ : Slimy Green")
                end


                if CheckStatusColorHaki("Green Lizard") then
                    CheckColorHaki_GreenLizard:Set("✅ : Green Lizard")
                end


                if CheckStatusColorHaki("Blue Jeans") then
                    CheckColorHaki_BlueJeans:Set("✅ : Blue Jeans")
                end


                if CheckStatusColorHaki("Plump Purple") then
                    CheckColorHaki_PlumpPurple:Set("✅ : Plump Purple")
                end


                if CheckStatusColorHaki("Fiery Rose") then
                    CheckColorHaki_FieryRose:Set("✅ : Fiery Rose")
                end


                if CheckStatusColorHaki("Heat Wave") then
                    CheckColorHaki_HeatWave:Set("✅ : Heat Wave")
                end


                if CheckStatusColorHaki("Absolute Zero") then
                    CheckColorHaki_AbsoluteZero:Set("✅ : Absolute Zero")
                end


                if CheckStatusColorHaki("Snow White") then
                    CheckColorHaki_SnowWhite:Set("✅ : Snow White")
                end


                if CheckStatusColorHaki("Pure Red") then
                    CheckColorHaki_PureRad:Set("✅ : Pure Red")
                end


                if CheckStatusColorHaki("Winter Sky") then
                    CheckColorHaki_WinterSky:Set("✅ : Winter Sky")
                end


                if CheckStatusColorHaki("Rainbow Saviour") then
                    CheckColorHaki_RainbowSaviour:Set("✅ : Rainbow Saviour")
                end

            -- ColorHaki
            wait(30)
        end
    end)
    spawn(function() -- CheckFruitPrice1M1
        while true do 
            -- CheckFruitPrice1M1 
                table.clear(CheckFruitPrice1)
                table.clear(CheckFruitPrice2)
                local CheckFruitPrice1M = game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getInventoryFruits")
                for i,v in pairs(CheckFruitPrice1M) do
                    if v["Price"] < 1000000 then
                        table.insert(CheckFruitPrice1 ,v.Name)
                    end
                end
                for i=1, #CheckFruitPrice1 do
                    if i > 0 then
                        CheckFruitPrice1M1:Set("✅ : " .. i .. " Fruit")
                    else
                        CheckFruitPrice1M1:Set("❌ : 0 Fruit")
                    end
                end

                for i,v in pairs(CheckFruitPrice1M) do
                    if v["Price"] >= 1000000 then
                        table.insert(CheckFruitPrice2 ,v.Name)
                    end
                end
                for i=1, #CheckFruitPrice2 do
                    if i > 0 then
                        CheckFruitPrice1M2:Set("✅ : " .. i .. " Fruit")
                    else
                        CheckFruitPrice1M2:Set("❌ : 0 Fruit")
                    end
                end
            -- CheckFruitPrice1M1
            wait(30)
        end
    end)
-- CheckStatus
