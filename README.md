# 인프런 Vue.js 강좌 연습
## [Vue.js 시작하기 - Age of Vue.js](https://www.inflearn.com/course/Age-of-Vuejs/dashboard)



```javascript
 <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```  
  

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