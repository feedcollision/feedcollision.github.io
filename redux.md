combineReducer
createStore第二个参数可用于设置state初始状态，可用于同构时将状态加载进来
let store = createStore(todoApp, window.STATE_FROM_SERVER)


const store = createStore(
   reducer, /* preloadedState, */
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
 );
 
 增加以上内容后启用redux extension可以获得store的变化


const logger = store => next => action => {
  console.group(action.type)
  console.info('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  console.groupEnd(action.type)
  return result
}

middleware通过方法参数的形式接收next()方法，而不是通过store实例获取。
function applyMiddleware(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()

  let dispatch = store.dispatch
  middlewares.forEach(middleware =>
    dispatch = middleware(store)(dispatch)
  )

  return Object.assign({}, store, { dispatch })
}

直接传入原有的方法
