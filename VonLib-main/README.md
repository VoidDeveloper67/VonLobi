# ✨ VonLib v1.3.0

**VonLib** is a rebuilt and optimized Roblox UI library — clean, minimal, and packed with a powerful elements API, live stats overlay, DPI-adaptive scaling, and a six-theme colour system.

- 🔹 Made by **von63rd**
- 🔹 Designed for **VonLib Hub** scripts
- 🔹 Open-source · Lightweight · DPI-aware

---

---

## 🆕 What's New in v1.3.0

| Feature | Description |
|---|---|
| **Stats HUD** | Draggable live overlay with 7 built-in stats + custom stat support |
| **DPI Auto-Scale** | Automatic UI scaling based on viewport resolution (mobile → 4K) |
| **Gradient Label Fix** | Properly clears old gradients before applying new ones |
| **Text Size Fix** | Gradient labels now render at readable 13pt instead of 10pt |
| **Version & Author API** | `Library:GetVersion()` and `Library:GetAuthor()` |

---


## 🚀 Quick Start

```lua
local Library = loadstring(game:HttpGet(
  "https://raw.githubusercontent.com/VoidDeveloper67/VonLib/refs/heads/main/main.luau"
))()

local Window = Library:MakeWindow({
  Title       = "My Hub : Game Name",
  SubTitle    = "by von63rd",
  ScriptFolder = "vonlib"
})
```

---

## 🪟 Window API

| Method | Description |
|---|---|
| `MakeTab(config)` | Create a new tab |
| `SelectTab(tab/number)` | Switch to a tab |
| `NewMinimizer(config)` | Keyboard + mobile minimizer |
| `Notify(config)` | Show a notification toast |
| `NewNotifyGroup(config)` | Create a notification group |
| `Dialog(config)` | Show a modal dialog |
| `Tag(config)` | Add a coloured badge to the topbar ⭐ |
| `SaveConfig(name)` | Save flagged element values ⭐ |
| `LoadConfig(name)` | Load and apply a saved config ⭐ |
| `ListConfigs()` | List all saved config files ⭐ |
| `DeleteConfig(name)` | Delete a config file ⭐ |
| `ResetConfig(name?)` | Reset elements to defaults ⭐ |
| `MakeStatsHUD(config)` | Create a draggable live-stats overlay ⭐ NEW |
| `SetBackgroundVideo(url, overlay?)` | Set a video background ⭐ |
| `PauseBackgroundVideo()` | Pause the background video ⭐ |
| `PlayBackgroundVideo()` | Resume the background video ⭐ |
| `SetBackgroundVideoOverlay(t)` | Adjust overlay transparency ⭐ |
| `GetVersion()` | Returns version string |
| `GetAuthor()` | Returns author name |
| `SetTitle(title)` | Update window title |
| `SetSubTitle(subtitle)` | Update window subtitle |
| `GetTitle()` | Get current title |
| `GetSubTitle()` | Get current subtitle |
| `MinimizeButton()` | Toggle minimize state |
| `Destroy()` | Destroy the entire UI |

---

## 📊 Stats HUD ⭐ NEW

A draggable floating overlay that displays live game statistics — ping, FPS, playtime, current time, memory usage, and more. It sits independently of the main window so it stays visible even when the menu is minimized.

```lua
-- Default stats: Ping, FPS, Playtime, Time
local HUD = Library:MakeStatsHUD()

-- Custom selection
local HUD = Library:MakeStatsHUD({
  Stats    = { "Ping", "FPS", "Playtime", "Time", "Memory", "Players" },
  Position = UDim2.new(0, 8, 0.5, 0),  -- starting position (optional)
  Visible  = true,                       -- shown by default
})

-- Show / hide
HUD:SetVisible(false)
HUD:SetVisible(true)
print(HUD:IsVisible())

-- Add a fully custom stat with any live value
HUD:AddStat("Speed", function()
  return tostring(math.floor(game.Players.LocalPlayer.Character
    and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
    and game.Players.LocalPlayer.Character.Humanoid.WalkSpeed or 0))
    .. " stud/s"
end)

-- Remove a stat row
HUD:RemoveStat("Memory")

-- Reposition programmatically
HUD:SetPosition(UDim2.new(1, -120, 0, 8))

-- Clean up
HUD:Destroy()
```

**Built-in stat keys:**

| Key | What it shows |
|---|---|
| `Ping` | Server round-trip in ms |
| `FPS` | Client frame rate |
| `Playtime` | Time since the HUD was created |
| `Time` | Current UTC time (`HH:MM:SS`) |
| `Memory` | Lua memory in KB |
| `Players` | Current player count |
| `Game` | Place name (first 18 characters) |

The HUD is draggable by its top handle bar and stays anchored wherever you drop it across sessions within a game run.

---

## 📐 DPI Auto-Scale ⭐ NEW

VonLib automatically detects the player's viewport size and applies an appropriate UI scale at startup, then recomputes it whenever the window is resized or the device is rotated. No manual configuration is needed.

| Device / Resolution | Behaviour |
|---|---|
| Small phone (< 600 px tall) | Scale reduced to ~72 % |
| Tablet / large phone (600–780 px) | Scale reduced to ~88 % |
| Standard desktop / 1080p (780–1000 px) | Scale at 100 % (default) |
| High-DPI / 4K / large display (> 1000 px) | Scale proportionally up to 140 % |

You can still override the scale manually via `Library:SetUIScale(value)` (range `0.6`–`1.6`).

```lua
Library:SetUIScale(1.2)
print(Library:GetMinScale())  -- 0.6
print(Library:GetMaxScale())  -- 1.6
```

---

## 🎨 Themes

Six fully distinct colour themes, each built around a unique neon accent hue. Switch at any time — all UI elements update instantly.

| Theme | Accent | Vibe |
|---|---|---|
| `Darker` | Electric blue | Void dark, default |
| `Midnight` | Neon violet | Deep space purple |
| `Ocean` | Neon cyan | Cyber teal on navy |
| `Rose` | Neon red | Blood moon |
| `Emerald` | Matrix green | Terminal black-green |
| `Sunset` | Hot pink / magenta | Synthwave neon |

```lua
Library:SetTheme("Midnight")
Library:SetTheme("Rose")
Library:SetTheme("Emerald")
Library:SetTheme("Sunset")

-- In a dropdown so users can pick live:
ConfigTab:AddDropdown({
  Name     = "Theme",
  Options  = Library:GetThemes(),
  Default  = Library:GetTheme().Name,
  Callback = function(v) Library:SetTheme(v) end
})
```

---

## 🏷️ Tags

Add coloured badge pills to the window topbar.

```lua
local MyTag = Window:Tag({
  Title = "v1.3.0",
  Color = "Amber",              -- named colour or Color3
  Icon  = "rbxassetid://...",   -- optional icon
})

MyTag:SetTitle("v1.3.0")
MyTag:SetColor("Green")
MyTag:SetIcon("")
MyTag:Destroy()
```

**Named colours:** `Amber` · `Red` · `Green` · `Blue` · `Purple` · `Pink` · `Cyan` · `Orange` · `Gray` · `White` · `Default`

---

## 🎬 Background Video

```lua
-- Asset ID
Window:SetBackgroundVideo("rbxassetid://123456789", 0.4)

-- Direct URL (auto-downloads and caches as vonlib_bgvideo.webm)
Window:SetBackgroundVideo("https://files.catbox.moe/yourfile.webm", 0.4)

-- Controls
Window:PauseBackgroundVideo()
Window:PlayBackgroundVideo()
Window:SetBackgroundVideoOverlay(0.6)  -- 0 = black overlay, 1 = no overlay
```

> 💡 Convert your video to `.webm` at [convertio.co](https://convertio.co) and host it free at [catbox.moe](https://catbox.moe).

---

## 💾 Config System

```lua
local Tab = Window:MakeTab({ Title = "Main", Icon = "Home" })

-- Elements with a Flag key are tracked automatically
Tab:AddToggle({ Name = "Auto Farm", Flag = "auto_farm", Default = false, Callback = function(v) end })
Tab:AddSlider({ Name = "Speed",     Flag = "speed",     Min = 0, Max = 100, Default = 16, Callback = function(v) end })

-- Save / Load / Delete
Window:SaveConfig("slot1")
Window:LoadConfig("slot1")
Window:DeleteConfig("slot1")

-- List all saved configs
local configs = Window:ListConfigs()  -- e.g. { "Default", "slot1" }

-- Reset: values only, or values + file
Window:ResetConfig()
Window:ResetConfig("slot1")
```

---

## 📑 Minimizer

```lua
local Minimizer = Window:NewMinimizer({
  KeyCode = Enum.KeyCode.LeftControl
})

-- Mobile button
local MobileBtn = Minimizer:CreateMobileMinimizer({
  Image  = "rbxassetid://101833678008843",
  Size   = UDim2.new(0, 35, 0, 35),
  Corner = { CornerRadius = UDim.new(0, 6) },
})
```

---

## 🔔 Notifications

```lua
Window:Notify({
  Title    = "Loaded!",
  Content  = "VonLib v1.3.0",
  Image    = "rbxassetid://101833678008843",
  Duration = 5
})
```

---

## 💬 Dialog

```lua
Window:Dialog({
  Title   = "Confirm Action",
  Content = "Are you sure you want to proceed?",
  Options = {
    { Name = "Yes", Callback = function(self) print("Confirmed") end },
    { Name = "No"  }
  }
})
```

---

## ⚙️ Options API

Every element supports:

| Method | Description |
|---|---|
| `SetTitle(title)` | Update displayed title |
| `SetDescription(desc)` | Update description text |
| `SetVisible(bool)` | Show or hide the element |
| `Destroy()` | Remove the element entirely |
| `AddCallback(fn)` | Attach an additional callback |
| `ApplyBadge(label)` | Apply a status badge ⭐ |

---

## 🔖 Badges

Attach a coloured status pill to any element title.

```lua
Tab:AddToggle({ Name = "Auto Farm", Badge = "New",     Default = false, Callback = function(v) end })
Tab:AddButton({ Name = "Teleport",  Badge = "Hot",     Callback = function() end })
Tab:AddSlider({ Name = "Speed",     Badge = "Fixed",   Min = 0, Max = 100, Default = 16, Callback = function(v) end })

-- Or apply after creation:
local toggle = Tab:AddToggle({ Name = "ESP", Default = false, Callback = function(v) end })
toggle:ApplyBadge("Beta")
```

**Available badges:** `Bug` 🔴 · `New` 🟢 · `Warning` 🟡 · `Fixed` 🔵 · `Beta` 🟣 · `Hot` 🟠 · `Soon` ⚫

---

## 🧩 Elements

### Section
```lua
Tab:AddSection("Section Title")
```

### Toggle
```lua
Tab:AddToggle({
  Name     = "Auto Farm",
  Badge    = "New",
  Default  = false,
  Flag     = "auto_farm",
  Callback = function(Value) end
})
```

### Button
```lua
Tab:AddButton({
  Name     = "My Button",
  Badge    = "Hot",
  Debounce = 0.5,
  Callback = function() end
})
```

### Slider
```lua
Tab:AddSlider({
  Name      = "Walk Speed",
  Badge     = "Fixed",
  Min       = 16,
  Max       = 200,
  Increment = 1,
  Default   = 16,
  Flag      = "walk_speed",
  Callback  = function(Value) end
})
```

### Keybind
```lua
Tab:AddKeybind({
  Name     = "Sprint Key",
  Default  = Enum.KeyCode.LeftShift,
  Flag     = "sprint_key",
  Callback = function(Key) print("Key:", Key.Name) end
})
```

### ColorPicker
```lua
Tab:AddColorPicker({
  Name     = "ESP Color",
  Badge    = "New",
  Default  = Color3.fromRGB(0, 180, 255),
  Flag     = "esp_color",
  Callback = function(Color) print(Color) end
})
```

### Label
```lua
Tab:AddLabel("Status: Active")
Tab:AddLabel({ Text = "VIP Only", Color = Color3.fromRGB(255, 215, 0) })
```

### Gradient Label ⭐ FIXED
```lua
-- Simple (default purple → cyan)
Tab:AddGradientLabel("VonLib v1.3.0")

-- Custom colours and rotation angle
Tab:AddGradientLabel({
  Text     = "Rainbow",
  Colors   = {
    Color3.fromRGB(255, 80,  80),
    Color3.fromRGB(255, 200, 50),
    Color3.fromRGB(80,  255, 120),
    Color3.fromRGB(80,  180, 255),
    Color3.fromRGB(180, 80,  255),
  },
  Rotation = 90
})

-- Dynamic updates
local lbl = Tab:AddGradientLabel({ Text = "Status", Colors = { Color3.fromRGB(0,200,255), Color3.fromRGB(200,0,255) } })
lbl:SetText("Online")
lbl:SetGradient({ Color3.fromRGB(0, 255, 100), Color3.fromRGB(0, 180, 255) }, 45)
```

> Gradient labels now correctly clear any previous gradient before applying a new one, and require at minimum two colours.

### Dropdown
```lua
Tab:AddDropdown({
  Name     = "Mode",
  Options  = { "Option A", "Option B", "Option C" },
  Default  = "Option A",
  Flag     = "mode",
  Callback = function(Value) end
})

-- Multi-select
Tab:AddDropdown({
  Name        = "Items",
  MultiSelect = true,
  Options     = { "one", "two", "three" },
  Default     = { "one", "two" },
  Callback    = function(Value) end
})
```

### TextBox
```lua
Tab:AddTextBox({
  Name         = "Enter Text",
  Placeholder  = "type...",
  ClearOnFocus = true,
  Flag         = "my_text",
  Callback     = function(Value) end
})
```

### Paragraph
```lua
Tab:AddParagraph("Title", "Some multi-line\ndescription text.")
```

### Discord Invite
```lua
Tab:AddDiscordInvite({
  Title       = "VonLib Hub | Community",
  Description = "Join our server!",
  Banner      = "rbxassetid://17382040552",
  Logo        = "rbxassetid://17382040552",
  Invite      = "https://discord.gg/your-invite",
  Members     = 470000,
  Online      = 20000,
})
```

---

## 🏁 Full Example

```lua
local Library = loadstring(game:HttpGet(
  "https://raw.githubusercontent.com/VoidDeveloper67/VonLib/refs/heads/main/main.luau"
))()

local Window = Library:MakeWindow({
  Title           = "VonLib Hub : Game",
  SubTitle        = "by von63rd",
  ScriptFolder    = "vonlib",
  BackgroundVideo = "https://files.catbox.moe/5f9loy.webm"
})

-- ── Topbar tags ─────────────────────────────────────────────────────────────
Window:Tag({ Title = "v1.3.0", Color = "Amber" })
Window:Tag({ Title = "Beta",   Color = "Purple" })

-- ── Stats HUD (draggable floating overlay) ──────────────────────────────────
local HUD = Library:MakeStatsHUD({
  Stats    = { "Ping", "FPS", "Playtime", "Time", "Memory", "Players", "Game" },
  Position = UDim2.new(0, 8, 0.5, 0),
})
-- Add a custom stat: local player walk speed
HUD:AddStat("Speed", function()
  local hum = game.Players.LocalPlayer.Character
    and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
  return hum and (math.floor(hum.WalkSpeed).." st/s") or "--"
end)

-- ── Minimizer ────────────────────────────────────────────────────────────────
local Minimizer = Window:NewMinimizer({ KeyCode = Enum.KeyCode.LeftControl })
Minimizer:CreateMobileMinimizer({
  Image  = "rbxassetid://101833678008843",
  Size   = UDim2.new(0, 35, 0, 35),
  Corner = { CornerRadius = UDim.new(0, 6) },
})

-- ── Tabs ─────────────────────────────────────────────────────────────────────
local MainTab   = Window:MakeTab({ Title = "Main",   Icon = "Home" })
local ConfigTab = Window:MakeTab({ Title = "Config", Icon = "Settings" })

-- ── Main Tab ─────────────────────────────────────────────────────────────────

MainTab:AddSection("Info")
MainTab:AddGradientLabel({
  Text   = "✦ VonLib v1.3.0",
  Colors = { Color3.fromRGB(120, 80, 255), Color3.fromRGB(80, 200, 255) },
})

MainTab:AddSection("Actions")
MainTab:AddButton({
  Name     = "Test Button",
  Badge    = "Hot",
  Callback = function()
    Window:Notify({ Title = "Clicked", Content = "Button pressed!", Duration = 3 })
  end
})

MainTab:AddSection("Toggles")
MainTab:AddToggle({
  Name     = "Auto Farm",
  Badge    = "New",
  Default  = false,
  Flag     = "auto_farm",
  Callback = function(v)
    Window:Notify({ Title = "Auto Farm", Content = tostring(v), Duration = 2 })
  end
})
MainTab:AddToggle({
  Name     = "ESP",
  Badge    = "Beta",
  Default  = false,
  Flag     = "esp_enabled",
  Callback = function(v) print("ESP:", v) end
})

MainTab:AddSection("Slider")
MainTab:AddSlider({
  Name      = "Walk Speed",
  Badge     = "Fixed",
  Min       = 16, Max = 200, Increment = 1, Default = 16,
  Flag      = "walk_speed",
  Callback  = function(v) print("Speed:", v) end
})

MainTab:AddSection("Keybind")
MainTab:AddKeybind({
  Name     = "Sprint Key",
  Default  = Enum.KeyCode.LeftShift,
  Flag     = "sprint_key",
  Callback = function(Key) print("Sprint:", Key.Name) end
})

MainTab:AddSection("Color Picker")
MainTab:AddColorPicker({
  Name     = "ESP Color",
  Badge    = "New",
  Default  = Color3.fromRGB(0, 180, 255),
  Flag     = "esp_color",
  Callback = function(c) print("Color:", c) end
})

MainTab:AddSection("Labels")
MainTab:AddLabel({ Text = "Status: Online", Color = Color3.fromRGB(80, 220, 80) })
MainTab:AddGradientLabel({
  Text     = "Premium Feature",
  Colors   = { Color3.fromRGB(255, 200, 0), Color3.fromRGB(255, 100, 0) },
  Rotation = 45
})

MainTab:AddSection("Dropdown")
MainTab:AddDropdown({
  Name     = "Select Fruit",
  Options  = { "Light", "Dough", "Leopard" },
  Default  = "Light",
  Flag     = "fruit_select",
  Callback = function(v) print("Fruit:", v) end
})

MainTab:AddSection("TextBox")
MainTab:AddTextBox({
  Name         = "Custom Text",
  Placeholder  = "type here...",
  ClearOnFocus = true,
  Callback     = function(v) print("Text:", v) end
})

MainTab:AddSection("Discord")
MainTab:AddDiscordInvite({
  Title       = "VoidHub",
  Description = "Best Server In the World.",
  Banner      = "rbxassetid://101833678008843",
  Logo        = "rbxassetid://101833678008843",
  Invite      = "https://discord.gg/Wsarxj9Gzz"
})

-- ── Config Tab ───────────────────────────────────────────────────────────────

ConfigTab:AddSection("UI Scale")
ConfigTab:AddSlider({
  Name      = "Scale",
  Min       = 0.6, Max = 1.6, Increment = 0.1, Default = 1,
  Callback  = function(v) Library:SetUIScale(v) end
})

ConfigTab:AddSection("Theme")
ConfigTab:AddDropdown({
  Name     = "Theme",
  Options  = Library:GetThemes(),
  Default  = Library:GetTheme().Name,
  Callback = function(v) Library:SetTheme(v) end
})

ConfigTab:AddSection("Stats HUD")
ConfigTab:AddToggle({
  Name     = "Show Stats HUD",
  Default  = true,
  Callback = function(v) HUD:SetVisible(v) end
})

ConfigTab:AddSection("Config Slots")
ConfigTab:AddButton({
  Name     = "Save Config",
  Badge    = "New",
  Callback = function()
    Window:SaveConfig("Default")
    Window:Notify({ Title = "Config", Content = "Saved!", Duration = 3 })
  end
})
ConfigTab:AddButton({
  Name     = "Load Config",
  Callback = function()
    local ok = Window:LoadConfig("Default")
    Window:Notify({ Title = "Config", Content = ok and "Loaded!" or "No config found", Duration = 3 })
  end
})
ConfigTab:AddButton({
  Name     = "Reset Config",
  Badge    = "Warning",
  Callback = function()
    Window:ResetConfig()
    Window:Notify({ Title = "Config", Content = "Reset to defaults", Duration = 3 })
  end
})

-- ── Startup notification ─────────────────────────────────────────────────────
Window:Notify({
  Title    = "VonLib Loaded",
  Content  = "v1.3.0 by von63rd  |  LeftCtrl to minimize  |  Stats HUD active",
  Image    = "rbxassetid://101833678008843",
  Duration = 5
})

Window:SelectTab(1)
```
