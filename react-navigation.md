# react-navigation

> react-navigation@6 을 기반로 작성

## [[typescript]] 설정
### nested navigator
+ https://javascript.plainenglish.io/react-navigation-v6-with-typescript-nested-navigation-part-2-87844f643e37
+ https://github.com/deptno/salji.ro/commit/f91889c48a35a7ac5fb382045df77b7a4d2c4825

## 문서
+ https://reactnavigation.org/docs/getting-started

### Fundamentals
#### Getting started
- install `@react-navigation/native` 종속성
  - `react-native-screens`
    - android navive 파일 수정 필요, 문서 참고
  - `react-native-safe-area-context`
  - and run cmd `bundle exec pod install`
- `index.js` or `App.tsx` 에서 `<NavigationContainer>` 로 감싸야한다

#### Hello React Navigation
- screen 간 전환 및 history 관리
- web과 다른점은 제스쳐와 애니메이션을 제공
- install
  - `@react-navigation/native-stack`
- `createNativeStackNavigator` 는 `Navigator`, `Screen` 을 리턴하고 이들로 네비게이터 구성
  - `Navigator` 에 `initialRouteName` 을 넣어 초기 스크린을 지정할 수 있다
  - Screen 의 component prop 에 inline 함수를 넣지 말것,  fast refresh 시에 state 유지 안됨
  - 기본옵션은 `Navigator.screenOptions` 에 override 를 위한 옵션은 `Screen.options` 주입한다
- `routeName` 는 case *in*sensitive
- Component 와 함께 RouteName static property 를 넣어주면 코드에서 `map` 돌리기 편할 것으로 기대
- `Screen` 에 children 으로 function: ReactElement 를 넣을 수 있으나 추천하지 않는다 주입은 Context 를 이용하라
  - 퍼포먼스 optimize 가 깨진다

#### Moving between screens
- Screen Component 는 `navigation` prop 을 주입받고 이를 통해 `navigation.navigate(ROUTE_NAME)` 함수로 스크린 전환
- `navigate` 는 동일 screen 으로의 이동을 무시한다
  - [ ] TODO: history 스택에 있는경우 이를 찾는다고 되어있다. 히스토리에 해당 스크린 이후 스크린들은 어떻게되는건지 확인 필요
    - summary 에 보면 해당 스크린 이후의 내용은 제거되는 것으로 보인다
- `push` 는 동일 스크린도 history 에 있는지 여부와 상관없이 추가한다
- `navigation.back` 버튼은 이전 스크린으로 이동, 유일한 스크린은 경우 `back` 버튼 자체가 노출되지 않음
  - [[android]] back 버튼도 동일한 함수를 trigger 한다
- `popToTop` 을 사용하여 *스택*의 첫번째 스크린으로 이동할 수 있다

#### Passing parameters to routes
- `navigate` 의 두번째로 `object` 를 전달
  - Screen 컴포넌트에서는 `route.params` 를 통해 받을 수 있다
- Screen 컴포넌트 *선언*  시에 `initialParmas` 을 주입할 수 있으며 이 경우 개별 params와 머지 후 전달된다
- `navigation.setParams` 을 통해서 자신의 `params` 을 업데이트할 수 있다
  - `options` 과는 다르니 주의하자,  `navigation.setOptions` 이 따로 있다
- `params` 을 previus screen 으로 전달할때는 `navigation.naviate` 를 이용해서 전달하자
  - [ ] TODO: `params: true` 옵션이 예제에 있는데 이건 찾아봐야한다
- 중첩 네비게이터를 사용하는 경우는 네비게이터가 포함된 스크린의으로 naviate를 하면서 추가적으로 `screen` 속성을 전달한다
```js
navigation.navigate('Account', {
  screen: 'Settings',
  params: { user: 'jane' },
});
```
- `params` 는 화면을 표현하는데 필요한 정보가 들어가야한다.
  - 여러 스크린에 걸친 데이터는 전역스토어로 간다.
  - 프리미티브를 전달해서 만료된 데이터를 보여지지 않도록 한다
  - `params` 이 후킹당했다고 생각했을때 핸들링이 가능해야한다
  - 최소화한다

#### Configuring the header bar
- Screen Component 의 `options` prop 을 통해서 `title` 등을 헤더바에 설정할 수 있다
  - 해당 prop 에 오브젝트 대신 함수쓰게되면 `{ navigation, route }` 를 주입받아 `route.params` 를 활용할 수 있게 된다
- `navigation.setOptions` 를 통해서 헤더 업데이트가 가능
- `headerStyle`, `headerTintColor`, and `headerTitleStyle` 를 통햇 헤더 스타일 제어
- `navigation.screenOptions` 를 Screen 전역에 걸쳐 설정이 가능하다
- `Screen.options` 에 `headerTitle` 를 통해서 ReactComponent 로 완전한 헤더 대체가 가능하다
  - 기본값은 `<Text />` 컴포넌트
  - `headerLeft`, `headerRight` 속성을 통해서 커스텀 컴포넌트 주입이 가능하다
    - Screen 컴포넌트와 상호작용이 필요한 경우 `setOptions` 을 통해 동일 한 UI를 한번더 그리면서 handler 를 **주입**한다
- [ ] TODO: 백 버튼 내용은 사용할때 참조할 것

#### Nesting navigators
- nested route 에 params 을 주입한 경우 그 사이의 부모는 해당 params 에 접근하지 못한다
- 핸들링 되지 않은 애들은 부모로 bubble up 된다
  - *설계에 참고* 예를 들어 `navigation.toggleDrawer` 를 한경우 부모에 `DrawerNaviagor`가 존재해야지 처리된다
  - 반대의 경우라면 `navigation.dispatch(DrawerActions.toggleDrawer())` 형태로 `dispatch` 가 가능하다
  - 중첩된 네비게이터 안의 스크린은 부모의 이벤트를 받지 못한다 받기위해서는 명시적으로 리스너를 달아야한다
```js
const unsubscribe = navigation
  .getParent('MyTabs')
  .addListener('tabPress', (e) => {
    // Do something
  });
```
- [ ] TODO: 패턴이 많아서 사용할 때 읽어볼것

#### Navigation lifecycle
- [ ] Q: a navigate b 를 하고 b nativate a 를 한 경우 b 는 unmount 되는지?
- `navigation` 에 `focus`, `blur` 이벤트가 존재
  - `useFocusedEffect`
  - `useIsFocused` 훅도 존재



## link
- [[react-native]]
