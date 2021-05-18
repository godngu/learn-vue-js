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














