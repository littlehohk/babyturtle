# Baby Turtle / 紅耳龜寶寶 — BREST Browser Edition

**Play / 開始遊玩:** https://littlehohk.github.io/babyturtle/

An iPhone-friendly, bilingual (English / 繁體中文) browser adaptation of the private ESP32 [BREST](https://github.com/littlehohk/BREST) aquarium project. It uses a self-contained HTML5 canvas and JavaScript. No app installation, external JavaScript libraries, or build steps are required.

**Language switch:** Tap **繁體中文 / English** at the top at any time. The game does not reload or reset. The chosen language is remembered separately from the turtle's saved progress.

## English

### Tank and controls

The browser game uses a **28 × 17 × 10 cm** world. Its right-hand land platform now begins at **X = 22.4 cm**, occupying **20% of the tank width**, in both Top and Side views (the original ESP32 project used X = 18 cm). The platform is at the water surface; **water remains beneath it**. The turtle is approximately 3 cm long in the game.

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

網頁版水族箱大小為 **28 × 17 × 10 厘米**，陸地平台現由 **X = 22.4 厘米**開始，在俯視及側視模式均只佔水族箱**右側約五分之一的寬度**（原 ESP32 版本由 X = 18 厘米起）。平台與水面齊平，**平台下方仍有水**。遊戲中的龜寶寶身長約 3 厘米。

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

### Smaller limbs and strict visible-mouth feeding / 四肢縮小及嚴格嘴尖接觸判定

The four limbs now render at **50% of their previous linear dimensions**, equivalent to **approximately 25% of their former visible area**. Their bases remain embedded beneath the shell, and the alternating front/rear swimming motion is retained.

Feeding now requires the rendered mouth tip to meet the animated bloodworm itself: the browser checks both 3D mouth-to-worm proximity and pixel-level contact with the worm's curved body. A worm touching the turtle's shell, chest, or limbs does not count as eaten. The top-view heading was also corrected so the rendered mouth points in the same direction as the turtle's movement and collision calculations.

四肢現縮至先前的**一半長度與寬度**，顯示面積約為原來的**四分之一**。四肢根部仍藏於龜殼之下，並保留前後肢交替划水的動作。

餵食判定須同時符合立體空間中的嘴尖接近，以及畫面中**實際顯示的嘴尖碰到紅蟲身體**；龜殼、胸部或四肢碰到紅蟲不會算作進食。俯視模式的朝向亦已修正，使畫面中的嘴尖與烏龜的行進及碰撞方向一致。

### Revised four-limb attachment and stroke / 四肢連接及划水修訂

All four limbs now use a matching broad-shouldered, striped, webbed-paddle shape, with modest front/rear proportional differences. The roots are drawn beneath the shell, not as narrow joints. In top view, diagonal pairs alternate; the hind kicks lag slightly. In side view, far limbs render behind the body, while near limbs are clipped under the shell rim. Each stroke extends on the power phase and folds back during recovery. The artwork is a procedural interpretation of the supplied BREST swimming references.

四肢現採用相近的寬根部、有條紋及趾爪的划水形態，前後肢只保留適量比例差異。所有肢體均從龜殼下伸出，不再以幼細的連接點懸掛。俯視模式以對角肢體交替划水，後肢略為延遲；側視模式把遠側肢體放在身體後方，近側肢體根部則隱藏於龜殼邊緣下。推水時伸展、回收時屈曲。動畫按 BREST 參考圖片以程式推算。

## Visible, broad paddles when swimming toward the viewer / 正面及側面划水修正

**English:** The four swimming flippers in Side view have been redrawn **wider and thicker**, retaining green body striping and a mostly horizontal rowing direction. The flipper projection now accounts for the turtle's **toward/away viewing direction**, so a turtle swimming straight toward the player spreads the two front and two hind flippers **outward on both sides** instead of projecting them inward beneath the carapace. Front and rear swimming flippers occupy slightly different heights, making the pairs distinct when viewed head-on. The clipping that previously hid the upper portions of near-side swimming paddles has been removed; clipping still applies to the downward, Z-shaped legs used for walking on land. The same **1+3 / 2+4** diagonal stroke timing remains intact, and sleeping legs remain tucked.

**繁體中文：** 側視游泳的四肢現繪得**更寬、更厚**，並保留綠色條紋與主要橫向划水的方向。四肢投影現會考慮烏龜是**面向觀眾還是背向觀眾**；當牠正面游向玩家時，兩隻前腳及兩隻後腳均會**向身體兩側伸展**，不再反向縮進龜殼之下。前後肢的高度稍有分別，讓正面觀察時能分辨四肢。同時取消先前遮擋近側游泳腳掌上半部的裁切；陸上步行的向下 Z 字形四肢仍保留原有裁切。**1、3／2、4** 交替划水的節奏不變，睡眠時亦繼續收起四肢。

## Horizontal side-view swimming paddles / 側視橫向划水

**English:** While swimming in Side view, all four limbs now extend and paddle predominantly **left–right**, alongside the turtle's body, rather than pointing down. The movement preserves the established diagonal two-step rhythm: legs **1 and 3** move together while **2 and 4** move in the opposing phase. A small vertical variation and direction-dependent foreshortening help the movement look natural as the turtle turns. The upper limb roots stay embedded under the shell. **Walking on land is unchanged:** the legs still bend downward into the Z-shaped walking pose; during **sleep** they remain tucked beneath the shell.

**繁體中文：** 側視游泳時，四肢現主要沿身體兩旁**左右橫向伸展及划水**，不再向下垂。動作保留原有兩步對角節奏：**1、3** 同步，**2、4** 以相反相位划水。配合少量上下角度變化及轉向時的透視縮短效果，令動作較自然。四肢根部仍藏在龜殼下。**陸上步行不變**，四肢繼續以向下彎曲的 **Z 字形**走動；**睡覺時**則收起四肢。

## Side-view movement and recovery visibility / 側視活動與呼吸恢復顯示

**English:** In Side view, a conscious turtle now continues paddling while surfacing and makes subtle breathing/head and flipper movements during breathing recovery or short rests. Recovery periods after long dives can last **5 or 15 real minutes**, so the aquarium shows a localized **Breathing** label and approximate remaining minutes rather than appearing frozen. Brief **Resting**, **Surfacing for air**, and **Sleeping** labels distinguish temporary inactivity from a fault. The turtle chooses another roaming destination promptly when it reaches its previous target; it no longer waits unnecessarily for the old wandering timer. Genuine sleep remains stationary with closed eyes and tucked legs. No survival thresholds, breathing recovery durations, saved-game keys, or language-switch behaviour have changed.

**繁體中文：** 側視模式中，清醒的烏龜在上浮換氣時會繼續划水；呼吸恢復或短暫休息時亦有輕微的呼吸、頭部及四肢動作。長時間潛水後，呼吸恢復可能需要**實際五分鐘或十五分鐘**。畫面會顯示「**呼吸恢復中**」及約略剩餘分鐘數，並用「正在休息」、「正在上浮換氣」和「睡眠中」區分暫時靜止的原因。到達原來的遊動目標後，烏龜會較快選擇下一個目的地，不必無故等候舊的漫游計時。真正睡覺時仍保持閉眼及收起四肢的靜止睡姿。原有生存條件、呼吸恢復時長、遊戲存檔識別碼及語言切換方式不變。

## Sleeping posture and shell geometry / 睡姿與龜殼造型

**English:** During sleep, the turtle no longer paddles. All four legs fold close to the body, with only small tucked tips remaining visible. On land, the flat underside rests directly on the basking shelf; in water, the turtle settles to the aquarium floor and rests there. The head and neck relax slightly into the shell. The closed eyes retain a complete **circular green eye outline**, with a curved closed eyelid inside, following the supplied sleeping references. On waking, normal eye and movement drawings return. In Side view, the shell has a **rounded, domed upper carapace with a straight, flat lower edge and plastron**, rather than an oval bottom. Its length and earlier compact height are retained.

**繁體中文：** 睡覺時烏龜停止划水，四肢全部收近身體，只稍微露出蜷縮的腳尖。在陸地上，身體的平坦底部貼著平台；在水中則沉到水族箱底部平躺休息。頭頸亦稍微縮回龜殼。閉上的眼睛保留**完整圓形的綠色眼眶**，內有弧形閉合眼瞼，參照提供的睡眠圖片繪製。醒來後恢復正常張眼及活動姿態。側視的龜殼現採用**上方渾圓拱起、下緣與腹甲平直**的輪廓，不再採用下方也呈橢圓形的龜殼，並保留原來的長度及較薄的厚度。

## Sleeping eyes / 睡眠閉眼動畫

**English:** When the turtle sleeps, both eyes close in Top view and any visible eyes close in Side view, including three-quarter frontal poses. The white eye areas and pupils are replaced by green eyelids and curved closed-eye lines. Eyes reopen as soon as the turtle wakes. This visual change does not affect the existing sleep or care rules.

**繁體中文：** 烏龜進入睡眠狀態時，俯視模式中的雙眼會閉上；側視模式及面向觀眾的斜角姿態中，所有看得見的眼睛亦會閉上。原本白色的眼球與瞳孔會改為綠色眼瞼和弧形閉眼線條。醒來後眼睛立即重新張開。這項修改不影響原有的睡眠或飼養規則。

## Thinner shell in side view / 側視龜殼厚度調整

**English:** The shell and plastron are now **50% of their previous vertical thickness** in Side view only. Their horizontal length and the position of the lower shell rim are preserved. The head, neck and legs keep their existing proportions; Top view is unchanged.

**繁體中文：** 側視模式的龜殼及腹甲**垂直厚度縮減至原先的 50%**，但保留原有的水平長度及龜殼下緣位置。頭部、頸部和四肢的比例不變；俯視模式亦不受影響。

## Diagonal swimming and walking gait / 對角划水與步行動作

**English:** In the top-view reference, the limbs are numbered **1 = front-left, 2 = front-right, 3 = rear-right, 4 = rear-left** (with the turtle's head pointing upward). Swimming uses a repeating two-step diagonal gait: **step 1 — legs 1 and 3 swing forward while 2 and 4 swing backward; step 2 — the motions reverse**. Side view uses the same physical limb pairings, with near and far limbs changing visible layering as the turtle turns; the strokes follow its travel direction rather than merely flipping an animation.

Walking on land retains **the same 1+3 versus 2+4 alternating timing**. In top view, the leg drawings are shorter and tucked under the shell to imply bent joints. In side view, each walking leg changes from an extended paddle into a **Z-shaped shoulder–knee–foot articulation**, with a planted foot and alternating steps. Swimming retains the earlier compact limb proportions.

**繁體中文：** 按俯視參考圖片，烏龜頭部朝上時，四肢編號為 **1＝左前肢、2＝右前肢、3＝右後肢、4＝左後肢**。游泳採用重複兩步的對角划水節奏：**第一步：1、3 向前，2、4 向後；第二步：1、3 向後，2、4 向前**。側視模式沿用相同的肢體組合；烏龜轉向時，會相應調整遠近四肢的顯示層次，划水方向亦會配合行進方向。

在陸地步行時，**仍採用 1、3 與 2、4 交替的節奏**。俯視的四肢會縮短及收近龜殼，表現關節彎曲；側視的步行四肢則呈現明顯的 **Z 字形肩／髖部—膝／肘部—腳掌**構造，腳掌配合步伐落地。水中划水仍保留先前較小巧的四肢比例。

## Narrower platform and 3D side-view turning / 縮窄平台及立體側視轉向

**English:** Both views now show a **20%-width land area** on the right; the remaining 80% is open water. In Side view, the land shelf sits at the actual water surface, with water rendered below it. The turtle's body is raised above the shelf when walking or sleeping, so it remains visible. Side-view turns are no longer made by simply flattening the profile: a rounded dome with directional scute patterns, a distinct shell rim and plastron, visible far/near legs, and a neck/head that changes occlusion create approximate 45-degree front and rear views. The side-view mouth marker follows the same heading-dependent facial geometry, retaining strict mouth-only feeding. Saved turtles already on land are repositioned onto the narrower platform without resetting their age, health, or care history.

**繁體中文：** 俯視及側視模式的陸地均縮至**右側約 20% 的寬度**，其餘約 80% 為水域。側視的陸地平台改為正確顯示於水面高度，下面仍有水。烏龜在平台上行走或睡覺時，其身體會顯示在平台上方，不再被平台遮蔽。側視轉向亦不再單純把平面的側身圖像壓扁，而是利用立體龜殼弧度、隨朝向變化的龜甲紋理、龜殼邊緣和腹甲，以及遠近四肢和頭頸遮擋關係，近似呈現前方或後方四十五度的姿態。進食的嘴尖位置亦配合新的朝向投影。已儲存並在陸地上的烏龜會移至新平台位置，而不會重設年齡、健康或飼養進度。

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
