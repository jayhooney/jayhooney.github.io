---
title: 'Typescript와 tsconfig.json'
excerpt: 'Typescript와 tsconfig.json에 대해 간단히 알아보기'
toc: true
toc_sticky: true
header:
  teaser: /assets/image/typescript_and_tsconfig.png

categories:
  - Typescript
tags:
  - Typescript
  - tsconfig.json
  - tsc-optimizer
last_modified_at: 2021-01-11
---

# tsconfig.json

## Typescript와 tsconfig.json 이란?

Typescript는 동적 타입 언어인 Javascript를 보완하기 위해 만들어진 언어입니다. Typescript로 개발된 소스는 컴파일을 통해 Javascript로 변환됩니다. 그리고 해당 컴파일에 필요한 옵션들을 지정하는 파일이 tsconfig.json 입니다.

## tsconfig.json 생성

아래 명령어를 실행시 tsconfig.json 파일이 생성됩니다.(nodejs와 typescript가 설치돼있어야 합니다.)

```
tsc --init
```

## Compile opitons

위 명령어를 실행시 기본적으로 생성되는 tsconfig.json은 아래와 같습니다.(영문 주석을 구글번역기로 한글로 번역했습니다.)

```json
{
  "compilerOptions": {
    /*이 파일에 대해 자세히 알아 보려면 https://aka.ms/tsconfig.json을 방문하십시오. */

    /* 기본 옵션 */
    // "incremental": true, /* 증분 컴파일 사용 */
    "target": "es5" /* ECMAScript 대상 버전 지정 : 'ES3'(기본값), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020'또는 'ESNEXT'. */,
    "module": "commonjs" /* 모듈 코드 생성 지정 : 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020'또는 'ESNext'. */,
    // "lib": [], /* 컴파일에 포함될 라이브러리 파일을 지정합니다. */
    // "allowJs": true, /* 자바 스크립트 파일이 컴파일되도록 허용합니다. */
    // "checkJs": true, /* .js 파일의 오류를보고합니다. */
    // "jsx": "preserve", /* JSX 코드 생성을 지정합니다 : 'preserve', 'react-native'또는 'react'. */
    // "declaration": true, /* 해당하는 '.d.ts'파일을 생성합니다. */
    // "declarationMap": true, /* 해당하는 각 '.d.ts'파일에 대한 소스 맵을 생성합니다. */
    // "sourceMap": true, /* 해당 '.map'파일을 생성합니다. */
    // "outFile": "./", /* 출력을 연결하여 단일 파일로 내 보냅니다. */
    // "outDir": "./", /* 출력 구조를 디렉토리로 리디렉션합니다. */
    // "rootDir": "./", /* 입력 파일의 루트 디렉토리를 지정합니다. --outDir로 출력 디렉토리 구조를 제어하는 ​​데 사용합니다. */
    // "composite": true, /* 프로젝트 컴파일 활성화 */
    // "tsBuildInfoFile": "./", /* 증분 컴파일 정보를 저장할 파일 지정 */
    // "removeComments": true, /* 주석을 출력하지 않습니다. */
    // "noEmit": true, /* 출력을 내 보내지 않습니다. */
    // "importHelpers": true, /* Import는 'tslib'에서 도우미를 내 보냅니다. */
    // "downlevelIteration": true, /* 'ES5'또는 'ES3'를 대상으로 할 때 'for-of', 스프레드 및 디스트 럭처링에서 이터 러블을 완벽하게 지원합니다. */
    // "isolatedModules": true, /* 각 파일을 별도의 모듈로 트랜스 파일합니다 ( 'ts.transpileModule'과 유사). */

    /* 엄격한 유형 검사 옵션 */
    "strict": true /* 모든 엄격한 유형 검사 옵션을 활성화합니다. */,
    // "noImplicitAny": true, /* 암시 된 'any'유형이있는 표현식 및 선언에서 오류를 발생시킵니다. */
    // "strictNullChecks": true, /* 엄격한 null 검사를 활성화합니다. */
    // "strictFunctionTypes": true, /* 함수 유형의 엄격한 검사를 활성화합니다. */
    // "strictBindCallApply": true, /* 함수에서 엄격한 '바인딩', '호출'및 '적용'메서드를 활성화합니다. */
    // "strictPropertyInitialization": true, /* 클래스에서 속성 초기화의 엄격한 검사를 활성화합니다. */
    // "noImplicitThis": true, /* 암시 된 'any'유형으로 'this'표현식에서 오류를 발생시킵니다. */
    // "alwaysStrict": true, /* Strict 모드에서 구문 분석하고 각 소스 파일에 대해 "use strict"를 내 보냅니다. */

    /* 추가 검사 */
    // "noUnusedLocals": true, /* 사용하지 않는 지역에 대한 오류를보고합니다. */
    // "noUnusedParameters": true, /* 사용하지 않는 매개 변수에 대한 오류를보고합니다. */
    // "noImplicitReturns": true, /* 함수의 모든 코드 경로가 값을 반환하지 않는 경우 오류를보고합니다. */
    // "noFallthroughCasesInSwitch": true, /* switch 문의 폴 스루 케이스에 대한 오류를보고합니다. */

    /* 모듈 확인 옵션 */
    // "moduleResolution": "node", /* 모듈 확인 전략을 'node'(Node.js) 또는 'classic'(1.6 이전 TypeScript)으로 지정합니다. */
    // "baseUrl": "./", /* 절대적이지 않은 모듈 이름을 확인하기위한 기본 디렉토리. */
    // "paths": {}, /* 'baseUrl'에 상대적인 조회 위치로 가져 오기를 다시 매핑하는 일련의 항목입니다. */
    // "rootDirs": [], /* 결합 된 콘텐츠가 런타임시 프로젝트의 구조를 나타내는 루트 폴더 목록입니다. */
    // "typeRoots": [], /* 유형 정의를 포함 할 폴더 목록입니다. */
    // "types": [], /* 컴파일에 포함될 유형 선언 파일. */
    // "allowSyntheticDefaultImports": true, /* 기본 내보내기없이 모듈에서 기본 가져 오기를 허용합니다. 이것은 코드 방출에 영향을 미치지 않고 단지 유형 검사에만 영향을 미칩니다. */
    "esModuleInterop": true /* 모든 가져 오기에 대한 네임 스페이스 개체 생성을 통해 CommonJS 및 ES 모듈 간의 상호 운용성을 방출합니다. 'allowSyntheticDefaultImports'를 의미합니다. */,
    // "preserveSymlinks": true, /* symlink의 실제 경로를 확인하지 않습니다. */
    // "allowUmdGlobalAccess": true, /* 모듈에서 UMD 전역 액세스 허용. */

    /* 소스 맵 옵션 */
    // "sourceRoot": "", /* 디버거가 소스 위치 대신 TypeScript 파일을 찾을 위치를 지정합니다. */
    // "mapRoot": "", /* 디버거가 생성 된 위치 대신 맵 파일을 찾을 위치를 지정합니다. */
    // "inlineSourceMap": true, /* 별도의 파일 대신 소스 맵이있는 단일 파일을 내 보냅니다. */
    // "inlineSources": true, /* 단일 파일 내에서 소스 맵과 함께 소스를 내 보냅니다. '--inlineSourceMap'또는 '--sourceMap'을 설정해야합니다. */

    /* 실험 옵션 */
    // "experimentalDecorators": true, /* ES7 데코레이터에 대한 실험적 지원을 활성화합니다. */
    // "emitDecoratorMetadata": true, /* 데코레이터의 유형 메타 데이터를 내보내는 실험적 지원을 사용합니다. */

    /* 고급 옵션 */
    "skipLibCheck": true /* 선언 파일의 유형 검사를 건너 뜁니다. */,
    "forceConsistentCasingInFileNames": true /* 동일한 파일에 대한 불일치 케이스 참조를 허용하지 않습니다. */
  }
}
```

필요에 따라 옵션을 추가,삭제하여 사용하면 됩니다.

## TSC compiler 최적화

AWS EC2의 t3a.micro 인스턴스(1C1G)에 개발한 서버를 배포후 빌드할 때 컴파일 단계에서 OOM이슈로 빌드가 되지 않는 이슈가 있었습니다.
그 때 참고한 포스팅 [optimizing-tsc](https://swatinem.de/blog/optimizing-tsc/). 저스펙환경에서 TS로 만든 프로젝트를 빌드할 경우 참고하기 좋은 글입니다.
