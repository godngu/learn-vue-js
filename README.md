# 인프런 Vue.js 강좌 연습
## [Vue.js 시작하기 - Age of Vue.js](https://www.inflearn.com/course/Age-of-Vuejs/dashboard)



```javascript
 <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```  
  
### 컴포넌트

##### 전역 컴포넌트 등록
- 플러그인이나 라이브러리를 구현할때 주로 사용한다.
```javascript
Vue.component('컴포넌트 이름', 컴포넌트 내용);

Vue.component('app-header', {
    template: '<h1>Header</h1>'
});

```

##### 지역 컴포넌트 등록
- 인스턴스를 생성할때마다 컴포넌트를 등록 해줘야 한다.
```javascript
new Vue({
    el: '#app',
    components: {
        '컴포넌트 이름': 컴포넌트 내용
    }
});

new Vue({
    el: '#app',
    components: {
        'app-footer': {
            template: '<footer>footer</footer>'
        }
    }
});
```


### 컴포넌트 통신 방법 - 기본
- 상위 컴포넌트에서 하위로 데이터를 내려줌, `props` 속성
- 하위 컴포넌트에서 상위로 이벤트롤 올려줌, `event` 발생


#### props 속성
- ROOT -> AppHeader로 데이터 내리기
- ROOT 속성의 data 값을 변경하면 하위 컴포넌트의 값도 같이 변경된다.  

`v-bind:프롭스 속성 이름="상위 컴포넌트의 데이터 이름"`
```html
<div id="app">
    <app-header v-bind:프롭스 속성 이름="상위 컴포넌트의 데이터 이름"></app-header>
    <app-header v-bind:propsdata="message"></app-header>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var appHeader = {
        template: '<h1>{{ propsdata }}</h1>',
        props: ['propsdata']
    }

    new Vue({
        el: '#app',
        components: {
            'app-header': appHeader
        },
        data: {
            message: 'hi'
        }
    })
</script>
```

#### event emit
`v-on:하위 컴포넌트에서 발생한 이벤트 이름="상위 컴포넌트의 메서드 이름"`
```html
<div id="app">
    <app-header v-on:하위 컴포넌트에서 발생한 이벤트 이름="상위 컴포넌트의 메서드 이름"></app-header>
    <app-header v-on:pass="logText"></app-header>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var appHeader = {
        template: '<button v-on:click="passEvent">click me</button>',
        methods: {
            passEvent: function() {
                this.$emit('pass');
            }
        }
    }

    new Vue({
        el: '#app',
        components: {
            'app-header': appHeader
        },
        methods: {
            logText: function() {
                console.log('hi');
            }
        }
    });
</script>
```

#### 뷰 인스턴스에서의 `this` 참고자료
- [The JavaScript this Keyword](https://www.w3schools.com/js/js_this.asp)
- [Understanding the “this” Keyword in JavaScript](https://betterprogramming.pub/understanding-the-this-keyword-in-javascript-cb76d4c7c5e8)


### 컴포넌트 통신 방법 - 응용
- 같은 레벨의 컴포넌트끼리 데이터 통신 방법
    - 목표: AppContent -> AppHeader
    - 방법: AppContent(`event`) -> Root -> AppHeader(`props`) 순서로 데이터를 이동시킨다.
        - `this.$emit('pass')` -> `num` -> `props['propsdata']`



### Router
- [https://router.vuejs.org/kr/installation.html](https://router.vuejs.org/kr/installation.html)
- 해당 페이지마다 보여질 컴포넌트를 등록한다.

#### CDN 방식
```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

#### router-view
```html
<div id="app">
    <router-view></router-view>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
    var LoginComponent = {
        template: '<div>login</div>'
    }
    var MainComponent = {
        template: '<div>main</div>'
    }

    var router = new VueRouter({
        mode: 'history' // url에서 해쉬값(#)을 없앤다.

        // 페이지의 라우팅 정보
        // 페이지의 개수만큼 객체의 개수가 필요하다.
        routes: [
            {
                path: '/login',             // 페이지의 url 이름
                component: LoginComponent   // 해당 url에서 표시될 컴포넌트
            },
            {
                path: '/main',
                component: MainComponent
            }
        ]
    });

    new Vue({
        el: '#app',
        router: router
    });
</script>
```

#### router-link
- href가 선언된 링크가 생성된다.
```html
<router-link to="이동할 url"></router-link>
```

```html
<div>
    <router-link to="/login">Login</router-link>
    <router-link to="/main">Main</router-link>
</div>
```


### http 통신 라이브러리 - axios
- [axios github](https://github.com/axios/axios)
- jquery ajax와 유사하다.
- 비동기 통신
- 테스트 데이터: [jsonplaceholder](https://jsonplaceholder.typicode.com/)

#### CDN 방식
```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```
#### 데이터 조회 예제
```html
<script>
    new Vue({
        el: '#app',
        data: {
            users: []
        },
        methods: {
            var vm = this;
            getData: function() {
                axios.get('https://jsonplaceholder.typicode.com/users')
                .then(function(response) {
                    console.log(response);
                    // 이곳에서 this.users를 하면 data를 바라보지 않는다.(실행 context가 다름)
                    // 따라서 함수 호출 전 vm으로 this를 담아서 사용하면 data.users에 접근이 가능하다.
                    vm.users = response.data;
                })
                .catch(function(error) {
                    console.log(error);
                })
            }
        }
    });
</script>

```

### 템플릿 문법

#### 데이터 바인딩
- `{{ message }}`
- `computed`속성
```html
<div id="app">
    <p>{{ num }}</p>
    <p>{{ doubleNum }}</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 10
        },
        computed: {
            doubleNum: function() {
                return this.num * 2;
            }
        }
    });
</script>
```


#### 디렉티브
- `v-`로 붙는 속성
- 속성 바인딩, if-else 예제
- `v-show` 
    - `display:none` 상태로 만든다.
```html
<div id="app">
    <p v-bind:id="uuid" v-bind:class="name">{{ num }}</p>
    <div v-if="loading">
        Loading...
    </div>
    <div v-else>
        test user has been logged in
    </div>
    <div v-show="loading">
        Loading...
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 10,
            uuid: 'abc1234',
            name: 'text-blue',
            loading: true
        }
    });
</script>
```
##### input 바인딩
```html
<input type="text" v-model="message" placeholder="input me">
<p>{{ message }}</p>
```

##### input에서 사용자 키보드 입력 후 enter 누를때 바인딩
```html
<input type="text" v-on:keyup.enter="logText">
```

#### watch vs computed

##### watch(newValue, oldValue)
- 데이터의 변화에 따라서 특정 로직을 실행할 수 있다.
- 매번 실행되기 부담스러운 로직에 사용

##### computed
- validation에 활용
- 간단한 텍스트 변경, 연산시 사용

###### 팁
- watch 보다는 computed를 활용하라.

##### computed 활용 - data 값에 따라 class 변경

###### data 속성 활용
```html
<style>
    .warning {
        color: red;
    }
</style>

<div id="app">
    // isError 값을 바인딩하여 class값을 변경한다.
    <p v-bind:class="{ warning: isError }">Hello</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            isError: false
        }
    });
</script>/
```

###### computed 속성 활용
- 더 명확한 의미를 전달할 수 있다.
```html
<style>
    .warning {
        color: red;
    }
</style>

<div id="app">
    <p v-bind:class="{errorTextColor}">Hello</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            isError: false
        },
        computed: {
            errorTextColor: function() {
                return this.isError ? 'warning' : null;
            }
        }
    });
</script>
```

### Vue CLI

- [Vue CLI Installation](https://cli.vuejs.org/guide/installation.html)

```bash
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

#### NPM Global Libraries 위치
`/usr/local/lib/node`
or
`/usr/local/lib/node_modules`


#### Vue CLI 버전별 명령어 차이

##### Vue CLI 2.x
```text
vue init '프로젝트 템플릿 유형' '프로젝트 폴더 위치'
vue init webpack-simple '프로젝트 폴더 위치'
```

##### Vue CLI 3.x
```text
vue create '프로젝트 폴더 위치'
```

#### 실행
```bash
$ npm run serve
```



### 싱글 파일 컴포넌트
- `<template></template>` 안에는 **한 개**의 element만 존재해야 한다.
```html
<!-- div가 두 개라 안됨 -->
<template>
    <div></div>
    <div></div>
</template>
```

#### vue파일에 data 선언시 함수 형태로 만들고 return을 해야 한다.
```html
<script>
// vue cli 이전 버전
// data: {
// }

export default {
  data: function() {
    return {
      str: 'hello'
    }
  }
}
</script>
```

#### vue 파일 생성시 반드시 두 단어 이상으로 조합해야 한다.
- 한 단어로 표시할 경우 html 표준태그와 충돌날 수 있다.

##### 정상
- AppHeader.vue

##### 잘못됨 
- Main.vue


### 사용자 입력 폼 만들기
#### form submit 동작 관리
##### submit event 바인딩 및 새로고침 방지(일반 javascript 방식)
```html
<template>
    <!-- form submit 이벤트 바인딩 -->
    <form v-on:submit="submitForm"></form>
</template>

<script>
    import axios from 'axios';
    
    export default {
        methods: {
            submitForm: function(event) {
                // 새로고침 방지
                event.preventDefault();
            }
        }
    }
</script>
```

##### submit event 바인딩 및 새로고침 방지(일반 javascript 방식)
```html
<template>
    <!-- form submit 이벤트 바인딩 -->
    <form v-on:submit.prevent="submitForm"></form>
</template>
```

#### axios를 이용한 데이터 전송
##### axios 설치
```bash
$ npm i axios
```
##### lib import
```html
import axios from 'axios';
```





### VS Code에서 사용하는 Vue.js 자동완성 팁
##### 기본 html 포맷 생성
입력 
- `! + tab`

결과
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```
---

##### id가 포함된 html element 생성
입력
`div#app`
결과
```html
<div id="app"></div>
```
---

##### javascript src
입력
- `script:src`

결과
```html
<script src=""></script>
```
---

##### html태그 중복 생성
입력
- `div*3`

결과
```html
<div></div>
<div></div>
<div></div>
```
---

##### vue SFC(Single File Component) 자동완성
입력
- `vue`  

결과
```html
<template>
  
</template>

<script>
export default {

}
</script>

<style>

</style>
```