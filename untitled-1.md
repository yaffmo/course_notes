# index

\[TOC\]

## - 檔案位置

> C:\Users\yfm\Documents\WebDev\react-learning\basic projects\2-tours\setup

## - 安裝

> 1. @reduxjs/toolkit
> 2. react-redux

## - 設定流程

### 1. 設定data資料源頭，並==存入全局State==

```text
//這是async的用法，要從api抓到資料之後，再存進State  
export const fetchTours = () => async (dispatch) => {
     try {
       const response = await fetch("https://course-api.com/react-tours-project");
       const tours = await response.json();
       dispatch(tourCreate(tours));
     } catch (error) {
       console.log(error);
     }
   };
```

### 2. 創造Data Slice

```text
   import { createSlice } from "@reduxjs/toolkit";

   export const toursSlice = createSlice({
     name: "tours",
     initialState: {},
     reducers: {
         //state是原本的data, action.payload是帶進來的值
       create: (state, action) => {
         state.tours = action.payload;
       },
       remove: (state, action) => {
         state.tours = state.tours.filter((tour) => tour.id !== action.payload);
       },
     },
   });
```

### 3. 重新設定Action名稱

```text
export const { remove: tourRemove, create: tourCreate } = toursSlice.actions;
```

### 4. 創造Store

```text
import { configureStore } from "@reduxjs/toolkit";
import { toursSlice } from "./TourSlice";

const reducer = {
  tours: toursSlice.reducer,
};

export default configureStore({ reducer });
```

### 5. 設定Provider連結react

```text
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import { Provider } from "react-redux";
import store from "./Store";

// 設定store

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

### 6. 將全局State透過==useSelector==倒入元件的state

```text
import { useSelector, useDispatch } from "react-redux";
import { fetchTours } from "./TourSlice";

function App() {
  const dispatch = useDispatch();
  const { tours } = useSelector((state) => state.tours);

  useEffect(() => {
    dispatch(fetchTours());
  }, [dispatch]);
```

### 7. 透過prop drilling，將State放進子元件

* 其實如果父子離太遠，也是可以用useSelector取得State

```text
const Tours = ({ tours }) => {
  return (
    <section>
      <div className="title">
        <h2>our tours</h2>
        <div className="underline"></div>
      </div>
      <div>
        {tours.map((tour) => (
          <Tour key={tour.id} {...tour}></Tour>
        ))}
      </div>
    </section>
  );
};
```

### 8. 使用==useDispatch==，讓子元件調用Action

* dispatch其實就是setState全局版

```text
import { useDispatch } from "react-redux";
import { tourRemove } from "./TourSlice";

const Tour = ({ id, name, info, image, price }) => {
  const dispatch = useDispatch();
  const [readMore, setReadMore] = useState(false);

  return (
    <article className="single-tour">
      <img src={image} alt={image} />
      <footer>
        <div className="tour-info">
          <h4>{name}</h4>
          <h4 className="tour-price">${price}</h4>
        </div>
        <p>
          {readMore ? info : `${info.substring(0, 200)}...`}
          <button onClick={() => setReadMore(!readMore)}>
            {readMore ? "Show Less" : "Read More"}
          </button>
        </p>
        <button onClick={() => dispatch(tourRemove(id))} className="delete-btn">
          not interesting
        </button>
      </footer>
    </article>
  );
};
```

