--[[
    1993 HUB v20.0 [MODDED]
    [!] Color Feature: Dynamic Randomized Theme Color on Executing.
    [+] Tabs: Quick Copy | Music | Skins | Pull Player (سحب شخص)
    [+] Feature: Custom Skin Customizer (مربع كتابة يوزر خارجي لتغيير السكن)
    [Speed Fix] Spam Command Loop delay reduced to 0.1 seconds!
    [Update] All Old & New Commands Merged Together!
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

    ---------------------------------------------------------
    -- [نظام مصفوفة الألوان العشوائية الفخمة]
    ---------------------------------------------------------
    local PremiumColors = {
        Color3.fromRGB(0, 255, 150),   -- أخضر نيون سايبر
        Color3.fromRGB(0, 200, 255),   -- أزرق ثلجي فخم
        Color3.fromRGB(255, 0, 127),   -- وردي حاد ماجينتا
        Color3.fromRGB(255, 120, 0),   -- برتقالي ناري مشع
        Color3.fromRGB(180, 0, 255),   -- بنفسجي غامض ملكي
        Color3.fromRGB(255, 215, 0),   -- ذهبي براق متوهج
        Color3.fromRGB(255, 50, 50)     -- أحمر قرمزي نقي
    }
    local RandomThemeColor = PremiumColors[math.random(1, #PremiumColors)]

    local MainGui = Instance.new("ScreenGui")
    MainGui.Name = "Spider_Hub_V20"
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
        NF.Size = UDim2.new(0, 240, 0, 35) NF.Position = UDim2.new(0.5, -140, 0, -50)
        NF.BackgroundColor3 = Color3.fromRGB(15, 15, 25) NF.BackgroundTransparency = 0.15 NF.Parent = MainGui
        CR.CornerRadius = UDim.new(0, 8) CR.Parent = NF
        local NS = Instance.new("UIStroke") NS.Color = RandomThemeColor NS.Thickness = 1.2 NS.Parent = NF
        TL.Size = UDim2.new(1, 0, 1, 0) TL.BackgroundTransparency = 1 TL.Font = Enum.Font.GothamBold
        TL.Text = msg TL.TextColor3 = Color3.fromRGB(255, 255, 255) TL.TextSize = 11 TL.Parent = NF
        NF:TweenPosition(UDim2.new(0.5, -120, 0, 20), "Out", "Back", 0.4, true)
        task.wait(2.2)
        local ft = _EX.T:Create(NF, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -120, 0, -50), BackgroundTransparency = 1})
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

    task.wait(2.0)
    local fadeSplash = _EX.T:Create(Splash, TweenInfo.new(0.5), {BackgroundTransparency = 1})
    _EX.T:Create(SplashStroke, TweenInfo.new(0.5), {Transparency = 1}):Play()
    _EX.T:Create(SplashLabel, TweenInfo.new(0.4), {TextTransparency = 1}):Play()
    fadeSplash:Play()
    fadeSplash.Completed:Connect(function() Splash:Destroy() end)

    ---------------------------------------------------------
    -- [الواجهة الرئيسية للسكربت - Main Container]
    ---------------------------------------------------------
    local Container = Instance.new("Frame") local ContainerCorner = Instance.new("UICorner") local ContainerStroke = Instance.new("UIStroke")
    local FullSize = UDim2.new(0, 560, 0, 300) local FullPos = UDim2.new(0.5, -280, 0.5, -150)

    Container.Size = UDim2.new(0, 0, 0, 0) Container.Position = UDim2.new(0.5, 0, 0.5, 0)
    Container.BackgroundColor3 = Color3.fromRGB(12, 12, 18) Container.BackgroundTransparency = 0.05 Container.ClipsDescendants = true Container.Parent = MainGui
    ContainerCorner.CornerRadius = UDim.new(0, 10) ContainerCorner.Parent = Container
    ContainerStroke.Color = RandomThemeColor ContainerStroke.Thickness = 1.5 ContainerStroke.Parent = Container
    MakeDraggable(Container)

    ---------------------------------------------------------
    -- [شريط القوائم الجانبي العلوي - Sidebar Tabs]
    ---------------------------------------------------------
    local Sidebar = Instance.new("Frame") local SBCorner = Instance.new("UICorner")
    Sidebar.Size = UDim2.new(0, 140, 1, 0) Sidebar.BackgroundColor3 = Color3.fromRGB(16, 16, 22) Sidebar.Parent = Container
    SBCorner.CornerRadius = UDim.new(0, 10) SBCorner.Parent = Sidebar

    local HubLogo = Instance.new("TextLabel")
    HubLogo.Size = UDim2.new(1, 0, 0, 40) HubLogo.BackgroundTransparency = 1 HubLogo.Font = Enum.Font.GothamBlack
    HubLogo.Text = "🕷 SPIDER HUB" HubLogo.TextColor3 = RandomThemeColor HubLogo.TextSize = 13 HubLogo.Parent = Sidebar

    local TabContainer = Instance.new("Frame") TabContainer.Size = UDim2.new(1, 0, 1, -110) TabContainer.Position = UDim2.new(0, 0, 0, 45) TabContainer.BackgroundTransparency = 1 TabContainer.Parent = Sidebar
    local TabLayout = Instance.new("UIListLayout") TabLayout.Padding = UDim.new(0, 5) TabLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center TabLayout.Parent = TabContainer

    ---------------------------------------------------------
    -- [معلومات الحساب أسفل اليسار]
    ---------------------------------------------------------
    local UserProfile = Instance.new("Frame") local UPCorner = Instance.new("UICorner")
    UserProfile.Size = UDim2.new(1, -10, 0, 50) UserProfile.Position = UDim2.new(0, 5, 1, -55) UserProfile.BackgroundColor3 = Color3.fromRGB(24, 24, 32) UserProfile.Parent = Sidebar
    UPCorner.CornerRadius = UDim.new(0, 6) UPCorner.Parent = UserProfile

    local AvatarImg = Instance.new("ImageLabel") local AICorner = Instance.new("UICorner")
    AvatarImg.Size = UDim2.new(0, 36, 0, 36) AvatarImg.Position = UDim2.new(0, 6, 0.5, -18) AvatarImg.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
    AvatarImg.Image = "rbxthumb://type=AvatarHeadShot&id=" .. _EX.L.UserId .. "&w=150&h=150" AvatarImg.Parent = UserProfile
    AICorner.CornerRadius = UDim.new(1, 0) AICorner.Parent = AvatarImg

    local DisplayNameLabel = Instance.new("TextLabel")
    DisplayNameLabel.Size = UDim2.new(1, -48, 0, 16) DisplayNameLabel.Position = UDim2.new(0, 46, 0, 8) DisplayNameLabel.BackgroundTransparency = 1 DisplayNameLabel.Font = Enum.Font.GothamBold
    DisplayNameLabel.Text = _EX.L.DisplayName DisplayNameLabel.TextColor3 = Color3.fromRGB(255, 255, 255) DisplayNameLabel.TextSize = 9 DisplayNameLabel.TextXAlignment = Enum.TextXAlignment.Left DisplayNameLabel.Parent = UserProfile

    local UsernameLabel = Instance.new("TextLabel")
    UsernameLabel.Size = UDim2.new(1, -48, 0, 14) UsernameLabel.Position = UDim2.new(0, 46, 0, 24) UsernameLabel.BackgroundTransparency = 1 UsernameLabel.Font = Enum.Font.Gotham
    UsernameLabel.Text = "@" .. _EX.L.Name UsernameLabel.TextColor3 = RandomThemeColor UsernameLabel.TextSize = 8 UsernameLabel.TextXAlignment = Enum.TextXAlignment.Left UsernameLabel.Parent = UserProfile

    ---------------------------------------------------------
    -- نظام الصفحات الداخلي للسكربت
    ---------------------------------------------------------
    local ContentFrame = Instance.new("Frame") ContentFrame.Size = UDim2.new(1, -155, 1, -15) ContentFrame.Position = UDim2.new(0, 148, 0, 8) ContentFrame.BackgroundTransparency = 1 ContentFrame.Parent = Container

    local Pages = {}
    local function CreateTab(name, icon)
        local Page = Instance.new("Frame")
        Page.Size = UDim2.new(1, 0, 1, 0) Page.BackgroundTransparency = 1 Page.Visible = false Page.Parent = ContentFrame
        Pages[name] = Page
        
        local TabBtn = Instance.new("TextButton") local TBC = Instance.new("UICorner")
        TabBtn.Size = UDim2.new(1, -10, 0, 32) TabBtn.BackgroundColor3 = Color3.fromRGB(24, 24, 32) TabBtn.Font = Enum.Font.GothamSemibold TabBtn.Text = icon .. " " .. name TabBtn.TextColor3 = Color3.fromRGB(160, 160, 160) TabBtn.TextSize = 10 TabBtn.Parent = TabContainer TBC.CornerRadius = UDim.new(0, 5) TBC.Parent = TabBtn
        
        TabBtn.MouseButton1Click:Connect(function()
            for _, p in pairs(Pages) do p.Visible = false end
            for _, b in ipairs(TabContainer:GetChildren()) do if b:IsA("TextButton") then b.BackgroundColor3 = Color3.fromRGB(24, 24, 32) b.TextColor3 = Color3.fromRGB(160, 160, 160) end end
            Page.Visible = true TabBtn.BackgroundColor3 = RandomThemeColor TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        end)
        return Page
    end

    local QuickCopyPage = CreateTab("النسخ السريع", "⚡")
    local MusicPage = CreateTab("الأغاني", "🎵")
    local SkinsPage = CreateTab("سكنات أولاد", "👕")
    local PullPlayerPage = CreateTab("سحب شخص", "🎯")

    Pages["النسخ السريع"].Visible = true
    TabContainer:FindFirstChildOfClass("TextButton").BackgroundColor3 = RandomThemeColor
    TabContainer:FindFirstChildOfClass("TextButton").TextColor3 = Color3.fromRGB(255, 255, 255)

    ---------------------------------------------------------
    -- [1. محتويات صفحة النسخ السريع واللاعبين]
    ---------------------------------------------------------
    local LeftControls = Instance.new("Frame")
    LeftControls.Size = UDim2.new(0, 190, 1, 0) LeftControls.BackgroundTransparency = 1 LeftControls.Parent = QuickCopyPage

    local NameInput = Instance.new("TextBox") local IC1 = Instance.new("UICorner")
    NameInput.Size = UDim2.new(1, 0, 0, 32) NameInput.Position = UDim2.new(0, 0, 0, 5)
    NameInput.BackgroundColor3 = Color3.fromRGB(22, 22, 30) NameInput.Font = Enum.Font.Gotham NameInput.PlaceholderText = "اسم اللاعب المستهدف..." NameInput.Text = "" NameInput.TextColor3 = Color3.fromRGB(255, 255, 255) NameInput.TextSize = 11 NameInput.Parent = LeftControls IC1.CornerRadius = UDim.new(0, 6) IC1.Parent = NameInput

    local AudioInput = Instance.new("TextBox") local AIC = Instance.new("UICorner")
    AudioInput.Size = UDim2.new(1, 0, 0, 32) AudioInput.Position = UDim2.new(0, 0, 0, 42)
    AudioInput.BackgroundColor3 = Color3.fromRGB(22, 22, 30) AudioInput.Font = Enum.Font.Gotham AudioInput.PlaceholderText = "كود الأغنية المنسوخ حالياً..." AudioInput.Text = "" AudioInput.TextColor3 = Color3.fromRGB(255, 200, 0) AudioInput.TextSize = 11 AudioInput.Parent = LeftControls AIC.CornerRadius = UDim.new(0, 6) AIC.Parent = AudioInput

    local ActionButton = Instance.new("TextButton") local BC1 = Instance.new("UICorner")
    ActionButton.Size = UDim2.new(1, 0, 0, 35) ActionButton.Position = UDim2.new(0, 0, 0, 82)
    ActionButton.BackgroundColor3 = RandomThemeColor ActionButton.Font = Enum.Font.GothamBold ActionButton.Text = "تفعيل حلقة الأوامر الإدارية" ActionButton.TextColor3 = Color3.fromRGB(255, 255, 255) ActionButton.TextSize = 11 ActionButton.Parent = LeftControls BC1.CornerRadius = UDim.new(0, 6) BC1.Parent = ActionButton

    local RainbowInput = Instance.new("TextBox") local IC2 = Instance.new("UICorner")
    RainbowInput.Size = UDim2.new(1, 0, 0, 35) RainbowInput.Position = UDim2.new(0, 0, 0, 124)
    RainbowInput.BackgroundColor3 = Color3.fromRGB(255, 25, 40) RainbowInput.Font = Enum.Font.GothamBold RainbowInput.PlaceholderText = "[ نص الرينبو + Enter ]" RainbowInput.Text = "" RainbowInput.TextColor3 = Color3.fromRGB(0, 200, 255) RainbowInput.TextSize = 11 RainbowInput.Parent = LeftControls IC2.CornerRadius = UDim.new(0, 6) IC2.Parent = RainbowInput

    local RightPlayersPanel = Instance.new("Frame")
    RightPlayersPanel.Size = UDim2.new(1, -200, 1, 0) RightPlayersPanel.Position = UDim2.new(0, 200, 0, 0) RightPlayersPanel.BackgroundColor3 = Color3.fromRGB(18, 18, 26) RightPlayersPanel.Parent = QuickCopyPage
    local RPC = Instance.new("UICorner") RPC.CornerRadius = UDim.new(0, 8) RPC.Parent = RightPlayersPanel
    local RPS = Instance.new("UIStroke") RPS.Color = Color3.fromRGB(40, 40, 50) RPS.Thickness = 1 RPS.Parent = RightPlayersPanel

    local PTitle = Instance.new("TextLabel") PTitle.Size = UDim2.new(1, 0, 0, 25) PTitle.BackgroundTransparency = 1 PTitle.Font = Enum.Font.GothamBold PTitle.Text = "👥 لاعبين متصلين بسيرفرك" PTitle.TextColor3 = Color3.fromRGB(255, 255, 255) PTitle.TextSize = 11 PTitle.Parent = RightPlayersPanel

    local PlayersScroll = Instance.new("ScrollingFrame") local PlayersListLayout = Instance.new("UIListLayout")
    PlayersScroll.Size = UDim2.new(1, -8, 1, -32) PlayersScroll.Position = UDim2.new(0, 4, 0, 28) PlayersScroll.BackgroundTransparency = 1 PlayersScroll.ScrollBarThickness = 3 PlayersScroll.ScrollBarImageColor3 = RandomThemeColor PlayersScroll.Parent = RightPlayersPanel
    PlayersListLayout.SortOrder = Enum.SortOrder.LayoutOrder PlayersListLayout.Padding = UDim.new(0, 4) PlayersListLayout.Parent = PlayersScroll

    local function UpdatePlayersList()
        for _, child in ipairs(PlayersScroll:GetChildren()) do if child:IsA("Frame") then child:Destroy() end end
        for _, player in ipairs(_EX.P:GetPlayers()) do
            local PFrame = Instance.new("Frame") local PFC = Instance.new("UICorner")
            PFrame.Size = UDim2.new(1, -6, 0, 30) PFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 40) PFrame.Parent = PlayersScroll
            PFC.CornerRadius = UDim.new(0, 5) PFC.Parent = PFrame
            local PName = Instance.new("TextButton") PName.Size = UDim2.new(1, -8, 1, 0) PName.Position = UDim2.new(0, 6, 0, 0) PName.BackgroundTransparency = 1 PName.Font = Enum.Font.GothamSemibold PName.Text = player.Name PName.TextColor3 = Color3.fromRGB(220, 220, 230) PName.TextSize = 10 PName.TextXAlignment = Enum.TextXAlignment.Left PName.Parent = PFrame
            PName.MouseButton1Click:Connect(function() NameInput.Text = player.Name CreateNotification("تم تحديد اللاعب: " .. player.Name) end)
        end
        PlayersScroll.CanvasSize = UDim2.new(0, 0, 0, PlayersListLayout.AbsoluteContentSize.Y + 5)
    end
    _EX.P.PlayerAdded:Connect(UpdatePlayersList) _EX.P.PlayerRemoving:Connect(UpdatePlayersList) UpdatePlayersList()

    ---------------------------------------------------------
    -- [2. قائمة الأغاني المطلوبة]
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
            task.spawn(function() CreateNotification("تم نسخ الكود للحافظة: " .. song.Code) end) task.wait(1.2)
            CopyBtn.Text = "نسخ 📋" CopyBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
        end)
    end
    task.wait() MusicScroll.CanvasSize = UDim2.new(0, 0, 0, MusicListLayout.AbsoluteContentSize.Y + 5)

    ---------------------------------------------------------
    -- [3. محتويات صفحة سكنات أولاد 1 و 2 بتوزيع مريح وكامل]
    ---------------------------------------------------------
    local SkinsContainer = Instance.new("Frame")
    SkinsContainer.Size = UDim2.new(1, 0, 1, 0) SkinsContainer.BackgroundTransparency = 1 SkinsContainer.Parent = SkinsPage

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

    ---------------------------------------------------------
    -- [مربع كود السكن/يوزر فوق على اليمين]
    ---------------------------------------------------------
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
            CustomSkinBtn.Text = "تم الإرسال! 🔥" CustomSkinBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
            CreateNotification("تم تطبيق السكن لـ: " .. targetUser)
            task.wait(1.2)
            CustomSkinBtn.Text = "تنفيذ السكن ⚡" CustomSkinBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
        else
            CreateNotification("الرجاء كتابة اسم أو كود المستخدم أولاً!")
        end
    end)

    -- [قائمة يوزرات الأولاد 1]
    local BoySkinsData = {
        {Label = "سكن Fikzyyx", User = "Fikzyyx"},
        {Label = "سكن A1CKER", User = "A1CKER"},
        {Label = "سكن ohorphic", User = "ohorphic"},
        {Label = "سكن uiu", User = "uiu"},
        {Label = "سكن 36", User = "36"},
        {Label = "سكن nvm", User = "nvm"},
        {Label = "سكن محمد الدون", User = "mohammeedd78"}
    }

    -- [قائمة يوزرات الأولاد 2]
    local BoySkinsNewData = {
        {Label = "سكن AINEENIAXX", User = "AINEENIAXX"},
        {Label = "سكن GIFT_JW26", User = "GIFT_JW26"},
        {Label = "سكن PAO_PAOPA", User = "PAO_PAOPA"},
        {Label = "سكن CHOCOBALL_22", User = "CHOCOBALL_22"},
        {Label = "سكن WEIQIANG79", User = "WEIQIANG79"},
        {Label = "سكن 7iujs", User = "7iujs"},
        {Label = "سكن 0ilw2", User = "0ilw2"},
        {Label = "سكن 2RTX_I", User = "2RTX_I"},
        {Label = "سكن 7ilix1i0", User = "7ilix1i0"},
        {Label = "سكن 2xlli6", User = "2xlli6"}
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
                SBtn.Text = "تم! 🔥" SBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
                CreateNotification("تم تنفيذ الأمر مباشرة: " .. fullCmd)
                task.wait(1.2)
                SBtn.Text = "تفعيل 👕" SBtn.BackgroundColor3 = btnColor
            end)
        end
        scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 5)
    end

    PopulateSkins(BoysScroll, BoysLayout, BoySkinsData, Color3.fromRGB(0, 120, 200))
    PopulateSkins(Boys2Scroll, Boys2Layout, BoySkinsNewData, Color3.fromRGB(0, 160, 150))

    ---------------------------------------------------------
    -- [4. صفحة سحب شخص المستقلة الكاملة الفخمة]
    ---------------------------------------------------------
    local PullPageContainer = Instance.new("Frame")
    PullPageContainer.Size = UDim2.new(1, 0, 1, 0) PullPageContainer.BackgroundTransparency = 1 PullPageContainer.Parent = PullPlayerPage

    local FullPullPanel = Instance.new("Frame") local FPPCorner = Instance.new("UICorner")
    FullPullPanel.Size = UDim2.new(1, 0, 1, 0) FullPullPanel.BackgroundColor3 = Color3.fromRGB(20, 20, 28) FullPullPanel.Parent = PullPageContainer
    FPPCorner.CornerRadius = UDim.new(0, 8) FPPCorner.Parent = FullPullPanel
    local FullP_Stroke = Instance.new("UIStroke") FullP_Stroke.Color = RandomThemeColor FullP_Stroke.Thickness = 1.2 FullP_Stroke.Parent = FullPullPanel

    local PTitle2 = Instance.new("TextLabel") PTitle2.Size = UDim2.new(1, 0, 0, 30) PTitle2.BackgroundTransparency = 1 PTitle2.Font = Enum.Font.GothamBold PTitle2.Text = "🎯 نظام سحب وتلبيوت اللاعبين المطور" PTitle2.TextColor3 = Color3.fromRGB(255, 215, 0) PTitle2.TextSize = 12 PTitle2.Parent = FullPullPanel

    local PullScroll = Instance.new("ScrollingFrame") local PullLayout = Instance.new("UIListLayout")
    PullScroll.Size = UDim2.new(1, -20, 1, -85) PullScroll.Position = UDim2.new(0, 10, 0, 35) PullScroll.BackgroundTransparency = 1 PullScroll.ScrollBarThickness = 3 PullScroll.ScrollBarImageColor3 = RandomThemeColor PullScroll.Parent = FullPullPanel
    PullLayout.SortOrder = Enum.SortOrder.LayoutOrder PullLayout.Padding = UDim.new(0, 5) PullLayout.Parent = PullScroll

    local PullButton = Instance.new("TextButton") local PBC = Instance.new("UICorner")
    PullButton.Size = UDim2.new(1, -20, 0, 36) PullButton.Position = UDim2.new(0, 10, 1, -42) PullButton.BackgroundColor3 = RandomThemeColor PullButton.Font = Enum.Font.GothamBold PullButton.Text = "اختر لاعباً من الأعلى للسحب تلقائياً" PullButton.TextColor3 = Color3.fromRGB(255, 255, 255) PullButton.TextSize = 11 PullButton.Parent = FullPullPanel PBC.CornerRadius = UDim.new(0, 6) PBC.Parent = PullButton

    local SelectedPullPlayer = ""

    local function UpdatePullList()
        for _, child in ipairs(PullScroll:GetChildren()) do if child:IsA("Frame") then child:Destroy() end end
        for _, player in ipairs(_EX.P:GetPlayers()) do
            if player ~= _EX.L then
                local Item = Instance.new("Frame") local IC = Instance.new("UICorner")
                Item.Size = UDim2.new(1, -6, 0, 34) Item.BackgroundColor3 = Color3.fromRGB(30, 30, 40) Item.Parent = PullScroll
                IC.CornerRadius = UDim.new(0, 5) IC.Parent = Item

                local Btn = Instance.new("TextButton")
                Btn.Size = UDim2.new(1, 0, 1, 0) Btn.BackgroundTransparency = 1 Btn.Font = Enum.Font.GothamSemibold Btn.Text = "  " .. player.Name .. " (" .. player.DisplayName .. ")" Btn.TextColor3 = Color3.fromRGB(230, 230, 240) Btn.TextSize = 10 Btn.TextXAlignment = Enum.TextXAlignment.Left Btn.Parent = Item

                Btn.MouseButton1Click:Connect(function()
                    SelectedPullPlayer = player.Name
                    PullButton.Text = "اضغط هنا لسحب: ;tp " .. player.Name
                    CreateNotification("تم اختيار: " .. player.Name)
                end)
            end
        end
        PullScroll.CanvasSize = UDim2.new(0, 0, 0, PullLayout.AbsoluteContentSize.Y + 5)
    end

    _EX.P.PlayerAdded:Connect(UpdatePullList) _EX.P.PlayerRemoving:Connect(function(p) if SelectedPullPlayer == p.Name then SelectedPullPlayer = "" PullButton.Text = "اختر لاعباً من الأعلى للسحب تلقائياً" end UpdatePullList() end) UpdatePullList()

    PullButton.MouseButton1Click:Connect(function()
        if SelectedPullPlayer ~= "" then
            local fullCmd = ";tp " .. SelectedPullPlayer
            pcall(function() _EX.R.HDAdminHDClient.Signals.RequestCommandModification:InvokeServer(unpack({fullCmd})) end)
            pcall(function() _EX.R.RemoteEvents.ChatEvent:FireServer(unpack({fullCmd})) end)
            CreateNotification("تم إرسال أمر السحب: " .. fullCmd)
        else
            CreateNotification("الرجاء اختيار لاعب من القائمة أولاً!")
        end
    end)

    ---------------------------------------------------------
    -- المحركات الخلفية وحلقات الأوامر والدردشة
    ---------------------------------------------------------
    local IsSpamming = false
    -- [تم دمج الأوامر القديمة والجديدة معاً بالكامل]
    local CustomCommands = {";re", ";logs", ";nv", ";kill", ";res", ";clogs", ";ice"}
    
    local function ProcessCommands(tn)
        if tn == "" then tn = _EX.L.Name end
        local t = {} 
        for _, c in ipairs(CustomCommands) do 
            table.insert(t, c .. " " .. tn) 
        end 
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

    local function ConvertToRainbowRichText(text)
        local totalChars = #text local result = ""
        local hexColors = {"#FF0000", "#FF7F00", "#FFFF00", "#00FF00", "#0000FF", "#4B0082", "#9400D3"}
        for i = 1, totalChars do
            local char = text:sub(i, i) local colorIndex = ((i - 1) % #hexColors) + 1 local color = hexColors[colorIndex]
            if char == " " then result = result .. " " else result = result .. "<font color='" .. color .. "'>" .. char .. "</font>" end
        end
        return result
    end

    RainbowInput.FocusLost:Connect(function(enterPressed)
        local baseText = RainbowInput.Text if baseText == "" then return end
        local rainbowRichText = ConvertToRainbowRichText(baseText)
        pcall(function()
            local chatService = game:GetService("TextChatService")
            if chatService and chatService.ChatVersion == Enum.ChatVersion.TextChatService then
                local channel = chatService.TextChannels:FindFirstChild("RBXGeneral") if channel then channel:SendAsync(rainbowRichText) end
            else
                _EX.R.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(rainbowRichText, "All")
            end
        end)
    end)

    Container.Visible = true
    _EX.T:Create(Container, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = FullSize, Position = FullPos}):Play()

    local TB = Instance.new("TextButton") local TC = Instance.new("UICorner") local TS = Instance.new("UIStroke")
    TB.Name = "OpenStationButton" TB.Size = UDim2.new(0, 40, 0, 40) TB.Position = UDim2.new(1, -55, 1, -55)
    TB.BackgroundColor3 = Color3.fromRGB(10, 10, 15) TB.Font = Enum.Font.GothamBold TB.Text = "🕷" TB.TextColor3 = RandomThemeColor TB.TextSize = 16 TB.ZIndex = 10 TB.Parent = MainGui
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
