
### `moduleDetection`
typescript 는 기본적으로 모든 ts파일을 전역 모듈로 본다. 때문에 별도 설정 없이 다른 파일에서 똑같은 변수명을 선언하면 에러가 발생한다. <br/>
아래의 두 변수는 결국에는 같은 영역 안에 있는 것으로 인식되어 에러가 노출된다.
```javascript
// index.ts
const a = 1;

// hello.ts 
const a = 1; // error: 블록 범위 변수 'a'를 다시 선언할 수 없습니다. 
```
-  해결 방법은??
1. `import`, `export {}` 같은 모듈 시스템을 사용하는 키워드를 하나라도 사용하면 해당 파일은 격리된 모듈로 판단되므로 해결된다.
2. tsconfig.json 의 `compilerOptions` 안에 `moduleDetection` 옵션값을 `'force'`로 설정한다. <br/>
(사실 1번의 모듈 시스템 키워드를 자동으로 추가해주는 옵션이다. 해당 추가 코드는 `module` 옵션이 commonJS냐 exNext냐 같은 거에 따라 달라진다.)

