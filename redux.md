combineReducer
createStore第二个参数可用于设置state初始状态，可用于同构时将状态加载进来
let store = createStore(todoApp, window.STATE_FROM_SERVER)
