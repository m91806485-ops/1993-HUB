--[[
    1993 HUB v21.0 [PREMIUM MODDED]
    [+] Replaced Rainbow Text with: التحكم في الاسم الملون المطور
    [+] Added New Tab: نسخ سريع V2 💀 (Full Advanced Copy System)
    [+] Added New Tab: اخرى 📀 (مانع التقطيع الأقوى - FPS Booster)
    [👑] Created by: mohammeedd78
--]]

local _OPI_DATA = "WkdGellYUnZjRndnS21sdmRYTmxjblpwWTJWbGNnbHlaV0ZrYVdkbGJsd3BZM0psWVhScFpYTWdLR1Z1ZDJsdmJuTXZJU0U3Q205d2FYUnVJR0Z3Y21Gd2NtdGxiV0Z1ZEdWeWN3b2dJR0Z3Y21Gd2NtdGxiV0Z1ZEdWeWN3b2dJSDA3Q21sdmRYTmxjblpwWTJWbGNnbHlaV0ZrYVdkbGJsd3BZM0psWVhScFpYTWdLR1Z1ZDJsdmJuTXZJU0U3Q205d2FYUnVJR0Z3Y21Gd2NtdGxiV0Z1ZEdWeWN3b2dJR0Z3Y21Gd2NtdGxiV0Z1ZEdWeWN3b2dJSDA3"
local _OPI_KEY = 0xFF44CC

local function _OPI_DEC(s)
    local b = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    s = string.gsub(s, '[^'..b..'=]', '')
    return (s:gsub('.', function(x)
        if (x == '=') then return '' end
        local r, f = '', (b:find(x) - 1)
        for i = 6, 1, -1 do r = r .. (f % 2^i - f % 2^(i-1) > 0 and '1' or '0') end
        return r;
    end):gsub('%d%d%d%d%d%d%d%d', function(x)
        local r = 0
        for i = 1, 8 do r = r + (x:sub(i, i) == '1' and 2^(8-i) or 0) end
        return string.char(r);
    end))
end

local function _OPI_XOR(d, k)
    local o = {}
    for i = 1, #d do
        local b = string.byte(d, i)
        o[i] = string.char(bit32 and bit32.bxor(b, k % 256) or b)
    end
    return table.concat(o)
end

local _ENV_BOX = getfenv and getfenv() or _ENV
local _S, _R = pcall(function()
    local decrypted = _OPI_XOR(_OPI_DEC(_OPI_DATA), _OPI_KEY)
    
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
    MainGui.Name = "1993_Hub_V21"
    MainGui.ResetOnSpawn = false
    MainGui.Parent = _EX.G

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

    -- تغيير حجم الحاوية الجانبية لتستوعب الصفحات الإضافية الجديدة مسببة السكرول والترتيب المريح
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
        TabBtn.Size = UDim2.new(1, -10, 0, 32) TabBtn.BackgroundColor3 = Color3.fromRGB(24, 24, 32) TabBtn.Font = Enum.Font.GothamSemibold TabBtn.Text = icon .. " " .. name TabBtn.TextColor3 = Color3.fromRGB(160, 160, 160) TabBtn.TextSize = 10 TabBtn.Parent = TabScrollContainer TBC.CornerRadius = UDim.new(0, 5) TBC.Parent = TabBtn
        
        TabBtn.MouseButton1Click:Connect(function()
            for _, p in pairs(Pages) do p.Visible = false end
            for _, b in ipairs(TabScrollContainer:GetChildren()) do if b:IsA("TextButton") then b.BackgroundColor3 = Color3.fromRGB(24, 24, 32) b.TextColor3 = Color3.fromRGB(160, 160, 160) end end
            Page.Visible = true TabBtn.BackgroundColor3 = RandomThemeColor TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        end)
        return Page
    end

    -- ترتيب الصفحات في الـ SideBar حسب طلبك الدقيق
    local PullPlayerPage = CreateTab("سحب شخص", "🎯")
    local MusicPage = CreateTab("الأغاني", "🎵")
    local SkinsPage = CreateTab("سكنات أولاد", "👕")
    local QuickCopyV2Page = CreateTab("نسخ سريع V2 💀", "🔥")
    local OtherPage = CreateTab("اخرى 📀", "🚀")

    Pages["سحب شخص"].Visible = true
    TabScrollContainer:FindFirstChildOfClass("TextButton").BackgroundColor3 = RandomThemeColor
    TabScrollContainer:FindFirstChildOfClass("TextButton").TextColor3 = Color3.fromRGB(255, 255, 255)
    TabScrollContainer.CanvasSize = UDim2.new(0, 0, 0, TabScrollLayout.AbsoluteContentSize.Y + 10)

    ---------------------------------------------------------
    -- [1. محتويات صفحة سحب اللاعبين الأساسية]
    ---------------------------------------------------------
    local LeftControls = Instance.new("Frame")
    LeftControls.Size = UDim2.new(0, 200, 1, 0) LeftControls.BackgroundTransparency = 1 LeftControls.Parent = PullPlayerPage

    local NameInput = Instance.new("TextBox") local IC1 = Instance.new("UICorner")
    NameInput.Size = UDim2.new(1, 0, 0, 32) NameInput.Position = UDim2.new(0, 0, 0, 5)
    NameInput.BackgroundColor3 = Color3.fromRGB(22, 22, 30) NameInput.Font = Enum.Font.Gotham NameInput.PlaceholderText = "اسم اللاعب المستهدف..." NameInput.Text = "" NameInput.TextColor3 = Color3.fromRGB(255, 255, 255) NameInput.TextSize = 11 NameInput.Parent = LeftControls IC1.CornerRadius = UDim.new(0, 6) IC1.Parent = NameInput

    local AudioInput = Instance.new("TextBox") local AIC = Instance.new("UICorner")
    AudioInput.Size = UDim2.new(1, 0, 0, 32) AudioInput.Position = UDim2.new(0, 0, 0, 42)
    AudioInput.BackgroundColor3 = Color3.fromRGB(22, 22, 30) AudioInput.Font = Enum.Font.Gotham AudioInput.PlaceholderText = "كود الأغنية المنسوخ حالياً..." AudioInput.Text = "" AudioInput.TextColor3 = Color3.fromRGB(255, 200, 0) AudioInput.TextSize = 11 AudioInput.Parent = LeftControls AIC.CornerRadius = UDim.new(0, 6) AIC.Parent = AudioInput

    local ActionButton = Instance.new("TextButton") local BC1 = Instance.new("UICorner")
    ActionButton.Size = UDim2.new(1, 0, 0, 35) ActionButton.Position = UDim2.new(0, 0, 0, 82)
    ActionButton.BackgroundColor3 = RandomThemeColor ActionButton.Font = Enum.Font.GothamBold ActionButton.Text = "تفعيل حلقة الأوامر الإدارية" ActionButton.TextColor3 = Color3.fromRGB(255, 255, 255) ActionButton.TextSize = 11 ActionButton.Parent = LeftControls BC1.CornerRadius = UDim.new(0, 6) BC1.Parent = ActionButton

    ---------------------------------------------------------
    -- [استبدال نص الرينبو بـ التحكم في الاسم الملون المطور]
    ---------------------------------------------------------
    local ColoredNamePanel = Instance.new("Frame") local CNCorner = Instance.new("UICorner") local CNStroke = Instance.new("UIStroke")
    ColoredNamePanel.Size = UDim2.new(1, 0, 0, 80) ColoredNamePanel.Position = UDim2.new(0, 0, 0, 124) ColoredNamePanel.BackgroundColor3 = Color3.fromRGB(20, 15, 30) ColoredNamePanel.Parent = LeftControls
    CNCorner.CornerRadius = UDim.new(0, 6) CNCorner.Parent = ColoredNamePanel
    CNStroke.Color = Color3.fromRGB(150, 0, 255) CNStroke.Thickness = 1 CNStroke.Parent = ColoredNamePanel

    local ColoredNameInput = Instance.new("TextBox") local CNICorner = Instance.new("UICorner")
    ColoredNameInput.Size = UDim2.new(1, -12, 0, 28) ColoredNameInput.Position = UDim2.new(0, 6, 0, 8) CustomSkinInput = ColoredNameInput
    ColoredNameInput.BackgroundColor3 = Color3.fromRGB(30, 20, 45) ColoredNameInput.Font = Enum.Font.GothamBold ColoredNameInput.Text = "ADF ON TOP" ColoredNameInput.TextColor3 = Color3.fromRGB(255, 255, 255) ColoredNameInput.TextSize = 11 ColoredNameInput.Parent = ColoredNamePanel
    CNICorner.CornerRadius = UDim.new(0, 4) CNICorner.Parent = ColoredNameInput

    local ColoredNameToggleBtn = Instance.new("TextButton") local CNBCorner = Instance.new("UICorner")
    ColoredNameToggleBtn.Size = UDim2.new(1, -12, 0, 30) ColoredNameToggleBtn.Position = UDim2.new(0, 6, 0, 42) ColoredNameToggleBtn.BackgroundColor3 = Color3.fromRGB(40, 20, 80) ColoredNameToggleBtn.Font = Enum.Font.GothamBold ColoredNameToggleBtn.Text = "🌈 تفعيل الاسم الملون" ColoredNameToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255) ColoredNameToggleBtn.TextSize = 11 ColoredNameToggleBtn.Parent = ColoredNamePanel
    CNBCorner.CornerRadius = UDim.new(0, 4) CNBCorner.Parent = ColoredNameToggleBtn

    local RGBRun = false
    ColoredNameToggleBtn.MouseButton1Click:Connect(function()
        if RGBRun then
            RGBRun = false
            ColoredNameToggleBtn.Text = "🌈 تفعيل الاسم الملون" ColoredNameToggleBtn.BackgroundColor3 = Color3.fromRGB(40, 20, 80)
            CreateNotification("تم إيقاف الاسم الملون")
        else
            RGBRun = true
            ColoredNameToggleBtn.Text = "🛑 إلغاء تفعيل الاسم" ColoredNameToggleBtn.BackgroundColor3 = Color3.fromRGB(150, 20, 20)
            CreateNotification("تم تنشيط الاسم الملون المتغير تلقائياً!")
            task.spawn(function()
                local hu = 0
                while RGBRun do
                    local r = _EX.R:FindFirstChild("ApplyTitle", true) or _EX.R:FindFirstChild("ChangeTitle", true)
                    if r then
                        pcall(function() r:FireServer(ColoredNameInput.Text, Color3.fromHSV(hu, 1, 1)) end)
                    end
                    hu = hu + 0.03
                    if hu >= 1 then hu = 0 end
                    task.wait(0.4)
                end
            end)
        end
    end)

    local RightPlayersPanel = Instance.new("Frame")
    RightPlayersPanel.Size = UDim2.new(1, -210, 1, 0) RightPlayersPanel.Position = UDim2.new(0, 210, 0, 0) RightPlayersPanel.BackgroundColor3 = Color3.fromRGB(18, 18, 26) RightPlayersPanel.Parent = PullPlayerPage
    local RPC = Instance.new("UICorner") RPC.CornerRadius = UDim.new(0, 8) RPC.Parent = RightPlayersPanel
    local RPS = Instance.new("UIStroke") RPS.Color = Color3.fromRGB(40, 40, 50) RPS.Thickness = 1 RPS.Parent = RightPlayersPanel

    local PTitle = Instance.new("TextLabel") PTitle.Size = UDim2.new(1, 0, 0, 25) PTitle.BackgroundTransparency = 1 PTitle.Font = Enum.Font.GothamBold PTitle.Text = "👥 نظام سحب وتلبيوت السيرفر" PTitle.TextColor3 = Color3.fromRGB(255, 255, 255) PTitle.TextSize = 11 PTitle.Parent = RightPlayersPanel

    local PullButton = Instance.new("TextButton") local PBC = Instance.new("UICorner")
    PullButton.Size = UDim2.new(1, -12, 0, 34) PullButton.Position = UDim2.new(0, 6, 1, -40) PullButton.BackgroundColor3 = RandomThemeColor PullButton.Font = Enum.Font.GothamBold PullButton.Text = "اختر لاعباً للسحب (;tp)" PullButton.TextColor3 = Color3.fromRGB(255, 255, 255) PullButton.TextSize = 11 PullButton.Parent = RightPlayersPanel PBC.CornerRadius = UDim.new(0, 5) PBC.Parent = PullButton

    local SelectedPullPlayer = ""
    local PlayersScroll = Instance.new("ScrollingFrame") local PlayersListLayout = Instance.new("UIListLayout")
    PlayersScroll.Size = UDim2.new(1, -8, 1, -75) PlayersScroll.Position = UDim2.new(0, 4, 0, 28) PlayersScroll.BackgroundTransparency = 1 PlayersScroll.ScrollBarThickness = 3 PlayersScroll.ScrollBarImageColor3 = RandomThemeColor PlayersScroll.Parent = RightPlayersPanel
    PlayersListLayout.SortOrder = Enum.SortOrder.LayoutOrder PlayersListLayout.Padding = UDim.new(0, 4) PlayersListLayout.Parent = PlayersScroll

    local function UpdatePlayersList()
        for _, child in ipairs(PlayersScroll:GetChildren()) do if child:IsA("Frame") then child:Destroy() end end
        for _, player in ipairs(_EX.P:GetPlayers()) do
            if player ~= _EX.L then
                local PFrame = Instance.new("Frame") local PFC = Instance.new("UICorner")
                PFrame.Size = UDim2.new(1, -6, 0, 30) PFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 40) PFrame.Parent = PlayersScroll
                PFC.CornerRadius = UDim.new(0, 5) PFC.Parent = PFrame
                local PName = Instance.new("TextButton") PName.Size = UDim2.new(1, -8, 1, 0) PName.Position = UDim2.new(0, 6, 0, 0) PName.BackgroundTransparency = 1 PName.Font = Enum.Font.GothamSemibold PName.Text = "🎯 " .. player.Name PName.TextColor3 = Color3.fromRGB(220, 220, 230) PName.TextSize = 10 PName.TextXAlignment = Enum.TextXAlignment.Left PName.Parent = PFrame
                PName.MouseButton1Click:Connect(function()
                    SelectedPullPlayer = player.Name NameInput.Text = player.Name
                    PullButton.Text = "اضغط لسحب: ;tp " .. player.Name
                    CreateNotification("تم اختيار المستهدف: " .. player.Name)
                end)
            end
        end
        PlayersScroll.CanvasSize = UDim2.new(0, 0, 0, PlayersListLayout.AbsoluteContentSize.Y + 5)
    end
    _EX.P.PlayerAdded:Connect(UpdatePlayersList) _EX.P.PlayerRemoving:Connect(UpdatePlayersList) UpdatePlayersList()

    PullButton.MouseButton1Click:Connect(function()
        if SelectedPullPlayer ~= "" then
            local fullCmd = ";tp " .. SelectedPullPlayer
            pcall(function() _EX.R.HDAdminHDClient.Signals.RequestCommandModification:InvokeServer(unpack({fullCmd})) end)
            pcall(function() _EX.R.RemoteEvents.ChatEvent:FireServer(unpack({fullCmd})) end)
            CreateNotification("تم تنفيذ أمر السحب لـ " .. SelectedPullPlayer)
        else
            CreateNotification("اختر لاعباً أولاً!")
        end
    end)

    ---------------------------------------------------------
    -- [2. مكتبة الأغاني]
    ---------------------------------------------------------
    local MusicScroll = Instance.new("ScrollingFrame") local MusicListLayout = Instance.new("UIListLayout")
    MusicScroll.Size = UDim2.new(1, 0, 1, 0) MusicScroll.BackgroundTransparency = 1 MusicScroll.ScrollBarThickness = 3 MusicScroll.ScrollBarImageColor3 = RandomThemeColor MusicScroll.Parent = MusicPage
    MusicListLayout.SortOrder = Enum.SortOrder.LayoutOrder MusicListLayout.Padding = UDim.new(0, 5) MusicListLayout.Parent = MusicScroll

    local VerifiedLibrary = {
        {Name = "🔥 تراك حماس تفجير باصات", Code = "116174401794512"},
        {Name = "🎵 ريمكس غربي تريند تيك توك", Code = "115951236010098"},
        {Name = "🎧 فونك سيارات وتفحيط حاد", Code = "111811908070601"},
        {Name = "⚡ ميجا ميكس هجولة أسطوري", Code = "140415473717614"},
        {Name = "🔮 نغمة غامضة سيلو لوفي", Code = "120871403922972"},
        {Name = "🎧 تراك أجنبي هيب هوب روعة", Code = "127840997774724"}
    }

    for _, song in ipairs(VerifiedLibrary) do
        local ItemFrame = Instance.new("Frame") local IFC = Instance.new("UICorner")
        ItemFrame.Size = UDim2.new(1, -6, 0, 34) ItemFrame.BackgroundColor3 = Color3.fromRGB(22, 22, 30) ItemFrame.Parent = MusicScroll
        IFC.CornerRadius = UDim.new(0, 5) IFC.Parent = ItemFrame
        local SongName = Instance.new("TextLabel") SongName.Size = UDim2.new(0, 220, 1, 0) SongName.Position = UDim2.new(0, 8, 0, 0) SongName.BackgroundTransparency = 1 SongName.Font = Enum.Font.GothamSemibold SongName.Text = song.Name SongName.TextColor3 = Color3.fromRGB(240, 240, 250) SongName.TextSize = 10 SongName.TextXAlignment = Enum.TextXAlignment.Left SongName.Parent = ItemFrame
        local CopyBtn = Instance.new("TextButton") local CBC = Instance.new("UICorner")
        CopyBtn.Size = UDim2.new(0, 65, 0, 24) CopyBtn.Position = UDim2.new(1, -75, 0.5, -12) CopyBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40) CopyBtn.Font = Enum.Font.GothamBold CopyBtn.Text = "نسخ 📋" CopyBtn.TextColor3 = Color3.fromRGB(255, 255, 255) CopyBtn.TextSize = 10 CopyBtn.Parent = ItemFrame CBC.CornerRadius = UDim.new(0, 4) CBC.Parent = CopyBtn
        CopyBtn.MouseButton1Click:Connect(function()
            setclipboard(song.Code) AudioInput.Text = song.Code CopyBtn.Text = "تم! ✔" CopyBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
            CreateNotification("تم نسخ كود الميوزك: " .. song.Code) task.wait(1) CopyBtn.Text = "نسخ 📋" CopyBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
        end)
    end
    MusicScroll.CanvasSize = UDim2.new(0, 0, 0, MusicListLayout.AbsoluteContentSize.Y + 5)

    ---------------------------------------------------------
    -- [3. محتويات صفحة السكنات للأولاد]
    ---------------------------------------------------------
    local SkinsContainer = Instance.new("Frame") SkinsContainer.Size = UDim2.new(1, 0, 1, 0) SkinsContainer.BackgroundTransparency = 1 SkinsContainer.Parent = SkinsPage
    local BoysPanel = Instance.new("Frame") local BPCorner = Instance.new("UICorner")
    BoysPanel.Size = UDim2.new(0.48, -4, 1, 0) BoysPanel.BackgroundColor3 = Color3.fromRGB(16, 20, 30) BoysPanel.Parent = SkinsContainer
    BPCorner.CornerRadius = UDim.new(0, 8) BPCorner.Parent = BoysPanel
    local BTitle = Instance.new("TextLabel") BTitle.Size = UDim2.new(1, 0, 0, 25) BTitle.BackgroundTransparency = 1 BTitle.Font = Enum.Font.GothamBold BTitle.Text = "👦 سكنات أولاد 1" BTitle.TextColor3 = Color3.fromRGB(0, 200, 255) BTitle.TextSize = 11 BTitle.Parent = BoysPanel

    local BoysScroll = Instance.new("ScrollingFrame") local BoysLayout = Instance.new("UIListLayout")
    BoysScroll.Size = UDim2.new(1, -10, 1, -30) BoysScroll.Position = UDim2.new(0, 5, 0, 25) BoysScroll.BackgroundTransparency = 1 BoysScroll.ScrollBarThickness = 2 BoysScroll.ScrollBarImageColor3 = Color3.fromRGB(0, 200, 255) BoysScroll.Parent = BoysPanel
    BoysLayout.SortOrder = Enum.SortOrder.LayoutOrder BoysLayout.Padding = UDim.new(0, 4) BoysLayout.Parent = BoysScroll

    local Boys2Panel = Instance.new("Frame") local GPCorner = Instance.new("UICorner")
    Boys2Panel.Size = UDim2.new(0.48, -4, 1, -75) Boys2Panel.Position = UDim2.new(0.52, 2, 0, 75) Boys2Panel.BackgroundColor3 = Color3.fromRGB(16, 24, 28) Boys2Panel.Parent = SkinsContainer
    GPCorner.CornerRadius = UDim.new(0, 8) GPCorner.Parent = Boys2Panel
    local GTitle = Instance.new("TextLabel") GTitle.Size = UDim2.new(1, 0, 0, 25) GTitle.BackgroundTransparency = 1 GTitle.Font = Enum.Font.GothamBold GTitle.Text = "⚡ سكنات أولاد 2" GTitle.TextColor3 = Color3.fromRGB(0, 255, 200) GTitle.TextSize = 11 GTitle.Parent = Boys2Panel

    local Boys2Scroll = Instance.new("ScrollingFrame") local Boys2Layout = Instance.new("UIListLayout")
    Boys2Scroll.Size = UDim2.new(1, -10, 1, -30) Boys2Scroll.Position = UDim2.new(0, 5, 0, 25) Boys2Scroll.BackgroundTransparency = 1 Boys2Scroll.ScrollBarThickness = 2 Boys2Scroll.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 200) Boys2Scroll.Parent = Boys2Panel
    Boys2Layout.SortOrder = Enum.SortOrder.LayoutOrder Boys2Layout.Padding = UDim.new(0, 4) Boys2Layout.Parent = Boys2Scroll

    local CustomSkinPanel = Instance.new("Frame") local CSPCorner = Instance.new("UICorner") local CSPStroke = Instance.new("UIStroke")
    CustomSkinPanel.Size = UDim2.new(0.48, -4, 0, 70) CustomSkinPanel.Position = UDim2.new(0.52, 2, 0, 0) CustomSkinPanel.BackgroundColor3 = Color3.fromRGB(22, 22, 32) CustomSkinPanel.Parent = SkinsContainer
    CSPCorner.CornerRadius = UDim.new(0, 6) CSPCorner.Parent = CustomSkinPanel
    CSPStroke.Color = RandomThemeColor CSPStroke.Thickness = 1 CSPStroke.Parent = CustomSkinPanel

    local CustomSkinInput = Instance.new("TextBox") local CSICorner = Instance.new("UICorner")
    CustomSkinInput.Size = UDim2.new(1, -12, 0, 26) CustomSkinInput.Position = UDim2.new(0, 6, 0, 6) CustomSkinInput.BackgroundColor3 = Color3.fromRGB(14, 14, 20) CustomSkinInput.Font = Enum.Font.GothamSemibold CustomSkinInput.PlaceholderText = "اكتب كود الاسكن/يوزر..." CustomSkinInput.Text = "" CustomSkinInput.TextColor3 = Color3.fromRGB(255, 255, 255) CustomSkinInput.TextSize = 10 CustomSkinInput.Parent = CustomSkinPanel
    CSICorner.CornerRadius = UDim.new(0, 4) CSICorner.Parent = CustomSkinInput

    local CustomSkinBtn = Instance.new("TextButton") local CSBCorner = Instance.new("UICorner")
    CustomSkinBtn.Size = UDim2.new(1, -12, 0, 26) CustomSkinBtn.Position = UDim2.new(0, 6, 0, 36) CustomSkinBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 255) CustomSkinBtn.Font = Enum.Font.GothamBold CustomSkinBtn.Text = "تنفيذ السكن ⚡" CustomSkinBtn.TextColor3 = Color3.fromRGB(255, 255, 255) CustomSkinBtn.TextSize = 10 CustomSkinBtn.Parent = CustomSkinPanel
    CSBCorner.CornerRadius = UDim.new(0, 4) CSBCorner.Parent = CustomSkinBtn

    CustomSkinBtn.MouseButton1Click:Connect(function()
        local targetUser = CustomSkinInput.Text
        if targetUser ~= "" then
            local fullCmd = ";char me " .. targetUser
            pcall(function() _EX.R.HDAdminHDClient.Signals.RequestCommandModification:InvokeServer(unpack({fullCmd})) end)
            pcall(function() _EX.R.RemoteEvents.ChatEvent:FireServer(unpack({fullCmd})) end)
            CreateNotification("تم تطبيق السكن المخصص لـ: " .. targetUser)
        end
    end)

    local BoySkinsData = {
        {Label = "سكن Fikzyyx", User = "Fikzyyx"}, {Label = "سكن A1CKER", User = "A1CKER"},
        {Label = "سكن ohorphic", User = "ohorphic"}, {Label = "سكن uiu", User = "uiu"},
        {Label = "سكن 36", User = "36"}, {Label = "سكن nvm", User = "nvm"},
        {Label = "سكن محمد الدون", User = "mohammeedd78"}
    }
    local BoySkinsNewData = {
        {Label = "سكن 4liill77", User = "4liill77"}, {Label = "سكن dnsnff", User = "dnsnff"},
        {Label = "سكن tllwp", User = "tllwp"}, {Label = "سكن tarknzal", User = "tarknzal"},
        {Label = "سكن LH_7n", User = "LH_7n"}, {Label = "سكن thunder5p", User = "thunder5p"},
        {Label = "سكن mes100244", User = "mes100244"}, {Label = "سكن 111ZeZoo111", User = "111ZeZoo111"}
    }

    local function PopulateSkins(scroll, layout, data, btnColor)
        for _, skin in ipairs(data) do
            local SFrame = Instance.new("Frame") local SFC = Instance.new("UICorner")
            SFrame.Size = UDim2.new(1, -4, 0, 32) SFrame.BackgroundColor3 = Color3.fromRGB(32, 32, 42) SFrame.Parent = scroll
            SFC.CornerRadius = UDim.new(0, 4) SFC.Parent = SFrame
            local SLabel = Instance.new("TextLabel") SLabel.Size = UDim2.new(0, 110, 1, 0) SLabel.Position = UDim2.new(0, 6, 0, 0) SLabel.BackgroundTransparency = 1 SLabel.Font = Enum.Font.GothamSemibold SLabel.Text = skin.Label SLabel.TextColor3 = Color3.fromRGB(235, 235, 245) SLabel.TextSize = 9 SLabel.TextXAlignment = Enum.TextXAlignment.Left SLabel.Parent = SFrame
            local SBtn = Instance.new("TextButton") local SBC = Instance.new("UICorner")
            SBtn.Size = UDim2.new(0, 55, 0, 22) SBtn.Position = UDim2.new(1, -60, 0.5, -11) SBtn.BackgroundColor3 = btnColor SBtn.Font = Enum.Font.GothamBold SBtn.Text = "تفعيل 👕" SBtn.TextColor3 = Color3.fromRGB(255, 255, 255) SBtn.TextSize = 8 SBtn.Parent = SFrame SBC.CornerRadius = UDim.new(0, 4) SBC.Parent = SBtn
            SBtn.MouseButton1Click:Connect(function()
                local fullCmd = ";char me " .. skin.User
                pcall(function() _EX.R.HDAdminHDClient.Signals.RequestCommandModification:InvokeServer(unpack({fullCmd})) end)
                pcall(function() _EX.R.RemoteEvents.ChatEvent:FireServer(unpack({fullCmd})) end)
                CreateNotification("تم تفعيل سكن: " .. skin.User)
            end)
        end
        scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 5)
    end
    PopulateSkins(BoysScroll, BoysLayout, BoySkinsData, Color3.fromRGB(0, 120, 200))
    PopulateSkins(Boys2Scroll, Boys2Layout, BoySkinsNewData, Color3.fromRGB(0, 160, 150))

    ---------------------------------------------------------
    -- [4. بناء صفحة نسخ سريع V2 💀 بالكامل]
    ---------------------------------------------------------
    local SelTable, Run, Mode, UseShortName = {}, false, "Hidden", true
    local CRem, ARem = nil, nil

    local function ScanRemotes()
        CRem = _EX.R:FindFirstChild("ChatEvent", true)
        for _, d in pairs(_EX.R:GetDescendants()) do
            if d:IsA("RemoteFunction") and (d.Name == "RequestCommandModification" or d.Name:match("Modification")) then
                ARem = d break
            end
        end
        if not ARem then ARem = _EX.R:FindFirstChild("RequestCommandModification", true) end
    end
    task.spawn(ScanRemotes)

    local V2LeftFrame = Instance.new("Frame")
    V2LeftFrame.Size = UDim2.new(0, 230, 1, 0) V2LeftFrame.BackgroundTransparency = 1 V2LeftFrame.Parent = QuickCopyV2Page

    -- قائمة التمرير للاعبين داخل الصفحة v2
    local V2PList = Instance.new("ScrollingFrame") local V2ListLayout = Instance.new("UIListLayout")
    V2PList.Size = UDim2.new(1, 0, 0, 100) V2PList.Position = UDim2.new(0, 0, 0, 5) V2PList.BackgroundColor3 = Color3.fromRGB(15, 10, 25) V2PList.BackgroundTransparency = 0.4 V2PList.ScrollBarThickness = 3 V2PList.Parent = V2LeftFrame
    V2ListLayout.SortOrder = Enum.SortOrder.LayoutOrder V2ListLayout.Padding = UDim.new(0, 4) V2ListLayout.Parent = V2PList
    Instance.new("UICorner", V2PList).CornerRadius = UDim.new(0, 8)

    -- صندوق البحث السريع بالاختصار الذكي
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

    -- صندوق الأوامر المخصصة اليدوية
    local V2Inp = Instance.new("TextBox") local V2InpCorner = Instance.new("UICorner")
    V2Inp.Size = UDim2.new(1, 0, 0, 32) V2Inp.Position = UDim2.new(0, 0, 0, 143) V2Inp.BackgroundColor3 = Color3.fromRGB(20, 15, 30) V2Inp.PlaceholderText = "اكتب أمر مخصص يدوي هنا..." V2Inp.Text = "" V2Inp.TextColor3 = Color3.fromRGB(210, 160, 255) V2Inp.Font = Enum.Font.GothamBold V2Inp.TextSize = 11 V2Inp.Parent = V2LeftFrame
    V2InpCorner.CornerRadius = UDim.new(0, 6) V2InpCorner.Parent = V2Inp

    -- شبكة أزرار النسخ السريع الجاهزة
    local QkFrame = Instance.new("Frame")
    QkFrame.Size = UDim2.new(1, 0, 0, 75) QkFrame.Position = UDim2.new(0, 0, 0, 180) QkFrame.BackgroundTransparency = 1 QkFrame.Parent = V2LeftFrame
    local V2Grid = Instance.new("UIGridLayout") V2Grid.CellSize = UDim2.new(0, 112, 0, 32) V2Grid.CellPadding = UDim2.new(0, 6, 0, 6) V2Grid.Parent = QkFrame

    local function CreateQkBtn(text, cmds)
        local btn = Instance.new("TextButton") local btnC = Instance.new("UICorner")
        btn.Text = text btn.BackgroundColor3 = Color3.fromRGB(35, 15, 55) btn.TextColor3 = Color3.fromRGB(235, 220, 255) btn.Font = Enum.Font.GothamBold btn.TextSize = 10 btn.Parent = QkFrame btnC.CornerRadius = UDim.new(0, 6) btnC.Parent = btn
        btn.MouseButton1Click:Connect(function() V2Inp.Text = cmds CreateNotification("تم تعيين نمط: " .. text) end)
    end
    CreateQkBtn("نسخ غامض", "/explode /logs /re /res /nv")
    CreateQkBtn("نسخ هيد admin", "/explode /warp /re /res /nv")
    CreateQkBtn("نسخ يعلق", "/logs /nv /re /res")
    CreateQkBtn("نسخ تعذيب", "/dog /char miri /jc /tp /ice")

    -- تبديل الأنماط بين النسخ المخفي والدردشة
    local MdsFrame = Instance.new("Frame") MdsFrame.Size = UDim2.new(1, 0, 0, 32) MdsFrame.Position = UDim2.new(0, 0, 0, 260) MdsFrame.BackgroundTransparency = 1 MdsFrame.Parent = V2LeftFrame
    local HidB = Instance.new("TextButton") local HB_C = Instance.new("UICorner")
    HidB.Size = UDim2.new(0, 112, 1, 0) HidB.BackgroundColor3 = Color3.fromRGB(0, 140, 60) HidB.Font = Enum.Font.GothamBold HidB.Text = "نسخ مخفي [نشط]" HidB.TextColor3 = Color3.fromRGB(255, 255, 255) HidB.TextSize = 9 HidB.Parent = MdsFrame HB_C.CornerRadius = UDim.new(0, 6) HB_C.Parent = HidB

    local ChtB = Instance.new("TextButton") local CB_C = Instance.new("UICorner")
    ChtB.Size = UDim2.new(0, 112, 1, 0) ChtB.Position = UDim2.new(0, 118, 0, 0) ChtB.BackgroundColor3 = Color3.fromRGB(0, 90, 40) ChtB.Font = Enum.Font.GothamBold ChtB.Text = "نسخ شات" ChtB.TextColor3 = Color3.fromRGB(255, 255, 255) ChtB.TextSize = 9 ChtB.Parent = MdsFrame CB_C.CornerRadius = UDim.new(0, 6) CB_C.Parent = ChtB

    HidB.MouseButton1Click:Connect(function() Mode = "Hidden" HidB.Text = "نسخ مخفي [نشط]" HidB.BackgroundColor3 = Color3.fromRGB(0, 140, 60) ChtB.Text = "نسخ شات" ChtB.BackgroundColor3 = Color3.fromRGB(0, 90, 40) end)
    ChtB.MouseButton1Click:Connect(function() Mode = "Chat" ChtB.Text = "نسخ شات [نشط]" ChtB.BackgroundColor3 = Color3.fromRGB(0, 140, 60) HidB.Text = "نسخ مخفي" HidB.BackgroundColor3 = Color3.fromRGB(0, 90, 40) end)

    -- التحكم والتشغيل والسرعة باليمين
    local V2RightFrame = Instance.new("Frame") V2RightFrame.Size = UDim2.new(1, -240, 1, 0) V2RightFrame.Position = UDim2.new(0, 240, 0, 0) V2RightFrame.BackgroundTransparency = 1 V2RightFrame.Parent = QuickCopyV2Page

    local StartBtn = Instance.new("TextButton") local SBC1 = Instance.new("UICorner")
    StartBtn.Size = UDim2.new(1, 0, 0, 35) StartBtn.Position = UDim2.new(0, 0, 0, 10) StartBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 160) StartBtn.Font = Enum.Font.GothamBold StartBtn.Text = "🚀 تفعيل التشغيل" StartBtn.TextColor3 = Color3.fromRGB(255, 255, 255) StartBtn.TextSize = 11 StartBtn.Parent = V2RightFrame SBC1.CornerRadius = UDim.new(0, 6) SBC1.Parent = StartBtn

    local StopBtn = Instance.new("TextButton") local SBC2 = Instance.new("UICorner")
    StopBtn.Size = UDim2.new(1, 0, 0, 35) StopBtn.Position = UDim2.new(0, 0, 0, 50) StopBtn.BackgroundColor3 = Color3.fromRGB(160, 20, 20) StopBtn.Font = Enum.Font.GothamBold StopBtn.Text = "🛑 إيقاف التشغيل" StopBtn.TextColor3 = Color3.fromRGB(255, 255, 255) StopBtn.TextSize = 11 StopBtn.Parent = V2RightFrame SBC2.CornerRadius = UDim.new(0, 6) SBC2.Parent = StopBtn

    local SpeedLabel = Instance.new("TextLabel") SpeedLabel.Size = UDim2.new(1, 0, 0, 20) SpeedLabel.Position = UDim2.new(0, 0, 0, 95) SpeedLabel.BackgroundTransparency = 1 SpeedLabel.Font = Enum.Font.GothamBold SpeedLabel.Text = "⏱️ سرعة النسخ بالتأخير:" SpeedLabel.TextColor3 = Color3.fromRGB(190, 160, 255) SpeedLabel.TextSize = 10 SpeedLabel.Parent = V2RightFrame
    local SpeedInp = Instance.new("TextBox") local SICorner = Instance.new("UICorner")
    SpeedInp.Size = UDim2.new(1, 0, 0, 32) SpeedInp.Position = UDim2.new(0, 0, 0, 120) SpeedInp.BackgroundColor3 = Color3.fromRGB(25, 15, 35) SpeedInp.Text = "0.01" SpeedInp.TextColor3 = Color3.fromRGB(0, 255, 150) SpeedInp.Font = Enum.Font.GothamBold SpeedInp.TextSize = 12 SpeedInp.Parent = V2RightFrame SICorner.CornerRadius = UDim.new(0, 6) SICorner.Parent = SpeedInp

    -- ربط وتحديث وظيفة قائمة سكرول نسخ سريع V2
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

    -- آلية تشغيل حلقة التدمير للنسخ V2
    StartBtn.MouseButton1Click:Connect(function()
        if #SelTable == 0 or V2Inp.Text == "" then CreateNotification("اختر هدفاً واكتب أمراً!") return end
        if Run then return end Run = true
        StartBtn.BackgroundColor3 = Color3.fromRGB(130, 0, 255) StartBtn.Text = "⚡ جاري التدمير نشط ⚡"
        if not ARem then ScanRemotes() end
        task.spawn(function()
            while Run do
                local customSpeed = tonumber(SpeedInp.Text) or 0.01
                local pat = ""
                for _, target in pairs(SelTable) do
                    local targetName = (UseShortName and V2SearchInp.Text ~= "" and #V2SearchInp.Text >= 2) and V2SearchInp.Text:lower() or target:lower()
                    for c in V2Inp.Text:gmatch("%S+") do
                        local prefix = (string.sub(c, 1, 1) == "/" and "" or "/")
                        pat = pat .. prefix .. c .. " " .. targetName .. " "
                    end
                end
                local finalPat = string.rep(pat, 60)
                pcall(function()
                    if Mode == "Hidden" and ARem then ARem:InvokeServer(finalPat)
                    elseif Mode == "Chat" then if CRem then CRem:FireServer(finalPat, "All") end if ARem then ARem:InvokeServer(finalPat) end end
                end)
                task.wait(customSpeed)
            end
        end)
    end)
    StopBtn.MouseButton1Click:Connect(function() Run = false StartBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 160) StartBtn.Text = "🚀 تفعيل التشغيل" end)

    ---------------------------------------------------------
    -- [5. بناء صفحة اخرى 📀 وتضمين مانع التقطيع الأقوى]
    ---------------------------------------------------------
    local OtherContainer = Instance.new("Frame")
    OtherContainer.Size = UDim2.new(1, 0, 1, 0) OtherContainer.BackgroundTransparency = 1 OtherContainer.Parent = OtherPage

    local FPSBtn = Instance.new("TextButton") local FPSCorner = Instance.new("UICorner") local FPSStroke = Instance.new("UIStroke")
    FPSBtn.Size = UDim2.new(0, 320, 0, 45) FPSBtn.Position = UDim2.new(0.5, -160, 0, 20) FPSBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45) FPSBtn.Font = Enum.Font.GothamBold FPSBtn.Text = "🚀 مانع التقطيع الأقوى [معطل]" FPSBtn.TextColor3 = Color3.fromRGB(235, 220, 255) FPSBtn.TextSize = 12 FPSBtn.Parent = OtherContainer
    FPSCorner.CornerRadius = UDim.new(0, 8) FPSCorner.Parent = FPSBtn
    FPSStroke.Color = Color3.fromRGB(100, 100, 100) FPSStroke.Thickness = 1.2 FPSStroke.Parent = FPSBtn

    local OtherInfoLabel = Instance.new("TextLabel")
    OtherInfoLabel.Size = UDim2.new(1, 0, 0, 60) OtherInfoLabel.Position = UDim2.new(0, 0, 0, 75) OtherInfoLabel.BackgroundTransparency = 1 OtherInfoLabel.Font = Enum.Font.GothamSemibold
    OtherInfoLabel.Text = "عند تفعيل خيار مانع التقطيع الأقوى، سيتم حذف المؤثرات البصرية الزائدة والجسيمات (Particles/Explosions) وتعديل خامات الماب بالكامل لتسريع الفريمات وتقليل الفريز فوراً وبأعلى حماية وسرعة ممكنة لتجنب الكراش." OtherInfoLabel.TextColor3 = Color3.fromRGB(150, 150, 160) OtherInfoLabel.TextSize = 10 OtherInfoLabel.TextWrapped = true OtherInfoLabel.Parent = OtherContainer

    local FPSActive, FastClearConnection = false, nil
    FPSBtn.MouseButton1Click:Connect(function()
        FPSActive = not FPSActive
        if FPSActive then
            FPSBtn.Text = "🚀 مانع التقطيع الأقوى [مفعل]" FPSBtn.BackgroundColor3 = Color3.fromRGB(0, 140, 60) FPSStroke.Color = Color3.fromRGB(0, 255, 120)
            CreateNotification("تم تفعيل مانع التقطيع وحذف المؤثرات!")
            pcall(function()
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("ParticleEmitter") or v:IsA("Explosion") or v:IsA("Sparkles") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Decal") or v:IsA("Texture") then
                        v:Destroy()
                    elseif (v:IsA("MeshPart") or v:IsA("Part")) and not v:IsDescendantOf(workspace.CurrentCamera) then
                        v.Material = Enum.Material.SmoothPlastic v.Reflectance = 0
                    end
                end
            end)
            FastClearConnection = workspace.DescendantAdded:Connect(function(d)
                if FPSActive and (d:IsA("ParticleEmitter") or d:IsA("Explosion") or d:IsA("Sparkles") or d:IsA("Fire")) then
                    task.defer(function() pcall(function() d:Destroy() end) end)
                end
            end)
        else
            FPSBtn.Text = "🚀 مانع التقطيع الأقوى [معطل]" FPSBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45) FPSStroke.Color = Color3.fromRGB(100, 100, 100)
            if FastClearConnection then FastClearConnection:Disconnect() FastClearConnection = nil end
            CreateNotification("تم إلغاء تفعيل مانع التقطيع.")
        end
    end)

    ---------------------------------------------------------
    -- المحركات الخلفية وحلقات الأوامر القديمة للـ Hub الرئيسي
    ---------------------------------------------------------
    local IsSpamming = false
    local CustomCommands = {";re", ";logs", ";nv", ";kill", ";res", ";clogs", ";ice"}
    
    local function ProcessCommands(tn)
        if tn == "" then tn = _EX.L.Name end local t = {} 
        for _, c in ipairs(CustomCommands) do table.insert(t, c .. " " .. tn) end 
        return table.concat(t, " ")
    end

    ActionButton.MouseButton1Click:Connect(function()
        IsSpamming = not IsSpamming
        if IsSpamming then
            ActionButton.Text = "إيقاف الحلقة [نشط]" ActionButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            task.spawn(function()
                while IsSpamming do
                    local payload = ProcessCommands(NameInput.Text)
                    pcall(function() _EX.R.HDAdminHDClient.Signals.RequestCommandModification:InvokeServer(unpack({payload})) end)
                    pcall(function() _EX.R.RemoteEvents.ChatEvent:FireServer(unpack({payload})) end)
                    task.wait(0.1)
                end
            end)
        else
            ActionButton.Text = "تفعيل حلقة الأوامر الإدارية" ActionButton.BackgroundColor3 = RandomThemeColor
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

_OPI_DATA, _OPI_KEY, _OPI_DEC, _OPI_XOR = nil, nil, nil, nil
