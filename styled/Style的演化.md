**[Build a better UI component library with Styled System](https://www.slideshare.net/cythilya/build-a-better-ui-component-library-with-styled-system-199358990)** from **[Hsin-Hao Tang](https://www.slideshare.net/cythilya)**

投影片[連結](https://www.slideshare.net/cythilya/build-a-better-ui-component-library-with-styled-system-199358990)、[下載](https://cythilya.github.io/assets/css/build-a-better-ui-component-library-with-styled-system.pdf)。

------

本文分為兩部份，第一部份從 CSS 歷史上所用過的元件化的方法談起，舉凡這些方法如何做元件化、解決什麼問題和沒有解決什麼問題；第二部份來看目前我所推薦的解法，也就是利用 Styled Components 和 Styled System 來建立元件庫，並且來看這樣的解法好在哪裡。

## Everything in CSS is global

CSS 只有 global 而沒有 local 的概念，也就是說，只要當前頁面的 DOM 結構符合載入樣式的規則，就會被套用這個樣式。這會產生兩個問題 - 命名衝突（name collision）、重用性（reusability）。

### 命名衝突（Name Collisions）

命名衝突是指因 CSS 樣式規則同名所造成樣式覆蓋的問題。如下圖所示，這裡分別有兩個元件 slideshow（左）和 tabs（右），它們各自擁有 `.list` 和 `.list .item` 的樣式，若再載入了一個同樣由 `.list` 和 `.list .item` 所撰寫或權重更高的樣式規則，就會把這個樣式套用上去，原先屬於這兩個元件的樣式就被覆蓋掉了，這就是命名衝突所造成的狀況。

![命名衝突 Name Collisions](https://cythilya.github.io/assets/css/name-collisions.png)

### 重用性（Reusability）

由於 CSS 不是程式語言，它只是樣式描述的方法，因此在撰寫上很鬆散，沒有模組的概念，難以重用。若無法重用，就會造成程式碼毫無限制的成長，終至難以擴充和維護。

------

稍後介紹的解法，主要都是解決「命名衝突」和「重用性」這兩個問題。

------

## CSS Methodologies

以下來一一介紹歷史上我們用過的一些解法。

- [OOCSS (Object-Oriented CSS)](http://oocss.org/)
- [BEM (Block, Element, Modifier)](http://getbem.com/)
- [SMACSS (Scalable and Modular Architecture for CSS)](http://smacss.com/)
- [CSS Modules](https://github.com/css-modules/css-modules)
- [CSS in JS](https://www.ruanyifeng.com/blog/2017/04/css_in_js.html)

這些都是撰寫 CSS 的方法，讓前端工程師能試圖將 CSS 樣式規則切分成獨立模組來開發，而不是撰寫一大堆不可分割的程式碼。

## OOCSS (Object-Oriented CSS)

第一個要介紹的是 OOCSS，OOCSS 只能用 class 來撰寫樣式，並且有兩個主要規則

- 結構與樣式分離（Separation of Structure from Skin）
- 內容與容器分離（Separation of Container and Content）

### 結構與樣式分離（Separation of Structure from Skin）

我們在切版的時候，幾乎都是先將網頁上的元素依照 [Box Model](https://www.w3schools.com/css/css_boxmodel.asp) 排列好，然後再填入色彩。

我們可以想像是在打造一棟房子，首先先建立房子的骨架，接著再為房子上油漆-排列這件事就是打造房子的骨架或結構，填入色彩就是加入呈現的樣式（建築物範例參考自[這裏](https://wcc723.github.io/css/2016/12/02/oocss-one/)）。

![結構與樣式分離 Separation of Structure from Skin](https://cythilya.github.io/assets/css/separation-of-structure-from-skin.png)

因此，我們可以將 CSS 指令分為兩種

- 第一種是結構，與排列元素相關，例如：margin、padding、display、position、vertical-align、width、height 等。
- 第二種是樣式，與色彩呈現相關，例如：color、background、opacity、font-size 等。

將結構與樣式結合，就是一個完整的元件。這樣做的好處是同樣的結構能替換不同的樣式，同樣的，同一樣式也能套用在不同結構上。

下面來看一個例子。

### Example 1

這裡有三個按鈕，分別是小的黃色按鈕、大的紅色按鈕、小的藍色按鈕，下方各自對應未優化的 CSS 樣式。

![結構與樣式分離 Separation of Structure from Skin](https://cythilya.github.io/assets/css/separation-of-structure-from-skin-example-1.png)

若需求改變

- 修改黃色按鈕是大的，紅色按鈕是小的。
- 新增一個小的綠色按鈕。

這樣撰寫的方式就很難修改、無法被重用，要重新刻一個很類似的按鈕。

經過「[結構與樣式分離](https://cythilya.github.io/2019/11/30/build-a-better-ui-component-library-with-styled-system/#/#結構與樣式分離separation-of-structure-from-skin)」的拆解後，我們將結構相關的 CSS 指令放在 button、small、large 這三個 class 裡面，與樣式相關的就分別放在 yellow、pink、blue、green 裡面，這樣之後就可以任意組合 button 的大小、間隔和顏色。

![結構與樣式分離 Separation of Structure from Skin](https://cythilya.github.io/assets/css/separation-of-structure-from-skin-example-2.png)

如下圖，剛剛提到的需求變更，就可以輕鬆組合出來，不用從頭到尾再刻一次相似的程式碼，提高了重用性。

![結構與樣式分離 Separation of Structure from Skin](https://cythilya.github.io/assets/css/separation-of-structure-from-skin-example-3.png)

### 內容與容器分離（Separation of Container and Content）

依舊以建築物為例，假設建築物本身是容器，門、窗等細節是內容，那麼這些細節應該要可以裝在各個建築物上（建築物範例參考自[這裏](https://wcc723.github.io/css/2016/12/02/oocss-one/)）。

![內容與容器分離 Separation of Container and Content](https://cythilya.github.io/assets/css/separation-of-container-and-content.png)

這樣內容就不會被限制於只能在哪個容器之內，元件就能更容易被組裝，提高重用性。

下面再看一個例子。

### Example 2

![內容與容器分離 Separation of Container and Content](https://cythilya.github.io/assets/css/separation-of-container-and-content-example.png)

一樣以按鈕（button）為例，不同的是外面包了一層表單（form）…

```
.form { ... }
.form .button { ... }
.form .button.yellow { ... }
.form .button.pink { ... }
.form .button.blue { ... }
```

上面這樣的寫法限制 button 只能在 form 裡面，改進如下。

```
.form { ... }
.list { ... }
.button { ... }
.button.yellow { ... }
.button.pink { ... }
.button.blue { ... }
```

改進後的寫法 button 沒有被包裹的容器限制，就可以有彈性的放在 form 或 list 裡面，這樣的組合方式更有彈性，更好重用。

以 OOCSS 為核心概念而開發的著名的框架即是 [Bootstrap](https://getbootstrap.com/)。

![Bootstrap](https://cythilya.github.io/assets/css/bootstrap.png)

------

總結，OOCSS 用這兩個原則「[結構與樣式分離](https://cythilya.github.io/2019/11/30/build-a-better-ui-component-library-with-styled-system/#結構與樣式分離separation-of-structure-from-skin)」、「[內容與容器分離](https://cythilya.github.io/2019/11/30/build-a-better-ui-component-library-with-styled-system/#內容與容器分離separation-of-container-and-content)」拆分 CSS 指令，讓樣式能利用 class 來分裝，使其便於組裝元件，增加重用性，但可惜這並沒有解決到命名衝突的問題。

------

## BEM (Block, Element, Modifier)

第二個要來看 BEM，BEM 將頁面元件分為三種類型

- B（Block）：區塊，像是 header、sidebar、container、footer。
- E（Element）區塊的一小部份，是可重用的元件。
- M（Modifier）：區塊或元件的狀態，右下角閃亮亮的部份即是這個元件被 highlight 的狀態。

![BEM (Block, Element, Modifier)](https://cythilya.github.io/assets/css/bem.png)

### Example 3

我們來看一個例子，如下圖所示

- 這裡有一群 card，class 的名稱就取名為 `.card-list`，單字之間可用一個 dash 分隔。
- 裡面元件稱為的 item，由於是 `.card-list` 的元件，因此用 **雙底線** 分隔，表示是誰的元件。
- 右下角這個被 highlight 元件，是為 item 的 highlight 狀態，因此用 **雙 dash** 分隔。

![BEM (Block, Element, Modifier)](https://cythilya.github.io/assets/css/bem-example.png)

------

由於 BEM 命名規則很特殊，因此主要有三個優點

- 一看就知道每個 class 的意思，誰是區塊、誰是屬於誰的元件、是誰的狀態，清楚明瞭，有助重用。
- 相較 OOCSS 來說，比較難同名，因此比較難發生命名衝突的問題。只是比較難，其實並沒有解決喔 `( ￣ 3￣)y▂ξ`
- 它不是用階層來表示誰的誰，而是用一層 class name 加上符號做區隔，層級少，瀏覽器在樣式計算階段的查找配對次數少，所以對瀏覽器的渲染效能（rendering performance）是比較好的，可參考[這裡](https://cythilya.github.io/2018/07/13/critical-rendering-path/#方法二簡化-css-selector-的複雜度)。

因為 BEM 的命名規則而導致名稱過長，可能會造成 HTML 變得很大包，記得做 [GZIP、Brotli](https://cythilya.github.io/2018/09/05/text-content/#壓縮文字資源)。

------

## SMACSS (Scalable and Modular Architecture for CSS )

隨著網頁發展愈來愈精美，要處理的細節也愈來愈多，那麼就可以使用 SMACSS。

它也是靠命名規則來解決命名衝突和重用性的問題，其規則是將網頁的元素分為五種類型

- Base：網頁基本樣式，例如：reset、全站共用樣式。
- Layout：排版相關，例如：header、sidebar、container、footer。
- Module：可重用的元件。
- State：元件的狀態。
- Theme：主題色，可用在 Layout 或 Module 上。

![SMACSS, Scalable and Modular Architecture for CSS](https://cythilya.github.io/assets/css/smacss.png)

### Example 4

範例如下圖所示

- Base：標籤的樣式。
- Layout：排版相關的樣式，以 `l` 或 `layout` 開頭，例如：`.l-sidebar`、`.l-container`。
- Module：可重用的元件，以 `mod` 開頭，例如 `.mod-card`。
- State：元件的狀態，以 `is` 開頭，例如：`.is-hightlight`。
- Theme：主題色，以 `theme` 開頭，`.theme-l-sidebar`、`.theme-card-dark`。

![SMACSS, Scalable and Modular Architecture for CSS](https://cythilya.github.io/assets/css/smacss-example.png)

------

OOCSS、BEM 與 SMACSS 皆是利用命名規則的方式，讓樣式能更模組化、更好重用。雖然命名規則能稍微避免一些命名衝突的問題，但仍無法徹底解決，所以每次在寫樣式時，前端工程師都不得不先將整個專案搜尋一下，或是在當前引用的 CSS 檔案找找有沒有同名的樣式規則，避免覆蓋，真的很辛苦 `(〒︿〒)`

------

## CSS Modules

拜工具所賜，我們可以利用 Webpack 設定 css-loader（[點此查看](https://webpack.js.org/loaders/css-loader/)），針對引用進來的 class name 轉成 hash，成為唯一的名稱，這樣就能將 CSS 樣式規則限制在特定元件底下，完全避免同名覆蓋的問題。

範例如下圖，左邊是之前我們的寫法，很容易重名；右邊是將 class name 轉為 hash，成為唯一的名稱，就不會同名了。

![CSS Modules](https://cythilya.github.io/assets/css/css-modules.png)

CSS Modules 所產生的 hash 的 class 層級和原本一樣多，所以 rendering performance 不如下面 CSS in JS 只有一層來得好。

------

以概念上來說，CSS Modules 想做的就是企圖把樣式鎖在特定的元件裡面。

------

## CSS in JS

![CSS in JS](https://cythilya.github.io/assets/css/css-in-js.png)

既然想把樣式限制在元件裡面，那直接寫在元件裡面如何？也由於 [SPA](https://blog.techbridge.cc/2017/09/16/frontend-backend-mvc/) 架構的盛行，前端程式碼以元件來構成頁面，因此以元件為單位來撰寫樣式就很直覺，將樣式寫在元件裡面，就是 CSS in JS。

## Styled Components

用 CSS in JS 這樣的概念開發的函式庫很多，這裡就以 [Styled Components](https://www.styled-components.com/) 為例。

![Styled System](https://cythilya.github.io/assets/css/styled-components.png)

利用 Styled Component 建立一個元件，樣式就寫在裡面。

```
import styled from 'styled-components';

const Button = styled.button`
  margin: 0 10px;
  padding: 10px;
  background: #fefefe;
  border-radius: 3px;
  border: 1px solid #ccc;
  color: #525252;
`;
```

使用 CSS in JS 的好處是

- 如同 [CSS Modules](https://cythilya.github.io/2019/11/30/build-a-better-ui-component-library-with-styled-system/#css-modules)，由於元件的樣式只會被轉為 hash 的 class name，是唯一的名稱，因此沒有命名衝突的問題。
- JS 與 CSS 都在同一支 JS 檔案，能共享變數來做邏輯判斷，不用間接新增/移除某個 class 來控制樣式。
- 每個元件只管自己的樣式，只要寫了都是有用的，不會有冗余、廢棄的程式碼，也方便重構。
- 由於樣式和元件合併了，因此只會載入要用到的 CSS 程式碼，不相關的都不會載入，增進瀏覽器的載入效能。

------

總結 CSS Modules 和 CSS in JS 到底解決了什麼問題？

- 由於 CSS Modules 和 CSS in JS 將 class name 做 hash 而為唯一的名稱，因此解決了**命名衝突**的問題。
- 傳統寫法容易造成**深層巢狀**的問題，而深層巢狀會有**權重覆蓋**和瀏覽器**渲染效能**的問題。由於 CSS in JS 只會產生一個 hash 的 class name，單一一層而已，成功解決這兩個問題。
- **重構**變得比較簡單，因為要重構這個元件的樣式，就只要找到這個元件就好了，而不是像傳統撰寫的 CSS 方式必須做全站或引入檔案的搜尋比對。

But！最重要的就是這個 But！

我們還有些問題尚未解決，像是…

- 由於元件庫的元件是由不同人或不同時間所開發的，**元件的屬性名稱可能會不一致**，這在後續維護上很令人困惑。
- 將樣式寫在元件裡面，要怎麼制定**主題樣式**？例如：切換 light 或 dark 模式？SMACC 用 theme 作為 prefix 的命名規則來做，那現在要怎麼辦呢？
- 將樣式寫在元件裡面，要怎麼做 **RWD**？
- 可以**不要一直重複撰寫同樣的程式碼**嗎？例如：media query `@media (...) { ... }`
- 元件的樣式會依據傳入的屬性值來做調整，關於這部份對應的程式碼實在很繁瑣，有**更簡潔**的寫法嗎？

------

## 利用 Styled System 建立一個更好的 UI 元件庫

這時候我們就可以用 [Styled System](https://styled-system.com/) 來幫我們做些改進了。Styled System 是一個搜集了許多 utility function 的 library，主要用來幫我們處理樣式，以下依照它的優點來看一些範例，了解到底要怎麼用它來改進我們的元件庫。

### 加快實作速度

由於 Styled System 提供許多便利的 utility function 來幫助我們更簡易的語法的撰寫樣式，因此我們能很快實作和修改程式碼。

#### Utility Functions

這裡有一個 Box 元件，它設定字體顏色為白色、背景為蕃茄紅色。

![Styled System](https://cythilya.github.io/assets/css/styled-system-example-1.png)

幾乎所有 CSS-in-JS 函式庫在建立元件時，都可接受函式（function）作為參數、並代入 props 來動態決定樣式，Styled Components 也不例外。

如下，color 與 background 的值是 props 傳入的，我們會在 styled component 取出單獨的值來一個個做設定。

```
const Box = styled.div`
  margin: 15px 0;
  padding: 15px;
  color: ${(props) => props.color};
  background: ${(props) => props.bg};
  border-radius: 10px;
`;
```

這樣很麻煩，所以自行實作一個函式 getStyles 來做這件事。

```
const getStyles = ({ color, bg }) => ({
  color,
  background: bg,
});

const Box = styled.div`
  ${getColor};
  margin: 15px 0;
  padding: 15px;
  border-radius: 10px;
`;
```

Styled System 提供 [color](https://styled-system.com/api#color) 這個 utility function，可達到相同的功能，可以想像 color 這個 utility function 挖了一個更大的洞一起填入 color 和 background 並幫我們做了一些繁瑣的 mapping 工作，對開發來說就便利許多。

```
import { color } from 'styled-system';

const Box = styled.div`
  ${color}
  margin: 15px 0;
  padding: 15px;
  border-radius: 10px;
`;
```

Styled System 針不同特性的樣式而也有許多的 utility function 可用，可到它的官網查詢，[點此查看](https://styled-system.com/api/)。

### 不一致

這部份我們會來看「主題樣式」和「不一致的屬性名稱」兩個議題。

#### 主題樣式

如何制定全域主題樣式呢？每個元件單獨設定是很麻煩的事情。

我們可以定義一個檔案，裡面放全站主題樣式的物件，在 root 放置 ThemeProvider，並將將這個物件傳給 ThemeProvider，ThemeProvider 會利用 [React Context](https://zh-hant.reactjs.org/docs/context.html) 來傳遞樣式的設定到後續所有的元件，這樣所有的元件就都可以取用這個主題物件所定義的設定了，不用一個個元件來做引用和設定。

以下是先準備好的 theme 物件，定義了背景色 bg 和字體的顏色 color。

```
const theme = {
  color: {
    white: '#fefefe',
  },
  bg: {
    tomato: 'tomato',
  },
};
```

當元件設定的值 color 或 bg 可在 theme 物件找到時，就會自動 mapping 並使用，達到定義全域主題樣式的目的，不用每個元件單獨設定，易於維持全站樣式的一致性。

```
<ThemeProvider theme={theme}>
  <Box color='white' bg='tomato' />
</ThemeProvider>
```

![Styled System](https://cythilya.github.io/assets/css/styled-system-example-2.png)

#### 定義元件的個別樣式

可在主題物件內定義元件個別的樣式，只要在 variant 指定查找的 key 即可。

```
<button variant="danger" size="large" />
const buttonStyle = variant({ key: 'buttons' });
const buttonSizeStyle = variant({ prop: 'size', key: 'buttons.size' });

const Button = styled.div`
  ${buttonStyle}
  ${buttonSizeStyle}
  padding: 15px;
`;
const theme = {
  buttons: {
    danger: {
      color: 'white',
      background: '#f25e7a',
    },
    size: {
      default: { height: 50 },
      large: { height: 100 },
    },
  },
};
```

#### Variants

除了全域設定 theme object 外，單一元件也可以利用 [variants](https://styled-system.com/variants/) 來做個別設定。

如下，我們為元件 Box 定義了兩種類別 primary 和 secondary。

```
import { variant } from 'styled-system';

const Box = styled('div')(
  variant({
    variants: {
      primary: { color: 'black', bg: 'tomato' },
      secondary: { color: 'black', bg: 'yellow' },
    },
  }),
);
```

元件在使用時，只要利用 variant 去指定要哪一種就可以了，就會自動對照是要用 primary 或 secondary 的字體顏色 color 和背景顏色 bg，其中 color 和 bg 的值也是去查找全域定義的 theme 物件。

![Styled System](https://cythilya.github.io/assets/css/styled-system-example-3.png)

#### 不一致的屬性名稱

由於元件的開發是不同人不同時間撰寫的，風格難以統一，屬性命名都不相同，後續可能會造成接手人員感到困惑、難以維護。雖然這可以透過文件（例如：[Storybook](https://storybook.js.org/)）或 linter 來規範，還有其他方法嗎？

![不一致的屬性名稱](https://cythilya.github.io/assets/css/inconsistent-props-and-behaviours.png)

上圖有兩個元件 `<Button>` 和 `<Label>`，它們都會設定字體的顏色，可能由於開發人員或時間的不同，而有 color 和 fontColor 兩種，props 的名稱不同卻是要做一樣的事情，這很讓人困惑。由於 Styled System 提供了 API 來做樣式的對應設定，就可以強迫開發人員一定要用一樣的名稱來定義屬性-統一為 color。

### Mobile-First

Styled System 提供簡易的 array syntax 語法來針對不同 breakpoint 設定各自的樣式，[點此查看](https://styled-system.com/responsive-styles)。

#### Responsive styles

Styled System 可用陣列傳入針對個別 breakpoint 所需要設定的值，預設的 breakpoint 是 40em、52em、64em。

一般傳統的寫法，針對每個 breakpoint 寫各自的樣式。

```
.thing {
  font-size: 16px;
  width: 100%;
}

@media screen and (min-width: 40em) {
  font-size: 20px;
  width: 50%;
}

@media screen and (min-width: 52em) {
  font-size: 24px;
}
```

以下改為 Styled System 的寫法，在設定好 breakpoint 後，只要用陣列傳入相對應的數值即可，好處是少寫一些程式碼，也清楚明瞭。

```
<Thing fontSize={[ 16, 20, 24 ]} width={[1, 1/2]} />
```

------

以上…我推薦 React + Styled Components + Styled System 是目前建立元件庫的最好解法。

![React + Styled Components + Styled System](https://cythilya.github.io/assets/css/best-solution.png)

------

## QnA

既然這東西這麼好，那我們可以直接放到專案裡面了嗎？

### Traditional CSS rules feat. Styled Components

我們首先遇到的第一個問題，就會是新舊規則並存的問題。我們無法一次就改完全站的樣式，勢必會有過渡時期，新舊並存，也就是傳統 CSS 樣式的寫法與 Styled Components 並存。如果遇到以下這種壓不過的狀況，就只能用 `!important` 來處理。

如下，第一條的分數是 101 分，第二條是 styled component 產出的樣式，會用單一層 hash class name 來包裝，得到 10 分，第一條壓過第二條。

```
/* in site.css, score: 100 + 1 = 101 */
#root div {
  color: red;
}

/* in styled component, score: 10 */
.jqouBD {
  color: black;
}
```

解法只能用 `!important` 來處理，得到一萬分！

```
/* in site.css, score: 100 + 1 = 101 */
#root div {
  color: red;
}

/* in styled component, score: 10000 */
.jqouBD {
  color: black !important;
}
```

### 亂碼的 class name 要怎麼做測試？

要怎麼產生一個不是 hash 的 class name 來做 end-to-end 測試？解法是用 Styled Components 的 attrs 加上 props「className」即可。

![亂碼的 class name 要怎麼做測試？](https://cythilya.github.io/assets/css/class-name-without-a-hash-for-end-to-end-testing.png)

## Demo

[Simple Example](http://bit.ly/35x16cL)

<iframe src="https://codesandbox.io/embed/styled-system-example-xr3jl?fontsize=14&amp;hidenavigation=1&amp;theme=dark" title="Styled System Example" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin" data-darkreader-inline-border-top="" data-darkreader-inline-border-right="" data-darkreader-inline-border-bottom="" data-darkreader-inline-border-left="" style="box-sizing: border-box; border: 0px; color: rgb(169, 159, 142); font-family: &quot;Microsoft JhengHei&quot;, &quot;PT Sans&quot;, Helvetica, Arial, sans-serif; font-size: 20px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(24, 23, 22); text-decoration-style: initial; text-decoration-color: initial; width: 703.333px; height: 500px; border-radius: 4px; overflow: hidden; --darkreader-inline-border-top: initial; --darkreader-inline-border-right: initial; --darkreader-inline-border-bottom: initial; --darkreader-inline-border-left: initial;"></iframe>



## References

以下是一些我閱讀和參考的資料，大家有興趣可以來看看。

- [Styled System](https://styled-system.com/)
- [We need a better UI component library - Styled System](http://bit.ly/2roJEsg)
- [The Three Tenets of Styled System](http://bit.ly/35ygrcV)
- [Styled System: Pseudo selectors in Variant](http://bit.ly/35yqGhy)
- [How to create responsive UI with styled-components](http://bit.ly/34lhxJ3)
- [CSS 實戰心法](http://bit.ly/2shXUn4)

------

## 後記

本投影片與講稿是參加今年 Modern Web 之 Anna Su 所分享的 [We need a better UI component library - Styled System](http://bit.ly/2roJEsg) 後，重新製作後分享給我們家團隊的，由於是面對非前端工程師，所以內容稍微簡單、以說明觀念居多 σ`∀´)σ