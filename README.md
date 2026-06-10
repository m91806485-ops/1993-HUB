--[[
    1993 HUB v26.4 [Hybrid Remotes & Optimization Update]
    [+] Modded version with RequestCommandModification for non-copy tabs.
    [+] Optimized Copy V1 to send a single large bundled command request.
    [👑] Modified & Maintained by: mohammeedd78
--]]

local _ENV_BOX = getfenv and getfenv() or _ENV
local _S, _R = pcall(function()
    
    print("1993 HUB v26.4 Loaded Successfully!")
    
    local _EX = {
        P = game:GetService("Players"),
        T = game:GetService("TweenService"),
        U = game:GetService("UserInputService"),
        R = game:GetService("ReplicatedStorage"),
        S = game:GetService("RunService")
    }
    _EX.L = _EX.P.LocalPlayer
    _EX.G = _EX.L:WaitForChild("PlayerGui")

    local PremiumColors = {
        Color3.fromRGB(0, 255, 150),
        Color3.fromRGB(0, 200, 255),
        Color3.fromRGB(255, 0, 127),
        Color3.fromRGB(255, 120, 0),
        Color3.fromRGB(180, 0, 255),
        Color3.fromRGB(255, 215, 0),
        Color3.fromRGB(255, 50, 50)
    }
    local RandomThemeColor = PremiumColors[math.random(1, #PremiumColors)]

    local MainGui = Instance.new("ScreenGui")
    MainGui.Name = "1993_Hub_V26"
    MainGui.ResetOnSpawn = false
    MainGui.Parent = _EX.G

    -- [الريموت الجديد لبقية الأقسام العادية - سحب، سكنات إلخ]
    local function FireOldRemote(commandText)
        task.spawn(function()
            pcall(function()
                local hdRemote = _EX.R:WaitForChild("HDAdminHDClient", 3):WaitForChild("Signals", 3):WaitForChild("RequestCommandModification", 3)
                if hdRemote then
                    hdRemote:InvokeServer(unpack({commandText}))
                else
                    -- حماية بديلة في حال عدم وجود ريموت HDAdmin
                    _EX.R:WaitForChild("DefaultChatSystemChatEvents", 2):WaitForChild("SayMessageRequest", 2):FireServer(commandText, "All")
                end
            end)
        end)
    end

    -- [الريموتات الخاصة بأقسام النسخ السريع V1 و V2]
    local function FireNewCopyRemotes(commandText)
        task.spawn(function()
            pcall(function()
                _EX.R:WaitForChild("RemoteEvents", 2):WaitForChild("DataService", 2):FireServer(unpack({commandText}))
            end)
            pcall(function()
                _EX.R:WaitForChild("HDAdminHDClient", 2):WaitForChild("Signals", 2):WaitForChild("RequestCommandModification", 2):InvokeServer(unpack({commandText}))
            end)
        end)
    end

    local function MakeDraggable(f)
        local d, di, ds, sp
        f.InputBegan:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
                d = true ds = i.Position sp = f.Position
                i.Changed:Connect(function() if i.UserInputState == Enum.UserInputState.End then d = false end end)
            end
        end)
        f.InputChanged:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch then di = i end end)
        _EX.U.InputChanged:Connect(function(i)
            if i == di and d then
                local del = i.Position - ds
                _EX.T:Create(f, TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = UDim2.new(sp.X.Scale, sp.X.Offset + del.X, sp.Y.Scale, sp.Y.Offset + del.Y)}):Play()
            end
        end)
    end

    local function CreateNotification(msg)
        local NF = Instance.new("Frame") local CR = Instance.new("UICorner") local TL = Instance.new("TextLabel")
        NF.Size = UDim2.new(0, 250, 0, 38) NF.Position = UDim2.new(0.5, -125, 0, -50)
        NF.BackgroundColor3 = Color3.fromRGB(15, 15, 25) NF.BackgroundTransparency = 0.15 NF.Parent = MainGui
        CR.CornerRadius = UDim.new(0, 8) CR.Parent = NF
        local NS = Instance.new("UIStroke") NS.Color = RandomThemeColor NS.Thickness = 1.2 NS.Parent = NF
        TL.Size = UDim2.new(1, 0, 1, 0) TL.BackgroundTransparency = 1 TL.Font = Enum.Font.GothamBold
        TL.Text = msg TL.TextColor3 = Color3.fromRGB(255, 255, 255) TL.TextSize = 11 TL.Parent = NF
        NF:TweenPosition(UDim2.new(0.5, -125, 0, 20), "Out", "Back", 0.4, true)
        task.wait(2.2)
        local ft = _EX.T:Create(NF, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -125, 0, -50), BackgroundTransparency = 1})
        _EX.T:Create(TL, TweenInfo.new(0.2), {TextTransparency = 1}):Play() ft:Play()
        ft.Completed:Connect(function() NF:Destroy() end)
    end

    ---------------------------------------------------------
    -- [نافذة الترحيب - Splash Screen]
    ---------------------------------------------------------
    local Splash = Instance.new("Frame") local SplashCorner = Instance.new("UICorner") local SplashStroke = Instance.new("UIStroke")
    local SplashLabel = Instance.new("TextLabel")
    Splash.Size = UDim2.new(0, 300, 0, 180) Splash.Position = UDim2.new(0.5, -150, 0.5, -90)
    Splash.BackgroundColor3 = Color3.fromRGB(10, 10, 14) Splash.Parent = MainGui
    SplashCorner.CornerRadius = UDim.new(0, 12) SplashCorner.Parent = Splash
    SplashStroke.Color = RandomThemeColor SplashStroke.Thickness = 2 SplashStroke.Parent = Splash
    SplashLabel.Size = UDim2.new(1, 0, 1, 0) SplashLabel.BackgroundTransparency = 1 SplashLabel.Font = Enum.Font.GothamBlack
    SplashLabel.Text = "by;mohammeedd78" SplashLabel.TextColor3 = RandomThemeColor SplashLabel.TextSize = 22 SplashLabel.Parent = Splash

    task.wait(1.5)
    local fadeSplash = _EX.T:Create(Splash, TweenInfo.new(0.5), {BackgroundTransparency = 1})
    _EX.T:Create(SplashStroke, TweenInfo.new(0.5), {Transparency = 1}):Play()
    _EX.T:Create(SplashLabel, TweenInfo.new(0.4), {TextTransparency = 1}):Play()
    fadeSplash:Play()
    fadeSplash.Completed:Connect(function() Splash:Destroy() end)

    ---------------------------------------------------------
    -- [الواجهة الرئيسية للسكربت - Main Container]
    ---------------------------------------------------------
    local Container = Instance.new("Frame") local ContainerCorner = Instance.new("UICorner") local ContainerStroke = Instance.new("UIStroke")
    local FullSize = UDim2.new(0, 600, 0, 340) local FullPos = UDim2.new(0.5, -300, 0.5, -170)

    Container.Size = UDim2.new(0, 0, 0, 0) Container.Position = UDim2.new(0.5, 0, 0.5, 0)
    Container.BackgroundColor3 = Color3.fromRGB(12, 12, 18) Container.BackgroundTransparency = 0.05 Container.ClipsDescendants = true Container.Parent = MainGui
    ContainerCorner.CornerRadius = UDim.new(0, 10) ContainerCorner.Parent = Container
    ContainerStroke.Color = RandomThemeColor ContainerStroke.Thickness = 1.5 ContainerStroke.Parent = Container
    MakeDraggable(Container)

    ---------------------------------------------------------
    -- [شريط القوائم الجانبي - Sidebar Tabs]
    ---------------------------------------------------------
    local Sidebar = Instance.new("Frame") local SBCorner = Instance.new("UICorner")
    Sidebar.Size = UDim2.new(0, 150, 1, 0) Sidebar.BackgroundColor3 = Color3.fromRGB(16, 16, 22) Sidebar.Parent = Container
    SBCorner.CornerRadius = UDim.new(0, 10) SBCorner.Parent = Sidebar

    local HubLogo = Instance.new("TextLabel")
    HubLogo.Size = UDim2.new(1, 0, 0, 40) HubLogo.BackgroundTransparency = 1 HubLogo.Font = Enum.Font.GothamBlack
    HubLogo.Text = "🔮 1993 HUB" HubLogo.TextColor3 = RandomThemeColor HubLogo.TextSize = 14 HubLogo.Parent = Sidebar

    local TabScrollContainer = Instance.new("ScrollingFrame")
    TabScrollContainer.Size = UDim2.new(1, 0, 1, -105) TabScrollContainer.Position = UDim2.new(0, 0, 0, 45) TabScrollContainer.BackgroundTransparency = 1
    TabScrollContainer.ScrollBarThickness = 2 TabScrollContainer.ScrollBarImageColor3 = RandomThemeColor TabScrollContainer.Parent = Sidebar
    local TabScrollLayout = Instance.new("UIListLayout") TabScrollLayout.Padding = UDim.new(0, 4) TabScrollLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center TabScrollLayout.Parent = TabScrollContainer

    ---------------------------------------------------------
    -- [معلومات الحساب أسفل اليسار]
    ---------------------------------------------------------
    local UserProfile = Instance.new("Frame") local UPCorner = Instance.new("UICorner")
    UserProfile.Size = UDim2.new(1, -10, 0, 50) UserProfile.Position = UDim2.new(0, 5, 1, -55) UserProfile.BackgroundColor3 = Color3.fromRGB(24, 24, 32) UserProfile.Parent = Sidebar
    UPCorner.CornerRadius = UDim.new(0, 6) UPCorner.Parent = UserProfile

    local AvatarImg = Instance.new("ImageLabel") local AICorner = Instance.new("UICorner")
    AvatarImg.Size = UDim2.new(0, 34, 0, 34) AvatarImg.Position = UDim2.new(0, 6, 0.5, -17) AvatarImg.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
    AvatarImg.Image = "rbxthumb://type=AvatarHeadShot&id=" .. _EX.L.UserId .. "&w=150&h=150" AvatarImg.Parent = UserProfile
    AICorner.CornerRadius = UDim.new(1, 0) AICorner.Parent = AvatarImg

    local DisplayNameLabel = Instance.new("TextLabel")
    DisplayNameLabel.Size = UDim2.new(1, -48, 0, 16) DisplayNameLabel.Position = UDim2.new(0, 44, 0, 8) DisplayNameLabel.BackgroundTransparency = 1 DisplayNameLabel.Font = Enum.Font.GothamBold
    DisplayNameLabel.Text = _EX.L.DisplayName DisplayNameLabel.TextColor3 = Color3.fromRGB(255, 255, 255) DisplayNameLabel.TextSize = 9 DisplayNameLabel.TextXAlignment = Enum.TextXAlignment.Left DisplayNameLabel.Parent = UserProfile

    local UsernameLabel = Instance.new("TextLabel")
    UsernameLabel.Size = UDim2.new(1, -48, 0, 14) UsernameLabel.Position = UDim2.new(0, 44, 0, 24) UsernameLabel.BackgroundTransparency = 1 UsernameLabel.Font = Enum.Font.Gotham
    UsernameLabel.Text = "@" .. _EX.L.Name UsernameLabel.TextColor3 = RandomThemeColor UsernameLabel.TextSize = 8 UsernameLabel.TextXAlignment = Enum.TextXAlignment.Left UsernameLabel.Parent = UserProfile

    ---------------------------------------------------------
    -- نظام الصفحات الداخلي للسكربت
    ---------------------------------------------------------
    local ContentFrame = Instance.new("Frame") ContentFrame.Size = UDim2.new(1, -165, 1, -15) ContentFrame.Position = UDim2.new(0, 158, 0, 8) ContentFrame.BackgroundTransparency = 1 ContentFrame.Parent = Container

    local Pages = {}
    local function CreateTab(name, icon)
        local Page = Instance.new("Frame")
        Page.Size = UDim2.new(1, 0, 1, 0) Page.BackgroundTransparency = 1 Page.Visible = false Page.Parent = ContentFrame
        Pages[name] = Page
        
        local TabBtn = Instance.new("TextButton") local TBC = Instance.new("UICorner")
        TabBtn.Size = UDim2.new(1, -10, 0, 32) TabBtn.BackgroundColor3 = Color3.fromRGB(24, 24, 32) TabBtn.Font = Enum.Font.GothamSemibold TabBtn.Text = name .. " " .. icon TabBtn.TextColor3 = Color3.fromRGB(160, 160, 160) TabBtn.TextSize = 10 TabBtn.Parent = TabScrollContainer TBC.CornerRadius = UDim.new(0, 5) TBC.Parent = TabBtn
        
        TabBtn.MouseButton1Click:Connect(function()
            for _, p in pairs(Pages) do p.Visible = false end
            for _, b in ipairs(TabScrollContainer:GetChildren()) do if b:IsA("TextButton") then b.BackgroundColor3 = Color3.fromRGB(24, 24, 32) b.TextColor3 = Color3.fromRGB(160, 160, 160) end end
            Page.Visible = true TabBtn.BackgroundColor3 = RandomThemeColor TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        end)
        return Page
    end

    local CopyV1Page = CreateTab("نسخ V1", "🎗")
    local PullPlayerPage = CreateTab("سحب شخص", "🎯")
    local QuickCopyV2Page = CreateTab("نسخ سريع V2", "🎭")
    local MusicPage = CreateTab("الاغاني", "🎧")
    local BoysSkinsPage = CreateTab("اسكنات الاولاد", "🎗") 
    local OtherPage = CreateTab("اخرى", "🎲")

    Pages["نسخ V1"].Visible = true
    TabScrollContainer:FindFirstChildOfClass("TextButton").BackgroundColor3 = RandomThemeColor
    TabScrollContainer:FindFirstChildOfClass("TextButton").TextColor3 = Color3.fromRGB(255, 255, 255)
    TabScrollContainer.CanvasSize = UDim2.new(0, 0, 0, TabScrollLayout.AbsoluteContentSize.Y + 10)

    ---------------------------------------------------------
    -- [1. صفحة نسخ V1 - تم تحويل البريفيكس إلى /]
    ---------------------------------------------------------
    local CopyV1Container = Instance.new("Frame")
    CopyV1Container.Size = UDim2.new(1, 0, 1, 0) CopyV1Container.BackgroundTransparency = 1 CopyV1Container.Parent = CopyV1Page

    local LeftControlsV1 = Instance.new("Frame")
    LeftControlsV1.Size = UDim2.new(0, 210, 1, 0) LeftControlsV1.BackgroundTransparency = 1 LeftControlsV1.Parent = CopyV1Container

    local NameInputV1 = Instance.new("TextBox") local NICV1 = Instance.new("UICorner")
    NameInputV1.Size = UDim2.new(1, 0, 0, 32) NameInputV1.Position = UDim2.new(0, 0, 0, 5)
    NameInputV1.BackgroundColor3 = Color3.fromRGB(22, 22, 30) NameInputV1.Font = Enum.Font.Gotham NameInputV1.PlaceholderText = "اسم اللاعب المستهدف بالحلقة..." NameInputV1.Text = "" NameInputV1.TextColor3 = Color3.fromRGB(255, 255, 255) NameInputV1.TextSize = 11 NameInputV1.Parent = LeftControlsV1 NICV1.CornerRadius = UDim.new(0, 6) NICV1.Parent = NameInputV1

    local ActionButtonV1 = Instance.new("TextButton") local BCV1 = Instance.new("UICorner")
    ActionButtonV1.Size = UDim2.new(1, 0, 0, 35) ActionButtonV1.Position = UDim2.new(0, 0, 0, 45)
    ActionButtonV1.BackgroundColor3 = RandomThemeColor ActionButtonV1.Font = Enum.Font.GothamBold ActionButtonV1.Text = "تفعيل حلقة الأوامر الإدارية" ActionButtonV1.TextColor3 = Color3.fromRGB(255, 255, 255) ActionButtonV1.TextSize = 11 ActionButtonV1.Parent = LeftControlsV1 BCV1.CornerRadius = UDim.new(0, 6) BCV1.Parent = ActionButtonV1

    local ColoredNamePanel = Instance.new("Frame") local CNCorner = Instance.new("UICorner") local CNStroke = Instance.new("UIStroke")
    ColoredNamePanel.Size = UDim2.new(1, 0, 0, 80) ColoredNamePanel.Position = UDim2.new(0, 0, 0, 90) ColoredNamePanel.BackgroundColor3 = Color3.fromRGB(20, 15, 30) ColoredNamePanel.Parent = LeftControlsV1
    CNCorner.CornerRadius = UDim.new(0, 6) CNCorner.Parent = ColoredNamePanel
    CNStroke.Color = Color3.fromRGB(150, 0, 255) CNStroke.Thickness = 1 CNStroke.Parent = ColoredNamePanel

    local ColoredNameInput = Instance.new("TextBox") local CNICorner = Instance.new("UICorner")
    ColoredNameInput.Size = UDim2.new(1, -12, 0, 28) ColoredNameInput.Position = UDim2.new(0, 6, 0, 8)
    ColoredNameInput.BackgroundColor3 = Color3.fromRGB(30, 20, 45) ColoredNameInput.Font = Enum.Font.GothamBold ColoredNameInput.Text = "ADF ON TOP" ColoredNameInput.TextColor3 = Color3.fromRGB(255, 255, 255) ColoredNameInput.TextSize = 11 ColoredNameInput.Parent = ColoredNamePanel
    CNICorner.CornerRadius = UDim.new(0, 4) CNICorner.Parent = ColoredNameInput

    local ColoredNameToggleBtn = Instance.new("TextButton") local CNBCorner = Instance.new("UICorner")
    ColoredNameToggleBtn.Size = UDim2.new(1, -12, 0, 30) ColoredNameToggleBtn.Position = UDim2.new(0, 6, 0, 42) ColoredNameToggleBtn.BackgroundColor3 = Color3.fromRGB(40, 20, 80) ColoredNameToggleBtn.Font = Enum.Font.GothamBold ColoredNameToggleBtn.Text = "🌈 تفعيل الاسم الملون" LazyName = true ColoredNameToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255) ColoredNameToggleBtn.TextSize = 11 ColoredNameToggleBtn.Parent = ColoredNamePanel
    CNBCorner.CornerRadius = UDim.new(0, 4) CNBCorner.Parent = ColoredNameToggleBtn

    local RightPanelV1 = Instance.new("Frame")
    RightPanelV1.Size = UDim2.new(1, -220, 1, 0) RightPanelV1.Position = UDim2.new(0, 220, 0, 0) RightPanelV1.BackgroundColor3 = Color3.fromRGB(18, 18, 26) RightPanelV1.Parent = CopyV1Container
    local RPCCV1 = Instance.new("UICorner") RPCCV1.CornerRadius = UDim.new(0, 8) RPCCV1.Parent = RightPanelV1
    local RPSSV1 = Instance.new("UIStroke") RPSSV1.Color = Color3.fromRGB(40, 40, 50) RPSSV1.Thickness = 1 RPSSV1.Parent = RightPanelV1

    local V1ListTitle = Instance.new("TextLabel") V1ListTitle.Size = UDim2.new(1, 0, 0, 25) V1ListTitle.BackgroundTransparency = 1 V1ListTitle.Font = Enum.Font.GothamBold V1ListTitle.Text = "👥 قايمه اللاعبين" V1ListTitle.TextColor3 = Color3.fromRGB(255, 255, 255) V1ListTitle.TextSize = 10 V1ListTitle.Parent = RightPanelV1

    local V1Scroll = Instance.new("ScrollingFrame") local V1ListLayout = Instance.new("UIListLayout")
    V1Scroll.Size = UDim2.new(1, -8, 1, -35) V1Scroll.Position = UDim2.new(0, 4, 0, 28) V1Scroll.BackgroundTransparency = 1 V1Scroll.ScrollBarThickness = 3 V1Scroll.ScrollBarImageColor3 = RandomThemeColor V1Scroll.Parent = RightPanelV1
    V1ListLayout.SortOrder = Enum.SortOrder.LayoutOrder V1ListLayout.Padding = UDim.new(0, 4) V1ListLayout.Parent = V1Scroll

    local function RefreshV1PlayerList()
        for _, child in ipairs(V1Scroll:GetChildren()) do if child:IsA("Frame") then child:Destroy() end end
        for _, player in ipairs(_EX.P:GetPlayers()) do
            if player ~= _EX.L then
                local PF = Instance.new("Frame") local PFC = Instance.new("UICorner")
                PF.Name = player.Name PF.Size = UDim2.new(1, -6, 0, 30) PF.BackgroundColor3 = Color3.fromRGB(28, 28, 40) PF.Parent = V1Scroll
                PFC.CornerRadius = UDim.new(0, 5) PFC.Parent = PF
                
                local PN = Instance.new("TextButton") PN.Size = UDim2.new(1, -8, 1, 0) PN.Position = UDim2.new(0, 6, 0, 0) PN.BackgroundTransparency = 1 PN.Font = Enum.Font.GothamSemibold 
                PN.Text = "👤 " .. player.Name .. " (" .. player.DisplayName .. ")" 
                PN.TextColor3 = Color3.fromRGB(220, 220, 230) PN.TextSize = 10 PN.TextXAlignment = Enum.TextXAlignment.Left PN.Parent = PF
                
                PN.MouseButton1Click:Connect(function()
                    NameInputV1.Text = player.Name
                    CreateNotification("تم اختيار المستهدف لنسخ V1: " .. player.Name)
                end)
            end
        end
        V1Scroll.CanvasSize = UDim2.new(0, 0, 0, V1ListLayout.AbsoluteContentSize.Y + 5)
    end

    _EX.P.PlayerAdded:Connect(RefreshV1PlayerList)
    _EX.P.PlayerRemoving:Connect(function(player)
        local existing = V1Scroll:FindFirstChild(player.Name)
        if existing then existing:Destroy() end
        V1Scroll.CanvasSize = UDim2.new(0, 0, 0, V1ListLayout.AbsoluteContentSize.Y + 5)
    end)
    RefreshV1PlayerList()

    local RGBRun = false
    ColoredNameToggleBtn.MouseButton1Click:Connect(function()
        if RGBRun then
            RGBRun = false 
            ColoredNameToggleBtn.Text = "🌈 تفعيل الاسم الملون" 
            ColoredNameToggleBtn.BackgroundColor3 = Color3.fromRGB(40, 20, 80)
            CreateNotification("تم إيقاف الاسم الملون وإزالته")
            
            pcall(function()
                local r = _EX.R:FindFirstChild("ApplyTitle", true) or _EX.R:FindFirstChild("ChangeTitle", true)
                if r then r:FireServer("", Color3.fromRGB(255, 255, 255)) end
            end)
        else
            RGBRun = true 
            ColoredNameToggleBtn.Text = "🛑 إلغاء تفعيل الاسم" 
            ColoredNameToggleBtn.BackgroundColor3 = Color3.fromRGB(150, 20, 20)
            CreateNotification("تم تنشيط الاسم الملون المتغير تلقائياً!")
            task.spawn(function()
                local hu = 0
                while RGBRun do
                    local r = _EX.R:FindFirstChild("ApplyTitle", true) or _EX.R:FindFirstChild("ChangeTitle", true)
                    if r then pcall(function() r:FireServer(ColoredNameInput.Text, Color3.fromHSV(hu, 1, 1)) end) end
                    hu = hu + 0.03 if hu >= 1 then hu = 0 end task.wait(0.4)
                end
            end)
        end
    end)

    local IsSpamming = false
    -- [تعديل]: تم تحويل البريفيكس هنا بالكامل لعلامة /
    local CustomCommands = {
        "/re", "/nv", "/res", "/clogs", "/logs", "/nv", "/res", "/logs", 
        "/clogs", "/clogs", "/nv", "/nv", "/ice", "/jc", "/nv", "/res", 
        "/logs", "/fire", "/logs"
    }

    ActionButtonV1.MouseButton1Click:Connect(function()
        IsSpamming = not IsSpamming
        if IsSpamming then
            ActionButtonV1.Text = "إيقاف الحلقة [نشط]" ActionButtonV1.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            task.spawn(function()
                while IsSpamming do
                    local targetName = NameInputV1.Text ~= "" and NameInputV1.Text or _EX.L.Name
                    
                    local bundledCommand = ""
                    for i, cmd in ipairs(CustomCommands) do
                        bundledCommand = bundledCommand .. cmd .. " " .. targetName .. (i < #CustomCommands and " " or "")
                    end
                    
                    FireNewCopyRemotes(bundledCommand)
                    task.wait(0.1)
                end
            end)
        else
            ActionButtonV1.Text = "تفعيل حلقة الأوامر الإدارية" ActionButtonV1.BackgroundColor3 = RandomThemeColor
        end
    end)

    ---------------------------------------------------------
    -- [2. صفحة سحب اللاعبين - تحويل البريفيكس إلى /]
    ---------------------------------------------------------
    local PullContainer = Instance.new("Frame")
    PullContainer.Size = UDim2.new(1, 0, 1, 0) PullContainer.BackgroundTransparency = 1 PullContainer.Parent = PullPlayerPage

    local LeftPullControls = Instance.new("Frame")
    LeftPullControls.Size = UDim2.new(0, 200, 1, 0) LeftPullControls.BackgroundTransparency = 1 LeftPullControls.Parent = PullContainer

    local TargetNameInput = Instance.new("TextBox") local TNC = Instance.new("UICorner")
    TargetNameInput.Size = UDim2.new(1, 0, 0, 35) TargetNameInput.Position = UDim2.new(0, 0, 0, 5)
    TargetNameInput.BackgroundColor3 = Color3.fromRGB(22, 22, 30) TargetNameInput.Font = Enum.Font.Gotham TargetNameInput.PlaceholderText = "اسم اللاعب المستهدف..." TargetNameInput.Text = "" TargetNameInput.TextColor3 = Color3.fromRGB(255, 255, 255) TargetNameInput.TextSize = 11 TargetNameInput.Parent = LeftPullControls TNC.CornerRadius = UDim.new(0, 6) TNC.Parent = TargetNameInput

    local ActionPullButton = Instance.new("TextButton") local APBC = Instance.new("UICorner")
    ActionPullButton.Size = UDim2.new(1, 0, 0, 38) ActionPullButton.Position = UDim2.new(0, 0, 0, 50)
    ActionPullButton.BackgroundColor3 = RandomThemeColor ActionPullButton.Font = Enum.Font.GothamBold ActionPullButton.Text = "تنفيذ أمر السحب المباشر (/tp)" ActionPullButton.TextColor3 = Color3.fromRGB(255, 255, 255) ActionPullButton.TextSize = 11 ActionPullButton.Parent = LeftPullControls APBC.CornerRadius = UDim.new(0, 6) APBC.Parent = ActionPullButton

    local RightPullPanel = Instance.new("Frame")
    RightPullPanel.Size = UDim2.new(1, -210, 1, 0) RightPullPanel.Position = UDim2.new(0, 210, 0, 0) RightPullPanel.BackgroundColor3 = Color3.fromRGB(18, 18, 26) RightPullPanel.Parent = PullContainer
    local RPCC = Instance.new("UICorner") RPCC.CornerRadius = UDim.new(0, 8) RPCC.Parent = RightPullPanel
    local RPSS = Instance.new("UIStroke") RPSS.Color = Color3.fromRGB(40, 40, 50) RPSS.Thickness = 1 RPSS.Parent = RightPullPanel

    local PTit = Instance.new("TextLabel") PTit.Size = UDim2.new(1, 0, 0, 25) PTit.BackgroundTransparency = 1 PTit.Font = Enum.Font.GothamBold PTit.Text = "👥 قائمة اللاعبين الحالية بالسيرفر" PTit.TextColor3 = Color3.fromRGB(255, 255, 255) PTit.TextSize = 10 PTit.Parent = RightPullPanel

    local PScroll = Instance.new("ScrollingFrame") local PListLayout = Instance.new("UIListLayout")
    PScroll.Size = UDim2.new(1, -8, 1, -35) PScroll.Position = UDim2.new(0, 4, 0, 28) PScroll.BackgroundTransparency = 1 PScroll.ScrollBarThickness = 3 PScroll.ScrollBarImageColor3 = RandomThemeColor PScroll.Parent = RightPullPanel
    PListLayout.SortOrder = Enum.SortOrder.LayoutOrder PListLayout.Padding = UDim.new(0, 4) PListLayout.Parent = PScroll

    local function RefreshPullList()
        for _, child in ipairs(PScroll:GetChildren()) do if child:IsA("Frame") then child:Destroy() end end
        for _, player in ipairs(_EX.P:GetPlayers()) do
            if player ~= _EX.L then
                local PF = Instance.new("Frame") local PFC = Instance.new("UICorner")
                PF.Name = player.Name
                PF.Size = UDim2.new(1, -6, 0, 30) PF.BackgroundColor3 = Color3.fromRGB(28, 28, 40) PF.Parent = PScroll
                PFC.CornerRadius = UDim.new(0, 5) PFC.Parent = PF
                local PN = Instance.new("TextButton") PN.Size = UDim2.new(1, -8, 1, 0) PN.Position = UDim2.new(0, 6, 0, 0) PN.BackgroundTransparency = 1 PN.Font = Enum.Font.GothamSemibold PN.Text = "🎯 " .. player.Name PN.TextColor3 = Color3.fromRGB(220, 220, 230) PN.TextSize = 10 PN.TextXAlignment = Enum.TextXAlignment.Left PN.Parent = PF
                PN.MouseButton1Click:Connect(function()
                    TargetNameInput.Text = player.Name
                    ActionPullButton.Text = "اضغط لسحب: /tp " .. player.Name
                    CreateNotification("تم تحديد الهدف: " .. player.Name)
                end)
            end
        end
        PScroll.CanvasSize = UDim2.new(0, 0, 0, PListLayout.AbsoluteContentSize.Y + 5)
    end

    _EX.P.PlayerAdded:Connect(function(player) RefreshPullList() end)
    _EX.P.PlayerRemoving:Connect(function(player) local existingFrame = PScroll:FindFirstChild(player.Name) if existingFrame then existingFrame:Destroy() end PScroll.CanvasSize = UDim2.new(0, 0, 0, PListLayout.AbsoluteContentSize.Y + 5) end)
    RefreshPullList()

    ActionPullButton.MouseButton1Click:Connect(function()
        local target = TargetNameInput.Text
        if target ~= "" then
            local fullCmd = "/tp " .. target
            FireOldRemote(fullCmd)
            CreateNotification("تم إرسال أمر السحب (/tp) لـ " .. target)
        else
            CreateNotification("يرجى اختيار أو كتابة اسم لاعب!")
        end
    end)

    ---------------------------------------------------------
    -- [3. صفحة نسخ سريع V2 - تعديل البريفيكس التلقائي إلى /]
    ---------------------------------------------------------
    local SelTable, Run, UseShortName = {}, false, true

    local V2LeftFrame = Instance.new("Frame")
    V2LeftFrame.Size = UDim2.new(0, 230, 1, 0) V2LeftFrame.BackgroundTransparency = 1 V2LeftFrame.Parent = QuickCopyV2Page

    local V2PList = Instance.new("ScrollingFrame") local V2ListLayout = Instance.new("UIListLayout")
    V2PList.Size = UDim2.new(1, 0, 0, 100) V2PList.Position = UDim2.new(0, 0, 0, 5) V2PList.BackgroundColor3 = Color3.fromRGB(15, 10, 25) V2PList.BackgroundTransparency = 0.4 V2PList.ScrollBarThickness = 3 V2PList.Parent = V2LeftFrame
    V2ListLayout.SortOrder = Enum.SortOrder.LayoutOrder V2ListLayout.Padding = UDim.new(0, 4) V2ListLayout.Parent = V2PList
    Instance.new("UICorner", V2PList).CornerRadius = UDim.new(0, 8)

    local V2SearchInp = Instance.new("TextBox") local V2SearchCorner = Instance.new("UICorner")
    V2SearchInp.Size = UDim2.new(0, 160, 0, 28) V2SearchInp.Position = UDim2.new(0, 0, 0, 110) V2SearchInp.BackgroundColor3 = Color3.fromRGB(25, 15, 35) V2SearchInp.PlaceholderText = "🔍 اكتب أول حرفين..." V2SearchInp.Text = "" V2SearchInp.TextColor3 = Color3.fromRGB(220, 180, 255) V2SearchInp.Font = Enum.Font.GothamBold V2SearchInp.TextSize = 10 V2SearchInp.Parent = V2LeftFrame
    V2SearchCorner.CornerRadius = UDim.new(0, 6) V2SearchCorner.Parent = V2SearchInp

    local ModeToggleBtn = Instance.new("TextButton") local MTCorner = Instance.new("UICorner")
    ModeToggleBtn.Size = UDim2.new(0, 65, 0, 28) ModeToggleBtn.Position = UDim2.new(0, 165, 0, 110) ModeToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 140, 60) ModeToggleBtn.Font = Enum.Font.GothamBold ModeToggleBtn.Text = "⚡ اختصار" ModeToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255) ModeToggleBtn.TextSize = 10 ModeToggleBtn.Parent = V2LeftFrame
    MTCorner.CornerRadius = UDim.new(0, 6) MTCorner.Parent = ModeToggleBtn

    ModeToggleBtn.MouseButton1Click:Connect(function()
        UseShortName = not UseShortName
        ModeToggleBtn.Text = UseShortName and "⚡ اختصار" or "👤 كامل"
        ModeToggleBtn.BackgroundColor3 = UseShortName and Color3.fromRGB(0, 140, 60) or Color3.fromRGB(130, 0, 130)
    end)

    local V2Inp = Instance.new("TextBox") local V2InpCorner = Instance.new("UICorner")
    V2Inp.Size = UDim2.new(1, 0, 0, 32) V2Inp.Position = UDim2.new(0, 0, 0, 143) V2Inp.BackgroundColor3 = Color3.fromRGB(20, 15, 30) V2Inp.PlaceholderText = "اكتب أمر مخصص يدوي هنا..." V2Inp.Text = "" V2Inp.TextColor3 = Color3.fromRGB(210, 160, 255) V2Inp.Font = Enum.Font.GothamBold V2Inp.TextSize = 11 V2Inp.Parent = V2LeftFrame
    V2InpCorner.CornerRadius = UDim.new(0, 6) V2InpCorner.Parent = V2Inp

    local QkFrame = Instance.new("Frame")
    QkFrame.Size = UDim2.new(1, 0, 0, 75) QkFrame.Position = UDim2.new(0, 0, 0, 180) QkFrame.BackgroundTransparency = 1 QkFrame.Parent = V2LeftFrame
    local V2Grid = Instance.new("UIGridLayout") V2Grid.CellSize = UDim2.new(0, 112, 0, 32) V2Grid.CellPadding = UDim2.new(0, 6, 0, 6) V2Grid.Parent = QkFrame

    local function CreateQkBtn(text, cmds)
        local btn = Instance.new("TextButton") local btnC = Instance.new("UICorner")
        btn.Text = text btn.BackgroundColor3 = Color3.fromRGB(35, 15, 55) btn.TextColor3 = Color3.fromRGB(235, 220, 255) btn.Font = Enum.Font.GothamBold btn.TextSize = 10 btn.Parent = QkFrame btnC.CornerRadius = UDim.new(0, 6) btnC.Parent = btn
        btn.MouseButton1Click:Connect(function() V2Inp.Text = cmds CreateNotification("تم تعيين نمط: " .. text) end)
    end
    -- تم تحديث الأنماط الجاهزة لتبدأ بـ / تلقائياً
    CreateQkBtn("نسخ غامض", "/explode /logs /re /res /nv")
    CreateQkBtn("نسخ هيد admin", "/explode /warp /re /res /nv")
    CreateQkBtn("نسخ يعلق", "/logs /nv /re /res")
    CreateQkBtn("نسخ تعذيب", "/dog /char miri /jc /tp /ice")

    local V2RightFrame = Instance.new("Frame") V2RightFrame.Size = UDim2.new(1, -240, 1, 0) V2RightFrame.Position = UDim2.new(0, 240, 0, 0) V2RightFrame.BackgroundTransparency = 1 V2RightFrame.Parent = QuickCopyV2Page

    local StartBtn = Instance.new("TextButton") local SBC1 = Instance.new("UICorner")
    StartBtn.Size = UDim2.new(1, 0, 0, 35) StartBtn.Position = UDim2.new(0, 0, 0, 10) StartBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 160) StartBtn.Font = Enum.Font.GothamBold StartBtn.Text = "🚀 تفعيل التشغيل" StartBtn.TextColor3 = Color3.fromRGB(255, 255, 255) StartBtn.TextSize = 11 StartBtn.Parent = V2RightFrame SBC1.CornerRadius = UDim.new(0, 6) SBC1.Parent = StartBtn

    local StopBtn = Instance.new("TextButton") local SBC2 = Instance.new("UICorner")
    StopBtn.Size = UDim2.new(1, 0, 0, 35) StopBtn.Position = UDim2.new(0, 0, 0, 50) StopBtn.BackgroundColor3 = Color3.fromRGB(160, 20, 20) StopBtn.Font = Enum.Font.GothamBold StopBtn.Text = "🛑 إيقاف التشغيل" StopBtn.TextColor3 = Color3.fromRGB(255, 255, 255) StopBtn.TextSize = 11 StopBtn.Parent = V2RightFrame SBC2.CornerRadius = UDim.new(0, 6) SBC2.Parent = StopBtn

    local SpeedLabel = Instance.new("TextLabel") SpeedLabel.Size = UDim2.new(1, 0, 0, 20) SpeedLabel.Position = UDim2.new(0, 0, 0, 95) SpeedLabel.BackgroundTransparency = 1 SpeedLabel.Font = Enum.Font.GothamBold SpeedLabel.Text = "⏱️ سرعة النسخ بالتأخير:" SpeedLabel.TextColor3 = Color3.fromRGB(190, 160, 255) SpeedLabel.TextSize = 10 SpeedLabel.Parent = V2RightFrame
    local SpeedInp = Instance.new("TextBox") local SICorner = Instance.new("UICorner")
    SpeedInp.Size = UDim2.new(1, 0, 0, 32) SpeedInp.Position = UDim2.new(0, 0, 0, 120) SpeedInp.BackgroundColor3 = Color3.fromRGB(25, 15, 35) SpeedInp.Text = "0.1"
    SpeedInp.TextColor3 = Color3.fromRGB(0, 255, 150) SpeedInp.Font = Enum.Font.GothamBold SpeedInp.TextSize = 12 SpeedInp.Parent = V2RightFrame SICorner.CornerRadius = UDim.new(0, 6) SICorner.Parent = SpeedInp

    local function UpV2PlayersList()
        for _, child in ipairs(V2PList:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        local st = V2SearchInp.Text:lower()
        for _, p in pairs(_EX.P:GetPlayers()) do
            if p ~= _EX.L and (st == "" or string.sub(p.Name:lower(), 1, #st) == st) then
                local b = Instance.new("TextButton", V2PList)
                b.Size = UDim2.new(1, 0, 0, 28) b.BackgroundColor3 = Color3.fromRGB(25, 15, 45) b.BackgroundTransparency = 0.4 b.Font = Enum.Font.Gotham b.TextSize = 10 b.TextColor3 = Color3.fromRGB(190, 130, 255) b.TextXAlignment = Enum.TextXAlignment.Left b.Text = "  🛸 " .. p.Name Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
                if table.find(SelTable, p.Name) then b.BackgroundColor3 = Color3.fromRGB(120, 0, 255) end
                b.MouseButton1Click:Connect(function()
                    local idx = table.find(SelTable, p.Name)
                    if idx then table.remove(SelTable, idx) b.BackgroundColor3 = Color3.fromRGB(25, 15, 45) else table.insert(SelTable, p.Name) b.BackgroundColor3 = Color3.fromRGB(120, 0, 255) end
                end)
            end
        end
        V2PList.CanvasSize = UDim2.new(0, 0, 0, V2ListLayout.AbsoluteContentSize.Y + 5)
    end

    V2SearchInp:GetPropertyChangedSignal("Text"):Connect(function()
        UpV2PlayersList() local text = V2SearchInp.Text:lower()
        if #text >= 2 then
            for _, p in pairs(_EX.P:GetPlayers()) do
                if p ~= _EX.L and string.sub(p.Name:lower(), 1, #text) == text and not table.find(SelTable, p.Name) then
                    table.insert(SelTable, p.Name) UpV2PlayersList()
                end
            end
        end
    end)
    _EX.P.PlayerAdded:Connect(UpV2PlayersList) _EX.P.PlayerRemoving:Connect(function(p) local idx = table.find(SelTable, p.Name) if idx then table.remove(SelTable, idx) end UpV2PlayersList() end) UpV2PlayersList()

    StartBtn.MouseButton1Click:Connect(function()
        if #SelTable == 0 or V2Inp.Text == "" then CreateNotification("اختر هدفاً واكتب أمراً!") return end
        if Run then return end Run = true
        StartBtn.BackgroundColor3 = Color3.fromRGB(130, 0, 255) StartBtn.Text = "⚡ جاري التدمير نشط ⚡"
        
        task.spawn(function()
            while Run do
                for _, target in pairs(SelTable) do
                    local targetName = (UseShortName and V2SearchInp.Text ~= "" and #V2SearchInp.Text >= 2) and V2SearchInp.Text:lower() or target:lower()
                    
                    for c in V2Inp.Text:gmatch("%S+") do
                        -- [تعديل]: التأكد من إضافة علامة / تلقائياً إذا لم يكتبها المستخدم يدوياً
                        local prefix = (string.sub(c, 1, 1) == "/" or string.sub(c, 1, 1) == ";") and "" or "/"
                        local commandText = prefix .. c .. " " .. targetName
                        
                        FireNewCopyRemotes(commandText) 
                    end
                end
                local customDelay = tonumber(SpeedInp.Text) or 0.1
                task.wait(customDelay)
            end
        end)
    end)
    StopBtn.MouseButton1Click:Connect(function() Run = false StartBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 160) StartBtn.Text = "🚀 تفعيل التشغيل" end)

    ---------------------------------------------------------
    -- [4. مكتبة الأغاني]
    ---------------------------------------------------------
    local MusicScroll = Instance.new("ScrollingFrame") local MusicListLayout = Instance.new("UIListLayout")
    MusicScroll.Size = UDim2.new(1, 0, 1, 0) MusicScroll.BackgroundTransparency = 1 MusicScroll.ScrollBarThickness = 3 MusicScroll.ScrollBarImageColor3 = RandomThemeColor MusicScroll.Parent = MusicPage
    MusicListLayout.SortOrder = Enum.SortOrder.LayoutOrder MusicListLayout.Padding = UDim.new(0, 5) MusicListLayout.Parent = MusicScroll

    local VerifiedLibrary = {
        {Name = "🎵 129793988394147", Code = "129793988394147"},
        {Name = "🎵 131732248464220", Code = "131732248464220"},
        {Name = "🎵 97362029498637", Code = "97362029498637"},
        {Name = "🎵 112355709978731", Code = "112355709978731"},
        {Name = "🎵 94308954886862", Code = "94308954886862"},
        {Name = "🎵 116174401794512", Code = "116174401794512"},
        {Name = "🎵 115951236010098", Code = "115951236010098"},
        {Name = "🎵 111811908070601", Code = "111811908070601"},
        {Name = "🎵 140415473717614", Code = "140415473717614"},
        {Name = "🎵 120871403922972", Code = "120871403922972"},
        {Name = "🎵 127840997774724", Code = "127840997774724"},
        {Name = "🎵 109473586258688", Code = "109473586258688"},
        {Name = "🎵 137486973114353", Code = "137486973114353"},
        {Name = "🎵 111351357978027", Code = "111351357978027"}
    }

    for _, song in ipairs(VerifiedLibrary) do
        local ItemFrame = Instance.new("Frame") local IFC = Instance.new("UICorner")
        ItemFrame.Size = UDim2.new(1, -6, 0, 34) ItemFrame.BackgroundColor3 = Color3.fromRGB(22, 22, 30) ItemFrame.Parent = MusicScroll
        IFC.CornerRadius = UDim.new(0, 5) IFC.Parent = ItemFrame
        local SongName = Instance.new("TextLabel") SongName.Size = UDim2.new(0, 220, 1, 0) SongName.Position = UDim2.new(0, 8, 0, 0) SongName.BackgroundTransparency = 1 SongName.Font = Enum.Font.GothamSemibold SongName.Text = song.Name SongName.TextColor3 = Color3.fromRGB(240, 240, 250) SongName.TextSize = 10 SongName.TextXAlignment = Enum.TextXAlignment.Left SongName.Parent = ItemFrame
        local CopyBtn = Instance.new("TextButton") local CBC = Instance.new("UICorner")
        CopyBtn.Size = UDim2.new(0, 65, 0, 24) CopyBtn.Position = UDim2.new(1, -75, 0.5, -12) CopyBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40) CopyBtn.Font = Enum.Font.GothamBold CopyBtn.Text = "نسخ 📋" CopyBtn.TextColor3 = Color3.fromRGB(255, 255, 255) CopyBtn.TextSize = 10 CopyBtn.Parent = ItemFrame CBC.CornerRadius = UDim.new(0, 4) CBC.Parent = CopyBtn
        CopyBtn.MouseButton1Click:Connect(function()
            setclipboard(song.Code) CopyBtn.Text = "تم! ✔" CopyBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
            CreateNotification("تم نسخ كود الميوزك: " .. song.Code) task.wait(1) CopyBtn.Text = "نسخ 📋" CopyBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
        end)
    end
    MusicScroll.CanvasSize = UDim2.new(0, 0, 0, MusicListLayout.AbsoluteContentSize.Y + 5)

    ---------------------------------------------------------
    -- [5. صفحة اسكنات الاولاد - استخدام بريفيكس / للأمر العادي]
    ---------------------------------------------------------
    local BoysScroll = Instance.new("ScrollingFrame") local BoysListLayout = Instance.new("UIListLayout")
    BoysScroll.Size = UDim2.new(1, 0, 1, 0) BoysScroll.BackgroundTransparency = 1 BoysScroll.ScrollBarThickness = 3 BoysScroll.ScrollBarImageColor3 = RandomThemeColor BoysScroll.Parent = BoysSkinsPage
    BoysListLayout.SortOrder = Enum.SortOrder.LayoutOrder BoysListLayout.Padding = UDim.new(0, 5) BoysListLayout.Parent = BoysScroll

    local TargetsBoysSkins = {
        "111ZeZoo111", "mes100244", "thunder5p", "LH_7n", "tarknzal", 
        "tllwp", "dnsnff", "4liill77", "nvm", "36", "ohorphic", 
        "A1CKER", "Fikzyyx", "mohammeedd78", "Dima09", "j773y", 
        "ksa_dodo", "abdelazizkopr", "fh_556", "v662v", "froztti", "28zzz28"
    }

    for _, boyName in ipairs(TargetsBoysSkins) do
        local SkinFrame = Instance.new("Frame") local SFC = Instance.new("UICorner")
        SkinFrame.Size = UDim2.new(1, -6, 0, 34) SkinFrame.BackgroundColor3 = Color3.fromRGB(24, 20, 35) SkinFrame.Parent = BoysScroll
        SFC.CornerRadius = UDim.new(0, 5) SFC.Parent = SkinFrame
        
        local BoyLabel = Instance.new("TextLabel") BoyLabel.Size = UDim2.new(0, 220, 1, 0) BoyLabel.Position = UDim2.new(0, 8, 0, 0) BoyLabel.BackgroundTransparency = 1 BoyLabel.Font = Enum.Font.GothamBold BoyLabel.Text = "🕺 " .. boyName BoyLabel.TextColor3 = Color3.fromRGB(255, 255, 255) BoyLabel.TextSize = 10 BoyLabel.TextXAlignment = Enum.TextXAlignment.Left BoyLabel.Parent = SkinFrame
        
        local UseSkinBtn = Instance.new("TextButton") local USBC = Instance.new("UICorner")
        UseSkinBtn.Size = UDim2.new(0, 75, 0, 24) UseSkinBtn.Position = UDim2.new(1, -85, 0.5, -12) UseSkinBtn.BackgroundColor3 = Color3.fromRGB(65, 25, 110) UseSkinBtn.Font = Enum.Font.GothamBold UseSkinBtn.Text = "تحويل ⚡" UseSkinBtn.TextColor3 = Color3.fromRGB(240, 220, 255) UseSkinBtn.TextSize = 9 UseSkinBtn.Parent = SkinFrame USBC.CornerRadius = UDim.new(0, 4) USBC.Parent = UseSkinBtn
        
        UseSkinBtn.MouseButton1Click:Connect(function()
            -- تم تغيير البريفيكس هنا أيضاً إلى /
            local charCommand = "/char me " .. boyName
            FireOldRemote(charCommand)
            
            UseSkinBtn.Text = "تم التنفيذ!" UseSkinBtn.BackgroundColor3 = Color3.fromRGB(0, 160, 80) UseSkinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            CreateNotification("تم تغيير السكن: " .. charCommand) task.wait(1) UseSkinBtn.Text = "تحويل ⚡" UseSkinBtn.BackgroundColor3 = Color3.fromRGB(65, 25, 110) UseSkinBtn.TextColor3 = Color3.fromRGB(240, 220, 255)
        end)
    end
    BoysScroll.CanvasSize = UDim2.new(0, 0, 0, BoysListLayout.AbsoluteContentSize.Y + 5)

    ---------------------------------------------------------
    -- [6. صفحة اخرى - مانع التقطيع]
    ---------------------------------------------------------
    local OtherContainer = Instance.new("Frame") OtherContainer.Size = UDim2.new(1, 0, 1, 0) OtherContainer.BackgroundTransparency = 1 OtherContainer.Parent = OtherPage

    local FPSBtn = Instance.new("TextButton") local FPSCorner = Instance.new("UICorner") local FPSStroke = Instance.new("UIStroke")
    FPSBtn.Size = UDim2.new(0, 320, 0, 45) FPSBtn.Position = UDim2.new(0.5, -160, 0, 20) FPSBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45) FPSBtn.Font = Enum.Font.GothamBold FPSBtn.Text = "🚀 مانع التقطيع الأقوى [معطل]" FPSBtn.TextColor3 = Color3.fromRGB(235, 220, 255) FPSBtn.TextSize = 12 FPSBtn.Parent = OtherContainer
    FPSCorner.CornerRadius = UDim.new(0, 8) FPSCorner.Parent = FPSBtn
    FPSStroke.Color = Color3.fromRGB(100, 100, 100) FPSStroke.Thickness = 1.2 FPSStroke.Parent = FPSBtn

    local OtherInfoLabel = Instance.new("TextLabel")
    OtherInfoLabel.Size = UDim2.new(1, 0, 0, 60) OtherInfoLabel.Position = UDim2.new(0, 0, 0, 75) OtherInfoLabel.BackgroundTransparency = 1 OtherInfoLabel.Font = Enum.Font.GothamSemibold
    OtherInfoLabel.Text = "عند تفعيل خيار مانع التقطيع الأقوى، سيتم حذف المؤثرات البصرية الزائدة والجسيمات وتعديل خامات الماب بالكامل لتسريع الفريمات وتقليل الفريز فوراً وبأعلى حماية وسرعة ممكنة لتجنب الكراش." OtherInfoLabel.TextColor3 = Color3.fromRGB(150, 150, 160) OtherInfoLabel.TextSize = 10 OtherInfoLabel.TextWrapped = true OtherInfoLabel.Parent = OtherContainer

    local FPSActive, FastClearConnection = false, nil
    FPSBtn.MouseButton1Click:Connect(function()
        FPSActive = not FPSActive
        if FPSActive then
            FPSBtn.Text = "🚀 مانع التقطيع الأقوى [مفعل]" FPSBtn.BackgroundColor3 = Color3.fromRGB(0, 140, 60) FPSStroke.Color = Color3.fromRGB(0, 255, 120)
            CreateNotification("تم تفعيل مانع التقطيع وحذف المؤثرات!")
            pcall(function()
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("ParticleEmitter") or v:IsA("Explosion") or v:IsA("Sparkles") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Decal") or v:IsA("Texture") then v:Destroy()
                    elseif (v:IsA("MeshPart") or v:IsA("Part")) and not v:IsDescendantOf(workspace.CurrentCamera) then v.Material = Enum.Material.SmoothPlastic v.Reflectance = 0 end
                end
            end)
            FastClearConnection = workspace.DescendantAdded:Connect(function(d)
                if FPSActive and (d:IsA("ParticleEmitter") or d:IsA("Explosion") or d:IsA("Sparkles") or d:IsA("Fire")) then task.defer(function() pcall(function() d:Destroy() end) end) end
            end)
        else
            FPSBtn.Text = "🚀 مانع التقطيع الأقوى [معطل]" FPSBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45) FPSStroke.Color = Color3.fromRGB(100, 100, 100)
            if FastClearConnection then FastClearConnection:Disconnect() FastClearConnection = nil end CreateNotification("تم إلغاء تفعيل مانع التقطيع.")
        end
    end)

    Container.Visible = true
    _EX.T:Create(Container, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = FullSize, Position = FullPos}):Play()

    local TB = Instance.new("TextButton") local TC = Instance.new("UICorner") local TS = Instance.new("UIStroke")
    TB.Name = "OpenStationButton" TB.Size = UDim2.new(0, 42, 0, 42) TB.Position = UDim2.new(1, -55, 1, -55)
    TB.BackgroundColor3 = Color3.fromRGB(10, 10, 15) TB.Font = Enum.Font.GothamBold TB.Text = "🔮" TB.TextColor3 = RandomThemeColor TB.TextSize = 16 TB.ZIndex = 10 TB.Parent = MainGui
    TC.CornerRadius = UDim.new(1, 0) TC.Parent = TB TS.Color = RandomThemeColor TS.Thickness = 1.5 TS.Parent = TB
    MakeDraggable(TB)

    local HubVisible = true
    TB.Activated:Connect(function()
        HubVisible = not HubVisible
        if HubVisible then
            Container.Visible = true _EX.T:Create(Container, TweenInfo.new(0.4, Enum.EasingStyle.Back), {Size = FullSize, Position = FullPos}):Play()
        else
            local close = _EX.T:Create(Container, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0,0,0,0), Position = UDim2.new(Container.Position.X.Offset + (Container.Size.X.Offset/2), 0, Container.Position.Y.Offset + (Container.Size.Y.Offset/2), 0)})
            close:Play() close.Completed:Connect(function() Container.Visible = false end)
        end
    end)
end)
