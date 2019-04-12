## https://react.docschina.org/docs/portals.html

Modal.tsx

```tsx
import React from 'react'
import _ from 'lodash'
import classNames from 'classnames'
import { FontIcon, IconEnum, Button } from '../index'

export interface Props {
  title: string
  visible: boolean
  footer?: (() => JSX.Element) | null
  onClose?: () => any
  onOk?: () => any
}
interface State {}

class Modal extends React.Component<Props, State> {
  bodyRender() {
    const { footer, children } = this.props
    const isFooter = _.isNull(footer)
    const bodyClass = classNames({
      'm-modal__body': true,
      'm-modal__body--nofooter': isFooter
    })
    return <div className={bodyClass}>{children}</div>
  }

  _close = () => {
    const { onClose } = this.props
    onClose && onClose()
  }

  _onOk = () => {
    const { onOk } = this.props
    onOk && onOk()
  }

  footerRender() {
    const { footer } = this.props
    if (_.isNull(footer)) return
    return (
      <div className="m-model-footer">
        <Button type="primary" onClick={this._onOk}>
          确定
        </Button>
        <Button onClick={this._close}>取消</Button>
      </div>
    )
  }

  render() {
    const { title, visible } = this.props
    return (
      <div
        style={{
          display: visible ? 'block' : 'none'
        }}
      >
        <div className="mock" />
        <div className="m-modal">
          <div className="m-modal__container">
            <div className="m-modal__close">
              <div className="m-modal__circlebg" onClick={this._close}>
                <FontIcon color="#0DA0DE" name={IconEnum.close} />
              </div>
            </div>
            <div className="m-modal__title">
              <span>{title}</span>
            </div>
            {this.bodyRender()}
            {this.footerRender()}
          </div>
        </div>
      </div>
    )
  }
}

export { Modal }
```

## portals.tsx

```tsx
import React from 'react'
import ReactDOM from 'react-dom'
import _ from 'lodash'
import { Modal as ModalContainer, Props as ModalProps } from './Modal'

interface Props extends ModalProps {}

interface State {}

class Modal extends React.Component<Props, State> {
  constructor(props: Props) {
    super(props)
  }

  private el = document.createElement('div')

  componentDidMount() {
    document.body.appendChild(this.el)
  }

  render() {
    return ReactDOM.createPortal(<ModalContainer {...this.props} />, this.el)
  }
}

export { Modal }
```

https://react.docschina.org/docs/react-component.html

## 局部热更新

yarn add react-dom@npm:@hot-loader/react-dom

此种方式无需修改配置文件即可实现局部热更新热

```jsx
import React from 'react'
import { hot } from 'react-hot-loader'

const App = () => <div>Hello World!</div>

export default (process.env.NODE_ENV === 'development' ? hot(module)(App) : App)
```

## 将 src 加入引入路径

```js
const path = require('path')
const { override } = require('customize-cra')

const overrideProcessEnv = () => config => {
  config.resolve.modules = [path.join(__dirname, 'src')].concat(
    config.resolve.modules
  )
  return config
}

module.exports = override(overrideProcessEnv())
```

## 路由用法

```jsx
const App = ({ match }) => (
  <div className="gx-main-content-wrapper">
    <Switch>
      <Route
        path={`${match.url}ChatRoom`}
        component={asyncComponent(() => import('./Chat'))}
      />
      <Route
        path={match.url}
        render={props => (
          <Redirect
            to={{
              pathname: '/ChatRoom',
              state: { from: props.location }
            }}
          />
        )}
      />
    </Switch>
  </div>
)
```

## 登录认证

```jsx
// 路由重定向
const RestrictedRoute = ({ component: Component, token, ...rest }) => (
  <Route
    {...rest}
    render={props =>
      token ? (
        <Component {...props} />
      ) : (
        <Redirect
          to={{
            pathname: '/signin',
            state: { from: props.location }
          }}
        />
      )
    }
  />
)

class App extends Component {
  render() {
    const { match, token } = this.props
    return (
      <LocaleProvider locale={antdCN}>
        <Switch>
          <Route exact path="/signin" component={() => <div>denglu</div>} />
          <RestrictedRoute
            path={`${match.url}`}
            token={token}
            component={() => <div>sd</div>}
          />
        </Switch>
      </LocaleProvider>
    )
  }
}
```
