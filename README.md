# ⚛️React + 🟦TypeScript + 🐈‍⬛Yarn Berry + ⚡️Vite + ⚡️Vitest + 🐙React testing library + 📕Storybook

## 소개

React를 위한 Template입니다.
Yarn Berry를 사용해 PnP 방식으로 의존성을 관리합니다. Vite React + TS를 기반으로 PnP 및 test 환경 구성을 위한 설정들이 포함되어 있습니다.

## 사용 스택

- React
- Typescript
- Yarn berry
- Vite
- Vitest
- React testing library
- Storybook

## Yarn Berry 및 Vite 구성

1. yarn을 초기화합니다.

```bash
corepack enable
yarn init -2

# 전역 캐시 대신 프로젝트 내 .yarn/cache 경로에 캐시할 경우
yarn config set enableGlobalCache false
```

2. `yarn create vite .`명령어로 현재 경로에 vite을 초기화합니다. 파일 관련 옵션에서 _Ignore files and continue_ 선택하여 파일을 덮어씁니다.
3. Vite 캐시 설정을 수정합니다.

```typescript
// vite.config.ts
...생략
export default defineConfig({
...다른 설정들
cacheDir: "./.vite",
}
```

4. `yarn`명령어로 의존성을 설치합니다.
5. `yarn dlx @yarnpkg/sdks vscode`을 실행하여 vscode 관련 설정을 추가합니다.

## Testing 환경 구성

1. 개발 의존성을 추가합니다.

```bash
yarn add -D jsdom vitest @vitest/coverage-v8 @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

2. eslint 설정에 플러그인을 추가합니다.

```javascript
// .eslintrc.cjs
module.exports = {
... 다른 설정들
plugins: ["react-refresh", "testing-library", "jest-dom"],
// ovverrides로 테스트 파일에만 룰 적용
overrides: [
	{
		files: ["**/?(*.)+(spec|test).[jt]s?(x)"],
		extends: ["plugin:testing-library/react", "plugin:jest-dom/recommended"],
		},
	],
};
```

## Storybook 설치

1. `yarn dlx storybook@latest init` 명령어를 실행하여 storybook을 초기화합니다.
2. Storybook은 `node_modules/.cache` 경로를 cache 기본 경로로 사용하기 때문에, pnp와 상관없이 node_modules가 생성될 수 있습니다.

## VSCode 설정

- vscode에서 intellisense 사용하려면 프로젝트에 포함된 typescript를 사용해야합니다. `.vscode/settings.json`파일의 `"typescript.tsdk": ".yarn/sdks/typescript/lib"` 설정을 참고해주세요.
- pnp 방식은 zip파일을 이용하기 때문에, vscode에 ZipFS 확장 프로그램을 설치해야 typescript가 의존성을 읽을 수 있습니다.
- VS Code 1.89이상 버전에서 tsserver가 실행이 안되는 vsCodeWatcher 관련 에러가 있습니다. setting.json에서 `"typescript.tsserver.experimental.useVsCodeWatcher"`를 `false`로 설정해야 합니다.

## .gitignore 추가 내용

```bash
# .gitignore

# vscode 설정 버전관리에 포함
!.vscode/settings.json

# yarn 관련 설정(zero-install 미사용)
.yarn/*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/sdks
!.yarn/versions
.pnp.*

# vite cache 제외
.vite

# Test coverage
coverage

# Storybook
storybook-static
*storybook.log
```

## 참고 링크

- yarn berry 설치 : https://yarnpkg.com/getting-started/install
- volta에서 corepack 설치 : https://yarnpkg.com/corepack#volta
- vite : https://ko.vitejs.dev/guide/
- react-testing-library 설치 : https://testing-library.com/docs/react-testing-library/intro/
- vitest 설치 : https://vitest.dev/guide/
- eslint-plugin-testing-library : https://github.com/testing-library/eslint-plugin-testing-library
- eslint-plugin-jest-dom : https://github.com/testing-library/eslint-plugin-jest-dom
- storybook 설치 :
- Storybook 설치/cache관련: https://storybook.js.org/docs/get-started/install
- vscode tsserver codeWatcher 에러 관련 이슈 : https://github.com/microsoft/vscode/issues/212042
- ZipFS : https://marketplace.visualstudio.com/items?itemName=arcanis.vscode-zipfs
