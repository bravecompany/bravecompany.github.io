
--리덕스 미들웨어 설치 (thunk)
npm install --save redux-thunk

--실행
npm start
```
<!-->
# app.js

``` javascript
import React from 'react';  // 리엑트 
import { createStore, applyMiddleware } from 'redux'; // 기본리덕스
import { Provider } from 'react-redux'; // 리엑트 전용 리덕스 (store 사용을 위한 Provider)
import thunk from 'redux-thunk'; // 리덕스미들웨어 
import LoginReducer from  './Component/LoginReducer';  // 기본 전역변수 , 로그인 로직 체크 
import Home from './Home.js'; // 기본페이지

const createStoreWithMiddleware = applyMiddleware(thunk)(createStore)
const store = createStoreWithMiddleware(LoginReducer);
class App extends React.Component {
    constructor(props) {
        super(props);
    }
    render() { 
        return (
        <Provider store={store}> //Provider를 통해 store 데이터전달 ( 브로드캐스트 )
            <Home></Home>
        </Provider>
        );
    }
}

export default App;
```


# Home.js

```javascript

import React from 'react';
import './App.css';
import logo from './logo.svg';
import { connect } from 'react-redux'
import Login from './Component/Login.js'

class Home extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      loginPage : false,
    };
  }

  render() { 
    return (
      <div className="App">
      {  this.props.isLogind &&
        <header className="App-header">
          <h3>{this.props.Users.userName} 님 환영합니다.</h3> 
          <img src={logo} className="App-logo" alt="logo" />
        </header>
      }
      { !this.props.isLogind &&
        <Login></Login>
      }
      </div>
    );
  }
}

const mapStateToProps = state => {
  return {
    Users: state.Users,
    isLogind: state.isLogind,
    apiUrl: state.apiUrl,
  }
}
export default connect(mapStateToProps, null)(Home);

```

# Login.js

```javascript
import React from 'react';
import '../App.css';
import PropTypes from 'prop-types'
import {connect} from 'react-redux'
import {fetchLogin} from './LoginAction'

class Login extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isLoginded: false,
            isLoaded : false,
            userId : null,
            userPw: null,
        };
        this.onLoginPress = this.onLoginPress.bind(this);
        this.handleChange = this.handleChange.bind(this);
    }
    componentWillMount(){
      //  console.log(this.props);
    }
    render() {
        return ( 
            <div className="App">
                <h1>Login Page</h1>
                    <input type='text' 
                    style={style.textfield} name="userId" 
                    placeholder="아이디 입력" 
                    onChange={this.handleChange} />
                    <br/>
                    <input type='password' 
                    style={style.textfield} name="userPw"  
                    placeholder="비밀번호 입력" 
                    onChange={this.handleChange}  />
                    <br/>
                    <button type='button' 
                    style={style.button}  id="confirm" 
                     onClick={this.onLoginPress} >Login 
                </button>
            </div>
        );
    }
    // TEXT 입력시 상태저장
    handleChange (e) {
        this.setState({
          [e.target.name]: e.target.value
        })
    }
    // 로그인 하기
    onLoginPress() {
        const data ={
          id : this.state.userId,
          pwd :  this.state.userPw,
          apiUrl :this.props.apiUrl
        }
        if (data.id && data.pwd) {
            fetch(apiUrl+'경로추가')
            .then(res => res.json())
            .then(
                (result) => {
                    if(result.rstState == "S2"){
                        if( result.userKey ) {
                            this.setState({
                                isLoginded : true
                            });
                           this.props.fetchLogin(result)
                        } else {
                            alert('회원정보가 올바르지 않습니다.');
                        }
                    }else{
                        alert('회원정보가 올바르지 않습니다.');
                    }
                },
                (error) => {
                  this.setState({
                    isLoaded: true,
                    error
                  });
                }
            )
        }else{
            alert('아이디 또는 비밀번호가 입력되지 않았습니다.');
        }
    }
}


const style = {
    textfield: {
        justifyContent: 'center',
        width: 250,
        height:50,
        top : 10,
        padding:'5px',
        marginTop: 10,
    },
    button: {
        fontSize: 20,
        width: 250,
        height:50,
        color: "#222222",
        marginTop: 10,
    },
};

Login.propTypes ={
    fetchLogin:PropTypes.func.isRequired
}

const mapStateToProps = state => {
    return {
        Users : state.Users,
        Error : '',
        isLogind : true,
        apiUrl : state.apiUrl
    }
} 
export default connect(mapStateToProps, {fetchLogin})(Login);
```
# Types.js

```javascript

export const LOGIN_REQUST = 'LOGIN_REQUST';
export const LOGIN_SUCCES = 'LOGIN_SUCCES';
export const LOGIN_FAILURE = 'LOGIN_FAILURE';

```

# LoginReducer.js

```javascript

import {LOGIN_REQUST,LOGIN_SUCCES,LOGIN_FAILURE } from './Types'
const initialState = {
    Users : [],
    Error : '',
    isLogind : false,
    apiUrl : 'baseApiUrl',
    siteSeq : 'code'
};

const LoginReducer = (state=initialState, action) => {
    switch(action.type){
        case 'LOGIN_REQUST' : 
            return { ...state, isLogind : true }
        case 'LOGIN_SUCCES' : 
            return { ...state, isLogind : true, Users : action.payload}
        case 'LOGIN_FAILURE' : 
            return { ...state, isLogind : false, Error : action.error }
        default : 
            return state;
    }
}
export default LoginReducer;

```
# LoginAction.js

```javascript
import {LOGIN_REQUST,LOGIN_SUCCES,LOGIN_FAILURE } from './Types'

export const LoginRequst = () => ({type : LOGIN_REQUST})
export const LoginSucces = (json) => ({type : LOGIN_SUCCES, payload: json})
export const LoginFailure = (error) => ({type : LOGIN_FAILURE, payload: error})

export const fetchLogin =(json) => {
   return  dispatch => {
    dispatch(LoginRequst());
        try {
            dispatch(LoginSucces(json));
        } catch (error) {
            dispatch(LoginFailure(error));
        }
    } 
}
```
-->