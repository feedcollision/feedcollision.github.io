combineReducer
createStore第二个参数可用于设置state初始状态，可用于同构时将状态加载进来
let store = createStore(todoApp, window.STATE_FROM_SERVER)


const store = createStore(
   reducer, /* preloadedState, */
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
 );
 
 增加以上内容后启用redux extension可以获得store的变化
