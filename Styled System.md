[toc]



### -前言

不知道大家有沒有發現，在元件化的時代，無論你是使用 React/Vue.js/Angular，在不同專案裡可能一直重複做同樣功能的元件，只有樣式需要被改變，例如 Button、Checkbox、Radio button、Modal、Carousel 等，我們觀察到大部分元件的行為是一致的，是否能夠彈性的變更樣式？要如何讓我們開發專案更有效率？

隨著時代的演進，各種 UI library 一定會有好與不好的地方，我們來回顧過往的 CSS 技術，希望可以從中學習各自的優良部份。

------

#### UI library 的迭代演化與優缺點

#### CSS Framework / UI Library

觀察早期的 CSS framework（例如：Bootstrap、Pure.css）可以直接在 mockup 加上 class 名稱，修改樣式和改變 DOM 結構是非常直覺的，但是缺少元件化，無法快速完成功能。

#### UI Component Library

元件化的 UI Library（例如：Material-UI、Ant Design）可以快速完成功能，但是在使用元件時，並不知道元件有哪些 DOM？ 這些 DOM 對應的 class 名稱是什麼? 必須找到對應的 class 名稱，再覆蓋樣式，修改樣式會變得很麻煩。

使用現成的 Library，可以快速開發，但是無法因應特殊需求，反而提高專案維護的困難度，所以客製化元件成為許多企業選擇的解決方案。

------

透過 `React` 搭配 `styled-components`，整理幾個基本範例 (可參考 [簡報內的範例](https://speakerdeck.com/annasu/we-need-a-better-ui-component-library-styled-system?slide=34) )。

將客製化元件時所遇到的問題整理如下：

### -客製化元件所遭遇的困難

#### props 命名

使用元件時，傳遞 props 名稱不一致，容易造成開發者的困惑。

#### 客製化樣式需傳入對應的 props，程式碼很繁冗

假如需要開發樣式非常彈性的元件，搭配 styled-components 寫 function 傳入 props 去設定特定樣式，會寫很多重複的程式碼。

#### 不同狀態顯示不同樣式時，程式碼需要封裝

如果我們必須根據不同狀態顯示不同樣式，為了讓程式碼更簡潔，可能需要寫一個 higher order function，傳入樣式的 object 和當前的狀態，使用元件時，再透過這個 function 定義樣式。

當元件的樣式更複雜時，例如按鈕有不同特定樣式（Default/Primary/Danger）和尺寸（Default/Small/Large），你可以試著思考看看，寫起來是否會變得非常麻煩？

#### RWD 元件開發複雜

你可能需要寫下面這樣的程式碼，根據不同設備重新再寫一次樣式做修改。

```react
const Article = styled.div`
  width: 100%;
  …

  @media (min-width: 567px) {
    width: 50%;
    …
  }

  @media (min-width: 768px) {
    width: 25%;
    …
  }
`;
```

綜合以上 UI Library 的優缺點：

- CSS Framework / UI Library => 缺乏元件化
- UI Component Library => 較難客製化樣式
- 客製化元件 => 開發過程會遇到一些問題

所以我們希望有一套 UI component Library 可以同時具有元件化的方便性，又能夠達成客製化的需求，幫助我們建立更有彈性的元件。

------

### -Styled System 介紹

[styled-system](https://github.com/styled-system/styled-system) 是開源專案，2017 Release [第 1 版](https://github.com/styled-system/styled-system/releases/tag/v1.0.0)，到 2018 的 [第 2 版](https://github.com/styled-system/styled-system/releases/tag/v2.0.0) 比較穩定，2019 6 月出 [第 5 版](https://github.com/styled-system/styled-system/releases/tag/v2.0.0)，作者 [Brent Jackson](https://twitter.com/jxnblk) 同時也是 GatsbyJS 的貢獻者。

將簡報提到的基本案例，[重新透過 Styled System 改寫](https://speakerdeck.com/annasu/we-need-a-better-ui-component-library-styled-system?slide=68) 之後，可以發現：

- 大幅降低 props 命名或格式不一致的問題
- 更好的抽象化樣式，程式碼變得更少
- 根據不同狀態，更彈性的修改對應樣式
- 靈活的處理 RWD 樣式
- 透過 theme 快速變更主題樣式

我們可以直接在元件上定義排版/字級/顏色/RWD 等樣式，例如：

```jsx
<Box m={2} />
<Box p={[2, 3]} />
<Box fontSize={[1, 2]} />
<Box color="primary" />
```

透過 Styled System，使用元件時，能夠直接設定樣式，變得非常直覺，但同時又能享受元件的語意化以及封裝好的樣式，程式碼更簡潔、組合更彈性、使用更方便，還能夠達成客製化的需求。

------

### -使用 Styled System 開發專案的心得

公司專案是在今年年初時導入 Styled System 開發，目前正和同仁們努力將我們做好的 UI Library 應用在不同的專案中，接續上一篇 [We need a better UI component library - Styled System](https://anna-su.com/tech/styled-system/)

本篇文章將 Styled System 的開發心得以及 2019 Modern Web 會眾提出的問題，整理成 Q&A，內容會不定期更新喔！

#### 什麼情況下適合使用 Styled System？

建議是與公司同仁取得共識，因為需要每個角色互相配合，共同定義規範、討論需變動的規格等細節。

比較適合有系統化目標的產品網站：

- 同一個公司，但有各種風格的子專案
- 不同公司或專案，但是有相似的功能

#### 如果 PM 或設計師提供的資訊不完整怎麼辦？

在實際專案中，我們的確很容易發生需求不夠明確、文件不足的狀況，此時溝通就會變得很重要，如果執行的工程師能夠主動提出關鍵問題並給予對應的解決方案，提供選項幫助對方抉擇，如此一來，就可以更快速的確認基本的需求並規劃專案的架構。

#### 元件的功能很複雜，自己實作的話，需要花很多時間，時程真的來不及，要怎麼取捨？

仍然可以去找滿足基本功能的現成 library 再去封裝成自己的元件、達到客製化的需求並符合時程，但還是要確認當前的需求，考量修改與維護的成本再取捨。

#### theme 透過 array 設定樣式時，在使用時較難對應，這樣在寫的時候是不是很麻煩？

例如： `space: [0, 1, 4, 8, 12, 16, 24, 32]`

- 設定 padding 為 8px, 可以寫成 `p: ${themeGet('space.3')}`

遭遇問題：需要反覆查看 theme 上面的 index 才能夠確認 space 的值

解決方案一：可以使用有『規律』的方式去定義 array，例如：

案例 1: `透過 2 的次方去定義 space`

```javascript
space: [1, 2, 4, 8, 16, 32, 64]
```

use: `${themeGet('space.3')}` => 2 的 3 次方是 8, 所以 `space.3`，會拿到 8px

彈性使用：實務上因為會大量用到 0, 可以多塞一個 0，在使用的時候就是去思考 2 的次方再加 1

```javascript
space: [0, 1, 2, 4, 8, 16, 32, 64]
```

use: `${themeGet('space.4')}` => 當 index 等於 4，會拿到 8px

2 的 3 次方是 8 , 3 + 1 = 4

案例 2: `使用遞減方式定義黑白階顏色，方便擴充與使用`

```javascript
colors: {
  blacks: [
    BLACK,
    rgba(BLACK, 0.9),
    rgba(BLACK, 0.8),
    rgba(BLACK, 0.7),
    rgba(BLACK, 0.6),
    rgba(BLACK, 0.5),
    rgba(BLACK, 0.4),
    rgba(BLACK, 0.3),
    rgba(BLACK, 0.2),
    rgba(BLACK, 0.1),
    rgba(BLACK, 0.05),
  ],
  whites: [
    WHITE,
    rgba(WHITE, 0.9),
    rgba(WHITE, 0.8),
    rgba(WHITE, 0.7),
    rgba(WHITE, 0.6),
    rgba(WHITE, 0.5),
    rgba(WHITE, 0.4),
    rgba(WHITE, 0.3),
    rgba(WHITE, 0.2),
    rgba(WHITE, 0.1),
  ],
  ...
},
```

解決方案二：如果有明確的用途，可以直接使用 Object 去定義，例如：

```javascript
colors: {
...
  primary: '#62c8e0',
  success: '#56d066',
  danger: DANGER,
  errorBackground: rgba(DANGER, 0.1),
  icon: rgba(BLACK, 0.4),
  link: '#88d5e7',
  border: rgba(BLACK, 0.2),
  transparent: 'transparent',
}
fontWeights: {
  normal: 400,
  semibold: 600,
}
```

結論：和設計師討論，共同定義並盡量使用規範內的數值去做設計與開發，依據能夠彈性的客製化樣式為原則，如果有例外也沒關係，就不使用 theme 的值即可。

#### 簡報範例都是 React 搭配 styled-components，其他 JS 框架也可以使用 Styled System 嗎？

[styled-system](https://github.com/styled-system/styled-system) 是近年來新推出的 library，作者是基於 React 去開發，所以無論是版本或功能相對起來完善很多。

基本上 Vue 也支援 Styled System，但現階段，如果你要在 Vue 的技術裡，導入 styled-system，可能要等其他大神開發的專案更穩定，或者自己嘗試做一個。

Styled System 是開源專案，也有提供 [Documentation](https://styled-system.com/)，所以仍然可以參考他的架構，思考在現有專案與技術裡，該如何去改善。

## Styled System研究

Styled System 是一包工具函式，用來輔助建立 styled component，它的優點主要是可以讓元件建立統一的 API、更便利的撰寫 responsive 樣式等。

### Install

由於要在 JS 中撰寫 inline-style，因此要安裝一個 CSS-in-JS 的 library，這裡選用 [styled-components](https://www.styled-components.com/)。

```
yarn add styled-system styled-components
```

### How it works & usage

幾乎所有 CSS-in-JS 函式庫在建立 styled component 時，都可接受函式（function）作為參數、並代入 props 來動態決定樣式，`styled-components` 也是。

如下所示，color 的值是由一個 function 決定的，其中 function 會取得 props 的 color，回傳單一一個 CSS string。

```react
import styled from 'styled-components';

const Box = styled.div`
  margin: 15px 0;
  padding: 15px;
  color: ${(props) => props.color};
  background: tomato;
  border-radius: 10px;
`;

export default Box;
<Box color='#fff'>Hello World</Box>
```

改成以下寫法會更簡單明瞭。

```react
import styled from 'styled-components';

const getColor = ({ color }) => `color: ${color}`;

const Box = styled.div`
  margin: 15px 0;
  padding: 15px;
  ${getColor};
  background: tomato;
  border-radius: 10px;
`;

export default Box;
```

承上，若有多個樣式要設定，是不是要一個個單獨設定呢？

其實，我們也可以將很多個樣式的指令組成一個物件放到 styled component 中，而不是一個個單獨設定，如下，getStyles 回傳一個樣式物件，裡面依照元件動態設定的 props 來產生的樣式。

```react
import styled from 'styled-components';

const getStyles = ({ color, bg }) => ({
  color,
  background: bg,
});

const Box = styled.div`
  margin: 15px 0;
  padding: 15px;
  ${getStyles};
  border-radius: 10px;
`;

export default Box;
<Box color='#fff' bg='tomato'>
  Hello World
</Box>
```

對照 Styled System 的寫法如下。

```react
import styled from 'styled-components';
import { color } from 'styled-system';

const Box = styled.div`
  ${color}
  margin: 15px 0;
  padding: 15px;
  border-radius: 10px;
`;

export default Box;
```

這就是 Styled System 簡單來說想要做的事情-取得 props 來回傳樣式物件-也就是開一個比較大洞、用比較有彈性、有效率的方式來填進樣式。當然也會幫我們做指令縮寫的 mapping 還有提供讓我們寫起來更順手的工具，讓我們的程式碼寫起來更簡潔、更具一致性。

### Theme Provider

#### Example 2：Theme Provider

利用 [``](https://www.styled-components.com/docs/advanced#theming) 和包裝好主題色的物件，就可以擁有樣式更一致的元件，原理是 `<ThemeProvider>` 利用 React Context（[註一](https://cythilya.github.io/2019/11/08/styeld-system/#備註)）傳遞樣式的設定值。

如下，利用 `<ThemeProvider>` 當作 root 節點，並將 theme 當成 props 傳進去，之後的元件（例如 `<Box>`）指定的樣式就會在 theme 這個物件查找來做設定。

index.js

`<Box>` 的 white 與 tomato 的色碼會到 theme.js 查找相對應的 key，得到 white 為 `#fefefe`，tomato 為 `tomato`，注意，若查找不到會自動降級（fallback）為瀏覽器所支援的 [CSS color name](https://www.w3schools.com/colors/colors_names.asp)，即 white 為 `#fff`。

```react
import React from 'react';
import ReactDOM from 'react-dom';
import { ThemeProvider } from 'styled-components';
import Box from './box';
import theme from './theme';

function App() {
  return (
    <ThemeProvider theme={theme}>
      <Box color='white' bg='tomato'>
        Hello World
      </Box>
    </ThemeProvider>
  );
}

ReactDOM.render(<App />, document.getElementById('root'));
```

box.js

```react
import styled from 'styled-components';
import { color } from 'styled-system';

const Box = styled.div`
  ${color}
  padding: 15px;
  border-radius: 10px;
`;

export default Box;
```

theme.js

```react
export default {
  colors: {
    white: '#fefefe',
  },
  bg: {
    tomato: 'tomato',
  },
};
```

#### Example 3：Variants

元件也可以設定自己的主題樣式，或來自 global 的 theme 設定。

index.js

```react
import React from 'react';
import ReactDOM from 'react-dom';
import { ThemeProvider } from 'styled-components';
import Box from './box';
import theme from './theme';

function App() {
  return (
    <ThemeProvider theme={theme}>
      <div className='App'>
        <Box variant='primary'>Primary</Box>
        <Box variant='secondary'>Secondary</Box>
      </div>
    </ThemeProvider>
  );
}

ReactDOM.render(<App />, document.getElementById('root'));
```

box.js

```react
import styled from 'styled-components';
import { variant } from 'styled-system';

const Box = styled('div')(
  variant({
    scale: 'boxes',
    variants: {
      primary: {},
      secondary: {},
    },
  }),
);

export default Box;
```

依照 scale 的值來查找，也就是「boxes」。

theme.js

```react
export default {
  boxes: {
    primary: {
      color: 'white',
      bg: 'tomato',
    },
    secondary: {
      color: 'white',
      bg: 'black',
    },
  },
};
```

#### Exmaple 4：真實範例

這裡有一個[萬年曆](https://github.com/cythilya/styled-system-demo)的元件，其中標題 `<CalendarTitle>` 會隨著不同的檢視狀態而有不同的樣式。在使用 Styled System 前稱為「Before」，利用 Styled System 修改後稱為「After」。

- Before：`<CalendarTitle>` 使用屬性 noHover 來決定是否要有 cursor pointer 和 hover 樣式。若 noHover 為 true，則無 cursor pointer（即為 default）且無 hover 背景色（即為白色）。
- After：為 `<CalendarTitle>` 設定元件的主題樣式 primary 與 secondary，secondary 為 cursor default 且無 hover 背景色。

Before。

```react
<CalendarTitle noHover />
export const CalendarTitle = styled.div`
  width: 200px;
  padding: 5px 0;
  font-weight: bold;
  text-align: center;
  border-radius: 5px;
  cursor: ${({ noHover }) => (noHover ? 'default' : 'pointer')};

  :hover {
    background: ${({ noHover }) => (noHover ? white : gray)};
  }
`;
```

After。

改為當需要有 hover 狀態時才加入背景和 cursor pointer。由於 Styled System 沒有支援 cursor 因此要用 system 幫忙擴充，並在 hover 樣式中使用 color function 來填入 hover 時的背景色。這樣的修改似乎非常零碎、無系統化。

```react
<CalendarTitle bg='gray' cursor='pointer' />
import { color, system } from 'styled-system';

export const CalendarTitle = styled.div`
  ${system({
    cursor: true,
  })}
  width: 200px;
  padding: 5px 0;
  font-weight: bold;
  text-align: center;
  border-radius: 5px;

  :hover {
    ${color}
  }
`;
```

再次修改如下，將有無 hover 當成是該元件的不同狀態，就可在 theme 或 variant 定義該元件不同狀態的主題樣式，在此用 variant，相較以上的例子來說，比起散落各個指令依照 props 來修改，variant 就更模組化，也更容易了解。

primary 有 hover 狀態。

```react
<CalendarTitle variant='primary' />
```

secondary 無 hover 狀態。

```react
<CalendarTitle variant='secondary' />
export const CalendarTitle = styled('div')(
  {
    width: '200px',
    padding: '5px 0',
    fontWeight: 'bold',
    textAlign: 'center',
    borderRadius: '5px',
  },
  variant({
    variants: {
      primary: {
        cursor: 'pointer',
        bg: 'white',
        '&:hover': {
          bg: 'gray',
        },
      },
      secondary: {
        cursor: 'default',
        '&:hover': {
          bg: 'white',
        },
      },
    },
  }),
);
```

[原始碼](https://github.com/cythilya/styled-system-demo/commit/d8a9ac4fd5559fc6f14a4d680f9fbf7a63cb0c51)。

#### Responsive Styles

無論是傳統上撰寫 CSS 或 styled component 的 responsive styles 方式是針對不同 breakpoint 來寫 media query，但這會有一些缺點

- 可能是因為沒有 guideline 或沒有遵守 guideline，而導致 media query 寫法不統一（例如：間隔的定義、巢狀結構的層次），後續維護不易。
- 重複撰寫大量 media query 相關語法。

Styled System 依照傳入的屬性值自動設定 media query 的效果，像是自動對照不同 breakpoint 所要的值、屬性縮寫。若改用 Styled System，則可改進

- Styled System 提供 API 可強制規範讓開發者用一致的方式撰寫。
- 不需要重複撰寫大量 media query 相關語法，只要在 props 傳入針對 breakpoint 指定的數值即可，簡單易懂。
- Styled System 提供縮寫，可讓程式碼更簡潔，例如：`padding-left` 可簡寫為 `pl`。

#### Example 5

設定元件 `<Box>` 的寬度，依照不同的 breakpoints 區間以陣列傳入寬度。

```react
// default breakpoints 為 ['40em', '52em', '64em']

<Box width={[1 / 3, 1 / 2, 1]} />
```

當螢幕寬度小於 40em 時。

```css
.dtQrEl {
  width: 33.33333333333333%;
}
```

當螢幕寬度介於 40em ~ 52em 時。

```css
media screen and (min-width: 40em) .dtQrEl {
  width: 50%;
}
```

當螢幕寬度介於 52em ~ 64em 時。

```css
@media screen and (min-width: 52em) .dtQrEl {
  width: 100%;
}
```

#### Example 6

若陣列設定的數字小於或等於 theme 的 index 個數，則會當成 index 查找 sizes 陣列。

```react
<Box width={[1, 0, 2]}>
// default breakpoints 為 ['40em', '52em', '64em']

export default {
  sizes: [100, 200, 300],
};
```

當螢幕寬度為

- 小於 40em 時，Box 寬度為 200px

```scss
.iJHGrn {
  width: 200px;
}
```

- 介於 40em ~ 52 em 時，Box 寬度為 100px

```css
@media screen and (min-width: 40em) .iJHGrn {
  width: 100px;
}
```

- 介於 52em ~ 64 em 時，Box 寬度為 300px

```css
@media screen and (min-width: 52em) .iJHGrn {
  width: 300px;
}
```

#### Example 7

若陣列設定的數字大於 theme 的 index 個數，則會轉換為數字，單位是 px。

```react
<Box width={[200, 300, 400]}>
// default breakpoints 為 ['40em', '52em', '64em']

export default {
  sizes: [100, 200, 300],
};
```

由於 200、300、400 皆大於 sizes 陣列的長度，無法當成 index 來取值，因此就直接當成 px 數，也就是當螢幕寬度為

- 小於 40em 時，Box 寬度為 200px

```scss
.ePOXVk {
  width: 200px;
}
```

- 介於 40em ~ 52 em 時，Box 寬度為 300px

```css
@media screen and (min-width: 40em) .ePOXVk {
  width: 300px;
}
```

- 介於 52em ~ 64 em 時，Box 寬度為 400px

```css
@media screen and (min-width: 52em) .ePOXVk {
  width: 400px;
}
```

#### Example 8

設定元件 `<Box>` 的左右 padding、上下 padding，其中 index 的 3 是指 16，依此類推。

```react
<Box px={[3, 4]} py={[5, 6]} />
// default breakpoints 為 ['40em', '52em', '64em']

const themes = {
  space: [0, 4, 8, 16, 32, 64, 128, 256], // margin and padding
};
```

當螢幕寬度小於 40em 時。

```css
.gyhZND {
  padding-left: 16px;
  padding-right: 16px;
  padding-top: 64px;
  padding-bottom: 64px;
}
```

當螢幕寬度介於 40em ~ 52em 時。

```css
@media screen and (min-width: 40em) .gyhZND {
  padding-left: 32px;
  padding-right: 32px;
  padding-top: 128px;
  padding-bottom: 128px;
}
```

#### Custom Style Props

若 CSS 指令沒有被 Styled System 支援（[註二](https://cythilya.github.io/2019/11/08/styeld-system/#備註)），則可利用客製化樣式屬性 system 來擴充 Styled System 建立客製化的 style function。

#### Exmaple 9

system 接受一組物件當作設定，並回傳一個函式。

如下，Styled System 並沒有支援 `text-decoration`，可用 system 來擴充。

```react
const Box = styled.div`
  ${system({
    textDecoration: true,
  })}
`;
<Box textDecoration='underline' />
```

#### 備註

- [註一] React Context：可當成一種用來和整串元件樹分享的全域資料，例如：當前使用者和語言、主題樣式，可參考[這裡](https://zh-hant.reactjs.org/docs/context.html#when-to-use-context)、[範例](https://codesandbox.io/s/damp-monad-jjv81)。
- [註二] 這裡的不支援是指 Styled System 沒有支援，也就是 Styled System 沒有挖洞給這個 CSS 指令做設定，而不是瀏覽器的支援度。

#### References

- [We need a better UI component library - Styled System](https://anna-su.com/tech/styled-system/)
- [Styled System](https://github.com/styled-system/styled-system)
- [@styled-system/basic-demo](https://codesandbox.io/s/github/styled-system/styled-system/tree/master/examples/basic)
- [How to create responsive UI with styled-components](https://medium.com/styled-components/how-to-create-responsive-ui-with-styled-components-c6b71a3ce172)
- [Pseudo selectors in Variant](https://codesandbox.io/s/l2j76lmzn9?from-embed)