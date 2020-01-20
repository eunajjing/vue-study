> 해당 강의는 [Vue.js 시작하기 - Age of Vue.js](https://www.inflearn.com/course/Age-of-Vuejs/)를 보고 정리한 내용 입니다. 저작권 문제 발생 시 해당 게시물은 삭제될 수 있습니다.

# 개발 환경 세팅

- `node.js`
- `Chrome` 확장 프로그램 `vue.js devtools`
- `vscode` 확장 프로그램
  - `Vetur`
  - `live server`

# `vue.js` 소개

## `Reactivity` 구현

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app"></div>

    <script>
      let div = document.querySelector("#app");
      let viewModel = {};

      // Object.defineProperty(대상 객체, 객체의 속성, {
      // 정의 할 내용
      // })
      // 객체의 동작을 재정의하는 api

      Object.defineProperty(viewModel, "str", {
        get: function() {
          // viewModel.str에 접근했을 때의 동작을 정의
        },
        set: function(newValue) {
          // viewModel.str에 값을 할당했을 때의 동작을 정의
          // 할당되는 새로운 값이 파라미터로 들어온다
          div.innerHTML = newValue;
        }
      });
    </script>
  </body>
</html>

```

#  `Vue` 인스턴스의 생성

인스턴스가 생성되면 `vue`는 자동으로 개발자 도구 상에서는 `Root`로 인식

만약 여러 개의 `vue`가 생성되었다면, `Root`는 여러 개 생성되며, 내부적인 인덱스를 통해 관리

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      let vm = new Vue();
      // vm을 콘솔에 찍으면 vue가 제공해주는 api와 속성들을 참고할 수 있다
      let vm = new Vue({
        el: "#app"
        // id가 app인 엘리먼트에다가 해당 뷰 인스턴스를 붙이겠다는 뜻
        // 인스턴스를 붙이지 않으면 사용이 불가
        // 생성자 안에는 객체가 들어감에 유의
      });
    </script>
  </body>
</html>
```

> **JS의 생성자 함수**
>
> - 앞의 문자가 대문자로 시작한다
>
> ```javascript
> function Person(name, job) {
> 	this.name = name
> 	this.job = job
> }
> 
> const kona = new Person('euna', 'vueDreamer')
> // 이 때 kona는 객체가 된다
> ```

```javascript
function Vue() {
	this.somefunc = function() {
		console.log("hi!")
	}
}

const vue = new Vue()
vue.somfunc()
// "hi!"
```

## 속성과 `API`

```javascript
new Vue({
	el: ,
	// 인스턴스가 그려지는 화면의 시작점 (특정 html 태그)
	template: ,
  // 화면에 표시할 요소 (html, css)
	data: ,
  // 뷰의 반응성이 반영된 데이터 속성
	methods: ,
  // 뷰의 라이프 사이클과 관련된 속성
	created: ,
  // data에서 정의한 속성이 변화했을 때 추가적인 동작을 수행할 수 있게 정의하는 속성
	watch: ,
  
  // key : value 형태로 되어 있음을 명심할 것
})
```

# 컴포넌트의 등록

## 전역 컴포넌트

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <app-header></app-header>
      <app-content></app-content>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      // Vue.component('컴포넌트 이름', 컴포넌트 내용)
      // 컴포넌트를 등록하는 방식
      // 전역 컴포넌트
      Vue.component("app-header", {
        template: "<h1>Header</h1>"
      });
      Vue.component("app-content", {
        template: "<div>Header</div>"
      });

      new Vue({
        el: "#app"
      });
    </script>
  </body>
</html>

```

## 지역 컴포넌트

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <app-header></app-header>
      <app-content></app-content>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      Vue.component("app-header", {
        template: "<h1>Header</h1>"
      });
      Vue.component("app-content", {
        template: "<div>Header</div>"
      });

      new Vue({
        el: "#app",
        components : {
          // 컴포넌'츠' 복수형임에 유의할 것
          // 이는 비단 components 뿐만 아니라 methods도 매한가지
          // "key" : value 형태를 기본으로 하며
          // componentName : value 형식으로 들어간다고 생각하면 된다
          // 이 형태는 위에서 기입한 Vue.component("컴포넌트 이름", 컴포넌트 내용)과 같다
          // 다만 여기에 등록하는 것은 지역 컴포넌트 등록 방식이라 한다
        }
      });
    </script>
  </body>
</html>
```

## 전역 컴포넌트와 지역 컴포넌트의 차이

- 지역 컴포넌트의 경우 어떤 컴포넌트의 하위 속성인지 바로 알 수 있다.
  - 대개 서비스를 만드는 컴포넌트에서 많이 사용된다
- 전역 컴포넌트의 경우 플러그인이나 라이브러리처럼 전역에서 사용해야 하는 컴포넌트만을 사용한다

> - 만약 여러 개의 `Vue`를 생성하여 여러 개의 `Root`가 있을 경우, 전역 컴포넌트는 추가적인 작업 없이 다른 `Root` 컴포넌트에서 가져다가 쓰는 것이 가능
> - 그러나 지역으로 생성된 인스턴스의 경우, `components` 라는 속성에 컴포넌트를 만들어주지 않으면 개발자 도구에서 컴포넌트가 제대로 등록되지 않았다는 에러가 발생

# 컴포넌트의 통신

- 뷰 컴포넌트는 각각 고유한 데이터 유효 범위를 갖는다
  - 데이터를 각각 관리
  - 이 데이터를 주고 받기 위해서는 `props`라는 속성이나 이벤트를 활용해야 한다

## 왜 이런 방식을 고수하게 되었나

기존 `MVC` 방식에서는 데이터 변화에 따라 어떤 컴포넌트가 변화하는지 짐작하기 어려웠기 때문에, 데이터의 움직임들로 인한 버그를 추적하기 굉장히 어려웠다. 그러나 데이터의 흐름을 강제하면, 데이터를 추적하기 굉장히 쉽다는 이점이 있다.

## `props`

상위 컴포넌트에서 하위 컴포넌트로 데이터를 내릴 때 사용

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <!-- <app-header v-bind:해당 컴포넌트의 프롭스 속성 이름="상위 컴포넌트의 데이터 이름"></app-header> -->
      <app-header v-bind:propsdata="message"></app-header>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      const appHeader = {
        template: "<h1>{{ propsdata }}</h1>",
        props: ["propsdata"]
      };

      new Vue({
        el: "#app",
        components: {
          "app-header": appHeader
          // 소스가 너무 길어지는 관계로 상위에서 정의를 미리 해준 뒤 사용하는 방식
        },
        data: {
          message: "hi"
        }
      });
    </script>
  </body>
</html>
```

## `Event`

하위 컴포넌트가 상위 컴포넌트에 데이터를 올릴 때 사용

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <!-- <app-header v-on:하위 컴포넌트에서 발생한 이벤트 이름="상위 컴포넌트의 메서드 이름"></app-header> -->
      <app-header v-on:pass="logText"></app-header>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>

      const appHeader = {
          template : '<button v-on:click="passEvent">click me</button>',
          methods: {
              passEvent = () => {
                this.$emit('pass')
              }
          }
      }

      new Vue({
          el : '#app',
          components : {
              'app-header' : 'appHeader'
          },
          methods : {
              logText : () => {
                  console.log("event!")
              }
          }
      })
    </script>
  </body>
</html>
```

## 같은 레벨 간의 통신

리액트와 거의 유사하다. 루트 컴포넌트에서 하위 컴포넌트로 내려주는 방식을 취하며, 같은 레벨 간의 통신은 원칙적으로 불가하기 때문에 상위 컴포넌트로 올렸다가 내리는 방식을 기본으로 한다.

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <app-header v-bind:propsdata="num"></app-header>
      <!-- 자신의 컴포넌트에 기재된 이름 = "부모에 기재된 이름" -->
      <app-content v-on:pass="deliverNum"></app-content>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      var appHeader = {
        template: "<div>header</div>",
        props: ["propsdata"]
      };

      var appContent = {
        template:
          '<div>content<button v-on:click="passNum">click</button></div>',
        methods: {
          passNum: function() {
            this.$emit("pass", 10);
          }
        }
      };

      new Vue({
        el: "#app",
        components: {
          "app-header": appHeader,
          "app-content": appContent
        },
        data: {
          num: 0
        },
        methods: {
          deliverNum: (value) => {
            this.num = value;
          }
        }
      });
    </script>
  </body>
</html>
```

> ES6 문법으로 사용도 가능하다

# 라우터

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div>
      <div>
        <router-link to="/login"></router-link>
        <router-link to="/main"></router-link>
      </div>
      <router-view></router-view>
      <!-- 뷰 인스턴스에 연결되어야 사용이 가능하며 해당 내용을 기재해야만 라우터에 연결한 컴포넌트들이 뜬다-->
    </div>

    <script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>
    <!-- 라우터 cdn 추가 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      const LoginComponent = {
        template: "<div>login</div>"
      };

      const MainComponent = {
        template: "<div>main</div>"
      };

      const router = new VueRouter({
        routes: [
          {
            path: "/login",
            // 페이지 url
            component: LoginComponent
            // 해당 url에서 표시될 컴포넌트
          },
          {
            path: "/main",
            component: MainComponent
          }
        ]
        // 라우팅이 필요한 페이지들이 객체 형태로 들어가며, 이들은 배열 상태로 routes에 들어간다.
        // 페이지 개수만큼 필요
      });

      new Vue({
        el: "#app",
        router
      });
    </script>
  </body>
</html>

```

> `VueRouter`의 속성, **mode**
>
> ```vue
> new VueRouter({
> 	mode : 'history',
> 	routes : [...]
> })
> ```
>
> 이 경우 `url`에 달려있던 `#`가 사라지는 모습을 확인할 수 있다

# `Vue`의 템플릿 문법

## 데이터 바인딩

```vue
<span>{{ data }}</span>
```

### `computed`

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <p>{{num}}</p>
      <p>{{doubleNum}}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      new Vue({
        el: "#app",
        data: {
          num: 10
        },
        computed: {
          // 가변적으로 바뀌는 값을 처리할 때 사용하는 속성
          doubleNum: function() {
            return this.num * 2;
            // arrow function 사용 시 먹지 않는다
            // this가 가리키는 대상이 달라지기 때문으로 추측됨
          }
        }
      });
    </script>
  </body>
</html>

```

## 디렉티브 `v-`

```vue
<span v-if="show"></span>
```

-  `v-bind`
  - 상위 컴포넌트의 `data`로 관리하던 값이 연결이 된다

- `v-if`, `v-else`
  - 위의 예제에서 `show`가 `true`면 해당 `span` 태그 안 문자가 보일 것
- `v-show`
  - `v-if`, `v-else`와 거의 유사하나, **`if`는 `DOM` 상에서도 보이지 않고, `show`는 `display` `none` 상태로 존재한다는 차이**

- `v-on`

  ```vue
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
    </head>
    <body>
      <div id="app">
        <button v-on:click="logText">click</button>
        <!-- click 실행 시 뒤에 기재된 메서드가 실행된다 -->
        <input type="text" v-on:keyup.enter="logText" />
        <!-- enter가 눌려졌을 때만 실행 -->
      </div>
  
      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      <script>
        new Vue({
          el: "#app",
          methods: {
            logText: () => console.log("clicked")
          }
        });
      </script>
    </body>
  </html>
  ```

# `Vue cli`의 생성

```shell
yarn global add @vue/cli
```

```shell
vue cli "프로젝트명"
```

## `src/main.js`

```vue
new Vue({
}).$mount('#app')
```

```vue
new Vue({
	el : '#app'
})
```

두 개의 소스는 동일하다.

### `render`

`Vue` 내부적으로 사용하는 함수이자 템플릿이라는 속성을 사용했을 때 실행된다.

```vue
const App = {
	template : '<div>App</div>'
}

new Vue({
  render: h => h(App),
}).$mount('#app')
```

```vue
new Vue({
	components : {
		'app' : App
	},
}).$mount('#app')
```

두 개의 소스는 동일하다.

# `Vue` 확장자

```vue
<template>
  <!-- html -->
	<!--
		const appHeader = {
			template : '<div>header</div>'
		}
	-->
	<div>
    header
  </div>
</template>

<script>
export default {
// new Vue({
//  ...
//	\})
  // Vue 인스턴스에 적용할 수 있는 속성들을 넣어줄 수 있다
	methods : {
    addNum : function() {
      
    }
  }
}
</script>
```

> 컴포넌트의 이름은 두 개의 단어 조합으로 하는 것이 좋다. `html`의 표준 `tag`인지, `vue`의 태그인지 알 길이 없기 때문

## 예제

### 하위 컴포넌트

```vue
<template>
  <div>
    <div>{{str}}</div>
    <!-- 리액트와 마찬가지로, 최상위 태그는 단 하나여야 한다 -->
    <app-header v-bind:propsdata="str" v-on:renew="renewStr" />
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
// vue라는 확장자까지 붙여주기를 권고한다

export default {
  data: function() {
    // 반드시 함수를 반환해야 한다
    return {
      str: "Header"
    };
  },
  components: {
    "app-header": AppHeader
  },
  methods: {
    renewStr: function() {
      this.str = "hi";
    }
  }
};
</script>
```

### 루트 컴포넌트

```vue
<template>
  <div>
    <div>{{str}}</div>
    <!-- 리액트와 마찬가지로, 최상위 태그는 단 하나여야 한다 -->
    <app-header v-bind:propsdata="str" v-on:renew="renewStr" />
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
// vue라는 확장자까지 붙여주기를 권고한다

export default {
  data: function() {
    // 반드시 함수를 반환해야 한다
    return {
      str: "Header"
    };
  },
  components: {
    "app-header": AppHeader
  },
  methods: {
    renewStr: function() {
      this.str = "hi";
    }
  }
};
</script>
```

# 전체 프로젝트

```vue
<template>
  <form v-on:submit.prevent="submitForm">
    <!-- 이벤트 기본 새로고침을 막겠다 -->
    <div>
      <label for="username">id : </label>
      <input v-model="username" id="username" type="text" />
    </div>
    <div>
      <label for="password">pw : </label>
      <input v-model="password" id="password" type="password" />
    </div>
    <button type="submit">login</button>
  </form>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      username: "",
      password: ""
    };
  },
  methods: {
    submitForm: function() {
      const url = "http://jsonplaceholder.typicode.com/users";
      const data = {
        username: this.username,
        password: this.password
      };
      axios
        .post(url, data)
        .then(res => console.log(res))
        .catch();
    }
  }
};
</script>
```

