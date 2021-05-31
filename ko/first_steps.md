### 첫 단계

이 문서에서는 Nest **의 핵심 기본 사항에 대해 알아 봅니다.** Nest 애플리케이션의 필수 구성 요소에 익숙해지기 위해 입문 수준에서 많은 부분을 다루는 기능이 포함 된 기본 CRUD 애플리케이션을 빌드 할 것입니다.

#### 언어

우리는 [TypeScript](https://www.typescriptlang.org/) 를 좋아하지만 무엇보다도 [Node.js](https://nodejs.org/en/) 를 좋아합니다. 이것이 Nest가 TypeScript 및 **순수 JavaScript** 와 호환되는 이유입니다. Nest는 최신 언어 기능을 활용하므로 바닐라 JavaScript와 함께 사용하려면 [Babel](https://babeljs.io/) 컴파일러가 필요합니다.

우리가 제공하는 예제에서는 대부분 TypeScript를 사용하지만 **코드 조각** 을 항상 바닐라 JavaScript 구문으로 전환 할 수 있습니다 (각 조각의 오른쪽 상단에있는 언어 버튼을 클릭하여 전환하기 만하면됩니다).

#### 전제 조건

[운영 체제에 Node.js](https://nodejs.org/) (&gt; = 10.13.0)가 설치되어 있는지 확인하십시오.

#### 설정

새 프로젝트를 설정하는 것은 [Nest CLI를](/cli/overview) 사용하면 매우 간단합니다. [npm이](https://www.npmjs.com/) 설치되면 OS 터미널에서 다음 명령을 사용하여 새 Nest 프로젝트를 만들 수 있습니다.

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
```

`project` 디렉토리가 생성되고 노드 모듈과 몇 가지 다른 상용구 파일이 설치되며 `src/` 디렉토리가 생성되고 여러 핵심 파일로 채워집니다.

<div class="file-tree">
  <div class="item">src</div>
  <div class="children">
    <div class="item">app.controller.ts</div>
    <div class="item">app.controller.spec.ts</div>
    <div class="item">app.module.ts</div>
    <div class="item">app.service.ts</div>
    <div class="item">main.ts</div>
  </div>
</div>

다음은 이러한 핵심 파일에 대한 간략한 개요입니다.

 |
--- | ---
`app.controller.ts` | 단일 경로가있는 기본 컨트롤러.
`app.controller.spec.ts` | 단위는 컨트롤러를 테스트합니다.
`app.module.ts` | 애플리케이션의 루트 모듈입니다.
`app.service.ts` | 단일 방법으로 기본 서비스.
`main.ts` | Nest 애플리케이션 인스턴스를 생성하기 위해 `NestFactory` 를 사용하는 애플리케이션의 엔트리 파일.

`main.ts` 에는 애플리케이션 **을 부트 스트랩** 하는 비동기 함수가 포함되어 있습니다.

```typescript
@@filename(main)

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
@@switch
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Nest 애플리케이션 인스턴스를 생성하기 위해 핵심 `NestFactory` 클래스를 사용합니다. `NestFactory` 는 애플리케이션 인스턴스를 만들 수있는 몇 가지 정적 메서드를 제공합니다. `create()` `INestApplication` 인터페이스를 충족하는 응용 프로그램 객체를 반환합니다. 이 개체는 다음 장에서 설명하는 일련의 메서드를 제공합니다. 위의 `main.ts` 예제에서는 애플리케이션이 인바운드 HTTP 요청을 기다릴 수 있도록 HTTP 리스너를 시작하기 만하면됩니다.

Nest CLI로 스캐 폴딩 된 프로젝트는 개발자가 각 모듈을 자체 전용 디렉터리에 보관하는 규칙을 따르도록 권장하는 초기 프로젝트 구조를 생성합니다.

<app-banner-courses></app-banner-courses>

#### 플랫폼

Nest는 플랫폼에 구애받지 않는 프레임 워크를 목표로합니다. 플랫폼 독립성을 통해 개발자가 여러 유형의 응용 프로그램에서 활용할 수있는 재사용 가능한 논리적 부분을 만들 수 있습니다. 기술적으로 Nest는 어댑터가 생성되면 모든 Node HTTP 프레임 워크에서 작동 할 수 있습니다. [기본적으로](https://www.fastify.io) 지원되는 두 가지 HTTP 플랫폼 인 [express](https://expressjs.com/) 및 fastify가 있습니다. 귀하의 필요에 가장 적합한 것을 선택할 수 있습니다.

 |
--- | ---
`platform-express` | [Express](https://expressjs.com/) 는 노드를위한 잘 알려진 미니멀 웹 프레임 워크입니다. 커뮤니티에서 구현 한 많은 리소스를 갖춘 전투 테스트를 거친 프로덕션 준비 라이브러리입니다. 기본적으로 `@nestjs/platform-express` 패키지가 사용됩니다. 많은 사용자가 Express를 잘 사용하고 있으며이를 활성화하기 위해 조치를 취할 필요가 없습니다.
`platform-fastify` | [Fastify](https://www.fastify.io/) 는 최대 효율성과 속도를 제공하는 데 중점을 둔 고성능 및 낮은 오버 헤드 프레임 워크입니다. [여기](/techniques/performance) 에서 사용 방법을 읽어보십시오.

어떤 플랫폼을 사용하든 자체 애플리케이션 인터페이스를 노출합니다. 이들은 각각 `NestExpressApplication` 및 `NestFastifyApplication` 됩니다.

아래 예제에서와 같이 `NestFactory.create()` 메서드에 유형을 전달 `app` 객체에 해당 특정 플랫폼에서만 사용할 수있는 메서드가 있습니다. 그러나 실제로 기본 플랫폼 API에 액세스하려는 **경우가 아니면** 유형을 지정할 **필요** 가 없습니다.

```typescript
const app = await NestFactory.create<NestExpressApplication>(AppModule);
```

#### 응용 프로그램 실행

설치 프로세스가 완료되면 OS 명령 프롬프트에서 다음 명령을 실행하여 인바운드 HTTP 요청을 수신하는 애플리케이션을 시작할 수 있습니다.

```bash
$ npm run start
```

`src/main.ts` 파일에 정의 된 포트에서 수신하는 HTTP 서버로 앱을 시작합니다. 애플리케이션이 실행되면 브라우저를 열고 `http://localhost:3000/` 합니다. `Hello World!` 가 보여야합니다! 메시지.
