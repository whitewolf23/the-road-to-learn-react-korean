# 코드 구성과 테스트 Code Organization and Testing

규모가 큰 애플리케이션에서 코드를 유지 보수하는 방법에 대해 설명할 것입니다. 모범 예제를 통해 폴더와 파일 구성 방법과 조직적인 코드를 작성하는 방법에 배울 것입니다. 또한 테스트도 알아보겠습니다. 코드를 강력하게 유지하는 것은 중요합니다. 이번 장은 애플리케이션에서 한 걸음 물러나서 이 두 가지 주제를 설명할 것입니다.

## ES6 모듈: Import/Export Modules: Import/Export

ES6에서는 모듈에서 기능을 가져오고(import) 내보낼 수 있습니다(export). 이 기능은 변수를 지정할 수 있는 함수, 클래스 컴포넌트, 상수 등이 해당됩니다. 모듈은 단일 파일이거나 index 파일이 있는 폴더 전체가 될 수 있습니다.

처음에 *create-react-app*으로 부트스트래핑한 후, 초기 파일에 `import`과 `export` 문이 있는 것을 보았을 것입니다. 이제 이들에 대해 알아보도록 하겠습니다.

`import`와 `export` 문으로 여러 파일에 같은 코드를 공유할 수 있습니다. 이에 대한 다양한 방법이 있었지만 표준화된 가이드가 없었기 때문에 혼란스러웠습니다. ES6 이후 `import`와 `export`로 표준화가 되었습니다.

`import`와 `export`으로 코드 분할이 가능합니다. 코드를 여러 파일에 분산하면 재사용 및 유지 관리가 편리해집니다. 지금은 한 파일에서 코드를 작성하고 있기 때문에 나중에 꼭 해보길 바랍니다.

하지만 적어도 코드 캡슐화를 생각하는데 도움이 됩니다. 파일에 있는 모든 함수를 밖으로 내보내야하는 것은 아닙니다. 많은 기능 중 일부만 파일에 정의하여 사용하면 됩니다. 파일 내보내기(exports)는 기본적으로 모든 파일에서 사용 가능한 공용 API와 같은 개념입니다. 내보내진 함수만 다른 곳에서 재사용할 수 있습니다. 캡슐화를 보여주는 좋은 예이지요.

실제 `import`와 `export` 문을 어떻게 사용하는지 알아봅시다. 아래 예제는 두 파일(file1.js와 file2.js)에서 하나 이상의 변수를 공유합니다. 이처럼 여러 파일에서 `import`와 `export` 문으로 많은 변수들을 서로 공유할 수 있습니다. 

`export`문으로 하나 또는 그 이상의 변수를 내보낼 수 있습니다.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'robin';
const lastname = 'wieruch';

export { firstname, lastname };
~~~~~~~~

첫 번째 파일의 상대 경로를 지정해 다른 파일로 가져와봅시다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname, lastname } from './file1.js';

console.log(firstname);
// output: robin
~~~~~~~~

또한 내보내진 변수를 다른 파일에서 한 객체로 가져올 수 있습니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import * as person from './file1.js';

console.log(person.firstname);
// output: robin
~~~~~~~~

import 문에서 별명을 만들 수 있습니다. 여러 파일에 함수 이름이 중복될 경우, 별명을 만들어 가져올 수 있습니다. 아래와 같이 별명을 지정합니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname as foo } from './file1.js';

console.log(foo);
// output: robin
~~~~~~~~

마지막으로  `default` 문이 있습니다. 몇 가지 사용 사례가 있습니다.

* 단일 함수를 가져오고 내보낼 때
* 모듈 API 내 주요 함수임을 강조하기 위해 
* 폴백 import 기능(fallback import functionality)을 위해

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const robin = {
  firstname: 'robin',
  lastname: 'wieruch',
};

export default robin;
~~~~~~~~

중괄호를 생략하고 기본값 가져오기(default export)를 import로 가져올 수 있습니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer from './file1.js';

console.log(developer);
// output: { firstname: 'robin', lastname: 'wieruch' }
~~~~~~~~

가져온 이름값과 기본 값으로 내보낼 이름값을 서로 다르게 처리할 수 있습니다. 명명된 export와 import 문을 붙여 사용할 수 있습니다. 

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'robin';
const lastname = 'wieruch';

const person = {
  firstname,
  lastname,
};

export {
  firstname,
  lastname,
};

export default person;
~~~~~~~~

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer, { firstname, lastname } from './file1.js';

console.log(developer);
// output: { firstname: 'robin', lastname: 'wieruch' }
console.log(firstname, lastname);
// output: robin wieruch
~~~~~~~~


명명된 exports에서 직접 변수를 내보낼 수 있습니다.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
export const firstname = 'robin';
export const lastname = 'wieruch';
~~~~~~~~

ES6 모듈의 주요 기능을 살펴보았습니다. 짜임새있는 코드를 구성하고 유지 관리는 물론 재사용 가능한 모듈 API를 설계를 할 수 있습니다. 함수를 내보내고 가져와서 테스트를 할 수 있습니다. 다음 장에서 이 내용에 대해 알아보겠습니다.

### 읽어보기

* [[MDN] ES6 import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* [[MDN] ES6 export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## ES6 모듈로 코드 구성하기 Code Organization with ES6 Modules

*src/App.js* 파일에서 코드 분할을 하지 않은 이유에 대해 궁금할 겁니다. 이 파일의 코드는 파일/폴더(모듈)로 구분할 수 있는 여러 컴포넌트가 있습니다. 처음 리액트를 배울 떄는 모든 컴포넌트를 한 파일에 모으는 것이 좋습니다. 하지만 애플리케이션 규모가 커지면 컴포넌트를 모듈로 분할하는 것을 고려해야합니다. 규모가 큰 애플리케이션에서 사용하도록 합니다.

아래와 같이 모듈로 나눠 볼 수 있을 겁니다. 이 책을 다 끝내고 실습하길 바랍니다. 책 내용을 단순하게 만들기 위해 코드 분할을 하지 않고 *src/App.js* 파일만을 계속 사용하겠습니다.

제가 제안하는 모듈 구조는 아래와 같습니다.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App.js
  App.test.js
  App.css
  Button.js
  Button.test.js
  Button.css
  Table.js
  Table.test.js
  Table.css
  Search.js
  Search.test.js
  Search.css
~~~~~~~~

하지만 컴포넌트를 파일로 분리시켰을 뿐, 파일명은 같지만 확장자만 다른 파일들이 여러 있습니다. 아래와 같이 좀더 체계화된 모듈 구조로 만들 수 있습니다.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App/
    index.js
    test.js
    index.css
  Button/
    index.js
    test.js
    index.css
  Table/
    index.js
    test.js
    index.css
  Search/
    index.js
    test.js
    index.css
~~~~~~~~

이전보다 훨씬 깨끗해 보입니다. `index.js`은 폴더의 첫 진입점인 파일을 말합니다. 일반적인 네이밍 컨벤션으로 다른 이름을 사용해도 괜찮습니다. 한 모듈은 컴포넌트가 정의된 자바스크립트 파일, 스타일 파일, 테스트 파일로 구성되어 있습니다.

다음 단계는 App 컴포넌트에서 상수와 변수를 꺼내는 것입니다. 상수는 해커 뉴스 URL를 구성할 때 사용한 상수를 말합니다.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
# leanpub-start-insert
  constants/
    index.js
  components/
# leanpub-end-insert
    App/
      index.js
      test.js
      index..css
    Button/
      index.js
      test.js
      index..css
    ...
~~~~~~~~

자연스럽게 *src/constants/* 와 *src/components/* 로 나뉩니다. 이제 *src/constants/index.js* 파일은 아래와 같을 겁니다.

{title="Code Playground: src/constants/index.js",lang="javascript"}
~~~~~~~~
export const DEFAULT_QUERY = 'redux';
export const DEFAULT_HPP = '100';
export const PATH_BASE = 'https://hn.algolia.com/api/v1';
export const PATH_SEARCH = '/search';
export const PARAM_SEARCH = 'query=';
export const PARAM_PAGE = 'page=';
export const PARAM_HPP = 'hitsPerPage=';
~~~~~~~~

그리고  *App/index.js* 파일에서 사용할 변수를 가져옵니다.

{title="Code Playground: src/components/App/index.js",lang=javascript}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
} from '../constants/index.js';

...
~~~~~~~~

*index.js* 네이밍 컨벤션을 사용하면 상대 경로에서 파일 이름을 생략할 수 있습니다.

{title="Code Playground: src/components/App/index.js",lang=javascript}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
# leanpub-start-insert
} from '../constants';
# leanpub-end-insert

...
~~~~~~~~

*index.js* 파일에 어떤 일이 숨겨져있는 걸까요? 이 컨벤션은 node.js에서 도입되었습니다. index 파일은 모듈의 진입점으로 모듈의 퍼블릭 API를 말합니다. 외부 모듈은 *index.js* 파일을 사용해 모듈에서 퍼블릭 코드를 가져올 수 있습니다. 아래와 같은 구조를 예를 들어봅시다.


{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  App/
    index.js
  Buttons/
    index.js
    SubmitButton.js
    SaveButton.js
    CancelButton.js
~~~~~~~~

The *Buttons/* 폴더에는 여튼 버튼 컴포넌트가 있습니다. 각 파일에  `export default`문으로 컴포넌트를 내고 *Buttons/index.js*으로 가져올 수 있습니다. *Buttons/index.js* 파일은 모든 버튼을 가져와 퍼블릭 모듈 API로서 내보냅니다.

{title="Code Playground: src/Buttons/index.js",lang="javascript"}
~~~~~~~~
import SubmitButton from './SubmitButton';
import SaveButton from './SaveButton';
import CancelButton from './CancelButton';

export {
  SubmitButton,
  SaveButton,
  CancelButton,
};
~~~~~~~~

*src/App/index.js*는 *index.js* 파일에 있는 퍼블릭 모듈 API에서 버튼을 가져올 수 있습니다.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
import {
  SubmitButton,
  SaveButton,
  CancelButton
} from '../Buttons';
~~~~~~~~

이렇게 제약조건을 두면, 모듈 내 *index.js*가 아닌 다른 파일로 접근하는 것은 좋지 않습니다. 캡슐화 규칙을 깨트리기 때문입니다.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
// 이렇게 하지 마세요.
import SubmitButton from '../Buttons/SubmitButton';
~~~~~~~~

여러분은 이번 장에서 캡슐화 제약으로 소스 코드를 리팩토링을 할 수 있는 방법을 배웠습니다. 앞에서 말씀드렸듯이, 이 책의 내용을 복잡하게 만들지 않고 간결하게 하기 위해 리팩토링하지 않을 겁니다. 이 책을 모두 마치고 숙제로 해보길 바랍니다.

### 실습하기

* 책을 다 마친 후 *src/App.js*파일을 여러 파일로 나누어 컴포넌트 모듈로 만듭니다.

## Jest 스냅샷테스트 Snapshot Tests with Jest

이 책에서 테스트를 깊게 다루지 않지만, 꼭 알고 있어야할 주제입니다. 프로그래밍에서 테스트는 필수입니다. 테스트야 말로 코드의 품질을 높이고 모든 것이 문제없이 잘 작동함을 보장합니다.

테스트 피라미드(testing pyramid)를 한 번쯤 들어봤을 겁니다. 테스트 미드는 종단간 테스트(end-to-end test), 통합 테스트(integration test), 단위 테스트(unit test)로 구성됩니다. 익숙하지 못한 분들을 위해 간략히 설명하겠습니다. 단위 테스트는 독립적이고 작은 코드 블록을 테스트 합니다. 단위 테스트 함수는 단일 함수가 됩니다. 그러나 독립적으로 문제가 없지만 여러 유닛을 결합하는 경우 잘 작동하지 않기도 합니다. 유닛을 그룹으로 묶어 테스트를 진행해야 합니다. 이를 위해 통합 테스트는 각 유닛들이 잘 작동하는지 확인할 수 있습니다. 마지막으로 실제 사용자 시나리오 시물레이션하는 종단간 테스트를합니다. 그 예로 로그인과 같은 기능을 웹 자동화 테스트(Web Automation Testing)로 수행할 수 있습니다.

그렇다면 각 유형별로 몇 개의 테스트가 필요할까요? 독립적인 함수를 위해 일 경우 많은 단위 테스트를 필요합니다. 그 다음 잘 작동하는 지 확인하기 위해 몇 가지 통합 테스트를 수행할 수 있습니다. 마지막으로 최종 사용자 시나리오를 점검하기 위해 몇 가지 종단간 테스트만 수행하는 것이 좋습니다. 지금까지 일반적인 테스트 과정에 대해 설명했습니다.

그렇다면 리액트 애플리케이션에 테스트를 어떻게 진행해야 할까요? 기본적인 리액트 테스트는 컴포넌트 테스트로 테스트로 유닛 테스트와 스냅샷 테스트입에 해당합니다. 다음 장에서 Enzyme 라이브러리로 테스트 코드를 작성해보겠습니다. 이번 장에서는 또다른 테스트 유형인 [Jest(제스트)](https://facebook.github.io/jest/)을 가지고 스냅샷 테스트에(snapshot test) 대해 알아보겠습니다.  

[제스트](https://facebook.github.io/jest/)은 페이스북에서 사용하는 자바스크립트 테스트 프레임워크로 컴포넌트 테스트를 위해 사용합니다. *create-react-app*에 이미 제스트가 있어서 새로 설치할 필요가 없습니다.

첫 번째 컴포넌트를 테스트해봅시다. 그 전에  *src/App.js*파일에서 테스트할 컴포넌트를 내보내야 합니다. 그렇게 해야 다른 파일에서 테스트할 수 있기 때문입니다. 이 내용은 이전 장에서 배웠던 것입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

class App extends Component {
  ...
}

...

export default App;

# leanpub-start-insert
export {
  Button,
  Search,
  Table,
};
# leanpub-end-insert
~~~~~~~~

 *create-react-app*에서제공하는 *App.test.js* 파일이 바로 첫 번째 테스트 파일입니다. 이 테스트는 App 컴포넌트가 오류없이 렌더링되고 있는지를 확인합니다.
 
{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
});
~~~~~~~~

"it"문은 테스트 케이스 입니다. 테스트 내용이 뒤따르고 테스트 시 성공 또는 실패합니다. 추후에 "describe" 문으로 "it" 전체를 테스트 슈트(test suit)로 감쌀 수 있습니다. 테스트 슈트는 하나의 특정 컴포넌트의 "it"문이 포함됩니다. 나중에 "describe"문을 확인하길 바랍니다. 이들 모두 테스트 케이스를 분리하고 구성하는데 사용합니다.

커맨드 라인에서  *create-react-app* 테스트 스크립트를 사용해 테스트를 진행할 수 있습니다. 모든 테스트 케이스에 대한 출력을 볼 수 있습니다. 아래 명령어로 확인해봅시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm test
~~~~~~~~

**중요** App 컴포넌트 유닛 테스트에서 오류가 발생하면, `componentDidMount()`메서드 안에 `fetchSearchTopStories()`에서 fetch 메서드가 작동하지 않음을 말합니다. 위 경우 아래 가이드를 참고해 해결합시다.

* 커맨드 라인에서 다음 명령어로 패키지를 설치합니다. `npm install isomorphic-fetch`
* *App.js* 파일에 다음 코드를 추가합니다: `import fetch from 'isomorphic-fetch';`

제스트를 사용해 스냅샷 테스트를 작성해봅시다. 이 테스트는 렌더링된 테스트의 스냅샷 만들어 차기 스냡샷(future snapshot)에 대해 기존 스냡샷을 실행합니다. 차기 스냡샷이 변경되면, 테스트가 알려줍니다. 오류 의도적으로 컴포넌트를 수정했을 경우, 또는 수정을 원치않은 경우에 스냅샷 변경을 수락할 수 있습니다. 렌더링 결과와 diffs(차이)만 테스트하기 때문에 단위 테스트를 잘 보완합니다.

컴포넌트 내 렌더된 출력을 위해 수정할 때, 변경된 스냅샷만 간단히 수락하면 되기 때문에 관리하기 어렵지 않습니다.

제스트는 스냅샷을 폴더에 저장합니다. 이런 방법으로 차기 스냡샷과 비교해 diff를 검증합니다. 또한 단일 폴더에 스냅샷을 공유할 수 있습니다.

제스트로 첫 번째 스냅샷 테스트를 작성하기 전, `react-test-renderer` 유틸리티 라이브러리를 설치합시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev react-test-renderer
~~~~~~~~

이제 첫 번째 스냡샷 테스트로 App 컴포넌트를 확장합시다. 먼저 노드 패키지에서 설치한 라이브러리를 가져오고, App 컴포넌트를 "it" 문으로 감싼 다음, 다시 "describe" 문으로 감쌉니다. 이 경우 테스트 수트는 App 컴포넌트입니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
# leanpub-start-insert
import renderer from 'react-test-renderer';
# leanpub-end-insert
import App from './App';

# leanpub-start-insert
describe('App', () => {
# leanpub-end-insert

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
  });

# leanpub-start-insert
});
# leanpub-end-insert
~~~~~~~~

이제 "test" 블록으로 첫 번째 스냅샷 테스트를 작성합시다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import App from './App';

describe('App', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
  });

# leanpub-start-insert
  test('has a valid snapshot', () => {
    const component = renderer.create(
      <App />
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });
# leanpub-end-insert

});
~~~~~~~~

다시 테스트를 실행해 테스트가 성공 또는 실패했는지 확인하세요. 성공해야 합니다. App 컴포넌트 내 렌더 내 출력을 변경하면 스냅샷 테스트가 실패합니다. 스냅샷을 업데이트하거나 App 컴포넌트를 확인할 수 있습니다.

`renderer.create()` 함수는 App 컴포넌트 스냅샷을 생성합니다. 가상으로 렌더링하여 스냅샷에 DOM을 저장합니다. 스냅샷은 마지막 실행한 스냡샷과 일치하는지 확인합니다. 이런 방식으로 DOM이 동일하게 유지되고 실수로 아무것도 변경되지 않은 것을 확인합니다.

독립적인 컴포넌트를 대상으로 테스트를 작성해봅시다. 

첫 번째, Search 컴포넌트부터 시작합시다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import App, { Search } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Search', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Search>Search</Search>, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Search>Search</Search>
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Search 컴포넌트는 App 컴포넌트와 비슷한 두 가지 테스트가 있습니다. 첫 번째 테스트는 Search 컴포넌트를 DOM에 렌더링하고 렌더링 프로세스 중에 오류가 없는지 확인합니다. 오류가 있는 경우, 테스트 블록 내 expect, match, equal 등 어설(assertion)이 없어도 테스트가 중단됩니다. 두 번째 스냅샷 테스트는 렌더링된 컴포넌트의 스냅샷을 저장하고 이전 스냅샷과 비교하여 실행합니다. 스냅샷이 번경되면 실패합니다.

두 번째, Button 컴포넌트는 Search 컴포넌트와 동일한 테스트 규칙이 적용되지만 테스트를 할 수 있습니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...
# leanpub-start-insert
import App, { Search, Button } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Button', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Button>Give Me More</Button>, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Button>Give Me More</Button>
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

마지막으로 Table 컴포넌트는 목록을 렌더링하기 위해 초기 props를 전달합니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...
# leanpub-start-insert
import App, { Search, Button, Table } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Table { ...props } />, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Table { ...props } />
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

스냅샷 테스트는 매우 기본적인 것만 다룹니다. 컴포넌트가 출력을 변경하지 않는지만 확인하면 됩니다. 일단 출력을 변경하면 변경 사항을 수락할지 여부를 결정해야 합니다. 그렇지 않으면 원하는 출력이 아닐 경우, 컴포넌트를 수정해야 합니다.

### 실습하기

* `render()`메서드에서 컴포넌트 리턴 값을 변경하면 스냅샷 테스트가 실패하는지 확인합니다.
  * 스냅샷 변경을 수락하거나 거부합니다.
* 다음 장에서 컴포넌트가 변경될 때마다 스냅샷 테스트를 최신 상태로 유지합니다.

### 읽어보기
* [[리액트 공식문서] Jest](https://facebook.github.io/jest/docs/tutorial-react.html)

## Enzyme 유닛 테스트

[Enzyme(엔자임)](https://github.com/airbnb/enzyme)는 에어비앤비가 만든 테스트 유틸리티로 리액트 컴포넌트를 확인, 조작, 통과시킵니다. 엔자임은 스냅샷 테스트를 보완하여 단위 테스트를 수행합니다.

엔자임을 어떻게 사용하는지 알아봅시다. 첫째, 엔자임은 *create-react-app*에서 제공하지 않기 때문에 먼저 설치합시다. 리액트 확장판도 함께 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16
~~~~~~~~

둘째, 엔자임을 테스트 설정에 추가하고 리액트가 사용할 수 있게 어댑터를 초기화합시다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
# leanpub-end-insert
import App, { Search, Button, Table } from './App';

# leanpub-start-insert
Enzyme.configure({ adapter: new Adapter() });
# leanpub-end-insert
~~~~~~~~

Table 컴포넌트의 "describe"블록에 첫 번째 단위 테스트를 작성합시다. You will use `shallow()`를 사용하여 컴포넌트를 렌더링하고 두 개의 목록을 전달하기 때문에 두 항목이 있다고 주장합니다. 어설션은 `table-row` 클래스에 두 요소가 있는지 간단히 체크합니다. 

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme, { shallow } from 'enzyme';
# leanpub-end-insert
import Adapter from 'enzyme-adapter-react-16';
import App, { Search, Button, Table } from './App';

...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  ...

# leanpub-start-insert
  it('shows two items in list', () => {
    const element = shallow(
      <Table { ...props } />
    );

    expect(element.find('.table-row').length).toBe(2);
  });
# leanpub-end-insert

});
~~~~~~~~

`shallow()`는 자식 컴포넌트가 없는 컴포넌트를 렌더링합니다. 이런 방식으로 하나의 컴포넌트를 전담해 테스트를 실시합니다.

엔자임은 API에 세 가지 렌더링 메커니즘을 가지고 있습니다. `shallow()`이외에 `mount()`와 `render()`도 있습니다. 두 인스턴스 모두 부모 컴포넌트 및 하위 컴포넌트의 인스턴스를 인스턴스화합니다. 또한 `mount()`는 컴포넌트 생명주기 메서드를 접근할 수 있습니다. 어떤 렌더링 매커니즘을 사용해야 할까요? 아래 몇 가지 규칙이 있습니다.

* shallow 테스트를 제일 먼저 시작합니다. 
* `componentDidMount()` 또는 `componentDidUpdate()` 테스트 시, `mount()`를 사용합니다.
* 컴포넌트의 생명주기와 자식 컴포넌트를 테스트 시, `mount()`를 사용합니다.
* `mount()`보다 적은 오버헤드(overhead)로 자식 렌터링을 테스트하고 생명주기 메서드와 무관하다면 `render()`을 사용합니다.

컴포넌트 테스트를 계속할 수 있습니다. 테스트를 간단하고 유지가능하게 만들도록 합니다. 그렇지 않으면 컴포넌트를 변경한 후에 리팩토링해야 합니다. 페이스북이 제스트와 함께 스냅샷 테스트를 도입한 이유가 바로 이 때문입니다.

### 실습하기

* Button 컴포넌트에 대한 Enzyme 유닛 테스트 코드를 작성해봅니다.
* 다음 장에서 유닛 테스트를 최신 상태로 유지합니다.

### 읽어보기

* [enzyme과 API 렌더링](https://github.com/airbnb/enzyme)

## PropTypes 컴포넌트 인터페이스 Component Interface with PropTypes

자바스크립트 타입 인터페이스인 [타입스크립트(TypeScript)](https://www.typescriptlang.org/) 또는 [플로우(Flow)](https://flowtype.org/)에 대해 한 번쯤 들어보았을 겁니다. 프로그래밍 된 텍스트는 프로그램 텍스트를 기반으로 유효성을 검사하므로 입력 오류가 발생하기 쉽습니다. 프로그램이 실행되기 전 에디터와 유틸리티로 오류를 체크할 수 있습니다. 프로그램을 더 강력하게 만들어 줍니다. 

이 책에서는 Flow 또는 TypeScript를 소개하지 않지만 컴포넌트 유형을 체크하는 방법으로 PropType를 사용합니다. 리액트는 버그를 방지하고자 내장된 타입 검사기가 있습니다. PropType는 컴포넌트 인터페이스입니다. 부모 컴포넌트에서 하위 컴퍼넌트로 전달되는 모든 props는 PropTypes 인터페이스에 따라 유효 여부를 점검받습니다. 

이 장에서는 PropType을 사용하여 모든 컴포넌트 타입을 안전하게 만들 수 있는 방법을 배웁니다. 다음 장을 위해 변경 내용을 생략할 것입니다. 불필요한 코드 리팩터링을 추가하기 때문입니다. 그러나 컴퍼넌트 인터페이스 타입을 안전하게 유지하기 위해 지속적으로 업데이트해야 합니다. 

먼저 리액트와 별도로 PropType 패키지를 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install prop-types
~~~~~~~~

다음 PropTypes를 불러옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
import PropTypes from 'prop-types';
# leanpub-end-insert
~~~~~~~~

컴포넌트에 props 인터페이스를 할당해봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({ onClick, className = '', children }) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

# leanpub-start-insert
Button.propTypes = {
  onClick: PropTypes.func,
  className: PropTypes.string,
  children: PropTypes.node,
};
# leanpub-end-insert
~~~~~~~~

기본적으로 이 것이 다 입니다. 함수 시그니처에서 모든 인자를 가져와 PropType을 할당합니다. PropType 은 아래와 같습니다.

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

또한 렌더링 가능한 노드(문자열, 리액트 요소 등)를 정의하는 PropType가 있습니다.

* PropTypes.node
* PropTypes.element

Button 컴포넌트에 이미 `node` PropType를 사용했습니다. 리액트 공식문서에서 더 PropType의 정의가 더 많이 있습니다.

Button에 정의된 모든 PropTypes은 선택 사항입니다. 매개 변수는 null이거나 정의되지 않을 수 있습니다. 그러나 여러 Props가 있는 경우, props를 반드시 정의해야 합니다. 컴포넌트로 props를 전달하는 것이 필수사항으로 설정할 수 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
Button.propTypes = {
# leanpub-start-insert
  onClick: PropTypes.func.isRequired,
# leanpub-end-insert
  className: PropTypes.string,
# leanpub-start-insert
  children: PropTypes.node.isRequired,
# leanpub-end-insert
};
~~~~~~~~

`className`의 기본값이 빈 문자열임으로 필수가 아닙니다. 다음 Table 컴포넌트의 PropType를 작성합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
Table.propTypes = {
  list: PropTypes.array.isRequired,
  onDismiss: PropTypes.func.isRequired,
};
# leanpub-end-insert
~~~~~~~~

`list` 내 각 프로퍼티의 PropType는 배열로 간결하게 정의합시다. 

{title="src/App.js",lang=javascript}
~~~~~~~~
Table.propTypes = {
  list: PropTypes.arrayOf(
    PropTypes.shape({
      objectID: PropTypes.string.isRequired,
      author: PropTypes.string,
      url: PropTypes.string,
      num_comments: PropTypes.number,
      points: PropTypes.number,
    })
  ).isRequired,
  onDismiss: PropTypes.func.isRequired,
};
~~~~~~~~

다른 프로퍼티 중 `objectID`의 PropTypes은 필수항목입니다. 왜냐하면 코드 일부가 `objectID`에 의존하기 때문입니다. 다른 프로퍼티는 애플리케이션에 표시만 때문에 필수항목은 아닙니다. 또한 해커 뉴스 API가 배열 내 각 객체에 대해 항상 정의된 프로퍼티를 가지고 있다고 단언할 수 없습니다.

지금까지 PropTypes에 대해 알아보았습니다. 그러나 한 가지가 더 남았습니다. 컴포넌트에 디폴트 props를 정의할 수 있습니다. Button 컴포넌트를 다시 봅시다. `className` 프로퍼티는 컴포넌트 시그니처에 기본 매개 변수를 가지고 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children
}) =>
  ...
~~~~~~~~

기본 매개 변수를 리액트 기본값 props으로 바꿔봅시다.
{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Button = ({
  onClick,
  className,
  children
}) =>
# leanpub-end-insert
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

# leanpub-start-insert
Button.defaultProps = {
  className: '',
};
# leanpub-end-insert
~~~~~~~~

ES6 기본 매개 변수와 동일하게, default prop은 부모 컴포넌트가 지정하지 않은 경우, 프로퍼티가 기본 값을 설정합니다. PropType 타입 검사는 default prop가 평가된 후에 수행됩니다.   Same as the ES6 default parameter,  the default prop ensures that the property is set to a default value when the parent component didn't specify it. The PropType type check happens after the default prop is evaluated.
. 
테스트를 다시 실행해, 커맨드 라인에서 컴포넌트 PropType 오류가 있는지 확인합니다. PropType 정의가 필수적인 테스트에서는 컴포넌트의 모든 props가 정의되지 않았기 때문에 오류가 발생할 수 있습니다. 테스트 자체는 통과됩니다. 이 오류를 피하기 위해 요구되는 모든 props를 전달합니다.

### 실습하기

* Search 컴포넌트의 PropType를 정의합니다.
* 다음 장에서 컴포넌트를 추가하거나 수정할 때 PropType도 함께 추가합니다.

### 읽어보기
* [[리액트 공식문서] 리액트 PropTypes](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)

{pagebreak}

여러분은 코드 구성과 테스트 방법을 배웠습니다! 이번 장에서 배운 내용을 정리해봅시다.

* 리액트
  * PropTypes는 컴포넌트의 타입을 정의합니다.
  * Jest는 컴포넌트의 스냅샷 테스트를 시행합니다.
  * Enzyme은 컴포넌트 유닛 테스트를 시행합니다.
* ES6
  * `import`와 `export` 문으로 코드를 구성합니다.
* 일반
  * 코드 구성은 애플리케이션을 확장할 수 있는 좋은 방법입니다.

실습 코드는 [깃허브 리퍼지토리](https://github.com/rwieruch/hackernews-client/tree/4.2)에서 확인할 수 있습니다.