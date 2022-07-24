# Route에서의 조건부 렌더링

<p>React에서 프로젝트를 진행할 때 모든 페이지에 nav bar와 footer가 필요하다면, route, path를 관리하는 route.js파일 혹은 App.jsx 파일의 Router 태그 안에 nav bar와 footer 컴포넌트를 넣어두고는 한다. </p>

<p>하지만 특정 페이지들에서만 두 컴포넌트가 보이지 말아야한다면?
이럴 때 필요한게 Route에서의 조건부 렌더링이다. </p>

<br />

## 적용 방법

### 1

route, path를 관리하는 파일에서 (1) hidden 여부를 boolean 형태로 가지는 state(ex. isHidden)와 (2) state를 props에 따라 바꿔주는 setState 함수(ex. changeHidden)를 정의한다.

### 2

`{!isHidden && <NavBar_n />}` 의 형태로 state에 따라 navbar, footer가 렌더링 되도록한다.

### 3

`<Route exact path='nav, footer가 불필요한 path' render={() => <SignUp isHidden={changeHidden(true)} />} />`
위 형태로 Router 안에서 path와 그에 따른 component를 정의할 때 `component={}`의 형태가 아니라
render함수 형태로, isHidden status에 props로 true를 넘겨준다.

### 4

위 과정으로 처리한 path에서만 nav bar, footer가 렌더링되지 않는다!

<br />

## 적용 예시

<p>Home, Detail, Signup 페이지를 가진 프로젝트에서 Signup 페이지만 nav bar와 footer가 보여지지않아야 한다고 하자.</p>

<p> route, path를 관리하는 파일 (여기선 App.jsx)에서 아래와 같이 작성하면 nav bar와 footer를 조건부 렌더링시킬 수 있다.</p>

<br />

```javascript
function App(props) {

  const [isHidden, setIsHidden] = useState(false)

  const changeHidden = (props) => {
    setIsHidden(props)
  }

  render() {
    return (
      <Router>
        {!isHidden && <NavBar_n />}

        <Switch>
          {/* Home - nav, footer 필요 */}
          <Route exact path="/main" component={Main} />

          {/* Detail - nav, footer 필요 */}
          <Route exact path="/main" component={Main} />

          {/* Signup - nav, footer 불필요 */}
          <Route exact path='/signup'
          render={() => <SignUp isHidden={changeHidden(true)} />}
          />
        </Switch>

        {!isHidden && <Footer_n />}
      </Router>
    );
    }
  }
}

export default App;

```
