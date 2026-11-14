# Baby Turtle / 紅耳龜寶寶 — BREST Browser Edition

**Play / 開始遊玩:** https://littlehohk.github.io/babyturtle/

An iPhone-friendly, bilingual (English / 繁體中文) browser adaptation of the private ESP32 [BREST](https://github.com/littlehohk/BREST) aquarium project. It uses a self-contained HTML5 canvas and JavaScript. No app installation, external JavaScript libraries, or build steps are required.

**Language switch:** Tap **繁體中文 / English** at the top at any time. The game does not reload or reset. The chosen language is remembered separately from the turtle's saved progress.

## English

### Tank and controls

The original BREST world is **28 × 17 × 10 cm**. The right-hand land platform begins at X = 18 cm and is level with the water surface; water continues underneath it. The turtle is approximately 3 cm long in the game.

- **Top / Side** (bottom-left controls) show two projections of the same 3D space; switching view does not move the turtle.
- **Tap the water in Top view** to release a bloodworm at the surface. Worms sink at **0.08 cm/second** toward a randomly chosen mid-water level (5 cm) or bottom (0 cm). They disappear after 180 seconds if not eaten. The turtle pursues them and eats within reach; each eaten worm reduces hunger by 0.1.
- **Swipe** across the tank to play. Each swipe reduces boredom by 0.1.
- **Hold the tank for two seconds** to request sleep or wake; normal sleep is scheduled automatically.
- **AirCon / Heater** changes the water temperature, one at a time. Tap an active control again to turn it off.
- **Light** toggles illumination and counts as a play action, limited to one accepted toggle per second.
- **Settings** provides city search, outside weather, manual temperature fallback, health/age, and instructions.
- **After death**, hold the bottom control strip for two seconds to restart.

The browser edition uses **procedural vector-style turtle artwork** rather than copying the embedded, flash-indexed ESP32 sprite assets. It includes animated flippers, a shell, red ear patches, legs, swimming, walking, resting, and sleep indications.

### Health and survival

| Parameter | Current BREST rule |
| --- | --- |
| Starting condition | Health 10.0, Hunger 2.5, Boredom 2.5 |
| Natural needs | Hunger and boredom each increase 0.1 every 20 minutes |
| Eating | Hunger −0.1 per worm actually consumed |
| Playing | Boredom −0.1 per swipe or accepted light toggle |
| Need penalty | Hunger or boredom outside the inclusive 1.0–4.0 range: −0.1 Health/hour per condition |
| Temperature penalty | Water below 14°C or above 28°C: −0.2 Health/hour |
| Good daytime care | If awake, naturally daytime and all needs/water safe: +0.05 Health/hour, capped at +0.5 per local day |
| Daily sleep comparison | Completed local day: sleep longer than daylight → +0.1 Health; otherwise −0.3 |
| Death | Health reaches 0; or Hunger remains exactly 5 for more than one hour |

Health is clamped to **0–10**. There are no immediate boredom or temperature deaths. The first partial local day is not scored for the sleep comparison. A city change begins a new, unscored partial day.

### Water, breathing and sleep

Natural water target depends on the outside temperature:

| Outside temperature | Water target |
| --- | --- |
| Above 30°C | Outside −6°C |
| Above 22°C through 30°C | Outside −3°C |
| 16°C through 22°C | Outside |
| 8°C to below 16°C | Outside +3°C |
| Below 8°C | Outside +6°C |

AirCon cools **1°C per 30 minutes**, Heater warms **1.5°C per 30 minutes**, and with neither active the water approaches its natural target by **0.2°C every five minutes**.

An awake dive is limited to 20 minutes; a sleeping dive to three hours. The turtle automatically surfaces to breathe. At the surface, recovery is 5–10 seconds after dives up to 20 minutes, five minutes after dives over 20 through 60 minutes, or fifteen minutes after longer dives.

The turtle has up to three 15-minute daytime naps, separated by at least two waking hours when the local daylight period permits. After sunset and five continuous minutes with the lamp off, it attempts to reach the platform and sleep until sunrise.

### Browser operation

The game starts in **Sha Tin, Hong Kong**, and uses [Open-Meteo](https://open-meteo.com/) for city lookup and outside temperatures. When offline, set the outside temperature manually. Natural daylight follows the selected coordinates and sun position.

State is saved in the browser's **localStorage**, and time elapsed while the webpage is closed is applied when the player returns. The same browser uses a single save for both languages; separate devices or browsers do not synchronize. Clearing site data or using private browsing can erase progress.

The layout scales to fit an iPhone in portrait or landscape. In landscape orientation, secondary text is hidden to keep the full aquarium and its control strip visible.

## 繁體中文

### 遊戲簡介及操作

本遊戲改編自 ESP32 **BREST 紅耳龜寶寶**水族箱，適合在 iPhone 的 Brave、Safari，以及電腦瀏覽器遊玩。頂部的 **繁體中文 / English** 按鈕可隨時切換語言，不會重新載入遊戲或重設烏龜。

原作水族箱大小為 **28 × 17 × 10 厘米**。右側由 X = 18 厘米起設有與水面齊平的陸地平台，**平台下方仍有水**。遊戲中的龜寶寶身長約 3 厘米。

- **俯視 / 側視：** 查看同一立體水族箱的不同角度。切換視角不會改變烏龜位置。
- **俯視模式輕觸水面：** 投放一條紅蟲。紅蟲每秒下沉 0.08 厘米，隨機選擇停在半水深（5 厘米）或底部（0 厘米）；三分鐘內未被吃掉便消失。烏龜會追逐及吃掉紅蟲，每吃下一條，飢餓指數減少 0.1。
- **滑動：** 與烏龜玩耍，每次令無聊指數減少 0.1。
- **長按水族箱兩秒：** 要求睡覺或醒來；正常睡眠會自動安排。
- **冷氣 / 暖水器：** 調節水溫，兩者不可同時使用。再次按已啟動的按鈕便可關閉。
- **燈光：** 開關照明燈，亦計作玩耍；每秒最多接受一次切換。
- **設定：** 搜尋城市、取得戶外天氣、手動設定氣溫、查看健康及年齡，或閱讀玩法說明。
- **死亡後：** 按住底部控制區兩秒，重新開始飼養。

本網頁版以程式繪製烏龜的龜殼、頭部、紅色耳部斑紋及四肢，模擬游泳、走動、休息和睡眠；並非直接移植 ESP32 的像素圖資。

### 健康及生存規則

| 項目 | 規則 |
| --- | --- |
| 初始數值 | 健康 10.0、飢餓 2.5、無聊 2.5 |
| 自然變化 | 飢餓及無聊每 20 分鐘增加 0.1 |
| 餵食 | 每吃下一條紅蟲，飢餓指數減少 0.1 |
| 玩耍 | 每次滑動或有效切換燈光，無聊指數減少 0.1 |
| 需求懲罰 | 飢餓或無聊指數低於 1.0 或高於 4.0，每項每小時扣除 0.1 健康 |
| 水溫懲罰 | 低於 14°C 或高於 28°C，每小時扣除 0.2 健康 |
| 白天照顧良好 | 自然白天、清醒及各項指標安全時，每小時回復 0.05 健康，每日本地日最多回復 0.5 |
| 每日睡眠比較 | 完整本地日的睡眠時間長於日照時間，加 0.1 健康；否則扣除 0.3 |
| 死亡 | 健康降至 0；或飢餓指數維持在 5 超過一小時 |

健康指數限定在 **0 至 10**。無聊或水溫過界不會直接導致即時死亡，但會慢慢損害健康。新遊戲開始後的首個不完整本地日不會進行睡眠比較；更改城市後亦會重新計算。

### 水溫、呼吸及睡眠

自然水溫目標按戶外氣溫而定：高於 30°C 減 6°C；高於 22°C 至 30°C 減 3°C；16–22°C 與戶外相同；8°C 至低於 16°C 加 3°C；低於 8°C 加 6°C。

冷氣每 30 分鐘降溫 **1°C**，暖水器每 30 分鐘升溫 **1.5°C**；兩者關閉時，水溫每五分鐘以 **0.2°C** 的速度接近自然目標。

清醒潛水上限為 20 分鐘；睡眠潛水上限為三小時。烏龜會自動上浮呼吸。潛水最多二十分鐘，呼吸恢復需要 5–10 秒；超過二十分鐘至六十分鐘需五分鐘；超過六十分鐘需十五分鐘。

白天最多安排三次每次十五分鐘的小睡，兩次之間至少清醒兩小時（實際安排視日照長短而定）。日落後如持續關燈五分鐘，烏龜會嘗試返回陸地平台，睡至日出。

### 天氣、存檔及手機顯示

預設城市為**香港沙田**，可透過 [Open-Meteo](https://open-meteo.com/) 搜尋其他城市及取得戶外氣溫。離線時可手動設定。日夜明暗按所選城市的太陽位置模擬。

遊戲以瀏覽器 **localStorage** 自動儲存，並按現實時間計算關閉網頁期間的年齡和需求。中英文共用同一存檔，不同瀏覽器、裝置之間不會自動同步。清除網站資料或使用私密瀏覽可能令進度遺失。

支援 iPhone 直向及橫向顯示。橫向時會隱藏次要文字，確保整個水族箱及底部控制區完整顯示。

## Animation and feeding update / 動畫及餵食改良

The current browser version adds five visual and interaction refinements based on the BREST reference images:

1. **Mouth-first feeding:** eating occurs only when the modelled **tip of the mouth** reaches within 0.28 cm of the bloodworm in 3D. Touching the shell, legs, or another body part does not count. The neck/mouth shows a brief feeding motion near the worm.
2. **Top-view face:** the turtle now has two large eyes and two red cheek/ear patches near its chin, which remain visible as it changes heading.
3. **Swimming and walking paddles:** four articulated limbs have opposing, phase-offset strokes with extended front flippers and smaller rear kicks, guided by the source swimming and walking pose references.
4. **Directional side view:** the turtle turns toward the left, right, near camera or far wall using interpolated foreshortening and swimming pitch; these intermediate poses are procedurally inferred where no exact reference frame exists.
5. **Depth cues:** the turtle appears smaller when diving toward the bottom in top view, and when swimming away toward the far glass in side view. Its physical position and game rules do not change when switching view.

These effects remain **procedural approximations** of the original BREST art rather than exact reproductions of every ESP32 sprite.

## 動畫及餵食更新

現行網頁版根據 BREST 儲存庫內的烏龜參考圖片，作出五項修改：

1. **只有嘴尖接觸紅蟲才可進食：** 必須在立體空間中讓嘴尖距離紅蟲不超過 0.28 厘米，龜殼或四肢碰到紅蟲不會當作已吃下。
2. **俯視造型：** 頭部有兩隻大眼睛，面頰／下巴附近各有一處紅色斑紋；轉向時仍可看見。
3. **四肢游泳動畫：** 前肢交替伸展及划水，後肢配合較小幅度的踢水，陸上走動與水中游泳採用不同節奏。
4. **側視轉向：** 向左、右、靠近或遠離觀看者，以及上浮或下潛時，會調節姿勢與透視比例。沒有原作參考畫面的中間角度，以程序動畫推算。
5. **深度效果：** 俯視模式中，潛向水底時烏龜會變小；側視模式中，游向遠處玻璃時亦會縮小，令畫面更有立體感。

這些圖像是根據原作風格製作的**程序化近似動畫**，並非逐格直接複製 ESP32 圖像。

## Deploy with GitHub Pages / GitHub Pages 發佈

1. Open repository **Settings → Pages**.
2. Choose **Deploy from a branch**.
3. Select **main** and **/(root)**, then save.
4. Open **https://littlehohk.github.io/babyturtle/** after deployment.

## Repository contents / 儲存庫檔案

- `index.html` — the complete bilingual iPhone browser game / 中英雙語 iPhone 網頁遊戲
- `readme.md` — this bilingual guide / 本中英雙語說明

**Implementation note:** This is a browser **adaptation**, not a byte-for-byte port of the ESP32 firmware. In particular, the original flash-indexed turtle sprite frames, hardware Wi-Fi setup, NVS records, and event-by-event precision of its long offline replay are not copied. The HTML version implements the central rules and a simplified procedural simulation. JavaScript syntax was checked, but actual iPhone testing remains outstanding.

**說明：** 本版本是網頁**改編版**，並非逐行移植 ESP32 程式；原有像素圖、硬體 Wi-Fi 設定及 NVS 儲存均未直接複製。網頁重現主要規則，採用較簡化的動畫及離線時間計算。已檢查 JavaScript 語法，但尚未在實體 iPhone 上全面測試。

*The game mechanics do not constitute real animal-care advice.／遊戲規則並非真實動物飼養建議。*
