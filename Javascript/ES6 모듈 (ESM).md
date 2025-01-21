# ES6 모듈 (ESM)

```html
<script type="module" src="app.mjs"></script>
```

* 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용이 가능한 코드 조각을 말함
* 일반적으로 모듈이 성립하려면 모듈은 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 함
* ES6 모듈을 사용하기 위해서는 `script` 태그에 모듈 어트리뷰트를 추가하면 해당 파일은 모듈로 동작함
	* 일반적인 `.js`가 아닌 ESM임을 명시하기 위해 ESM의 파일 확장자는 `.mjs`로 사용을 권장함
	* ESM은 클래스와 마찬가지로 기본적으로 `strict mode`가 적용됨

### 모듈 스코프

* 일반적인 자바스크립트 파일은 `script` 태그로 분리해도 독자적인 모듈 스코프를 갖지 않음
* ESM은 파일 자체의 독자적인 모듈 스코프를 제공해 모듈 내의 `var` 변수는 전역 변수가 아니게 됨
	* 마찬가지로 일반 자바스크립트 파일과 달리 `window` 객체의 프로퍼티가 되지 않음
* 모듈 내에서 선언한 식별자는 모듈 스코프가 다르기 때문에 모듈 외부에서 참조할 수 없음

### export 키워드

```js
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
	return x * x;
}
```

* 모듈은 독자적인 모듈 스코프를 갖기 때문에 모든 식별자는 기본적으로 모듈 내부에서만 참조할 수 있음
* 모듈 내부에 선언된 식별자를 외부에 공개하기 위해서는 `export` 키워드를 사용해야 함
* 선언문 앞에 `export`를 넣어도 되고, export할 대상을 하나의 객체로 구성하여 한 번에 export할 수 있음

```js
export default x => x * x;
// 모듈에서 하나의 값만 export한다면 default 키워드를 사용할 수 있으며, 기본적으로 이름 없이 하나의 값을 export 한다. 이때 var, let, const 키워드는 사용할 수 없다.
```

* 모듈에서 하나의 값만 export한다면, `default` 키워드를 사용할 수 있으며, 이름 없이 값을 export함
* 이때 `var`, `let`, `const` 키워드는 사용할 수 없으며, `import`를 할 때 임의의 이름으로 import함

### import 키워드

```js
// 일반적인 사용
import { pi, square } from './lib.mjs';

// 모듈 내에서 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import함
import * as lib from './lib.mjs';

// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import함
import { pi as PI, square as sq, Person as P } from './lib.mjs';
```

* 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 `import` 키워드를 사용함
	* 로드를 하기 위한 파일명을 입력할 때 ESM의 경우 파일 확장자를 생략할 수 없음